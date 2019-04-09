---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: Parte 2. Livello di accesso ai dati | Microsoft Docs
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 2 illustra l'aggiunta di livello di accesso ai dati.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 3fbc6fe4d94534a038a81532b3cd8ca30ddf9b11
ms.sourcegitcommit: 0f1119340e4464720cfd16d0ff15764746ea1fea
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/09/2019
ms.locfileid: "59378390"
---
# <a name="part-2-data-access-layer"></a>Parte 2. Livello di accesso ai dati

da [Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks viene illustrato come saliente è davvero semplice per creare applicazioni potenti e scalabili per la piattaforma .NET. Illustra come usare le nuove funzionalità in ASP.NET 4 per creare un negozio online, tra cui acquisti, estrazione e l'amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tailspin Spyworks. Parte 2 illustra l'aggiunta di livello di accesso ai dati.


## <a id="_Toc260221668"></a>  Aggiunta di livello di accesso ai dati

L'applicazione di e-commerce dipenderà da due database.

Per informazioni sui clienti useremo database di appartenenze ASP.NET standard. Per il nostro catalogo di prodotto e carrello acquisti implementeremo un database SQL Express come indicato di seguito.

![](tailspin-spyworks-part-2/_static/image1.jpg)

Dopo aver creato il database (Commerce.mdf) nell'App dell'applicazione\_cartella dati è possibile procedere per creare il livello di accesso ai dati usando .NET Entity Framework.

Si creerà una cartella denominata "dati\_accesso" e fare clic con il pulsante destro su tale cartella e scegliere "Aggiungi nuovo elemento".

In "Modelli installati" elemento e quindi selezionare "ADO.NET Entity Data Model" Immettere EDM\_Commerce.edmx come il nome e fare clic sul pulsante "Aggiungi".

![](tailspin-spyworks-part-2/_static/image2.jpg)

Scegliere "Genera da Database".

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

Salvare e compilare.

A questo punto siamo pronti aggiungere la prima funzionalità – un menu di categoria di prodotto.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-1.md)
> [Successivo](tailspin-spyworks-part-3.md)
