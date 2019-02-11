---
layout: post
title:  "Leggere le info dell'utente associato a Visual Studio da una estensione"
date:   2019-02-11 9:00:00 +0200
categories: blog
---
{: style="text-align: justify"}
Come ho già bloggato tempo fa, sul [marketplace di Visual Studio](https://marketplace.visualstudio.com/items?itemName=GiancarloLelli.AvanadeCRMToolkit) ho pubblicato una estensione - ad oggi già compatibile con Visual Studio 2019 - che permette di gestire in maniera più strutturata lo sviluppo di customizzazioni Front-end per Dynamics 365. Nello sviluppo di questa estensione, mi sono trovato nella necessità di mettere in piedi un livello di telemetria che mi facesse capire come gli utenti alla fine della fiera utilizzano questo mio prodotto. Alla ricerca di informazioni sempre più significative, mi sono imbattuto nel need di leggere le informazioni dell'utente attualmente collegato in Visual Studio. CI sono riuscito senza troppa fatica e vi riporto il codice in basso. Lo snippet nel dettaglio, vi permette di leggere dal registro di Windows le seguenti info: **EmailAddress**, **DisplayName**  e **ProfileURI**
```csharp
using CRMDevLabs.Toolkit.Models.Telemetry;
using Microsoft.Win32;

namespace CRMDevLabs.Toolkit.Telemetry
{
    public class RegistryKeyReader
    {
        public static VisualStudioUserInfo GetUserInfo()
        {
            VisualStudioUserInfo info = null;

            try
            {
                var subKey = "Software\\Microsoft\\VSCommon\\ConnectedUser\\IdeUser\\Cache";
                var emailAddressKeyName = "EmailAddress";
                var userNameKeyName = "DisplayName";
                var profileKeyName = "ProfileUri";

                var root = Registry.CurrentUser;
                var sk = root.OpenSubKey(subKey);

                if (sk != null)
                {
                    var email = sk.GetValue(emailAddressKeyName) as string;
                    var user = sk.GetValue(userNameKeyName) as string;
                    var profile = sk.GetValue(profileKeyName) as string;

                    info = new VisualStudioUserInfo
                    {
                        Email = email,
                        Name = user,
                        Url = profile
                    };
                }
            }
            catch (System.Exception)
            {

            }

            return info;
        }
    }
}
```
