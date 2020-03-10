---
uid: mvc/overview/getting-started/introduction/getting-started
title: Introduzione con ASP.NET MVC 5 | Microsoft Docs
author: Rick-Anderson
ms.author: riande
ms.date: 10/04/2018
ms.assetid: f3d8adbe-55e7-4fd4-84a8-7155bc45c676
msc.legacyurl: /mvc/overview/getting-started/introduction/getting-started
msc.type: authoredcontent
ms.openlocfilehash: ca39bc37c757c0452cf56624c8e37c04df4b41f2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78602767"
---
# <a name="getting-started-with-aspnet-mvc-5"></a>Introduzione a ASP.NET MVC 5

di [Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE [consider RP](../../../../includes/razor.md)]

Questa esercitazione illustra le nozioni di base per la creazione di un'app Web ASP.NET MVC 5 con [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Il codice sorgente finale per l'esercitazione è disponibile in [GitHub](https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie/MvcMovie).

Questa esercitazione è stata scritta da [Scott Guthrie](https://weblogs.asp.net/scottgu/) ([ Twitter@scottgu](https://twitter.com/scottgu) ), [Scott hanselt](http://www.hanselman.com/blog/) (Twitter: [@shanselman](https://twitter.com/shanselman) ) e [Rick Anderson](https://twitter.com/RickAndMSFT) ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) )

Per distribuire l'app in Azure è necessario un account Azure:

- È possibile [aprire un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: si ottengono crediti che è possibile usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, è possibile tenere l'account e usare i servizi di Azure gratuiti.
- È possibile [attivare i benefici della sottoscrizione MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A443DD604): con la sottoscrizione MSDN ogni mese si accumulano crediti che è possibile usare per i servizi di Azure a pagamento.

## <a name="get-started"></a>Introduzione

Per iniziare, [installare Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Quindi, aprire Visual Studio.

Visual Studio è un IDE o Integrated Development Environment. Proprio come si usa Microsoft Word per scrivere documenti, si userà un IDE per creare applicazioni. In Visual Studio è disponibile un elenco lungo il fondo che mostra le varie opzioni disponibili. È anche disponibile un menu che fornisce un altro modo per eseguire attività nell'IDE. Ad esempio, invece di selezionare **nuovo progetto** nella **pagina iniziale**, è possibile usare la barra dei menu e selezionare **file** > **nuovo progetto**.

![](getting-started/_static/image1.png)

## <a name="create-your-first-app"></a>Creare la prima app

Nella **pagina iniziale**selezionare **nuovo progetto**. Nella finestra di dialogo **nuovo progetto** selezionare la categoria **Visual C#**  a sinistra, quindi **Web**e quindi selezionare il modello di progetto **applicazione Web ASP.NET (.NET Framework)** . Denominare il progetto "MvcMovie", quindi scegliere **OK**.

![](getting-started/_static/image2.png)

Nella finestra di dialogo **nuova applicazione Web ASP.NET** scegliere **MVC** , quindi scegliere **OK**.

![](getting-started/_static/image3.png)

Visual Studio ha usato un modello predefinito per il progetto MVC ASP.NET appena creato ed è quindi disponibile un'applicazione funzionante senza eseguire alcuna operazione. Si tratta di una semplice "Hello World!" il progetto è una soluzione ideale per avviare l'applicazione.

![](getting-started/_static/image4.png)

Premere **F5** per avviare il debug. Quando si preme **F5**, Visual Studio avvia [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ed esegue l'app Web. Visual Studio avvia quindi un browser e apre la home page dell'applicazione. Si noti che la barra degli indirizzi del browser indica `localhost:port#` e non un elemento come `example.com`. Questo perché `localhost` fa sempre riferimento al computer locale, che in questo caso esegue l'applicazione appena creata. Quando si esegue un progetto Web in Visual Studio, viene utilizzata una porta casuale per il server Web. Nell'immagine seguente il numero di porta è 1234. Quando si esegue l'applicazione, viene visualizzato un numero di porta diverso.

![](getting-started/_static/image5.png)

Il modello predefinito fornisce le pagine `Home`, `Contact`e `About`. L'immagine seguente non Mostra i collegamenti **Home**, **About**e **Contact** . A seconda delle dimensioni della finestra del browser, potrebbe essere necessario fare clic sull'icona di spostamento per visualizzare i collegamenti.

![](getting-started/_static/image6.png)

L'applicazione fornisce anche il supporto per la registrazione e l'accesso. Il passaggio successivo consiste nel modificare il funzionamento di questa applicazione e apprendere un po' di ASP.NET MVC. Chiudere l'applicazione MVC ASP.NET e modificare il codice.

Per un elenco delle esercitazioni correnti, vedere [gli articoli consigliati per MVC](../mvc-learning-sequence.md).

## <a name="see-this-app-running-on-azure"></a>Vedi questa app in esecuzione in Azure

Si desidera visualizzare il sito finito in esecuzione come app Web Live? È possibile distribuire una versione completa dell'app nell'account Azure semplicemente facendo clic sul pulsante seguente.

[![](https://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/?repository=https://github.com/dotnet/AspNetDocs/tree/master/aspnet/mvc/overview/getting-started/introduction/sample/MvcMovie&amp;WT.mc_id=deploy_azure_aspnet)

Per distribuire questa soluzione in Azure, è necessario un account Azure. Se non si dispone già di un account, utilizzare una delle seguenti opzioni per crearne uno:

- [Apri un account Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A443DD604) gratuitamente: puoi ottenere crediti che puoi usare per provare i servizi di Azure a pagamento e, anche dopo che sono stati usati, puoi tenere l'account e usare i servizi di Azure gratuiti.
- [Attiva i vantaggi per gli abbonati di Visual Studio](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers) : la tua sottoscrizione di Visual Studio ti offre crediti ogni mese che puoi usare per i servizi di Azure a pagamento.

> [!div class="step-by-step"]
> [avanti](adding-a-controller.md)
