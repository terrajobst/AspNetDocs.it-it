---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Creazione di pagine della Guida per API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Questa esercitazione con il codice Mostra come creare pagine della Guida per API Web ASP.NET in ASP.NET 4. x.
ms.author: riande
ms.date: 04/01/2013
ms.custom: seoapril2019
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 8308dab8bd66aa8f5a3c5fb4133fc7a3df78f671
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556875"
---
# <a name="creating-help-pages-for-aspnet-web-api"></a>Creazione di pagine della Guida per API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questa esercitazione con il codice Mostra come creare pagine della Guida per API Web ASP.NET in ASP.NET 4. x.

Quando si crea un'API Web, è spesso utile creare una pagina della guida, in modo che altri sviluppatori sappiano come chiamare l'API. È possibile creare tutta la documentazione manualmente, ma è preferibile eseguire la generazione del più possibile. Per semplificare questa attività, API Web ASP.NET fornisce una libreria per la generazione automatica di pagine della Guida in fase di esecuzione.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Creazione di pagine della Guida dell'API

Installare [ASP.NET and Web Tools aggiornamento 2012,2](https://go.microsoft.com/fwlink/?LinkId=282650). Questo aggiornamento integra le pagine della guida nel modello di progetto API Web.

Successivamente, creare un nuovo progetto ASP.NET MVC 4 e selezionare il modello di progetto API Web. Il modello di progetto crea un esempio di controller API denominato `ValuesController`. Il modello crea anche le pagine della Guida dell'API. Tutti i file di codice per la pagina della guida vengono inseriti nella cartella aree del progetto.

![](creating-api-help-pages/_static/image2.png)

Quando si esegue l'applicazione, il home page contiene un collegamento alla pagina della Guida dell'API. Dal home page, il percorso relativo è/help.

![](creating-api-help-pages/_static/image3.png)

Questo collegamento consente di portare a una pagina di riepilogo dell'API.

![](creating-api-help-pages/_static/image4.png)

La visualizzazione MVC per questa pagina è definita in areas/HelpPage/views/help/index. cshtml. È possibile modificare questa pagina per modificare il layout, l'introduzione, il titolo, gli stili e così via.

La parte principale della pagina è una tabella di API, raggruppate per controller. Le voci della tabella vengono generate in modo dinamico, usando l'interfaccia **IApiExplorer** . (Parlerò di questa interfaccia più avanti). Se si aggiunge un nuovo controller API, la tabella viene aggiornata automaticamente in fase di esecuzione.

La colonna "API" elenca il metodo HTTP e l'URI relativo. La colonna "Description" contiene la documentazione relativa a ogni API. Inizialmente, la documentazione è semplicemente testo segnaposto. Nella sezione successiva verrà illustrato come aggiungere la documentazione dai commenti XML.

Ogni API dispone di un collegamento a una pagina con informazioni più dettagliate, inclusi i corpi di richiesta e risposta di esempio.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Aggiunta di pagine della guida a un progetto esistente

È possibile aggiungere pagine della guida a un progetto API Web esistente usando Gestione pacchetti NuGet. Questa opzione è utile per iniziare da un modello di progetto diverso da quello del modello "API Web".

Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare **console di gestione pacchetti**. Nella finestra [console di gestione pacchetti](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) Digitare uno dei comandi seguenti:

Per un' **C#** applicazione: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Per un'applicazione **Visual Basic** : `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Sono disponibili due pacchetti, uno per C# e uno per Visual Basic. Assicurarsi di usare quello che corrisponde al progetto.

Questo comando installa gli assembly necessari e aggiunge le visualizzazioni MVC per le pagine della guida (che si trovano nella cartella areas/HelpPage). È necessario aggiungere manualmente un collegamento alla pagina della guida. L'URI è/help. Per creare un collegamento in una visualizzazione Razor, aggiungere quanto segue:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Assicurarsi anche di registrare le aree. Nel file Global. asax aggiungere il codice seguente all' **applicazione\_** metodo di avvio, se non è già presente:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Aggiunta della documentazione dell'API

Per impostazione predefinita, le pagine della guida contengono stringhe segnaposto per la documentazione. È possibile utilizzare i commenti relativi alla [documentazione XML](https://msdn.microsoft.com/library/b2s063f7.aspx) per creare la documentazione. Per abilitare questa funzionalità, aprire il file areas/HelpPage/app\_Start/HelpPageConfig. cs e rimuovere il commento dalla riga seguente:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Abilitare ora la documentazione XML. In Esplora soluzioni fare clic con il pulsante destro del mouse sul progetto e scegliere **Proprietà**. Selezionare la pagina **Compila** .

![](creating-api-help-pages/_static/image6.png)

In **output**selezionare **file di documentazione XML**. Nella casella di modifica digitare "app\_data/XmlDocument. xml".

![](creating-api-help-pages/_static/image7.png)

Aprire quindi il codice per il controller API `ValuesController`, che è definito in/Controllers/ValuesController.cs. Aggiungere alcuni commenti alla documentazione ai metodi del controller. Esempio:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Suggerimento: se si posiziona il cursore sulla riga sopra il metodo e si digita tre barre, Visual Studio inserisce automaticamente gli elementi XML. È quindi possibile compilare gli spazi vuoti.

A questo punto, compilare ed eseguire nuovamente l'applicazione e passare alle pagine della guida. Le stringhe di documentazione dovrebbero essere visualizzate nella tabella API.

![](creating-api-help-pages/_static/image8.png)

La pagina della guida legge le stringhe dal file XML in fase di esecuzione. Quando si distribuisce l'applicazione, assicurarsi di distribuire il file XML.

## <a name="under-the-hood"></a>Dietro le quinte

Le pagine della guida si basano sulla classe **ApiExplorer** , che fa parte del Framework API Web. La classe **ApiExplorer** fornisce il materiale non elaborato per la creazione di una pagina della guida. Per ogni API, **ApiExplorer** contiene un **ApiDescription** che descrive l'API. A questo scopo, un'"API" viene definita come la combinazione di metodo HTTP e URI relativo. Ad esempio, di seguito sono riportate alcune API distinte:

- OTTENERE/api/Products
- OTTENERE/api/Products/{id}
- POST/api/Products

Se un'azione del controller supporta più metodi HTTP, **ApiExplorer** considera ogni metodo come un'API distinta.

Per nascondere un'API da **ApiExplorer**, aggiungere l'attributo **ApiExplorerSettings** all'azione e impostare *IgnoreApi* su true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

È inoltre possibile aggiungere questo attributo al controller, in modo da escludere l'intero controller.

La classe ApiExplorer ottiene le stringhe di documentazione dall'interfaccia **IDocumentationProvider** . Come illustrato in precedenza, la libreria pagine della guida fornisce un **IDocumentationProvider** che ottiene la documentazione dalle stringhe di documentazione XML. Il codice si trova in/Areas/HelpPage/XmlDocumentationProvider.cs. È possibile ottenere la documentazione da un'altra origine scrivendo il proprio **IDocumentationProvider**. Per eseguire il collegamento, chiamare il metodo di estensione **SetDocumentationProvider** , definito in **HelpPageConfigurationExtensions**

**ApiExplorer** chiama automaticamente nell'interfaccia **IDocumentationProvider** per ottenere le stringhe di documentazione per ogni API. Li archivia nella proprietà **Documentation** degli oggetti **ApiDescription** e **ApiParameterDescription** .

## <a name="next-steps"></a>Passaggi successivi

Non si è limitati alle pagine della Guida visualizzate qui. In realtà, **ApiExplorer** non è limitato alla creazione di pagine della guida. Yao Huang Lin ha scritto alcuni ottimi post di Blog per iniziare a pensarci:

- [Aggiunta di un client di test semplice alla pagina della Guida di API Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Creazione di API Web ASP.NET pagina della Guida lavoro sui servizi indipendenti](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generazione in fase di progettazione della pagina della guida (o client) per API Web ASP.NET](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizzazioni della pagina della guida avanzate](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
