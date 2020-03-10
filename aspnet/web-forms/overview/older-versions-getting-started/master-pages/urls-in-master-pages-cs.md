---
uid: web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
title: URL nelle pagine master (C#) | Microsoft Docs
author: rick-anderson
description: Risolve il modo in cui gli URL nella pagina master possono interrompersi perché il file della pagina master si trova in una directory diversa rispetto alla pagina contenuto. Esamina la riassegnazione...
ms.author: riande
ms.date: 06/10/2008
ms.assetid: 48b58a18-5ea4-468c-b326-f35331b3e1e9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/urls-in-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 2551a5361256234883bb37e46e794037284445a4
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78585848"
---
# <a name="urls-in-master-pages-c"></a>URL nelle pagine master (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_04_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_04_CS.pdf)

> Risolve il modo in cui gli URL nella pagina master possono interrompersi perché il file della pagina master si trova in una directory diversa rispetto alla pagina contenuto. Esamina la riassegnazione degli URL tramite ~ nella sintassi dichiarativa e l'uso di ResolveUrl e ResolveClientUrl a livello di codice. (Vedere anche

## <a name="introduction"></a>Introduzione

In tutti gli esempi che abbiamo visto finora, le pagine master e di contenuto si trovano nella stessa cartella (la cartella radice del sito Web). Ma non esiste alcun motivo per cui le pagine master e di contenuto devono trovarsi nella stessa cartella. È certamente possibile creare pagine di contenuto nelle sottocartelle. Analogamente, è possibile creare una cartella `~/MasterPages/` in cui posizionare le pagine master del sito.

Un potenziale problema con l'inserimento di pagine master e di contenuto in cartelle diverse comporta URL interrotti. Se la pagina master contiene URL relativi in collegamenti ipertestuali, immagini o altri elementi, il collegamento non sarà valido per le pagine di contenuto che si trovano in una cartella diversa. In questa esercitazione si esaminano l'origine del problema e le soluzioni alternative.

## <a name="the-problem-with-relative-urls"></a>Il problema con gli URL relativi

Un URL in una pagina Web viene definito *URL relativo* se il percorso della risorsa a cui fa riferimento è relativo alla posizione della pagina Web nella struttura di cartelle del sito Web. Qualsiasi URL che non inizia con una barra (`/`) o un protocollo (ad esempio `http://`) iniziale è relativo perché viene risolto dal browser in base alla posizione della pagina Web che contiene l'URL.

Il sito Web, ad esempio, include una cartella `~/Images/` con un file di immagine singolo `PoweredByASPNET.gif`. Il file della pagina master `Site.master` dispone di un elemento `<img>` nell'area `footerContent` con il markup seguente:

[!code-html[Main](urls-in-master-pages-cs/samples/sample1.html)]

Il valore dell'attributo `src` nell'elemento `<img>` è un URL relativo perché non inizia con `/` o `http://`. In breve, il valore dell'attributo `src` indica al browser di esaminare la sottocartella `Images` per un file denominato `PoweredByASPNET.gif`.

Quando si visita una pagina di contenuto, il markup precedente viene inviato direttamente al browser. Esaminare `About.aspx` e visualizzare l'origine HTML inviata al browser. Si noterà che il markup esatto della pagina master è stato inviato al browser.

[!code-html[Main](urls-in-master-pages-cs/samples/sample2.html)]

Se la pagina contenuto si trova nella cartella radice (così come `About.aspx`) tutto funziona come previsto perché esiste una sottocartella `Images` relativa alla cartella radice. Tuttavia, le cose si interrompono se la pagina contenuto si trova in una cartella diversa da quella della pagina master. Per illustrare questo problema, creare una sottocartella denominata `Admin`. Successivamente, aggiungere una pagina contenuto denominata `Default.aspx` alla cartella `Admin`, assicurandosi di associare la nuova pagina alla pagina master `Site.master`.

> [!NOTE]
> Nell'esercitazione [*specificare il titolo, i metatag e altre intestazioni HTML nella pagina master*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) è stata creata una classe di pagina base personalizzata denominata `BasePage` che imposta automaticamente il titolo della pagina di contenuto (se non è stata assegnata in modo esplicito). Non dimenticare di fare in modo che la classe code-behind della pagina appena creata derivi da `BasePage` per poter usare questa funzionalità.

Dopo aver creato questa pagina di contenuto, il Esplora soluzioni dovrebbe essere simile a quello illustrato nella figura 1.

![Al progetto sono state aggiunte una nuova cartella e una pagina ASP.NET](urls-in-master-pages-cs/_static/image1.png)

**Figura 01**: al progetto sono state aggiunte una nuova cartella e una pagina ASP.NET

Aggiornare quindi il file di `Web.sitemap` per includere una nuova voce `<siteMapNode>` per questa lezione. Nel codice XML seguente viene illustrato il markup completo `Web.sitemap`, che ora include l'aggiunta di un terzo elemento `<siteMapNode>`.

[!code-xml[Main](urls-in-master-pages-cs/samples/sample3.xml)]

La pagina `Default.aspx` appena creata deve avere quattro controlli contenuto corrispondenti ai quattro ContentPlaceHolders in `Site.master`. Aggiungere testo al controllo contenuto che fa riferimento al `MainContent` ContentPlaceHolder, quindi visitare la pagina tramite un browser. Come illustrato nella figura 2, il browser non riesce a trovare il file di immagine del `PoweredByASPNET.gif`. Che cosa succede qui?

Alla pagina contenuto `~/Admin/Default.aspx` viene inviato lo stesso codice HTML per l'area `footerContent` come la pagina `About.aspx`:

[!code-html[Main](urls-in-master-pages-cs/samples/sample4.html)]

Poiché l'attributo `src` dell'elemento `<img>` è un URL relativo, il browser tenta di cercare una cartella `Images` relativa al percorso della cartella della pagina Web. In altre parole, il browser sta cercando il file di immagine `Admin/Images/PoweredByASPNET.gif`.

[![il file di immagine PoweredByASPNET. gif non è stato trovato](urls-in-master-pages-cs/_static/image3.png)](urls-in-master-pages-cs/_static/image2.png)

**Figura 02**: Impossibile trovare il file di immagine del `PoweredByASPNET.gif` ([fare clic per visualizzare l'immagine con dimensioni complete](urls-in-master-pages-cs/_static/image4.png))

### <a name="replacing-relative-urls-with-absolute-urls"></a>Sostituzione degli URL relativi con URL assoluti

Il contrario di un URL relativo è un *URL assoluto*, ovvero uno che inizia con una barra (`/`) o un protocollo, ad esempio `http://`. Poiché un URL assoluto specifica il percorso di una risorsa da un punto fisso noto, lo stesso URL assoluto è valido in qualsiasi pagina Web, indipendentemente dalla posizione della pagina Web nella struttura di cartelle del sito Web.

Per risolvere l'immagine interruppe mostrata nella figura 2, è necessario aggiornare l'attributo `src` dell'elemento `<img>` in modo che usi un URL assoluto anziché uno relativo. Per determinare l'URL assoluto corretto, visitare una delle pagine Web del sito Web ed esaminare la barra degli indirizzi. Come illustrato nella figura 2 della barra degli indirizzi, il percorso completo dell'applicazione Web viene `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/`. Pertanto, è possibile aggiornare l'attributo `src` dell'elemento `<img>` a uno dei due URL assoluti seguenti:

- `/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`
- `http://localhost:3908/ASPNET_MasterPages_Tutorial_04_CS/Images/PoweredByASPNET.gif`

Aggiornare l'attributo `src` dell'elemento `<img>` a un URL assoluto usando uno dei moduli indicati in precedenza e quindi visitare la pagina `~/Admin/Default.aspx` tramite un browser. Questa volta il browser troverà e visualizzerà correttamente il file di immagine `PoweredByASPNET.gif` (vedere la figura 3).

[![viene ora visualizzata l'immagine PoweredByASPNET. gif](urls-in-master-pages-cs/_static/image6.png)](urls-in-master-pages-cs/_static/image5.png)

**Figura 03**: ora viene visualizzata l'immagine `PoweredByASPNET.gif` ([fare clic per visualizzare l'immagine con dimensioni complete](urls-in-master-pages-cs/_static/image7.png))

Sebbene la codifica hardware nell'URL assoluto funzioni, associa strettamente il codice HTML al percorso del server e della cartella del sito Web, che può cambiare. L'uso di un URL assoluto del modulo `http://localhost:3908/...` è fragile perché il numero di porta precedente `localhost` viene selezionato automaticamente ogni volta che viene avviato il server Web di sviluppo ASP.NET predefinito di Visual Studio. Analogamente, la parte `http://localhost` è valida solo quando si esegue il test in locale. Una volta che il codice è stato distribuito in un server di produzione, la base dell'URL verrà modificata in altro modo, ad esempio `http://www.yourserver.com`. L'URL assoluto nel formato `/ASPNET_MasterPages_Tutorial_04_CS/...` soffre anche della stessa fragilità, perché spesso il percorso dell'applicazione è diverso tra i server di sviluppo e di produzione.

La novità è che ASP.NET offre un metodo per generare un URL relativo valido in fase di esecuzione.

## <a name="usingandresolveclienturl"></a>Utilizzo di`~`e`ResolveClientUrl`

Anziché utilizzare un URL assoluto, ASP.NET consente agli sviluppatori di pagine di utilizzare la tilde (`~`) per indicare la radice dell'applicazione Web. Ad esempio, in precedenza in questa esercitazione ho utilizzato la notazione `~/Admin/Default.aspx` nel testo per fare riferimento alla pagina `Default.aspx` nella cartella `Admin`. Il `~` indica che la cartella `Admin` è una sottocartella della radice dell'applicazione Web.

Il [metodo`ResolveClientUrl`](https://msdn.microsoft.com/library/system.web.ui.control.resolveclienturl.aspx) della classe `Control` accetta un URL e lo modifica in un URL relativo appropriato per la pagina Web in cui risiede il controllo. Ad esempio, la chiamata di `ResolveClientUrl("~/Images/PoweredByASPNET.gif")` da `About.aspx` restituisce `Images/PoweredByASPNET.gif`. La chiamata da `~/Admin/Default.aspx`, tuttavia, restituisce `../Images/PoweredByASPNET.gif`.

> [!NOTE]
> Poiché tutti i controlli server ASP.NET derivano dalla classe `Control`, tutti i controlli server hanno accesso al metodo `ResolveClientUrl`. Anche la classe `Page` deriva dalla classe `Control`, vale a dire che è possibile usare questo metodo direttamente dalle classi code-behind delle pagine ASP.NET.

### <a name="usingin-the-declarative-markup"></a>Uso di`~`nel markup dichiarativo

Diversi controlli Web ASP.NET includono proprietà correlate all'URL: il controllo HyperLink ha una proprietà `NavigateUrl`; il controllo immagine ha una proprietà `ImageUrl`; E così via. Quando ne viene eseguito il rendering, questi controlli passano i relativi valori di proprietà correlati all'URL a `ResolveClientUrl`. Di conseguenza, se queste proprietà contengono un `~` per indicare la radice dell'applicazione Web, l'URL verrà modificato in un URL relativo valido.

Tenere presente che solo i controlli server ASP.NET trasformano le `~` nelle proprietà relative all'URL. Se un `~` viene visualizzato nel markup HTML statico, ad esempio `<img src="~/Images/PoweredByASPNET.gif" />`, il motore ASP.NET invia il `~` al browser insieme al resto del contenuto HTML. Il browser presuppone che il `~` faccia parte dell'URL. Ad esempio, se il browser riceve il markup `<img src="~/Images/PoweredByASPNET.gif" />` presuppone che sia presente una sottocartella denominata `~` con una sottocartella `Images` che contiene il file di immagine `PoweredByASPNET.gif`.

Per correggere il markup dell'immagine in `Site.master`, sostituire l'elemento `<img>` esistente con un controllo Web ASP.NET Image. Impostare la `ID` del controllo Web Image su `PoweredByImage`, la relativa proprietà `ImageUrl` su `~/Images/PoweredByASPNET.gif`e la relativa proprietà `AlternateText` su "Powered by ASP.NET!"

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample5.aspx)]

Dopo aver apportato questa modifica alla pagina master, rivedere nuovamente la pagina `~/Admin/Default.aspx`. Questa volta il file di immagine `PoweredByASPNET.gif` viene visualizzato nella pagina (vedere la figura 3). Quando viene eseguito il rendering del controllo Web Image, viene usato il metodo `ResolveClientUrl` per risolvere il relativo valore della proprietà `ImageUrl`. In `~/Admin/Default.aspx` il `ImageUrl` viene convertito in un URL relativo appropriato, come illustrato nel seguente frammento di codice sorgente HTML:

[!code-html[Main](urls-in-master-pages-cs/samples/sample6.html)]

> [!NOTE]
> Oltre a essere utilizzato nelle proprietà di un controllo Web basato su URL, è possibile utilizzare la `~` anche quando si chiamano i metodi `Response.Redirect` e `Server.MapPath`, tra gli altri. Inoltre, il metodo `ResolveClientUrl` può essere richiamato direttamente da un markup dichiarativo di una pagina master o ASP.NET, se necessario. vedere il post del Blog di [Fritz Onion](https://www.pluralsight.com/blogs/fritz/) [con `ResolveClientUrl` nel markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx).

## <a name="fixing-the-master-pages-remaining-relative-urls"></a>Correzione degli URL relativi rimanenti della pagina master

Oltre all'elemento `<img>` nel `footerContent` appena corretto, la pagina master contiene un URL relativo che richiede attenzione. L'area `topContent` include il collegamento "esercitazioni sulle pagine master", che fa riferimento a `Default.aspx`.

[!code-html[Main](urls-in-master-pages-cs/samples/sample7.html)]

Poiché questo URL è relativo, invierà l'utente alla pagina `Default.aspx` nella cartella della pagina di contenuto che sta visitando. Per fare in modo che questo collegamento indichi sempre `Default.aspx` nella cartella radice, è necessario sostituire l'elemento `<a>` con un controllo Web collegamento ipertestuale, in modo da poter usare la notazione `~`.

Rimuovere il markup dell'elemento `<a>` e aggiungere al suo posto un controllo collegamento ipertestuale. Impostare la `ID` del collegamento ipertestuale su `lnkHome`, la relativa proprietà `NavigateUrl` su `~/Default.aspx`e la relativa proprietà `Text` su "esercitazioni su pagine master".

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample8.aspx)]

La procedura è terminata. A questo punto, tutti gli URL della pagina master sono basati correttamente quando viene eseguito il rendering da una pagina di contenuto indipendentemente dalle cartelle in cui si trovano la pagina master e la pagina contenuto.

### <a name="automatic-url-resolution-in-theheadsection"></a>Risoluzione automatica degli URL nella sezione`<head>`

Nell'esercitazione [*creazione di un layout a livello di sito tramite le pagine master*](creating-a-site-wide-layout-using-master-pages-cs.md) è stato aggiunto un `<link>` al file di `Styles.css` nell'area `<head>`:

[!code-aspx[Main](urls-in-master-pages-cs/samples/sample9.aspx)]

Mentre l'attributo `href` dell'elemento `<link>` è relativo, viene convertito automaticamente in un percorso appropriato in fase di esecuzione. Come illustrato in [*specifica del titolo, dei metatag e di altre intestazioni HTML nell'esercitazione della pagina master*](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md) , l'area `<head>` è in realtà un controllo lato server, che consente di modificare il contenuto dei controlli interni quando ne viene eseguito il rendering.

Per verificarlo, visitare nuovamente la pagina `~/Admin/Default.aspx` e visualizzare l'origine HTML inviata al browser. Come illustrato nel frammento di codice seguente, l'attributo `href` dell'elemento `<link>` è stato modificato automaticamente in un URL relativo appropriato, `../Styles.css`.

[!code-html[Main](urls-in-master-pages-cs/samples/sample10.html)]

## <a name="summary"></a>Riepilogo

Spesso le pagine master includono collegamenti, immagini e altre risorse esterne che devono essere specificate tramite un URL. Poiché la pagina master e le pagine di contenuto potrebbero non esistere nella stessa cartella, è importante astenersi da usare URL relativi. Sebbene sia possibile usare URL assoluti hardcoded, l'URL assoluto viene abbinato all'applicazione Web. Se l'URL assoluto viene modificato, come spesso accade quando si trasferisce o si distribuisce un'applicazione Web, è necessario ricordare di tornare indietro e aggiornare gli URL assoluti.

L'approccio ideale consiste nell'usare la tilde (`~`) per indicare la radice dell'applicazione. I controlli Web ASP.NET che contengono le proprietà correlate all'URL mappano i `~` alla radice dell'applicazione in fase di esecuzione. Internamente, i controlli Web usano il metodo `ResolveClientUrl` della classe `Control` per generare un URL relativo valido. Questo metodo è pubblico e disponibile da ogni controllo server (inclusa la classe `Page`), quindi può essere usato a livello di programmazione dalle classi code-behind, se necessario.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Pagine master in ASP.NET](http://www.odetocode.com/Articles/419.aspx)
- [Riassegnazione URL in una pagina master](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx#urls)
- [Uso di `ResolveClientUrl` nel markup](https://www.pluralsight.com/blogs/fritz/archive/2006/02/06/18596.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 3,5 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Scott può essere raggiunto in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](specifying-the-title-meta-tags-and-other-html-headers-in-the-master-page-cs.md)
> [Successivo](control-id-naming-in-content-pages-cs.md)
