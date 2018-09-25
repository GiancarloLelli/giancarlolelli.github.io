---
layout: post
title:  "Password reset account domain controller su Azure VM"
date:   2018-09-24 9:00:00 +0200
categories: troubleshooting
---
{: style="text-align: justify"}
Da poco, mi son rietrovato nella sgradevole sensazione di non ricordarmi più la password dell'account domain controller di una mia VM su Azure che ospita una versione on-premise di Dynamics 365. Il che ovviamente è un problema, in quanto mi serviva creare delle nuove utente e fare altre attività di manutenzione. Fortunatamente, sono riuscito a risolvere tramite la funzionalità di remote command execution che il portale di Azure mette a disposizione. Tramite l'opzione *Run Powershell script* ho eseguito uno script di password reset e sono poi riuscito a ri loggarmi all'interno della VM - il comando che ho usato è il seguente:
```powershell
Set-ADAccountPassword -Identity MyUser -NewPassword (ConvertTo-SecureString -AsPlainText "qwert@12345" -Force) -Reset
```
{: style="text-align: justify"}
E' strano notare come la funzionalità dedicata del portale di Azure di password reset falliva - in quanto a detta del messaggio di errore i domain controller, o gli account con ruolo domain controller non sono supportati. Fortunatamente per me questo CMDLet invece ha fatto il suo dovere.

