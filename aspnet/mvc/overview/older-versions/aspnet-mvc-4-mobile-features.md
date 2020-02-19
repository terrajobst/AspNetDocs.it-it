---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funzionalità per dispositivi mobili MVC 4 ASP.NET | Microsoft Docs
author: Rick-Anderson
description: È ora disponibile una versione MVC 5 di questa esercitazione con esempi di codice in distribuire un'applicazione Web ASP.NET MVC 5 per dispositivi mobili in siti Web di Azure.
ms.author: riande
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: 9716def069ca9f7115af32e16381f41bd4d13342
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457648"
---
# <a name="aspnet-mvc-4-mobile-features"></a>ASP.NET MVC 4 Mobile Features

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> È ora disponibile una versione MVC 5 di questa esercitazione con esempi di codice in [distribuire un'applicazione web ASP.NET MVC 5 per dispositivi mobili in siti Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).

Questa esercitazione illustra le nozioni di base su come usare le funzionalità per dispositivi mobili in un'applicazione Web ASP.NET MVC 4. Per questa esercitazione, è possibile utilizzare [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) o Visual web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer o VWD&quot;). Se è già stato fatto, è possibile usare la versione Professional di Visual Studio.

Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (scelta consigliata) o Visual Studio Web Developer Express SP1. Visual Studio 2012 contiene ASP.NET MVC 4. Se si usa Visual Web Developer 2010, è necessario installare [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Sarà inoltre necessario disporre di un emulatore del browser per dispositivi mobili. Eseguire una o più delle operazioni seguenti:

- [Emulatore Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Si tratta dell'emulatore usato nella maggior parte delle schermate di questa esercitazione).
- Modificare la stringa dell'agente utente per emulare un iPhone. Vedere [questo](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) post di Blog.
- [Emulatore Opera Mobile](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) con l'agente utente impostato su iPhone. Per istruzioni su come impostare l'agente utente in Safari su "iPhone", vedere la pagina relativa [a come lasciare che Safari faccia finta che](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) sia il Blog di David Alison.

Sono disponibili alcuni progetti con codice sorgente C\# a integrazione di questo argomento:

- [Download del progetto Starter](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Download del progetto completato](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Scopo dell'esercitazione

Per questa esercitazione, si aggiungeranno le funzionalità per dispositivi mobili alla semplice applicazione Conference-Listing fornita nel [progetto Starter](https://go.microsoft.com/fwlink/?LinkId=228307). Lo screenshot seguente mostra la pagina tag dell'applicazione completata, come illustrato nell' [emulatore Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Per semplificare l'input da tastiera, vedere [mappatura della tastiera per Windows Phone emulatore](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) .

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

È possibile utilizzare Internet Explorer versione 9 o 10, FireFox o Chrome per sviluppare l'applicazione per dispositivi mobili impostando la [stringa dell'agente utente](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Nell'immagine seguente viene illustrata l'esercitazione completata utilizzando Internet Explorer emulando un iPhone. Per eseguire il debug dell'applicazione, è possibile usare gli strumenti di sviluppo di Internet Explorer F-12 e lo [strumento Fiddler](http://www.fiddler2.com/fiddler2/) .

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Acquisizione di competenze

In questa esercitazione si apprenderà:

- Il modo in cui i modelli ASP.NET MVC 4 usano l'attributo `viewport` HTML5 e il rendering adattivo per migliorare la visualizzazione sui dispositivi mobili.
- Come creare visualizzazioni specifiche per dispositivi mobili.
- Come creare uno strumento di selezione delle visualizzazioni che consente agli utenti di passare da una visualizzazione mobile a una visualizzazione desktop dell'applicazione.

### <a name="getting-started"></a>Guida introduttiva

Scaricare l'applicazione Conference-Listing per il progetto Starter usando il collegamento seguente: [download](https://go.microsoft.com/fwlink/?LinkId=228307). Quindi, in Esplora risorse, fare clic con il pulsante destro del mouse sul file *MvcMobile. zip* e scegliere **Proprietà**. Nella finestra di dialogo **Proprietà MvcMobile. zip** scegliere il pulsante **Sblocca** . L'operazione di sblocco impedisce la visualizzazione dell'avviso di sicurezza quando si tenta di utilizzare un file *ZIP* scaricato dal Web.

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Fare clic con il pulsante destro del mouse sul file *MvcMobile. zip* e selezionare **Estrai tutto** per decomprimere il file. In Visual Studio aprire il file *MvcMobile. sln* .

Premere CTRL + F5 per eseguire l'applicazione, che verrà visualizzata nel browser desktop. Avviare l'emulatore di browser per dispositivi mobili, copiare l'URL dell'applicazione Conference nell'emulatore e quindi fare clic sul collegamento **Sfoglia per tag** . Se si usa l'emulatore Windows Phone, fare clic sulla barra URL e premere il tasto pausa per ottenere l'accesso alla tastiera. Nell'immagine seguente viene illustrata la vista *AllTags* (scegliendo **Sfoglia per tag**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Il display è molto leggibile su un dispositivo mobile. Scegliere il collegamento ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

La visualizzazione dei tag ASP.NET è molto ingombrante. Ad esempio, la colonna **date** è molto difficile da leggere. Più avanti in questa esercitazione verrà creata una versione della vista *AllTags* che è specificamente per i browser per dispositivi mobili e che renderà la visualizzazione leggibile.

Nota: attualmente esiste un bug nel motore di memorizzazione nella cache per dispositivi mobili. Per le applicazioni di produzione, è necessario installare il pacchetto [DisplayModes Nugget fisso](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) . Per informazioni dettagliate sulla correzione, vedere la pagina relativa a [ASP.NET MVC 4 Mobile Caching bug](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) .

## <a name="css-media-queries"></a>Query supporti CSS

Le [query multimediali CSS](http://www.w3.org/TR/css3-mediaqueries/) sono un'estensione del CSS per i tipi di supporto. Consentono di creare regole che sostituiscono le regole CSS predefinite per browser specifici (agenti utente). Una regola comune per CSS destinata ai browser per dispositivi mobili è la definizione delle dimensioni massime dello schermo. Il file *Content\Site.CSS* creato quando si crea un nuovo progetto ASP.NET MVC 4 Internet contiene la query supporti seguente:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Se la finestra del browser è di 850 pixel di larghezza o inferiore, utilizzerà le regole CSS all'interno di questo blocco multimediale. È possibile utilizzare le query di supporto CSS come questa per offrire una visualizzazione migliore del contenuto HTML in piccoli browser (ad esempio, browser per dispositivi mobili) rispetto alle regole CSS predefinite progettate per le visualizzazioni più ampie dei browser desktop.

## <a name="the-viewport-meta-tag"></a>Tag meta del viewport

La maggior parte dei browser per dispositivi mobili definisce la larghezza della finestra del browser virtuale (il *viewport*) molto più grande della larghezza effettiva del dispositivo mobile. Questo consente ai browser per dispositivi mobili di adattarsi all'intera pagina Web all'interno dello schermo virtuale. Gli utenti possono quindi eseguire lo zoom avanti sul contenuto interessante. Tuttavia, se si imposta la larghezza del viewport sulla larghezza effettiva del dispositivo, non è necessario lo zoom, perché il contenuto si adatta al browser per dispositivi mobili.

Il viewport `<meta>` tag nel file di layout MVC 4 ASP.NET imposta il viewport sulla larghezza del dispositivo. La riga seguente mostra il `<meta>` tag del viewport nel file di layout MVC 4 ASP.NET.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Esame dell'effetto delle query multimediali CSS e del tag meta del viewport

Aprire il file *Views\Shared\\_Layout. cshtml* nell'editor e impostare come commento il viewport `<meta>` tag. Il markup seguente mostra la riga impostata come commento.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Aprire il file *MvcMobile\Content\Site.CSS* nell'editor e impostare la larghezza massima della query supporti su zero pixel. In questo modo si eviterà l'uso delle regole CSS nei browser per dispositivi mobili. La riga seguente mostra la query supporti modificata:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Salvare le modifiche e passare all'applicazione Conference in un emulatore di browser per dispositivi mobili. Il testo minuscolo nell'immagine seguente è il risultato della rimozione del viewport `<meta>` tag. Senza `<meta>` tag del viewport, il browser esegue lo zoom indietro rispetto alla larghezza predefinita del viewport (850 pixel o più ampia per la maggior parte dei browser per dispositivi mobili).

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Annulla le modifiche: rimuovere il commento dal viewport `<meta>` tag nel file di layout e ripristinare la query supporti in 850 pixel nel file *site. CSS* . Salvare le modifiche e aggiornare il browser mobile per verificare che sia stato ripristinato lo schermo adatto per dispositivi mobili.

Il viewport `<meta>` tag e la query supporti CSS non sono specifici di ASP.NET MVC 4 ed è possibile sfruttare queste funzionalità in qualsiasi applicazione Web. Ma ora sono incorporati nei file generati quando si crea un nuovo progetto ASP.NET MVC 4.

Per altre informazioni sul tag `<meta>` del viewport, vedere il [racconto di due viewport, parte 2](http://www.quirksmode.org/mobile/viewports2.html).

Nella sezione seguente verrà illustrato come creare visualizzazioni specifiche del browser per dispositivi mobili.

## <a name="overriding-views-layouts-and-partial-views"></a>Override di viste, layout e visualizzazioni parziali

Una nuova funzionalità significativa di ASP.NET MVC 4 è un meccanismo semplice che consente di eseguire l'override di qualsiasi visualizzazione (inclusi layout e visualizzazioni parziali) per i browser per dispositivi mobili in generale, per un singolo browser per dispositivi mobili o per qualsiasi browser specifico. Per fornire una vista specifica per dispositivi mobili è possibile copiare un file di visualizzazione e aggiungere *.Mobile* al nome del file. Ad esempio, per creare una visualizzazione *Indice* per dispositivi mobili, copiare *Views\Home\Index.cshtml* in *Views\Home\Index.mobile.cshtml*.

In questa sezione verrà illustrato come creare un file di layout specifico per dispositivi mobili.

Per iniziare, copiare *Views\Shared\\_Layout. cshtml* in *Views\Shared\\_Layout. mobile. cshtml*. Aprire *\_layout. mobile. cshtml* e modificare il titolo da **MVC4 Conference** a **Conference (mobile)** .

In ogni chiamata di `Html.ActionLink` rimuovere "Browse by" in ogni collegamento *ActionLink*. Il codice seguente illustra la sezione del corpo completata del file di layout per dispositivi mobili.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Copiare il file *Views\Home\AllTags.cshtml* in *Views\Home\AllTags.mobile.cshtml*. Aprire il nuovo file e modificare l'elemento `<h2>` da "Tags" in "Tags (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Passare alla pagina Tags utilizzando un browser desktop e un emulatore di browser per dispositivi mobili. Nell'emulatore di browser per dispositivi mobili vengono visualizzate le due modifiche apportate.

[![p2m_layoutTags. mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Al contrario, la visualizzazione del desktop non è cambiata.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Visualizzazioni specifiche del browser

Oltre a visualizzazioni specifiche del dispositivo mobile e del desktop, è possibile anche creare visualizzazioni per un particolare browser. Ad esempio, è possibile creare visualizzazioni specifiche per il browser iPhone. In questa sezione verrà illustrato come creare un layout per il browser di iPhone e una versione iPhone della visualizzazione *AllTags* .

Aprire il file *Global. asax* e aggiungere il codice seguente al metodo `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Questo codice definisce una nuova modalità di visualizzazione denominata "iPhone" che verrà associata a ciascuna richiesta in ingresso. Se la richiesta in ingresso soddisfa la condizione definita, ovvero se l'agente utente contiene la stringa"iPhone", ASP.NET MVC cercherà le visualizzazioni il cui nome contiene il suffisso"iPhone".

Nel codice fare clic con il pulsante destro del mouse su `DefaultDisplayMode`, scegliere **Risolvi** quindi scegliere `using System.Web.WebPages;`. In questo modo viene aggiunto un riferimento allo spazio dei nomi `System.Web.WebPages`, ovvero la posizione in cui i tipi `DisplayModes` e `DefaultDisplayMode` vengono definiti.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

In alternativa, è possibile aggiungere manualmente la riga seguente alla sezione `using` del file.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Di seguito è riportato il contenuto completo del file *Global. asax* .

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Salvare le modifiche. Copiare il file *MvcMobile\Views\Shared\\_Layout. mobile. cshtml* in *MvcMobile\Views\Shared\\_Layout. iPhone. cshtml*. Aprire il nuovo file e modificare l'intestazione `h1` da `Conference (Mobile)` a `Conference (iPhone)`.

Copiare il file *MvcMobile\Views\Home\AllTags.mobile.cshtml* in *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. Nel nuovo file modificare l'elemento `<h2>` da "Tags (M)" in "Tags (iPhone)".

Eseguire l'applicazione. Eseguire un emulatore di browser per dispositivi mobili, assicurarsi che il relativo agente utente sia impostato su "iPhone" e passare alla visualizzazione *AllTags* . La schermata seguente mostra la visualizzazione *AllTags* visualizzata nel browser [Safari](http://www.apple.com/safari/download/) . È possibile scaricare Safari per Windows [qui](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

In questa sezione è stato spiegato come creare layout e visualizzazioni mobili e come creare layout e visualizzazioni per dispositivi specifici, ad esempio iPhone. Nella sezione successiva verrà illustrato come sfruttare jQuery Mobile per visualizzare più interessanti visualizzazioni per dispositivi mobili.

## <a name="using-jquery-mobile"></a>Uso di jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) Library fornisce un Framework dell'interfaccia utente che funziona su tutti i principali browser per dispositivi mobili. jQuery Mobile applica un *miglioramento progressivo* ai browser per dispositivi mobili che supportano CSS e JavaScript. Il miglioramento progressivo consente a tutti i browser di visualizzare il contenuto di base di una pagina Web, consentendo al contempo a browser e dispositivi più potenti di ottenere una visualizzazione più dettagliata. I file JavaScript e CSS inclusi nello stile jQuery Mobile sono molti elementi adatti ai browser per dispositivi mobili senza apportare modifiche al markup.

In questa sezione verrà installato il pacchetto NuGet *jQuery. mobile. Mvc* , che installa jQuery Mobile e un widget View-Switcher.

Per iniziare, eliminare i file *shared\\_Layout. mobile. cshtml* e *Shared\\_Layout. iPhone. cshtml* creati in precedenza.

Rinominare i file *Views\Home\AllTags.mobile.cshtml* e *Views\Home\AllTags.iPhone.cshtml* in *Views\Home\AllTags.iPhone.cshtml.Hide* e *Views\Home\AllTags.mobile.cshtml.Hide*. Poiché i file non hanno più un'estensione *cshtml* , non verranno usati dal runtime di ASP.NET MVC per eseguire il rendering della visualizzazione *AllTags* .

Installare il pacchetto NuGet *jQuery. mobile. Mvc* eseguendo questa operazione:

1. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare **console di gestione pacchetti**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. Nella **console di gestione pacchetti**immettere `Install-Package jQuery.Mobile.MVC -version 1.0.0`

La figura seguente mostra i file aggiunti e modificati nel progetto MvcMobile dal pacchetto NuGet jQuery. mobile. MVC. I file aggiunti hanno [Add] accodato dopo il nome del file. L'immagine non Mostra i file GIF e PNG aggiunti alla cartella *Content\images*

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Il pacchetto NuGet jQuery. mobile. MVC installa gli elementi seguenti:

- L' *App\_file Start\BundleMobileConfig.cs* , che è necessario per fare riferimento ai file CSS e JavaScript di jQuery aggiunti. È necessario seguire le istruzioni riportate di seguito e fare riferimento al bundle mobile definito in questo file.
- file CSS per jQuery Mobile.
- Un widget del controller di `ViewSwitcher` (*Controllers\ViewSwitcherController.cs*).
- file JavaScript per dispositivi mobili jQuery.
- Un file di layout con stile Mobile jQuery (*Views\Shared\\_Layout. mobile. cshtml*).
- Visualizzazione parziale dello switcher di visualizzazione *(MvcMobile\Views\Shared\\_ViewSwitcher. cshtml*) che fornisce un collegamento nella parte superiore di ogni pagina per passare dalla visualizzazione desktop alla visualizzazione per dispositivi mobili e viceversa.
- Diversi file di immagine con<em>estensione png</em> e <em>gif</em> nella cartella <em>Content\images</em> .

Aprire il file *Global. asax* e aggiungere il codice seguente come ultima riga del metodo `Application_Start`.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Nel codice seguente viene illustrato il file *Global. asax* completo.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Se si utilizza Internet Explorer 9 e non viene visualizzata la riga di `BundleMobileConfig` precedente in evidenziazione gialla, fare clic sull'immagine del pulsante [visualizzazione compatibilità](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![del pulsante visualizzazione compatibilità (disattivato)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Immagine del pulsante visualizzazione compatibilità (disattivato)") in IE per modificare l'icona da un' ![immagine del contorno del pulsante visualizzazione compatibilità (disattivato)](https://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "Immagine del pulsante visualizzazione compatibilità (disattivato)") a un immagine a tinta unita del pulsante visualizzazione compatibilità ( ![su)](https://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "Immagine del pulsante visualizzazione compatibilità (su)"). In alternativa, è possibile visualizzare questa esercitazione in FireFox o Chrome.

Aprire il file *MvcMobile\Views\Shared\\_Layout. mobile. cshtml* e aggiungere il markup seguente direttamente dopo la chiamata `Html.Partial`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Il file *MvcMobile\Views\Shared\\_Layout. mobile. cshtml* completo è illustrato di seguito:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Compilare l'applicazione e nell'emulatore di browser per dispositivi mobili passare alla visualizzazione *AllTags* . Saranno visualizzate le informazioni illustrate nell'immagine seguente:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> È possibile eseguire il debug del codice specifico per dispositivi mobili impostando [la stringa dell'agente utente](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) per IE o Chrome su iPhone e quindi usando gli strumenti di sviluppo F-12. Se il browser per dispositivi mobili non Visualizza i collegamenti **Home**, **speaker**, **tag**e **date** come pulsanti, i riferimenti agli script jQuery per dispositivi mobili e ai file CSS probabilmente non sono corretti.

Oltre alle modifiche apportate allo stile, viene visualizzata la **visualizzazione per dispositivi mobili** e un collegamento che consente di passare dalla visualizzazione mobile alla visualizzazione desktop. Scegliere il collegamento **visualizzazione desktop** . verrà visualizzata la visualizzazione desktop.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

La visualizzazione desktop non fornisce un modo per tornare direttamente alla visualizzazione per dispositivi mobili. Questa correzione verrà risolta. Aprire il file *Views\Shared\\_Layout. cshtml* . Sotto l'elemento Page `body` aggiungere il codice seguente, che esegue il rendering del widget View-Switcher:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Aggiornare la visualizzazione *AllTags* nel browser per dispositivi mobili. È ora possibile spostarsi tra le visualizzazioni desktop e per dispositivi mobili.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Nota di debug: è possibile aggiungere il codice seguente alla fine di Views\Shared\\_ViewSwitcher. cshtml per semplificare il debug delle visualizzazioni quando si usa un browser la stringa dell'agente utente impostata su un dispositivo mobile.
>
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
>
> e aggiungendo l'intestazione seguente al file *Views\Shared\\_Layout. cshtml* .
>
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]

Passare alla pagina *AllTags* in un browser desktop. Il widget View-Switcher non viene visualizzato in un browser desktop perché viene aggiunto solo alla pagina del layout per dispositivi mobili. Più avanti nell'esercitazione verrà illustrato come aggiungere il widget View-Switcher alla visualizzazione desktop.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Miglioramento dell'elenco di relatori

Nel browser per dispositivi mobili selezionare il collegamento **Speakers** . Poiché non è disponibile alcuna visualizzazione per dispositivi mobili (*AllSpeakers. mobile. cshtml*), viene eseguito il rendering della visualizzazione degli altoparlanti predefiniti (*AllSpeakers. cshtml*) utilizzando la visualizzazione layout mobile ( *\_layout. mobile. cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

È possibile disabilitare a livello globale una visualizzazione predefinita (non mobile) dal rendering all'interno di un layout mobile impostando `RequireConsistentDisplayMode` su `true` nel file *Views\\_ViewStart. cshtml* , come indicato di seguito:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Quando `RequireConsistentDisplayMode` è impostato su `true`, il layout mobile (<em>\_layout. mobile. cshtml</em>) viene usato solo per le visualizzazioni per dispositivi mobili. Ovvero il formato del file di visualizzazione è <em>* * viewName</em><em>. Mobile. cshtml</em>.) potrebbe essere necessario impostare `RequireConsistentDisplayMode` su `true` se il layout mobile non funziona correttamente con le visualizzazioni non per dispositivi mobili. La schermata seguente illustra come viene eseguito il rendering della pagina degli <em>altoparlanti</em> quando `RequireConsistentDisplayMode` è impostato su `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

È possibile disabilitare la modalità di visualizzazione coerente in una vista impostando `RequireConsistentDisplayMode` su `false` nel file di visualizzazione. Il markup seguente nel file *Views\Home\AllSpeakers.cshtml* imposta `RequireConsistentDisplayMode` `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Creazione di una visualizzazione di altoparlanti mobile

Come si è appena osservato, la vista *Speakers* è leggibile, ma i collegamenti sono di dimensioni ridotte e difficili da selezionare con un tocco su un dispositivo mobile. In questa sezione verrà creata una visualizzazione di *altoparlanti* specifici per dispositivi mobili simile a un'applicazione per dispositivi mobili moderna, che Visualizza collegamenti di grandi dimensioni e facili da toccare e contiene una casella di ricerca per trovare rapidamente gli speaker.

Copiare *AllSpeakers. cshtml* in *AllSpeakers. mobile. cshtml*. Aprire il file *AllSpeakers. mobile. cshtml* e rimuovere l'elemento heading `<h2>`.

Nel tag `<ul>` aggiungere l'attributo `data-role` e impostarne il valore su `listview`. Analogamente ad altri [attributi`data-*`](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` rende più semplice il tocco degli elementi elenco di grandi dimensioni. Il markup completato è simile al seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Aggiornare il browser per dispositivi mobili. La vista aggiornata avrà un aspetto simile al seguente:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Sebbene la visualizzazione per dispositivi mobili sia stata migliorata, è difficile esplorare il lungo elenco di relatori. Per risolvere il problema, nel tag `<ul>` aggiungere l'attributo `data-filter` e impostarlo su `true`. Il codice seguente mostra il markup `ul`.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Nell'immagine seguente viene mostrata la casella filtro di ricerca nella parte superiore della pagina risultante dall'attributo `data-filter`.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Quando si digita ogni lettera nella casella di ricerca, jQuery Mobile filtra l'elenco visualizzato come illustrato nell'immagine seguente.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Miglioramento dell'elenco dei tag

Analogamente alla visualizzazione dei *relatori* predefiniti, la visualizzazione dei *tag* è leggibile, ma i collegamenti sono di piccole dimensioni e difficili da toccare su un dispositivo mobile. In questa sezione si correggerà la visualizzazione dei *tag* nello stesso modo in cui è stata corretta la visualizzazione *Speakers* .

Rimuovere il &quot;nascondere&quot; suffisso nel file *Views\Home\AllTags.mobile.cshtml.Hide* in modo che il nome sia *Views\Home\AllTags.mobile.cshtml*. Aprire il file rinominato e rimuovere l'elemento `<h2>`.

Aggiungere gli attributi `data-role` e `data-filter` al tag `<ul>`, come illustrato di seguito:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

L'immagine seguente mostra il filtro della pagina dei tag sulla lettera `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Miglioramento dell'elenco di date

È possibile migliorare la visualizzazione delle *date* , ad esempio se sono state migliorate le visualizzazioni *Speakers* e *tag* , in modo che sia più facile da usare in un dispositivo mobile.

Copiare il file *Views\Home\AllDates.cshtml* in *Views\Home\AllDates.mobile.cshtml*. Aprire il nuovo file e rimuovere l'elemento `<h2>`.

Aggiungere `data-role="listview"` al tag `<ul>`, come segue:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

L'immagine seguente mostra l'aspetto della pagina relativa alla **Data** con l'attributo `data-role` sul posto.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Sostituire il contenuto del file *Views\Home\AllDates.mobile.cshtml* con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Questo codice raggruppa tutte le sessioni in base ai giorni. Viene creato un divisore di elenco per ogni nuovo giorno e vengono elencate tutte le sessioni per ogni giorno sotto un divisore. Ecco come appare quando viene eseguito questo codice:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Miglioramento della visualizzazione SessionsTable

In questa sezione verrà creata una visualizzazione specifica per dispositivi mobili delle sessioni. Le modifiche apportate saranno più estese rispetto ad altre viste create.

Nel browser per dispositivi mobili, toccare il pulsante **altoparlante** , quindi immettere `Sc` nella casella di ricerca.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Toccare il collegamento **Scott hanseln** .

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Come si può notare, la visualizzazione è difficile da leggere in un browser per dispositivi mobili. La colonna della data è di difficile lettura e la colonna Tag non è più visualizzata. Per risolvere il problema, copiare *Views\Home\SessionsTable.cshtml* in *Views\Home\SessionsTable.mobile.cshtml*e quindi sostituire il contenuto del file con il codice seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Il codice rimuove le colonne room e Tags e formatta verticalmente il titolo, il relatore e la data, in modo che tutte queste informazioni siano leggibili in un browser per dispositivi mobili. Nell'immagine seguente è illustrato l'aspetto risultante dopo aver apportato le modifiche al codice.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Miglioramento della visualizzazione SessionByCode

Infine, si creerà una visualizzazione specifica per dispositivi mobili della vista *SessionByCode* . Nel browser per dispositivi mobili, toccare il pulsante **altoparlante** , quindi immettere `Sc` nella casella di ricerca.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Toccare il collegamento **Scott hanseln** . Vengono visualizzate le sessioni di Scott Hanselr.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Scegliere **una panoramica del collegamento Microsoft Web Stack of Love** .

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

La visualizzazione desktop predefinita è corretta, ma è possibile migliorarla.

Copiare *Views\Home\SessionByCode.cshtml* in *Views\Home\SessionByCode.mobile.cshtml* e sostituire il contenuto del file *Views\Home\SessionByCode.mobile.cshtml* con il markup seguente:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Il nuovo markup usa l'attributo `data-role` per migliorare il layout della visualizzazione.

Aggiornare il browser per dispositivi mobili. Nell'immagine seguente è illustrato l'aspetto risultante dopo aver apportato le ultime modifiche:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup ed esaminare

In questa esercitazione sono state introdotte le nuove funzionalità per dispositivi mobili di ASP.NET MVC 4 Developer Preview. Le funzionalità per dispositivi mobili includono:

- Possibilità di eseguire l'override di layout, visualizzazioni e visualizzazioni parziali, sia a livello globale sia per una singola visualizzazione.
- Controllo sul layout e l'imposizione di override parziale usando la proprietà `RequireConsistentDisplayMode`.
- Un widget View-Switcher per le visualizzazioni per dispositivi mobili che può essere visualizzato anche in visualizzazioni desktop.
- Supporto per il supporto di browser specifici, ad esempio il browser iPhone.

## <a name="see-also"></a>Vedere anche

- sito [jQuery per dispositivi mobili](http://jquerymobile.com) .
- [Panoramica di jQuery Mobile](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [Procedure consigliate per applicazioni Web W3C per dispositivi mobili](http://www.w3.org/TR/mwabp/)
- [Candidate recommendation W3C per query sui supporti](http://www.w3.org/TR/css3-mediaqueries/)
