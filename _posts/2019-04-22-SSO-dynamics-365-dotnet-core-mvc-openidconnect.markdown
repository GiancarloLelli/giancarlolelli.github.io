---
layout: post
title:  "SSO con ASP.NET Core e Dynamics 365"
date:   2019-04-22 9:00:00 +0200
categories: post
---
{: style="text-align: justify"}
Qualche giorno fa mi sono imbattuto nella seguente richiesta *implementare il SSO tra Dynamics 365 e una applicazione ASP.NET Core" con l'obbiettivo di avere un token e un contesto di autenticazione valido sia per l'applicazione custom che per Dynamics 365. Nel contesto di Core era una cosa che avevo già visto, quindi dopo aver configurato il middleware di OpenIdConnect come si deve sono riuscito nell'intento. Di seguito il codice:

**In Startup.cs**
```csharp
public void ConfigureServices(IServiceCollection services)
{
    Configuration.Bind("DYN365", _dynamicsData);
    services.AddSingleton<TokenDataContainer>(_tokenDataContainer);

    services.AddAuthentication(sharedOptions =>
    {
        sharedOptions.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        sharedOptions.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddOpenIdConnect(options =>
    {
        Configuration.Bind("SSO", options);
        options.Events.OnAuthorizationCodeReceived = OnAuthorizationCodeReceived;
    })
    .AddCookie();

    services.AddMvc(options =>
    {
        var policy = new AuthorizationPolicyBuilder()
                        .RequireAuthenticatedUser()
                        .Build();

        options.Filters.Add(new AuthorizeFilter(policy));
    });
}

private async Task OnAuthorizationCodeReceived(AuthorizationCodeReceivedContext arg)
{
    var creds = new ClientCredential(arg.Options.ClientId, _dynamicsData.Secret);
    var redirectUri = new Uri($"{arg.Request.Scheme}://{arg.Request.Host}{arg.Options.CallbackPath}");

    var authContext = new AuthenticationContext(_dynamicsData.Authority);
    var authResult = await authContext.AcquireTokenByAuthorizationCodeAsync(arg.ProtocolMessage.Code, redirectUri, creds);

    _tokenDataContainer.Add(arg.Principal.Identity.Name, authResult);

    arg.HandleCodeRedemption(authResult.AccessToken, arg.ProtocolMessage.IdToken);
}
```
Storia diversa per ASP.NET MVC su FullFX. Con questo stack non avevo mai provato, ma fortunatamente la similitudine dell'API e dei concetti mi ha consentito di adattare anche qua il middleware e impostare lo stesso tipo di meccanismo di SSO. Di seguito il codice.

**In Startup.Auth.cs**
```csharp
public partial class Startup
{
    private static string ClientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string AADInstance = ConfigurationManager.AppSettings["ida:AADInstance"];
    private static string TenantId = ConfigurationManager.AppSettings["ida:TenantId"];
    private static string PostLogoutRedirectUri = ConfigurationManager.AppSettings["ida:PostLogoutRedirectUri"];
    private static string ClientSecret = ConfigurationManager.AppSettings["ida:ClientSecret"];
    private static string Resource = ConfigurationManager.AppSettings["ida:Resource"];
    private static string Authority = AADInstance + TenantId;

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);
        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        OpenIdConnectAuthenticationOptions openIdConnectOptions = new OpenIdConnectAuthenticationOptions();
        openIdConnectOptions.ClientId = ClientId;
        openIdConnectOptions.Authority = Authority;
        openIdConnectOptions.PostLogoutRedirectUri = PostLogoutRedirectUri;
        openIdConnectOptions.ResponseType = "code id_token";
        openIdConnectOptions.Resource = Resource;

        if (openIdConnectOptions.Notifications == null)
            openIdConnectOptions.Notifications = new OpenIdConnectAuthenticationNotifications();

        openIdConnectOptions.Notifications.AuthorizationCodeReceived = AuthorizationCodeReceived;

        app.UseOpenIdConnectAuthentication(openIdConnectOptions);
    }

    private async Task AuthorizationCodeReceived(AuthorizationCodeReceivedNotification arg)
    {
        var creds = new ClientCredential(arg.Options.ClientId, ClientSecret);
        var authContext = new AuthenticationContext(Authority);
        var redirectUri = new Uri(PostLogoutRedirectUri);
        var authResult = await authContext.AcquireTokenByAuthorizationCodeAsync(arg.ProtocolMessage.Code, redirectUri, creds);
        MvcApplication.Container.Add(arg.AuthenticationTicket.Identity.Name, authResult);
    }
}
```
Il succo di tutto è l'ottenimento di un *authentication code* dal flusso OAuth che permette poi tramite ADAL di chiedere un **Access Token** vero e proprio. 