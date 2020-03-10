---
uid: visual-studio/overview/2013/using-browser-link
title: Utilizzo di Browser Link in Visual Studio 2013 | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 723a38de4569b0bb58817c70aabb84fef8e19591
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622906"
---
# <a name="using-browser-link-in-visual-studio-2013"></a>Utilizzo di Browser Link in Visual Studio 2013

di [Mike Wasson](https://github.com/MikeWasson)

Browser Link è una nuova funzionalità di Visual Studio 2013 che consente di creare un canale di comunicazione tra l'ambiente di sviluppo e uno o più Web browser. È possibile utilizzare Browser Link per aggiornare l'applicazione Web in diversi browser contemporaneamente, operazione utile per i test tra browser.

- [Aggiornamento del browser](#browser-refresh)
- [Visualizzazione del Dashboard Browser Link](#dashboard)
- [Abilitazione di Browser Link per i file HTML statici](#static-html)
- [Disabilitazione di Browser Link](#disabling)
- [Come funziona?](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>Aggiornamento del browser

Con l'aggiornamento del browser è possibile aggiornare più browser connessi a Visual Studio tramite Browser Link.

Per usare l'aggiornamento del browser, creare prima un'applicazione ASP.NET usando uno dei modelli di progetto. Eseguire il debug dell'applicazione premendo F5 o facendo clic sull'icona della freccia sulla barra degli strumenti:

![](using-browser-link/_static/image1.png)

È anche possibile usare l'elenco a discesa per selezionare un browser specifico per il debug.

![](using-browser-link/_static/image2.png)

Per eseguire il debug con più browser, selezionare **Sfoglia con**. Nella finestra di dialogo **Sfoglia con** tenere premuto il tasto CTRL per selezionare più di un browser. Fare clic su **Sfoglia** per eseguire il debug con i browser selezionati. Browser Link funziona anche se si avvia un browser dall'esterno di Visual Studio e si passa all'URL dell'applicazione.

![](using-browser-link/_static/image3.png)

I controlli Browser Link si trovano nell'elenco a discesa con l'icona a forma di freccia circolare. L'icona a freccia è il pulsante **Aggiorna** .

![](using-browser-link/_static/image4.png)

Per visualizzare i browser connessi, passare il puntatore del mouse sul pulsante di **aggiornamento** durante il debug. I browser connessi vengono visualizzati in una finestra di descrizione comando.

![](using-browser-link/_static/image5.png)

Per aggiornare i browser connessi, fare clic sul pulsante **Aggiorna** oppure premere CTRL + ALT + INVIO. Ad esempio, lo screenshot seguente mostra un progetto ASP.NET, creato con il modello di progetto MVC 5. È possibile visualizzare l'applicazione in esecuzione in due browser nella parte superiore. Nella parte inferiore il progetto è aperto in Visual Studio.

![](using-browser-link/_static/image6.png)

In Visual Studio è stata modificata l'intestazione &lt;H1&gt; per il home page:

![](using-browser-link/_static/image7.png)

Quando si è fatto clic sul pulsante **Aggiorna** , la modifica viene visualizzata in entrambe le finestre del browser:

![](using-browser-link/_static/image8.png)

**Note**

- Per abilitare Browser Link, impostare `debug=true` nell'elemento [&gt;di compilazione&lt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx) nel file Web. config del progetto.
- L'applicazione deve essere in esecuzione su localhost.
- L'applicazione deve essere destinata a .NET 4,0 o versione successiva.

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>Visualizzazione del Dashboard Browser Link

Il Dashboard Browser Link Visualizza le informazioni sulle connessioni Browser Link. Per visualizzare il dashboard, selezionare il menu a discesa Browser Link (la piccola freccia accanto al pulsante di **aggiornamento** ). Quindi fare clic su **browser link dashboard**.

![](using-browser-link/_static/image9.png)

Il dashboard elenca i browser connessi e l'URL a cui è stato spostato ogni browser.

![](using-browser-link/_static/image10.png)

La sezione **prerequisiti** illustra tutti i passaggi necessari per abilitare browser link per il progetto. Ad esempio, nella schermata seguente viene illustrato un progetto in cui "debug" è impostato su false nel file Web. config.

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>Abilitazione di Browser Link per i file HTML statici

Per abilitare Browser Link per i file HTML statici, aggiungere il codice seguente al file Web. config.

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

Per motivi di prestazioni, rimuovere questa impostazione quando si pubblica il progetto.

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Disabilitazione di Browser Link

Browser Link è abilitato per impostazione predefinita. Esistono diversi modi per disabilitarlo:

- Nel menu a discesa Browser Link deselezionare **abilita browser link**. 

    ![](using-browser-link/_static/image12.png)
- Nel file Web. config aggiungere una chiave denominata "vs: EnableBrowserLink" con il valore "false" nella sezione appSettings. 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Nel file Web. config, impostare debug su false. 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>Come funziona?

Browser Link USA [SignalR](../../../signalr/index.md) per creare un canale di comunicazione tra Visual Studio e il browser. Quando Browser Link è abilitato, Visual Studio funge da server SignalR a cui possono connettersi più client (browser). Browser Link registra anche un modulo HTTP con ASP.NET. Questo modulo inserisce uno script di &lt;speciale&gt; riferimenti in ogni richiesta di pagina dal server. È possibile visualizzare i riferimenti agli script selezionando "Visualizza origine" nel browser.

![](using-browser-link/_static/image13.png)

I file di origine non vengono modificati. Il modulo HTTP inserisce i riferimenti allo script in modo dinamico.

Poiché il codice sul lato browser è tutto JavaScript, funziona su tutti i browser [supportati da SignalR](../../../signalr/overview/getting-started/supported-platforms.md), senza richiedere alcun plug-in del browser.
