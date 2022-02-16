---
layout: post
title:  "Ottenere il Tenant ID di Azure AD in maniera anonima con .NET Core"
date:   2019-02-18 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Recentemente un amico aveva la necesità di scoprire il Tenant ID di una Azure Active Directory avendo solamente a disposizione in nome del dominio (es: pluto.onmicrosoft.com) - A prima vista sembra una cosa banale, ma in effetti portare a termine questo mini task richiede la conoscenza di come funziona il meccanismo di discovery di OpenID Connect. Infatti dopo una veloce ricerca sul web, che inevitabilmente ci ha potato su StackOverflow abbiamo visto come l'endpoint: https://login.windows.net/tenant_name.onmicrosoft.com/.well-known/openid-configuration se sollecitato restituisce un JSON che in varie su proprietà riporta proprio il Tenant ID. [Questa](http://docs.identityserver.io/en/latest/endpoints/discovery.html) la doc ufficiale di IdentityServer che da qualche dettgalio in più. In basso un esempio di JSON per un mio tenant personale:
```json
{
"authorization_endpoint": "https://login.windows.net/f61774b9-redacted-redacted-8a41-653537db1428/oauth2/authorize",
"token_endpoint": "https://login.windows.net/f61774b9-redacted-redacted-8a41-653537db1428/oauth2/token",
"token_endpoint_auth_methods_supported": [
    "client_secret_post",
    "private_key_jwt",
    "client_secret_basic"
],
"jwks_uri": "https://login.windows.net/common/discovery/keys",
"response_modes_supported": [
    "query",
    "fragment",
    "form_post"
],
"subject_types_supported": [
    "pairwise"
],
"id_token_signing_alg_values_supported": [
    "RS256"
],
"http_logout_supported": true,
"frontchannel_logout_supported": true,
"end_session_endpoint": "https://login.windows.net/f61774b9-redacted-redacted-8a41-653537db1428/oauth2/logout",
"response_types_supported": [
    "code",
    "id_token",
    "code id_token",
    "token id_token",
    "token"
],
"scopes_supported": [
    "openid"
],
"issuer": "https://sts.windows.net/f61774b9-redacted-redacted-8a41-653537db1428/",
"claims_supported": [
    "sub",
    "iss",
    "cloud_instance_name",
    "cloud_instance_host_name",
    "cloud_graph_host_name",
    "msgraph_host",
    "aud",
    "exp",
    "iat",
    "auth_time",
    "acr",
    "amr",
    "nonce",
    "email",
    "given_name",
    "family_name",
    "nickname"
],
"microsoft_multi_refresh_token": true,
"check_session_iframe": "https://login.windows.net/f61774b9-redacted-redacted-8a41-653537db1428/oauth2/checksession",
"userinfo_endpoint": "https://login.windows.net/f61774b9-redacted-redacted-8a41-653537db1428/openid/userinfo",
"tenant_region_scope": "EU",
"cloud_instance_name": "microsoftonline.com",
"cloud_graph_host_name": "graph.windows.net",
"msgraph_host": "graph.microsoft.com",
"rbac_url": "https://pas.windows.net"
}
```
Nei prossimi giorni pubblicherò un package NuGet che si occuperà di fare questo sporco lavoro, oppure una PR sul repo di ADAL...vediamo.