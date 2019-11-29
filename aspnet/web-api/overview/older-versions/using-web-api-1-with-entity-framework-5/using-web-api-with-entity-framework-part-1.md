---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: 'Parte 1: panoramica e creazione del progetto | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/03/2012
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: a76a18f2bd95969358452085ef342fdca8a386e2
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600318"
---
# <a name="part-1-overview-and-creating-the-project"></a>Parte 1: panoramica e creazione del progetto

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework è un Framework di mapping relazionale a oggetti. Esegue il mapping degli oggetti di dominio nel codice alle entità in un database relazionale. Nella maggior parte dei casi, non è necessario preoccuparsi del livello di database, perché Entity Framework lo gestisce automaticamente. Il codice manipola gli oggetti e le modifiche vengono salvate in modo permanente in un database.

## <a name="about-the-tutorial"></a>Informazioni sull'esercitazione

In questa esercitazione verrà creata una semplice applicazione di archiviazione. Sono presenti due parti principali per l'applicazione. Gli utenti normali possono visualizzare i prodotti e creare gli ordini:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Gli amministratori possono creare, eliminare o modificare i prodotti:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Competenze apprese

Ecco cosa si apprenderà:

- Come usare Entity Framework con API Web ASP.NET.
- Come usare knockout. js per creare un'interfaccia utente client dinamica.
- Come usare l'autenticazione basata su form con l'API Web per autenticare gli utenti.

Sebbene questa esercitazione sia autonoma, è consigliabile leggere prima le esercitazioni seguenti:

- [Il primo API Web ASP.NET](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Creazione di un'API Web che supporta operazioni CRUD](../creating-a-web-api-that-supports-crud-operations.md)

È inoltre utile una certa conoscenza di [ASP.NET MVC](../../../../mvc/index.md) .

## <a name="overview"></a>Panoramica di

A livello generale, di seguito è illustrata l'architettura dell'applicazione:

- ASP.NET MVC genera le pagine HTML per il client.
- API Web ASP.NET espone le operazioni CRUD sui dati (prodotti e ordini).
- Entity Framework converte i C# modelli utilizzati dall'API Web in entità di database.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

Il diagramma seguente illustra il modo in cui gli oggetti di dominio sono rappresentati a diversi livelli dell'applicazione, ovvero il livello del database, il modello a oggetti e infine il formato wire, usato per trasmettere i dati al client tramite HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Creare il progetto di Visual Studio

È possibile creare il progetto Tutorial utilizzando Visual Web Developer Express o la versione completa di Visual Studio.

Nella pagina **iniziale** fare clic su **nuovo progetto**.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Denominare il progetto "ProductStore" e fare clic su **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

Nella finestra di dialogo **New ASP.NET MVC 4 Project** selezionare **applicazione Internet** e fare clic su **OK**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

Il modello "applicazione Internet" crea un'applicazione MVC ASP.NET che supporta l'autenticazione basata su form. Se l'applicazione viene eseguita adesso, dispone già di alcune funzionalità:

- I nuovi utenti possono registrarsi facendo clic sul collegamento "Register" nell'angolo superiore destro.
- Gli utenti registrati possono eseguire l'accesso facendo clic sul collegamento "Accedi".

Le informazioni di appartenenza vengono rese permanente in un database che viene creato automaticamente. Per ulteriori informazioni sull'autenticazione basata su form in ASP.NET MVC, vedere [procedura dettagliata: uso dell'autenticazione basata su form in ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Aggiornare il file CSS

Questo passaggio è cosmetico, ma renderà il rendering delle pagine come le schermate precedenti.

In Esplora soluzioni espandere la cartella contenuto e aprire il file denominato site. CSS. Aggiungere gli stili CSS seguenti:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

> [!div class="step-by-step"]
> [avanti](using-web-api-with-entity-framework-part-2.md)
