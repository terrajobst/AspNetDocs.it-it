---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Parte 2: livello di accesso ai dati | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella seconda parte viene illustrata l'aggiunta del livello di accesso ai dati.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 342d2c54dfba5d052570e890f85dcf9739f9884f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78573248"
---
# <a name="part-2-data-access-layer"></a>Parte 2: livello di accesso ai dati

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. Nella seconda parte viene illustrata l'aggiunta del livello di accesso ai dati.

## <a id="_Toc260221668"></a>Aggiunta del livello di accesso ai dati

L'applicazione di eCommerce dipenderà da due database.

Per informazioni sul cliente verrà usato il database di appartenenza ASP.NET standard. Per il carrello acquisti e il catalogo dei prodotti verrà implementato un database di SQL Express come indicato di seguito.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Dopo aver creato il database (Commerce. MDF) nella cartella app\_data dell'applicazione, è possibile procedere con la creazione del livello di accesso ai dati usando il Entity Framework .NET.

Verrà creata una cartella denominata "data\_Access", facendo clic con il pulsante destro del mouse sulla cartella e scegliendo "Aggiungi nuovo elemento".

Nell'elemento "modelli installati", quindi selezionare "ADO.NET Entity Data Model" immettere EDM\_Commerce. edmx come nome e fare clic sul pulsante "Aggiungi".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Scegliere "genera da database".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Salvare e compilare.

A questo punto è possibile aggiungere la prima funzionalità, ovvero un menu di categoria di prodotti.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-1.md)
> [Successivo](tailspin-spyworks-part-3.md)
