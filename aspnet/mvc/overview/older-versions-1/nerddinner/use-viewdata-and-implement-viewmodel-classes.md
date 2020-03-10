---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: Usare ViewData e implementare classi ViewModel | Microsoft Docs
author: microsoft
description: Nel passaggio 6 viene illustrato come abilitare il supporto per gli scenari di modifica dei moduli più completi e vengono inoltre illustrati due approcci che possono essere utilizzati per passare i dati dai controller alle visualizzazioni:...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: ca9775417c2e25952511a73096fb76d5d4edaea2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541608"
---
# <a name="use-viewdata-and-implement-viewmodel-classes"></a>Usare ViewData e implementare classi ViewModel

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Questo è il passaggio 6 di un' [esercitazione gratuita sull'applicazione "NerdDinner"](introducing-the-nerddinner-tutorial.md) che illustra come creare un'applicazione Web di piccole dimensioni ma completa usando ASP.NET MVC 1.
> 
> Il passaggio 6 Mostra come abilitare il supporto per gli scenari di modifica dei moduli più completi e illustra anche due approcci che possono essere usati per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.
> 
> Se si usa ASP.NET MVC 3, è consigliabile seguire le esercitazioni [Introduzione con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) .

## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a>NerdDinner passaggio 6: ViewData e ViewModel

Abbiamo trattato una serie di scenari post di form e ho illustrato come implementare il supporto di creazione, aggiornamento ed eliminazione (CRUD). Ora l'implementazione di DinnersController verrà ulteriormente abilitata e il supporto per gli scenari di modifica dei moduli più completi. In questo modo verranno illustrati due approcci che è possibile usare per passare i dati dai controller alle visualizzazioni: ViewData e ViewModel.

### <a name="passing-data-from-controllers-to-view-templates"></a>Passaggio di dati dai controller ai modelli di visualizzazione

Una delle caratteristiche di definizione del modello MVC è la stretta "separazione dei problemi" che aiuta a applicare tra i diversi componenti di un'applicazione. I modelli, i controller e le visualizzazioni presentano ruoli e responsabilità ben definiti e comunicano tra loro in modo ben definito. Questo consente di promuovere la testabilità e il riutilizzo del codice.

Quando una classe controller decide di eseguire il rendering di una risposta HTML in un client, è responsabile del passaggio esplicito al modello di visualizzazione di tutti i dati necessari per eseguire il rendering della risposta. I modelli di visualizzazione non devono mai eseguire alcun recupero di dati o logica dell'applicazione e devono invece limitarsi a avere solo codice di rendering che è determinato dal modello o dai dati passati dal controller.

Al momento i dati del modello passati dalla classe DinnersController ai modelli di visualizzazione sono semplici e diretti, ovvero un elenco di oggetti Dinner nel caso di index () e di un singolo oggetto Dinner nel caso di Details (), Edit (), create () ed Delete (). Quando si aggiungono altre funzionalità dell'interfaccia utente all'applicazione, è spesso necessario passare più di questi dati per eseguire il rendering delle risposte HTML all'interno dei modelli di visualizzazione. Ad esempio, potrebbe essere necessario modificare il campo "paese" all'interno della casella di testo HTML in un controllo DropDownList per modificare e creare visualizzazioni. Anziché impostare come hardcoded l'elenco a discesa dei nomi dei paesi nel modello di visualizzazione, è possibile che si desideri generarlo da un elenco di paesi supportati popolati in modo dinamico. È necessario un modo per passare l'oggetto Dinner *e* l'elenco dei paesi supportati dal controller ai modelli di visualizzazione.

Si osserveranno due modi per eseguire questa operazione.

### <a name="using-the-viewdata-dictionary"></a>Uso del dizionario ViewData

La classe di base controller espone una proprietà del dizionario "ViewData" che può essere usata per passare elementi di dati aggiuntivi dai controller alle visualizzazioni.

Ad esempio, per supportare lo scenario in cui si vuole modificare la casella di testo "Country" nella visualizzazione di modifica da una casella di testo HTML a un oggetto DropDownList, è possibile aggiornare il metodo di azione Edit () in modo da passare (oltre a un oggetto Dinner) un oggetto Selecting che può essere usato come m Odel di un oggetto DropDownList dei paesi.

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

Il costruttore della selezione precedente accetta un elenco di contee per popolare l'elenco a discesa con e il valore attualmente selezionato.

È quindi possibile aggiornare il modello di visualizzazione Edit. aspx per usare il metodo helper HTML. DropDownList () anziché il metodo helper HTML. TextBox () usato in precedenza:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

Il metodo helper HTML. DropDownList () precedente accetta due parametri. Il primo è il nome dell'elemento del form HTML da restituire. Il secondo è il modello "Select" passato tramite il dizionario ViewData. Viene utilizzata la C# parola chiave "As" per eseguire il cast del tipo all'interno del dizionario come oggetto Select.

Ora, quando si esegue l'applicazione e si accede all'URL di */dinners/Edit/1* all'interno del browser, si noterà che l'interfaccia utente di modifica è stata aggiornata per visualizzare un DropDownList di paesi anziché una casella di testo:

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

Poiché viene inoltre eseguito il rendering del modello di visualizzazione di modifica dal metodo HTTP-POST Edit (negli scenari in cui si verificano errori), è necessario assicurarsi di aggiornare anche questo metodo per aggiungere l'oggetto Select a ViewData quando viene eseguito il rendering del modello di visualizzazione in scenari di errore:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

Ora lo scenario di modifica di DinnersController supporta un oggetto DropDownList.

### <a name="using-a-viewmodel-pattern"></a>Uso di un modello ViewModel

L'approccio del dizionario ViewData ha il vantaggio di essere abbastanza veloce e facile da implementare. Alcuni sviluppatori non amano usare i dizionari basati su stringa, tuttavia, poiché gli errori di digitazione possono causare errori che non verranno intercettati in fase di compilazione. Il dizionario ViewData non tipizzato richiede anche l'uso dell'operatore "As" o del cast quando si usa un linguaggio fortemente C# tipizzato, come in un modello di visualizzazione.

Un approccio alternativo che è possibile usare è spesso definito modello "ViewModel". Quando si usa questo modello, vengono create classi fortemente tipizzate ottimizzate per gli scenari di visualizzazione specifici e che espongono le proprietà per i valori dinamici o il contenuto necessari per i modelli di visualizzazione. Le classi controller possono quindi popolare e passare le classi ottimizzate per la visualizzazione al modello di visualizzazione da usare. In questo modo è possibile abilitare l'indipendenza dai tipi, il controllo in fase di compilazione e l'editor IntelliSense nei modelli di visualizzazione.

Ad esempio, per abilitare gli scenari di modifica del modulo Dinner è possibile creare una classe "DinnerFormViewModel", come riportato di seguito, che espone due proprietà fortemente tipizzate: un oggetto Dinner e il modello di selezione necessario per popolare il valore DropDownList dei paesi:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

È quindi possibile aggiornare il metodo di azione Edit () per creare il DinnerFormViewModel usando l'oggetto Dinner recuperato dal repository, quindi passarlo al modello di visualizzazione:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

Si aggiornerà quindi il modello di visualizzazione in modo che preveda "DinnerFormViewModel" invece di un oggetto "Dinner" cambiando l'attributo "Inherits" nella parte superiore della pagina Edit. aspx come segue:

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

Una volta eseguita questa operazione, IntelliSense della proprietà "Model" all'interno del modello di visualizzazione verrà aggiornato per riflettere il modello a oggetti del tipo DinnerFormViewModel che viene passato:

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

Possiamo quindi aggiornare il codice di visualizzazione per risolverlo. Si noti che di seguito viene illustrato come non modificare i nomi degli elementi di input creati (gli elementi del modulo verranno comunque denominati "title", "Country"), ma si stanno aggiornando i metodi helper HTML per recuperare i valori usando la classe DinnerFormViewModel:

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

Si aggiornerà anche il metodo Edit post per usare la classe DinnerFormViewModel durante il rendering degli errori:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

È anche possibile aggiornare i metodi di azione Create () per riutilizzare esattamente la stessa classe *DinnerFormViewModel* per abilitare il DropDownList dei paesi all'interno di tali metodi. Di seguito è riportata l'implementazione HTTP-GET:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

Di seguito è riportata l'implementazione del metodo HTTP-POST-creazione:

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

E ora le schermate di modifica e creazione supportano drop-downlists per la selezione del paese.

### <a name="custom-shaped-viewmodel-classes"></a>Classi ViewModel a forma personalizzata

Nello scenario precedente, la classe DinnerFormViewModel espone direttamente l'oggetto modello Dinner come proprietà, insieme a una proprietà del modello Selecting di supporto. Questo approccio funziona bene per gli scenari in cui l'interfaccia utente HTML che si vuole creare all'interno del modello di visualizzazione corrisponde relativamente strettamente agli oggetti del modello di dominio.

Per gli scenari in cui questo non è il caso, un'opzione che è possibile utilizzare consiste nel creare una classe ViewModel personalizzata il cui modello a oggetti è più ottimizzato per l'utilizzo da parte della vista e che potrebbe sembrare completamente diverso dall'oggetto modello di dominio sottostante. Ad esempio, potrebbe esporre nomi di proprietà e/o proprietà di aggregazione diversi raccolti da più oggetti modello.

Le classi ViewModel personalizzate possono essere usate sia per passare dati da controller a visualizzazioni per eseguire il rendering, sia per gestire i dati del modulo di cui è stato eseguito il postback al metodo di azione di un controller. Per questo scenario successivo, è possibile fare in modo che il metodo di azione aggiorni un oggetto ViewModel con i dati inviati dal form, quindi usare l'istanza di ViewModel per eseguire il mapping o il recupero di un oggetto modello di dominio effettivo.

Le classi ViewModel a forma personalizzata possono fornire una grande flessibilità ed esaminare ogni volta che si trova il codice di rendering all'interno dei modelli di visualizzazione o il codice di invio del modulo all'interno dei metodi di azione che iniziano a diventare troppo complicati. Si tratta spesso di un segno che i modelli di dominio non corrispondono in modo corretto all'interfaccia utente generata e che può essere utile una classe ViewModel intermedia a forma personalizzata.

### <a name="next-step"></a>Passo successivo

Esaminiamo ora come possiamo usare i componenti parziali e le pagine master per riutilizzare e condividere l'interfaccia utente nell'applicazione.

> [!div class="step-by-step"]
> [Precedente](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [Successivo](re-use-ui-using-master-pages-and-partials.md)
