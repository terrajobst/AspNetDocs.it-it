---
uid: webhooks/receiving/receivers
title: Ricevitori webhook ASP.NET | Microsoft Docs
author: rick-anderson
description: Ricevitori webhook ASP.NET
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 6cdea089-15b2-4732-8c68-921ca561a8f1
ms.openlocfilehash: d771a588b23abcd7b1b33e694af17b219683fc48
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574193"
---
# <a name="aspnet-webhooks-receivers"></a>Ricevitori webhook ASP.NET

La ricezione dei webhook dipende dall'identità del mittente. A volte sono presenti passaggi aggiuntivi per la registrazione di un webhook che verifica che il Sottoscrittore sia effettivamente in ascolto. Alcuni webhook forniscono un modello push a Pull in cui la richiesta HTTP POST contiene solo un riferimento alle informazioni sull'evento che devono essere recuperate in modo indipendente. Spesso il modello di sicurezza varia leggermente.

Lo scopo dei webhook di Microsoft ASP.NET consiste nel rendere più semplice e coerente la connessione dell'API senza spendere molto tempo per capire come gestire una particolare variante di webhook.

Un ricevitore del webhook è responsabile dell'accettazione e della verifica dei webhook da un determinato mittente. Un ricevitore webhook può supportare un numero qualsiasi di Webhook, ognuno con la propria configurazione. Il ricevitore del webhook GitHub, ad esempio, può accettare webhook da un numero qualsiasi di repository GitHub.

## <a name="webhook-receiver-uris"></a>URI del ricevitore del webhook

Installando Microsoft ASP.NET webhook si ottiene un controller webhook generale che accetta le richieste di Webhook da un numero aperto di servizi. Quando arriva una richiesta, viene scelto il ricevitore appropriato installato per la gestione di un mittente del webhook specifico.

L'URI di questo controller è l'URI del webhook registrato con il servizio ed è nel formato seguente:

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

Per motivi di sicurezza, molti ricevitori di Webhook richiedono che l'URI sia un URI *https* e, in alcuni casi, deve contenere anche un parametro di query aggiuntivo che viene usato per fare in modo che solo l'entità prevista possa inviare webhook all'URI sopra.

Il componente `<receiver>` è il nome del ricevitore, ad esempio `github` o `slack`.

*{ID}* è un identificatore facoltativo che può essere usato per identificare una particolare configurazione del ricevitore del webhook. Questa operazione può essere usata per registrare N webhook con un destinatario specifico. Ad esempio, è possibile usare i tre URI seguenti per eseguire la registrazione per tre webhook indipendenti:

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a>Installazione di un ricevitore webhook

Per ricevere webhook con Microsoft ASP.NET webhook, è necessario innanzitutto installare il pacchetto NuGet per il provider webhook o i provider da cui si vogliono ricevere i webhook. I pacchetti NuGet sono denominati [Microsoft. AspNet. webhooks. receivers. *,](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) dove l'ultima parte indica che il servizio è supportato. Esempio:

[Microsoft. AspNet. webhooks. receivers. github](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce supporto per la ricezione di Webhook da GitHub e [Microsoft. AspNet. webhooks. receivers. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce il supporto per la ricezione di Webhook generati da webhook ASP.NET.

È possibile trovare supporto per Dropbox, GitHub, MailChimp, PayPal, Pushr, Salesforce, Slack, striping, Trello e WordPress, ma è possibile supportare un numero qualsiasi di altri provider.

## <a name="configuring-a-webhook-receiver"></a>Configurazione di un ricevitore webhook

I ricevitori webhook sono configurati tramite il interfaccia [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) e le specifiche implementazioni di tale interfaccia possono essere registrate usando qualsiasi modello di inserimento delle dipendenze. L'implementazione predefinita usa le impostazioni dell'applicazione che possono essere impostate nel file Web. config oppure, se si usa app Web di Azure, possono essere impostate tramite il [portale di Azure](https://portal.azure.com/).

![Impostazioni app di Azure](_static/AzureAppSettings.png)

Il formato per le chiavi di impostazione dell'applicazione è il seguente:

```
MS_WebHookReceiverSecret_<receiver>
```

Il valore è un elenco delimitato da virgole di valori corrispondenti ai valori *{ID}* per i quali sono stati registrati i webhook, ad esempio:

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a>Inizializzazione di un ricevitore webhook

I ricevitori webhook vengono inizializzati tramite la relativa registrazione, in genere nella classe statica *WebApiConfig* , ad esempio:

```csharp
namespace WebHookReceivers
{
    public static class WebApiConfig
    {
        public static void Register(HttpConfiguration config)
        {
            // Web API configuration and services

            // Web API routes
            config.MapHttpAttributeRoutes();

            config.Routes.MapHttpRoute(
                name: "DefaultApi",
                routeTemplate: "api/{controller}/{id}",
                defaults: new { id = RouteParameter.Optional }
            );

            // Load receivers
            config.InitializeReceiveGitHubWebHooks();
        }
    }
}
```
