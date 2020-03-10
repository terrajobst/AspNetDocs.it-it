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
# <a name="aspnet-webhooks-receivers"></a><span data-ttu-id="dbcdf-103">Ricevitori webhook ASP.NET</span><span class="sxs-lookup"><span data-stu-id="dbcdf-103">ASP.NET WebHooks receivers</span></span>

<span data-ttu-id="dbcdf-104">La ricezione dei webhook dipende dall'identità del mittente.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-104">Receiving WebHooks depends on who the sender is.</span></span> <span data-ttu-id="dbcdf-105">A volte sono presenti passaggi aggiuntivi per la registrazione di un webhook che verifica che il Sottoscrittore sia effettivamente in ascolto.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-105">Sometimes there are additional steps registering a WebHook verifying that the subscriber is really listening.</span></span> <span data-ttu-id="dbcdf-106">Alcuni webhook forniscono un modello push a Pull in cui la richiesta HTTP POST contiene solo un riferimento alle informazioni sull'evento che devono essere recuperate in modo indipendente.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-106">Some WebHooks provide a push-to-pull model where the HTTP POST request only contains a reference to the event information which is then to be retrieved independently.</span></span> <span data-ttu-id="dbcdf-107">Spesso il modello di sicurezza varia leggermente.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-107">Often the security model varies quite a bit.</span></span>

<span data-ttu-id="dbcdf-108">Lo scopo dei webhook di Microsoft ASP.NET consiste nel rendere più semplice e coerente la connessione dell'API senza spendere molto tempo per capire come gestire una particolare variante di webhook.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-108">The purpose of Microsoft ASP.NET WebHooks is to make it both simpler and more consistent to wire up your API without spending a lot of time figuring out how to handle any particular variant of WebHooks.</span></span>

<span data-ttu-id="dbcdf-109">Un ricevitore del webhook è responsabile dell'accettazione e della verifica dei webhook da un determinato mittente.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-109">A WebHook receiver is responsible for accepting and verifying WebHooks from a particular sender.</span></span> <span data-ttu-id="dbcdf-110">Un ricevitore webhook può supportare un numero qualsiasi di Webhook, ognuno con la propria configurazione.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-110">A WebHook receiver can support any number of WebHooks, each with their own configuration.</span></span> <span data-ttu-id="dbcdf-111">Il ricevitore del webhook GitHub, ad esempio, può accettare webhook da un numero qualsiasi di repository GitHub.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-111">For example, the GitHub WebHook receiver can accept WebHooks from any number of GitHub repositories.</span></span>

## <a name="webhook-receiver-uris"></a><span data-ttu-id="dbcdf-112">URI del ricevitore del webhook</span><span class="sxs-lookup"><span data-stu-id="dbcdf-112">WebHook Receiver URIs</span></span>

<span data-ttu-id="dbcdf-113">Installando Microsoft ASP.NET webhook si ottiene un controller webhook generale che accetta le richieste di Webhook da un numero aperto di servizi.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-113">By installing Microsoft ASP.NET WebHooks you get a general WebHook controller which accepts WebHook requests from an open-ended number of services.</span></span> <span data-ttu-id="dbcdf-114">Quando arriva una richiesta, viene scelto il ricevitore appropriato installato per la gestione di un mittente del webhook specifico.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-114">When a request arrives, it picks the appropriate receiver that you have installed for handling a particular WebHook sender.</span></span>

<span data-ttu-id="dbcdf-115">L'URI di questo controller è l'URI del webhook registrato con il servizio ed è nel formato seguente:</span><span class="sxs-lookup"><span data-stu-id="dbcdf-115">The URI of this controller is the WebHook URI that you register with the service and is of the form:</span></span>

```
https://<host>/api/webhooks/incoming/<receiver>/{id}
```

<span data-ttu-id="dbcdf-116">Per motivi di sicurezza, molti ricevitori di Webhook richiedono che l'URI sia un URI *https* e, in alcuni casi, deve contenere anche un parametro di query aggiuntivo che viene usato per fare in modo che solo l'entità prevista possa inviare webhook all'URI sopra.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-116">For security reasons, many WebHook receivers require that the URI is an *https* URI and in some cases it must also contain an additional query parameter which is used to enforce that only the intended party can send WebHooks to the URI above.</span></span>

<span data-ttu-id="dbcdf-117">Il componente `<receiver>` è il nome del ricevitore, ad esempio `github` o `slack`.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-117">The `<receiver>` component is the name of the receiver, for example `github` or `slack`.</span></span>

<span data-ttu-id="dbcdf-118">*{ID}* è un identificatore facoltativo che può essere usato per identificare una particolare configurazione del ricevitore del webhook.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-118">The *{id}* is an optional identifier which can be used to identify a particular WebHook receiver configuration.</span></span> <span data-ttu-id="dbcdf-119">Questa operazione può essere usata per registrare N webhook con un destinatario specifico.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-119">This can be used to register N WebHooks with a particular receiver.</span></span> <span data-ttu-id="dbcdf-120">Ad esempio, è possibile usare i tre URI seguenti per eseguire la registrazione per tre webhook indipendenti:</span><span class="sxs-lookup"><span data-stu-id="dbcdf-120">For example, the following three URIs can be used to register for three independent WebHooks:</span></span>

```
https://<host>/api/webhooks/incoming/github
https://<host>/api/webhooks/incoming/github/12345
https://<host>/api/webhooks/incoming/github/54321
```

## <a name="installing-a-webhook-receiver"></a><span data-ttu-id="dbcdf-121">Installazione di un ricevitore webhook</span><span class="sxs-lookup"><span data-stu-id="dbcdf-121">Installing a WebHook Receiver</span></span>

<span data-ttu-id="dbcdf-122">Per ricevere webhook con Microsoft ASP.NET webhook, è necessario innanzitutto installare il pacchetto NuGet per il provider webhook o i provider da cui si vogliono ricevere i webhook.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-122">To receive WebHooks using Microsoft ASP.NET WebHooks, you first install the Nuget package for the WebHook provider or providers you want to receive WebHooks from.</span></span> <span data-ttu-id="dbcdf-123">I pacchetti NuGet sono denominati [Microsoft. AspNet. webhooks. receivers. \*,](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) dove l'ultima parte indica che il servizio è supportato.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-123">The Nuget packages are named [Microsoft.AspNet.WebHooks.Receivers.\*](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers) where the last part indicates the service supported.</span></span> <span data-ttu-id="dbcdf-124">Esempio:</span><span class="sxs-lookup"><span data-stu-id="dbcdf-124">For example</span></span>

<span data-ttu-id="dbcdf-125">[Microsoft. AspNet. webhooks. receivers. github](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) fornisce supporto per la ricezione di Webhook da GitHub e [Microsoft. AspNet. webhooks. receivers. Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) fornisce il supporto per la ricezione di Webhook generati da webhook ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-125">[Microsoft.AspNet.WebHooks.Receivers.GitHub](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.GitHub) provides support for receiving WebHooks from GitHub and [Microsoft.AspNet.WebHooks.Receivers.Custom](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers.Custom) provides support for receiving WebHooks generated by ASP.NET WebHooks.</span></span>

<span data-ttu-id="dbcdf-126">È possibile trovare supporto per Dropbox, GitHub, MailChimp, PayPal, Pushr, Salesforce, Slack, striping, Trello e WordPress, ma è possibile supportare un numero qualsiasi di altri provider.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-126">Out of the box you can find support for Dropbox, GitHub, MailChimp, PayPal, Pusher, Salesforce, Slack, Stripe, Trello, and WordPress but it is possible to support any number of other providers.</span></span>

## <a name="configuring-a-webhook-receiver"></a><span data-ttu-id="dbcdf-127">Configurazione di un ricevitore webhook</span><span class="sxs-lookup"><span data-stu-id="dbcdf-127">Configuring a WebHook Receiver</span></span>

<span data-ttu-id="dbcdf-128">I ricevitori webhook sono configurati tramite il interfaccia [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) e le specifiche implementazioni di tale interfaccia possono essere registrate usando qualsiasi modello di inserimento delle dipendenze.</span><span class="sxs-lookup"><span data-stu-id="dbcdf-128">WebHook Receivers are configured through the [IWebHookReceiverConfig](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/WebHooks/IWebHookReceiverConfig.cs) inteface and particular implementations of that interface can be registered using any dependency injection model.</span></span> <span data-ttu-id="dbcdf-129">L'implementazione predefinita usa le impostazioni dell'applicazione che possono essere impostate nel file Web. config oppure, se si usa app Web di Azure, possono essere impostate tramite il [portale di Azure](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="dbcdf-129">The default implementation uses Application Settings which can either be set in the Web.config file, or, if using Azure Web Apps, can be set through the [Azure Portal](https://portal.azure.com/).</span></span>

![Impostazioni app di Azure](_static/AzureAppSettings.png)

<span data-ttu-id="dbcdf-131">Il formato per le chiavi di impostazione dell'applicazione è il seguente:</span><span class="sxs-lookup"><span data-stu-id="dbcdf-131">The format for Application Setting keys is as follows:</span></span>

```
MS_WebHookReceiverSecret_<receiver>
```

<span data-ttu-id="dbcdf-132">Il valore è un elenco delimitato da virgole di valori corrispondenti ai valori *{ID}* per i quali sono stati registrati i webhook, ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dbcdf-132">The value is a comma-separated list of values matching the *{id}* values for which WebHooks have been registered, for example:</span></span>

```
MS_WebHookReceiverSecret_GitHub = <secret1>, 12345=<secret2>, 54321=<secret3>
```

## <a name="initializing-a-webhook-receiver"></a><span data-ttu-id="dbcdf-133">Inizializzazione di un ricevitore webhook</span><span class="sxs-lookup"><span data-stu-id="dbcdf-133">Initializing a WebHook Receiver</span></span>

<span data-ttu-id="dbcdf-134">I ricevitori webhook vengono inizializzati tramite la relativa registrazione, in genere nella classe statica *WebApiConfig* , ad esempio:</span><span class="sxs-lookup"><span data-stu-id="dbcdf-134">WebHook Receivers are initialized by registering them, typically in the *WebApiConfig* static class, for example:</span></span>

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
