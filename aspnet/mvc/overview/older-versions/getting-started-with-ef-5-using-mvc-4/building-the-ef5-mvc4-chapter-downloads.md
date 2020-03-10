---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: Creazione del capitolo Download per le esercitazioni di EF 5 MVC 4 | Microsoft Docs
author: Rick-Anderson
description: L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando il Entity Framework 5 Code First e Visual Studio...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 10237af40e3914b65e5181f17555697e86adea4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78599463"
---
# <a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a>Creazione del capitolo Download per le esercitazioni di EF 5 MVC 4

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[Scarica progetto completato](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> L'applicazione Web di esempio di Contoso University illustra come creare applicazioni ASP.NET MVC 4 usando i Entity Framework 5 Code First e Visual Studio 2012. Per informazioni sulla serie di esercitazioni, vedere la [prima esercitazione della serie](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).

## <a name="building-the-chapter-downloads"></a>Creazione dei download dei capitoli

1. Scaricare e decomprimere il file zip di esempio del progetto. Nel pacchetto di download decompresso sono disponibili file zip aggiuntivi, uno per il completamento di ogni capitolo.
2. Fare clic con il pulsante destro del mouse sul file zip desiderato, scegliere **Proprietà**e fare clic sul pulsante **Sblocca** .  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. Decomprimere il file.
4. Fare doppio clic sul file *CUx. sln* per avviare Visual Studio.
5. Dal menu **strumenti** fare clic su **Gestione pacchetti NuGet**e quindi su **console di gestione pacchetti**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. Nella console di gestione pacchetti (PMC) fare clic su **Ripristina**.  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. Uscire da Visual Studio.
8. Riavviare Visual Studio, aprendo il file della soluzione chiuso nel passaggio precedente.
9. Nella console di gestione pacchetti (PMC) immettere il comando `Update-Database`:  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > Se viene restituito l'errore seguente:  
    >   
    >  *Il termine "Update-database" non è riconosciuto come nome di un cmdlet, di una funzione, di un file di script o di un programma eseguibile. Controllare l'ortografia del nome o, se è stato incluso un percorso, verificare che il percorso sia corretto e riprovare.*  
    > Chiudere e riavviare Visual Studio.

    Ogni migrazione verrà eseguita, quindi verrà eseguito il metodo di inizializzazione. È ora possibile eseguire l'app.

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

> [!div class="step-by-step"]
> [Precedente](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
