---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Uso dei moduli HTML nei siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Un modulo è una sezione di un documento HTML in cui è possibile inserire controlli di input utente, ad esempio caselle di testo, caselle di controllo, pulsanti di opzione ed elenchi a discesa. Si usano i moduli WH...
ms.author: riande
ms.date: 02/10/2014
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: c7d4802063c8610a246afe67bd15eea429f7304a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639706"
---
# <a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Uso dei moduli HTML nei siti Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come elaborare un form HTML (con caselle di testo e pulsanti) quando si lavora in un sito Web Pagine Web ASP.NET (Razor).
> 
> **Cosa si apprenderà:** 
> 
> - Come creare un form HTML.
> - Come leggere l'input dell'utente dal modulo.
> - Come convalidare l'input dell'utente.
> - Come ripristinare i valori del modulo dopo l'invio della pagina.
> 
> Questi sono i concetti di programmazione di ASP.NET introdotti nell'articolo:
> 
> - Oggetto `Request`.
> - Convalida dell'input.
> - Codifica HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

## <a name="creating-a-simple-html-form"></a>Creazione di un form HTML semplice

1. Creare un nuovo sito Web.
2. Nella cartella radice creare una pagina Web denominata *form. cshtml* e immettere il markup seguente:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Avviare la pagina nel browser. In WebMatrix, nell'area di lavoro **file** , fare clic con il pulsante destro del mouse sul file e scegliere **Avvia nel browser**. Viene visualizzato un form semplice con tre campi di input e un pulsante **Submit** .

    ![Screenshot di un modulo con tre caselle di testo.](4-working-with-forms/_static/image1.png)

    A questo punto, se si fa clic sul pulsante **Submit (Invia** ), non viene eseguita alcuna operazione. Per rendere il modulo utile, è necessario aggiungere il codice che viene eseguito nel server.

## <a name="reading-user-input-from-the-form"></a>Lettura dell'input utente dal modulo

Per elaborare il form, è necessario aggiungere il codice che legge i valori dei campi inviati ed esegue un'operazione con essi. Questa procedura illustra come leggere i campi e visualizzare l'input dell'utente nella pagina. In un'applicazione di produzione, in genere si eseguono operazioni più interessanti con l'input dell'utente. Questa operazione verrà eseguita nell'articolo sull'utilizzo dei database.

1. Nella parte superiore del file *form. cshtml* immettere il codice seguente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Quando l'utente richiede per la prima volta la pagina, viene visualizzato solo il modulo vuoto. L'utente (che corrisponde a) compila il modulo e quindi fa clic su **Submit (Invia**). In questo modo viene inviato (post) l'input dell'utente al server. Per impostazione predefinita, la richiesta passa alla stessa pagina (vale a dire, *form. cshtml*).

    Quando si invia la pagina questa volta, i valori immessi vengono visualizzati appena sopra il modulo:

    ![Screenshot che mostra i valori immessi visualizzati nella pagina.](4-working-with-forms/_static/image2.png)

    Esaminare il codice per la pagina. Usare prima di tutto il metodo `IsPost` per determinare se la pagina viene pubblicata &#8212; , ovvero se un utente ha fatto clic sul pulsante **Submit (Invia** ). Se si tratta di un post, `IsPost` restituisce true. Questo è il modo standard in Pagine Web ASP.NET per determinare se si sta lavorando con una richiesta iniziale (una richiesta GET) o un postback (richiesta POST). Per ulteriori informazioni su GET e POST, vedere l'intestazione laterale "HTTP GET e POST e la proprietà nopost" in [Introduzione alla programmazione pagine Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).

    A questo punto, si ottengono i valori che l'utente ha compilato dall'oggetto `Request.Form` e li si inserisce nelle variabili per un momento successivo. L'oggetto `Request.Form` contiene tutti i valori che sono stati inviati con la pagina, ciascuno identificato da una chiave. La chiave equivale all'attributo `name` del campo del modulo che si desidera leggere. Ad esempio, per leggere il campo `companyname` (casella di testo), è possibile usare `Request.Form["companyname"]`.

    I valori dei moduli vengono archiviati nell'oggetto `Request.Form` come stringhe. Pertanto, quando è necessario utilizzare un valore come un numero o una data o un altro tipo, è necessario convertirlo da una stringa a tale tipo. Nell'esempio viene usato il metodo `AsInt` della `Request.Form` per convertire il valore del campo Employees (che contiene un numero Employee) in un valore integer.
2. Avviare la pagina nel browser, compilare i campi del modulo e fare clic su **Submit (Invia**). Nella pagina vengono visualizzati i valori immessi.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codifica HTML per aspetto e sicurezza
> 
> Il codice HTML ha usi speciali per i caratteri come `<`, `>`e `&`. Se questi caratteri speciali vengono visualizzati quando non sono previsti, possono rovinare l'aspetto e la funzionalità della pagina Web. Ad esempio, il browser interpreta il carattere `<` (a meno che non sia seguito da uno spazio) come inizio di un elemento HTML, ad esempio `<b>` o `<input ...>`. Se il browser non riconosce l'elemento, viene semplicemente eliminata la stringa che inizia con `<` fino a quando non viene raggiunta una nuova funzionalità riconosciuta. Ovviamente, questo può causare un rendering strano nella pagina.
> 
> La codifica HTML sostituisce questi caratteri riservati con un codice che i browser interpretano come il simbolo corretto. Ad esempio, il carattere `<` viene sostituito con `&lt;` e il carattere di `>` viene sostituito con `&gt;`. Il browser esegue il rendering di queste stringhe di sostituzione come i caratteri che si desidera visualizzare.
> 
> È consigliabile usare la codifica HTML ogni volta che vengono visualizzate stringhe (input) ottenute da un utente. In caso contrario, un utente può tentare di ottenere una pagina Web per eseguire uno script dannoso o eseguire un'altra operazione che compromette la sicurezza del sito o che non è quello che si desidera. Questo aspetto è particolarmente importante se si prende l'input dell'utente, lo si archivia in un punto e &#8212; quindi lo si visualizza in un secondo momento, ad esempio come commento del Blog, revisione utente o qualcosa di simile.
> 
> Per evitare questi problemi, Pagine Web ASP.NET codifica automaticamente in HTML tutti i contenuti di testo restituiti dal codice. Ad esempio, quando si Visualizza il contenuto di una variabile o di un'espressione che usa codice come `@MyVar`, Pagine Web ASP.NET codifica automaticamente l'output.

## <a name="validating-user-input"></a>Convalida dell'input utente

Gli utenti commettono errori. Chiedere loro di compilare un campo e dimenticarlo, oppure di immettere il numero di dipendenti e digitare invece un nome. Per assicurarsi che un modulo sia stato compilato correttamente prima di elaborarlo, è necessario convalidare l'input dell'utente.

Questa procedura illustra come convalidare tutti e tre i campi del modulo per assicurarsi che l'utente non li lasci vuoti. Si verifica inoltre che il valore del conteggio dei dipendenti sia un numero. Se si verificano errori, verrà visualizzato un messaggio di errore che indica all'utente i valori che non hanno superato la convalida.

1. Nel file *form. cshtml* sostituire il primo blocco di codice con il codice seguente: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Per convalidare l'input dell'utente, usare l'helper `Validation`. Per registrare i campi necessari, chiamare `Validation.RequireField`. Si registrano altri tipi di convalida chiamando `Validation.Add` e specificando il campo da convalidare e il tipo di convalida da eseguire.

    Quando viene eseguita la pagina, ASP.NET esegue tutte le operazioni di convalida. È possibile controllare i risultati chiamando `Validation.IsValid`, che restituisce true se tutti gli elementi sono stati superati e false se non è stato possibile convalidare alcun campo. In genere, è possibile chiamare `Validation.IsValid` prima di eseguire qualsiasi elaborazione sull'input dell'utente.
2. Aggiornare l'elemento `<body>` aggiungendo tre chiamate al metodo `Html.ValidationMessage`, come indicato di seguito:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Per visualizzare i messaggi di errore di convalida, è possibile chiamare HTML.`ValidationMessage` e passargli il nome del campo per il quale si desidera il messaggio.
3. Eseguire la pagina. Lasciare vuoti i campi e fare clic su **Invia**. Vengono visualizzati messaggi di errore.

    ![Screenshot che mostra i messaggi di errore visualizzati se l'input dell'utente non supera la convalida.](4-working-with-forms/_static/image3.jpg)
4. Aggiungere una stringa, ad esempio "ABC", al campo **conteggio dipendenti** e fare di nuovo clic su **Invia** . Questa volta viene visualizzato un errore che indica che la stringa non è nel formato corretto, ovvero un numero intero.

    ![Screenshot che mostra i messaggi di errore visualizzati se gli utenti immettono una stringa per il campo Employees.](4-working-with-forms/_static/image4.jpg)

Pagine Web ASP.NET offre più opzioni per la convalida dell'input dell'utente, inclusa la possibilità di eseguire automaticamente la convalida usando uno script client, in modo che gli utenti ottengano feedback immediato nel browser. Per ulteriori informazioni, vedere la sezione [risorse aggiuntive](#Additional_Resources) in un secondo momento.

## <a name="restoring-form-values-after-postbacks"></a>Ripristino dei valori del modulo dopo i postback

Quando la pagina è stata testata nella sezione precedente, si potrebbe notare che se si è verificato un errore di convalida, tutto ciò che è stato immesso (non solo i dati non validi) era scomparso ed era necessario immettere di nuovo i valori per tutti i campi. Viene illustrato un punto importante: quando si invia una pagina, la si elabora e quindi si esegue nuovamente il rendering della pagina, la pagina viene ricreata da zero. Come si è visto, ciò significa che tutti i valori presenti nella pagina al momento dell'invio vengono persi.

Tuttavia, è possibile risolvere questo problema in modo semplice. È possibile accedere ai valori inviati (nell'oggetto `Request.Form`, in modo da poterli inserire di nuovo nei campi del form quando viene eseguito il rendering della pagina.

1. Nel file *form. cshtml* sostituire gli attributi `value` degli elementi `<input>` utilizzando l'attributo `value`: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    L'attributo `value` degli elementi `<input>` è stato impostato per leggere in modo dinamico il valore del campo dall'oggetto `Request.Form`. La prima volta che la pagina viene richiesta, i valori nell'oggetto `Request.Form` sono tutti vuoti. Questa operazione è corretta, perché in questo modo il form è vuoto.
2. Avviare la pagina nel browser, compilare i campi del modulo o lasciarli vuoti e fare clic su **Submit (Invia**). Viene visualizzata una pagina che mostra i valori inviati.

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [1.001 modi per ottenere input da utenti Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Uso di moduli ed elaborazione dell'input utente](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Convalida dell'input utente nelle pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Uso del completamento automatico nei moduli HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
