---
uid: web-pages/overview/ui-layouts-and-themes/installing-helpers
title: Installazione di un helper in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive come installare un helper in un sito Web di Pagine Web ASP.NET (Razor). Un helper è un componente riutilizzabile che include codice e markup a per...
ms.author: riande
ms.date: 02/18/2014
ms.assetid: 5e968ead-906a-45ea-ac2a-c70e57e1a9b1
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/installing-helpers
msc.type: authoredcontent
ms.openlocfilehash: 41e33c04a53a6ad257c3937cdadcec767e9217c8
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78638586"
---
# <a name="installing-a-helper-in-an-aspnet-web-pages-razor-site"></a>Installazione di un helper in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come installare un helper in un sito Web di Pagine Web ASP.NET (Razor). Un *Helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che può essere noiosa o complessa.
> 
> Contenuto dell'esercitazione:
> 
> - Come installare un helper in un sito Web creato con WebMatrix 3.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - WebMatrix 3

## <a name="overview-of-helpers"></a>Panoramica degli helper

Alcune attività che spesso gli utenti desiderano eseguire sulle pagine Web richiedono una grande quantità di codice o richiedono una conoscenza aggiuntiva. Gli esempi includono la visualizzazione di un grafico per i dati; inserimento di un pulsante "follow" di Twitter in una pagina; invio di messaggi di posta elettronica dal sito Web; ritaglio o ridimensionamento delle immagini; utilizzando PayPal per il sito. Per semplificare l'esecuzione di questi tipi di operazioni, Pagine Web ASP.NET consente di utilizzare gli *Helper*. Gli helper sono componenti da installare per un sito e che consentono di eseguire attività tipiche usando solo una riga o due di codice Razor.

Pagine Web ASP.NET dispone di alcuni Helper incorporati. Tuttavia, molti helper sono disponibili nei pacchetti (componenti aggiuntivi) forniti tramite Gestione pacchetti NuGet. NuGet consente di selezionare un pacchetto da installare e quindi si occupa di tutti i dettagli dell'installazione.

## <a name="installing-a-helper-in-webmatrix-3"></a>Installazione di un helper in WebMatrix 3

1. In WebMatrix 3 fare clic sul pulsante **NuGet** .

    ![Finestra di dialogo raccolta NuGet in WebMatrix](installing-helpers/_static/image1.png)
2. Viene avviato Gestione pacchetti NuGet e vengono visualizzati i pacchetti disponibili. Nella casella di ricerca immettere una parola chiave per l'helper che si vuole installare.

    ![Finestra di dialogo raccolta NuGet in WebMatrix](installing-helpers/_static/image2.png)
3. Selezionare il pacchetto e quindi fare clic su **Installa**. Fare clic su **Sì** quando viene chiesto se si desidera installare il pacchetto e indicare di accettare le condizioni.

     Se è la prima volta che si installa un helper, NuGet crea le cartelle nel sito Web per il codice che costituisce l'helper.
4. Per disinstallare un helper, fare clic sul pulsante **raccolta** , fare clic sulla scheda **installato** e selezionare il pacchetto che si desidera disinstallare.

## <a name="installing-the-twitter-helper"></a>Installazione dell'helper Twitter

La versione più recente dell'API Twitter non è compatibile con l'helper di Twitter che viene installato tramite NuGet. Per informazioni su come configurare l'helper di Twitter nel progetto, vedere l'argomento [Helper di Twitter con WebMatrix](twitter-helper.md) .

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione a Pagine Web ASP.NET 2-Nozioni fondamentali sulla programmazione](../getting-started/introducing-razor-syntax-c.md)

[Helper di Twitter con WebMatrix](twitter-helper.md)
