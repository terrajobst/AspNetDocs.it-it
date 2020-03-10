---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Informazioni sul processo di esecuzione di ASP.NET MVC | Microsoft Docs
author: microsoft
description: Informazioni su come il framework ASP.NET MVC elabora una richiesta del browser in modo dettagliato.
ms.author: riande
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 28940947253e0af43886cf1231f8aaf4615526cc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541517"
---
# <a name="understanding-the-aspnet-mvc-execution-process"></a>Informazioni sul processo di esecuzione di ASP.NET MVC

[Microsoft](https://github.com/microsoft)

> Informazioni su come il framework ASP.NET MVC elabora una richiesta del browser in modo dettagliato.

Le richieste a un'applicazione Web basata su MVC ASP.NET passano prima di tutto l'oggetto **UrlRoutingModule** , che è un modulo HTTP. Questo modulo analizza la richiesta ed esegue la selezione delle route. L'oggetto **UrlRoutingModule** seleziona il primo oggetto route che corrisponde alla richiesta corrente. Un oggetto route è una classe che implementa **RouteBase**ed è in genere un'istanza della classe **Route** . Se nessuna route corrisponde, l'oggetto **UrlRoutingModule** non esegue alcuna operazione e consente alla richiesta di eseguire il fallback alla normale elaborazione della richiesta ASP.NET o IIS.

Dall'oggetto **Route** selezionato, l'oggetto **UrlRoutingModule** Ottiene l'oggetto **IRouteHandler** associato all'oggetto **Route** . In genere, in un'applicazione MVC questa sarà un'istanza di **MvcRouteHandler**. L'istanza **IRouteHandler** crea un oggetto **IHttpHandler** e lo passa all'oggetto **IHttpContext** . Per impostazione predefinita, l'istanza di **IHttpHandler** per MVC è l'oggetto **MvcHandler** . L'oggetto **MvcHandler** seleziona quindi il controller che gestirà la richiesta.

> [!NOTE]
> Quando un'applicazione MVC ASP.NET viene eseguita in IIS 7.0, per i progetti MVC non è necessaria alcuna estensione di file. In IIS 6.0 il gestore richiede tuttavia che si esegua il mapping dell'estensione di file mvc alla DLL ISAPI ASP.NET.

Il modulo e il gestore sono i punti di ingresso per il framework MVC ASP.NET. e consentono di eseguire le seguenti azioni:

- Selezionare il controller appropriato in un'applicazione Web MVC.
- Ottenere un'istanza del controller specifica.
- Chiamare il metodo **Execute** del controller.

Di seguito sono elencate le fasi di esecuzione per un progetto Web MVC:

- Ricezione della prima richiesta per l'applicazione 

    - Nel file Global. asax gli oggetti **Route** vengono aggiunti all'oggetto **RouteTable** .
- Esecuzione del routing 

    - Il modulo **UrlRoutingModule** usa il primo oggetto **Route** corrispondente nella raccolta **RouteTable** per creare l'oggetto **RouteData** , che viene quindi usato per creare un oggetto **RequestContext** (**IHttpContext**).
- Creazione del gestore di richieste MVC 

    - L'oggetto **MvcRouteHandler** crea un'istanza della classe **MvcHandler** e la passa all'istanza **RequestContext** .
- Creazione del controller 

    - L'oggetto **MvcHandler** usa l'istanza **RequestContext** per identificare l'oggetto **IControllerFactory** (in genere un'istanza della classe **DefaultControllerFactory** ) con cui creare l'istanza del controller.
- Esegui controller: l'istanza di **MvcHandler** chiama il metodo **Execute** del controller. |
- Richiamo dell'azione 

    - La maggior parte dei controller eredita dalla classe di base del **controller** . Per i controller che eseguono questa operazione, l'oggetto **ControllerActionInvoker** associato al controller determina il metodo di azione della classe controller da chiamare e quindi chiama tale metodo.
- Esecuzione del risultato 

    - Un metodo di azione tipico può ricevere l'input dell'utente, preparare i dati di risposta appropriati, quindi eseguire il risultato restituendo un tipo di risultato. I tipi di risultati predefiniti che è possibile eseguire includono i seguenti: **ViewResult** (che esegue il rendering di una visualizzazione ed è il tipo di risultato usato più spesso), **RedirectToRouteResult**, **RedirectResult**, **ContentResult**, **JsonResult**e **EmptyResult**.
