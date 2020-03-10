---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Impalcature ASP.NET in Visual Studio 2013 | Microsoft Docs
author: Rick-Anderson
description: L'impalcatura ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: cf4669b769cee28475e2dd6a6ddf07ea1434d04d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557981"
---
# <a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding di ASP.NET in Visual Studio 2013

di [Tom FitzMacken](https://github.com/tfitzmac)

> L'impalcatura ASP.NET è una nuova funzionalità inclusa in Visual Studio 2013.

## <a name="overview"></a>Panoramica

L'impalcatura ASP.NET è un Framework per la generazione di codice per le applicazioni Web ASP.NET. Visual Studio 2013 include i generatori di codice preinstallati per i progetti MVC e API Web. Quando si vuole aggiungere rapidamente codice che interagisce con i modelli di dati, si aggiungono le impalcature al progetto. L'uso dell'impalcatura può ridurre la quantità di tempo necessario per sviluppare operazioni dati standard nel progetto.

Per impostazione predefinita, Visual Studio 2013 non supporta la generazione di codice per un progetto Web Form, ma è possibile usare l'impalcatura con i Web form aggiungendo dipendenze MVC al progetto o installando un'estensione. Di seguito sono illustrati entrambi gli approcci.

Visual Studio 2013 Update 2 (attualmente RC) offre la possibilità di estendere l'impalcatura ASP.NET per soddisfare i requisiti dello scenario. Con questa funzionalità è possibile creare un modello di impalcatura personalizzato e aggiungerlo alla finestra di dialogo Aggiungi nuova impalcatura. All'interno del modello personalizzato, si specifica il codice generato quando si aggiunge un elemento con impalcatura. Per ulteriori informazioni, vedere la pagina relativa alla [creazione di un'impalcatura personalizzata per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Prerequisiti

Per usare l'impalcatura ASP.NET, è necessario disporre di:

- Microsoft Visual Studio 2013
- Strumenti di sviluppo Web (parte dell'installazione Visual Studio 2013 predefinita)
- Framework e strumenti Web di ASP.NET 2013 (parte dell'installazione predefinita di Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Aggiungere un elemento con impalcatura a MVC o API Web

Per aggiungere un patibolo, fare clic con il pulsante destro del mouse sul progetto o su una cartella all'interno del progetto e selezionare **Aggiungi** , **nuovo elemento con impalcatura**, come illustrato nella figura seguente.

![Aggiungi elemento impalcatura](aspnet-scaffolding-overview/_static/image1.png)

Dalla finestra **Aggiungi impalcatura** selezionare il tipo di impalcatura da aggiungere.

![Selezionare il tipo di impalcatura](aspnet-scaffolding-overview/_static/image2.png)

La finestra **Aggiungi controller** consente di selezionare le opzioni per la generazione del controller, ad esempio se si vogliono usare le nuove funzionalità asincrone di Entity Framework 6.

![Aggiungi controller](aspnet-scaffolding-overview/_static/image3.png)

Le classi e le pagine pertinenti vengono create per lo scenario in uso. Ad esempio, l'immagine seguente mostra il controller MVC e le visualizzazioni create tramite l'impalcatura per una classe modello denominata Movies.

![File creati](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Aggiungere un elemento con impalcatura a Web Form

Per aggiungere l'impalcatura che genera codice Web Form, è necessario installare un'estensione in Visual Studio o aggiungere dipendenze MVC. Entrambi gli approcci sono illustrati di seguito, ma è sufficiente eseguire uno di questi approcci.

### <a name="web-forms-scaffolding-extension"></a>Estensione di ponteggi Web Form

È possibile installare un'estensione di Visual Studio che consente di usare l'impalcatura con un progetto Web Form. In Visual Studio selezionare **strumenti** , quindi **estensioni e aggiornamenti**. Da questa finestra di dialogo cercare in Visual Studio Gallery per l' **impalcatura di Web Form**.

![installare l'impalcatura di Web Form](aspnet-scaffolding-overview/_static/image5.png)

Per ulteriori informazioni, vedere [ponteggi Web Form](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dipendenze MVC

Per aggiungere le dipendenze MVC, selezionare **aggiungi** - **nuovo elemento con impalcatura**. Nella finestra Aggiungi impalcatura selezionare **dipendenze MVC**, come illustrato di seguito.

![Aggiungi dipendenze MVC](aspnet-scaffolding-overview/_static/image6.png)

Sono disponibili due opzioni per l'impalcatura MVC; Minimo e completo. Se si seleziona minimal, al progetto vengono aggiunti solo i pacchetti NuGet e i riferimenti per ASP.NET MVC. Se si seleziona l'opzione completa, vengono aggiunte le dipendenze minime, nonché i file di contenuto necessari per un progetto MVC. Per usare facilmente l'impalcatura, selezionare dipendenze complete.

![Selezionare le dipendenze complete](aspnet-scaffolding-overview/_static/image7.png)

Dopo aver aggiunto le dipendenze, viene visualizzato un file **Readme. txt** . Seguire attentamente le istruzioni riportate in questo file per assicurarsi che il progetto funzioni correttamente.

Una volta completati i passaggi nel file Readme. txt, è possibile aggiungere un nuovo elemento con impalcatura, come illustrato nella sezione precedente su MVC e l'API Web. Le visualizzazioni e il controller generati automaticamente funzioneranno correttamente nel progetto.

## <a name="tutorials"></a>Esercitazioni

Per creare un'impalcatura personalizzata, vedere [la pagina relativa alla creazione di un'impalcatura personalizzata per Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Per personalizzare i file generati, vedere [come personalizzare i file generati dalla finestra di dialogo nuovo elemento con impalcature](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Per un esempio di come usare l'impalcatura con **database First sviluppo**, vedere [EF database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Per un esempio di utilizzo dell'impalcatura in un progetto **MVC** , vedere [Introduzione con ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Per un esempio di come usare l'impalcatura in un progetto **API Web** , vedere [creare un'API REST con routing degli attributi nell'API Web 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
