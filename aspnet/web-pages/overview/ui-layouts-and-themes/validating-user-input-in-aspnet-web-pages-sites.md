---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: Convalida dell'input utente nei siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo illustra come convalidare le informazioni che si ottengono dagli utenti &mdash;, per assicurarsi che gli utenti immettano informazioni valide nei moduli HTML in un come...
ms.author: riande
ms.date: 02/20/2014
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: e6f8e1051d09d11f1756bfada44a73ba7c2a1db2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563504"
---
# <a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a>Convalida dell'input utente nei siti Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come convalidare le informazioni che si ottengono dagli utenti &mdash;, per assicurarsi che gli utenti immettano informazioni valide nei moduli HTML in un sito di Pagine Web ASP.NET (Razor).
> 
> Contenuto dell'esercitazione:
> 
> - Come verificare che l'input di un utente corrisponda ai criteri di convalida definiti.
> - Come determinare se tutti i test di convalida sono stati superati.
> - Come visualizzare gli errori di convalida e il modo in cui formattarli.
> - Come convalidare i dati che non provengono direttamente dagli utenti.
> 
> Questi sono i concetti di programmazione di ASP.NET introdotti nell'articolo:
> 
> - Helper `Validation`.
> - I metodi `Html.ValidationSummary` e `Html.ValidationMessage`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

In questo articolo sono contenute le sezioni seguenti:

- [Panoramica della convalida dell'input dell'utente](#Overview_of_User_Input_Validation)
- [Convalida dell'input utente](#Validating_User_Input)
- [Aggiunta della convalida sul lato client](#Adding_Client-Side_Validation)
- [Formattazione degli errori di convalida](#Formatting_Validation_Errors)
- [Convalida dei dati che non provengono direttamente dagli utenti](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a>Panoramica della convalida dell'input dell'utente

Se si richiede agli utenti di immettere informazioni in una pagina, ad esempio in un modulo, è importante assicurarsi che i valori immessi siano validi. Ad esempio, non si vuole elaborare un modulo in cui mancano informazioni critiche.

Quando gli utenti immettono valori in un form HTML, i valori immessi sono stringhe. In molti casi, i valori necessari sono altri tipi di dati, ad esempio numeri interi o date. È pertanto necessario assicurarsi che i valori immessi dagli utenti possano essere convertiti correttamente nei tipi di dati appropriati.

È possibile che siano presenti anche alcune restrizioni per i valori. Anche se gli utenti immettono correttamente un numero intero, ad esempio, potrebbe essere necessario assicurarsi che il valore rientri in un determinato intervallo.

![Errori di convalida che usano classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> **Importante** La convalida dell'input dell'utente è importante anche per la sicurezza. Quando si limitano i valori che gli utenti possono immettere nei moduli, si riduce la probabilità che un utente possa immettere un valore che può compromettere la sicurezza del sito.

<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a>Convalida dell'input utente

In Pagine Web ASP.NET 2, è possibile usare l'helper `Validator` per testare l'input dell'utente. L'approccio di base consiste nell'eseguire le operazioni seguenti:

1. Determinare gli elementi di input (campi) che si desidera convalidare.

    In genere si convalidano i valori negli elementi `<input>` in un form. Tuttavia, è consigliabile convalidare tutti gli input, anche quelli provenienti da un elemento soggetto a vincoli come un elenco `<select>`. Ciò consente di verificare che gli utenti non ignorino i controlli in una pagina e inviino un modulo.
2. Nel codice della pagina aggiungere singoli controlli di convalida per ogni elemento di input usando i metodi dell'helper `Validation`.

    Per verificare i campi obbligatori, usare `Validation.RequireField(field, [error message])` (per un singolo campo) o `Validation.RequireFields(field1, field2, ...))` (per un elenco di campi). Per altri tipi di convalida, utilizzare `Validation.Add(field, ValidationType)`. Per `ValidationType`, è possibile usare le opzioni seguenti:

    `Validator.DateTime ([error message])`  
   `Validator.Decimal([error message])`  
   `Validator.EqualsTo(otherField [, error message])`  
   `Validator.Float([error message])`  
   `Validator.Integer([error message])`  
   `Validator.Range(min, max [, error message])`  
   `Validator.RegEx(pattern [, error message])`  
   `Validator.Required([error message])`  
   `Validator.StringLength(length)`  
   `Validator.Url([error message])`
3. Quando la pagina viene inviata, controllare se la convalida è stata superata controllando `Validation.IsValid`:

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    Se si verificano errori di convalida, si ignora la normale elaborazione della pagina. Se, ad esempio, lo scopo della pagina è aggiornare un database, non è necessario eseguire questa operazione fino a quando non sono stati corretti tutti gli errori di convalida.
4. Se si verificano errori di convalida, visualizzare i messaggi di errore nel markup della pagina utilizzando `Html.ValidationSummary` o `Html.ValidationMessage`o entrambi.

Nell'esempio seguente viene illustrata una pagina in cui vengono illustrati questi passaggi.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

Per verificare il funzionamento della convalida, eseguire questa pagina ed effettuare intenzionalmente errori. Ecco, ad esempio, l'aspetto della pagina se si dimentica di immettere un nome di corso, se si immette un e se si immette una data non valida:

![Errori di convalida nella pagina di cui è stato eseguito il rendering](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a>Aggiunta della convalida lato client

Per impostazione predefinita, l'input dell'utente viene convalidato dopo che gli utenti hanno inviato la pagina, ovvero la convalida viene eseguita nel codice del server. Uno svantaggio di questo approccio consiste nel fatto che gli utenti non sanno che hanno apportato un errore fino a quando non inviano la pagina. Se un modulo è lungo o complesso, è possibile che la segnalazione degli errori solo dopo l'invio della pagina non sia agevole per l'utente.

È possibile aggiungere il supporto per eseguire la convalida nello script client. In tal caso, la convalida viene eseguita mentre gli utenti lavorano nel browser. Si supponga, ad esempio, di specificare che un valore deve essere un numero intero. Se un utente immette un valore non Integer, l'errore viene segnalato non appena l'utente lascia il campo di immissione. Gli utenti ottengono un feedback immediato, che è molto utile. La convalida basata su client può anche ridurre il numero di volte in cui l'utente deve inviare il modulo per correggere più errori.

> [!NOTE]
> Anche se si usa la convalida lato client, la convalida viene sempre eseguita nel codice del server. L'esecuzione della convalida nel codice del server è una misura di sicurezza, nel caso in cui gli utenti ignorino la convalida basata su client.

1. Registrare le librerie JavaScript seguenti nella pagina:  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

   Due librerie sono caricabili da una rete per la distribuzione di contenuti (CDN), pertanto non è necessario che siano necessariamente presenti nel computer o nel server. Tuttavia, è necessario disporre di una copia locale di *jQuery. Validate. unintrusiv. js*. Se non si sta già lavorando con un modello WebMatrix (ad esempio, un **sito iniziale** ) che include la libreria, creare un sito Web Pages basato sul **sito Starter**. Copiare quindi il file con *estensione js* nel sito corrente.
2. Nel markup, per ogni elemento che si sta convalidando, aggiungere una chiamata a `Validation.For(field)`. Questo metodo genera attributi utilizzati dalla convalida lato client. Anziché emettere codice JavaScript effettivo, il metodo genera attributi come `data-val-...`. Questi attributi supportano la convalida client non intrusiva che usa jQuery per eseguire il lavoro.

Nella pagina seguente viene illustrato come aggiungere funzionalità di convalida client all'esempio illustrato in precedenza.

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

Non tutti i controlli di convalida vengono eseguiti nel client. In particolare, la convalida del tipo di dati (Integer, date e così via) non viene eseguita nel client. I controlli seguenti funzionano sia sul client che sul server:

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

In questo esempio, il test per una data valida non funzionerà nel codice client. Il test verrà tuttavia eseguito nel codice del server.

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a>Formattazione degli errori di convalida

È possibile controllare la modalità di visualizzazione degli errori di convalida definendo classi CSS con i nomi riservati seguenti:

- `field-validation-error` Definisce l'output del metodo di `Html.ValidationMessage` quando viene visualizzato un errore.
- `field-validation-valid` Definisce l'output del metodo `Html.ValidationMessage` quando non è presente alcun errore.
- `input-validation-error` Definisce la modalità di rendering degli elementi `<input>` quando si verifica un errore. Ad esempio, è possibile usare questa classe per impostare il colore di sfondo di un &lt;input&gt; elemento su un colore diverso se il relativo valore non è valido.) Questa classe CSS viene utilizzata solo durante la convalida del client (in Pagine Web ASP.NET 2).
- `input-validation-valid` Definisce l'aspetto degli elementi `<input>` quando non è presente alcun errore.
- `validation-summary-errors` Definisce l'output del metodo `Html.ValidationSummary` in cui viene visualizzato un elenco di errori.
- `validation-summary-valid` Definisce l'output del metodo `Html.ValidationSummary` quando non è presente alcun errore.

Il `<style>` blocco seguente mostra le regole per le condizioni di errore.

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

Se si include questo blocco di stile nelle pagine di esempio riportate in precedenza in questo articolo, la visualizzazione degli errori sarà simile alla figura seguente:

![Errori di convalida che usano classi di stile CSS](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> Se non si usa la convalida client in Pagine Web ASP.NET 2, le classi CSS per gli elementi `<input>` (`input-validation-error` e `input-validation-valid` non hanno alcun effetto.

### <a name="static-and-dynamic-error-display"></a>Visualizzazione degli errori statici e dinamici

Le regole CSS sono disponibili in coppie, ad esempio `validation-summary-errors` e `validation-summary-valid`. Queste coppie consentono di definire le regole per entrambe le condizioni: una condizione di errore e una condizione "normale" (non di errore). È importante comprendere che il markup per la visualizzazione degli errori viene sempre sottoposto a rendering, anche se non si verificano errori. Se, ad esempio, una pagina dispone di un metodo `Html.ValidationSummary` nel markup, l'origine della pagina conterrà il markup seguente anche quando la pagina viene richiesta per la prima volta:

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

In altre parole, il metodo `Html.ValidationSummary` esegue sempre il rendering di un elemento `<div>` e di un elenco, anche se l'elenco errori è vuoto. Analogamente, il metodo `Html.ValidationMessage` esegue sempre il rendering di un elemento `<span>` come segnaposto per un singolo errore di campo, anche se non è presente alcun errore.

In alcune situazioni, la visualizzazione di un messaggio di errore può comportare il riflusso della pagina e causare lo spostamento degli elementi della pagina. Le regole CSS che terminano con `-valid` consentono di definire un layout che può aiutare a evitare questo problema. È ad esempio possibile definire `field-validation-error` e `field-validation-valid` in modo che abbiano le stesse dimensioni fisse. In questo modo, l'area di visualizzazione del campo sarà statica e non modificherà il flusso di pagina se viene visualizzato un messaggio di errore.

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a>Convalida dei dati che non provengono direttamente dagli utenti

A volte è necessario convalidare le informazioni che non provengono direttamente da un form HTML. Un esempio tipico è una pagina in cui un valore viene passato in una stringa di query, come nell'esempio seguente:

`http://server/myapp/EditClassInformation?classid=1022`

In questo caso, è necessario assicurarsi che il valore passato alla pagina (in questo caso, 1022 per il valore di `classid`) sia valido. Non è possibile usare direttamente l'helper `Validation` per eseguire questa convalida. Tuttavia, è possibile usare altre funzionalità del sistema di convalida, ad esempio la possibilità di visualizzare i messaggi di errore di convalida.

> [!NOTE] 
> 
> **Importante** Convalidare sempre i valori ottenibili da *qualsiasi* origine, inclusi i valori dei campi del form, i valori della stringa di query e i valori dei cookie. È facile per gli utenti modificare questi valori, ad esempio per scopi dannosi. Quindi, è necessario controllare questi valori per proteggere l'applicazione.

Nell'esempio seguente viene illustrato come è possibile convalidare un valore passato in una stringa di query. Il codice verifica che il valore non sia vuoto e che sia un numero intero.

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

Si noti che il test viene eseguito quando la richiesta non è un modulo di invio (`if(!IsPost)`). Questo test verrebbe superato la prima volta che la pagina viene richiesta, ma non quando la richiesta è un invio del modulo.

Per visualizzare questo errore, è possibile aggiungere l'errore all'elenco degli errori di convalida chiamando `Validation.AddFormError("message")`. Se la pagina contiene una chiamata al metodo `Html.ValidationSummary`, l'errore viene visualizzato in tale posizione, proprio come un errore di convalida dell'input dell'utente.

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Utilizzo dei moduli HTML nei siti Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202892)
