---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: Introduzione ai siti di debug Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Il debug è il processo di individuazione e correzione degli errori nelle tabelle codici. In questo capitolo vengono illustrati alcuni strumenti e tecniche che è possibile utilizzare per eseguire il debug e l'analisi...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: ae7d871e56326610c043dc20fe6e0919e1b4ac89
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624369"
---
# <a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introduzione ai siti di debug Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra vari modi per eseguire il debug di pagine in un sito Web Pagine Web ASP.NET (Razor). Il debug è il processo di individuazione e correzione degli errori nelle tabelle codici.
>
> **Cosa si apprenderà:**
>
> - Come visualizzare informazioni che consentono di analizzare ed eseguire il debug delle pagine.
> - Come usare gli strumenti di debug in Visual Studio.
>
>
> Queste sono le funzionalità di ASP.NET introdotte nell'articolo:
>
> - Helper `ServerInfo`.
> - `ObjectInfo` helper.
>
>
> ## <a name="software-versions"></a>Versioni del software
>
>
> - Pagine Web ASP.NET (Razor) 3
> - Visual Studio 2013
>
>
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2. È possibile usare WebMatrix 3, ma il debugger integrato non è supportato.

Un aspetto importante della risoluzione dei problemi e degli errori nel codice consiste nell'evitare che siano in primo luogo. È possibile eseguire questa operazione inserendo sezioni del codice che potrebbero causare errori nei blocchi di `try/catch`. Per altre informazioni, vedere la sezione relativa alla gestione degli errori in [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

Il `ServerInfo` Helper è uno strumento di diagnostica che offre una panoramica delle informazioni sull'ambiente del server Web che ospita la pagina. Mostra anche le informazioni sulla richiesta HTTP che vengono inviate quando un browser richiede la pagina. L'helper `ServerInfo` Visualizza l'identità dell'utente corrente, il tipo di browser che ha effettuato la richiesta e così via. Questo tipo di informazioni può essere utile per risolvere i problemi comuni.

1. Creare una nuova pagina Web denominata *la. cshtml*.
2. Alla fine della pagina, immediatamente prima del tag di chiusura `</body>` aggiungere `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    È possibile aggiungere il codice `ServerInfo` in un punto qualsiasi della pagina. Tuttavia, l'aggiunta alla fine manterrà l'output separato dal contenuto di altre pagine, semplificando la lettura.

    > [!NOTE]
    >
    > **Importante** Prima di spostare le pagine Web in un server di produzione, è necessario rimuovere il codice di diagnostica dalle pagine Web. Questo vale per l'helper `ServerInfo` e per le altre tecniche di diagnostica in questo articolo che implicano l'aggiunta di codice a una pagina. Non si vuole che i visitatori del sito Web visualizzino informazioni sul nome del server, i nomi utente, i percorsi sul server e dettagli simili, perché questo tipo di informazioni può essere utile per gli utenti con finalità dannose.
3. Salvare la pagina ed eseguirla in un browser.

    ![Debug-1](introduction-to-debugging/_static/image1.jpg)

    L'helper `ServerInfo` Visualizza quattro tabelle di informazioni nella pagina:

   - Configurazione del server. In questa sezione vengono fornite informazioni sul server Web di hosting, incluso il nome del computer, la versione di ASP.NET in esecuzione, il nome di dominio e l'ora del server.
   - Variabili del server ASP.NET. In questa sezione vengono fornite informazioni dettagliate sui numerosi dettagli del protocollo HTTP, denominati variabili HTTP, e sui valori che fanno parte di ogni richiesta di pagina Web.
   - Informazioni sul runtime HTTP. In questa sezione vengono fornite informazioni dettagliate su come la versione del Framework Microsoft .NET in cui viene eseguita la pagina Web, il percorso, i dettagli relativi alla cache e così via. Come si è appreso in [Introduzione alla programmazione Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890), pagine Web ASP.NET usando il sintassi Razor sono basati sulla tecnologia server Web ASP.NET di Microsoft, che è basata su un'ampia libreria di sviluppo software denominata .NET Framework.
   - Variabili di ambiente. In questa sezione viene fornito un elenco di tutte le variabili di ambiente locali e dei relativi valori sul server Web.

     Una descrizione completa di tutte le informazioni sul server e sulla richiesta esula dall'ambito di questo articolo, ma è possibile notare che il `ServerInfo` helper restituisce numerose informazioni di diagnostica. Per ulteriori informazioni sui valori restituiti da `ServerInfo`, vedere [variabili di ambiente riconosciute](https://technet.microsoft.com/library/dd560744(WS.10).aspx) nel sito Web Microsoft TechNet e [variabili del server IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) nel sito Web MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Incorporamento di espressioni di output per visualizzare i valori di pagina

Un altro modo per vedere cosa accade nel codice consiste nell'incorporare espressioni di output nella pagina. Come è noto, è possibile restituire direttamente il valore di una variabile aggiungendo qualcosa come `@myVariable` o `@(subTotal * 12)` alla pagina. Per il debug, è possibile inserire queste espressioni di output in punti strategici nel codice. In questo modo è possibile visualizzare il valore delle variabili chiave o il risultato dei calcoli quando viene eseguita la pagina. Al termine del debug, è possibile rimuovere le espressioni o impostarle come commento. In questa procedura viene illustrato un modo tipico per utilizzare le espressioni incorporate per facilitare il debug di una pagina.

1. Creare una nuova pagina WebMatrix denominata *OutputExpression. cshtml*.
2. Sostituire il contenuto della pagina con il codice seguente:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    Nell'esempio viene utilizzata un'istruzione `switch` per verificare il valore della variabile `weekday` e quindi visualizzare un messaggio di output diverso a seconda del giorno della settimana in cui si trova. Nell'esempio, il blocco `if` all'interno del primo blocco di codice modifica arbitrariamente il giorno della settimana aggiungendo un giorno al valore corrente del giorno della settimana. Si tratta di un errore introdotto a scopo illustrativo.
3. Salvare la pagina ed eseguirla in un browser.

    Nella pagina viene visualizzato il messaggio relativo al giorno errato della settimana. Indipendentemente dal giorno della settimana in cui si trova, il messaggio verrà visualizzato per un giorno dopo. Sebbene in questo caso si conosca il motivo per cui il messaggio è disattivato (poiché il codice imposta deliberatamente il valore del giorno errato), in realtà è spesso difficile sapere dove si verificano problemi nel codice. Per eseguire il debug, è necessario scoprire cosa accade al valore di oggetti chiave e variabili, ad esempio `weekday`.
4. Aggiungere espressioni di output inserendo `@weekday` come illustrato nelle due posizioni indicate dai commenti nel codice. Queste espressioni di output visualizzeranno i valori della variabile in quel punto nell'esecuzione del codice.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Salvare ed eseguire la pagina in un browser.

    La pagina Visualizza prima di tutto il giorno della settimana, quindi il giorno della settimana aggiornato risultante dall'aggiunta di un giorno, quindi il messaggio risultante dall'istruzione `switch`. L'output delle due espressioni variabili (`@weekday`) non contiene spazi tra i giorni perché non è stato aggiunto alcun tag HTML `<p>` all'output; le espressioni sono solo per i test.

    ![Debug-2](introduction-to-debugging/_static/image2.jpg)

    A questo punto è possibile visualizzare la posizione dell'errore. Quando si visualizza per la prima volta la variabile `weekday` nel codice, viene visualizzato il giorno corretto. Quando viene visualizzata la seconda volta, dopo il blocco `if` nel codice, il giorno è disattivato di uno. Quindi, è noto che si è verificato un evento tra il primo e il secondo aspetto della variabile giorno della settimana. Se si trattasse di un bug reale, questo tipo di approccio consente di limitare il percorso del codice che causa il problema.
6. Per correggere il codice nella pagina, rimuovere le due espressioni di output aggiunte e rimuovere il codice che modifica il giorno della settimana. Il blocco di codice rimanente e completo ha un aspetto simile all'esempio seguente:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Eseguire la pagina in un browser. Questa volta viene visualizzato il messaggio corretto visualizzato per il giorno della settimana effettivo.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Uso dell'helper ObjectInfo per visualizzare i valori degli oggetti

Il `ObjectInfo` Helper Visualizza il tipo e il valore di ogni oggetto passato. È possibile usarlo per visualizzare il valore di variabili e oggetti nel codice, come per le espressioni di output nell'esempio precedente, oltre a visualizzare informazioni sul tipo di dati relative all'oggetto.

1. Aprire il file denominato *OutputExpression. cshtml* creato in precedenza.
2. Sostituire tutto il codice nella pagina con il blocco di codice seguente:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Salvare ed eseguire la pagina in un browser.

    ![Debug-4](introduction-to-debugging/_static/image3.jpg)

    In questo esempio, l'helper `ObjectInfo` Visualizza due elementi:

   - Tipo. Per la prima variabile, il tipo viene `DayOfWeek`. Per la seconda variabile, il tipo viene `String`.
   - Valore. In questo caso, poiché è già visualizzato il valore della variabile di saluto nella pagina, il valore viene nuovamente visualizzato quando si passa la variabile a `ObjectInfo`.

     Per gli oggetti più complessi, l'helper `ObjectInfo` può visualizzare in sostanza &#8212; altre informazioni, ma è in grado di visualizzare i tipi e i valori di tutte le proprietà di un oggetto.

## <a name="using-debugging-tools-in-visual-studio"></a>Uso degli strumenti di debug in Visual Studio

Per un'esperienza di debug più completa, usare [Visual Studio](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017). Con Visual Studio è possibile impostare un punto di interruzione nel codice in corrispondenza della riga che si desidera controllare.

![Imposta punto di interruzione](introduction-to-debugging/_static/image1.png)

Quando si esegue il test del sito Web, il codice in esecuzione si interrompe in corrispondenza del punto di interruzione.

![punto di interruzione REACH](introduction-to-debugging/_static/image2.png)

È possibile esaminare i valori correnti delle variabili e scorrere il codice riga per riga.

![Vedi valori](introduction-to-debugging/_static/image3.png)

Per informazioni sull'uso del debugger integrato in Visual Studio per eseguire il debug di ASP.NET Razor Pages, vedere [programming pagine Web ASP.NET (Razor) using Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Risorse aggiuntive

- [Programmazione Pagine Web ASP.NET (Razor) con Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variabili del server IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Variabili di ambiente riconosciute](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
