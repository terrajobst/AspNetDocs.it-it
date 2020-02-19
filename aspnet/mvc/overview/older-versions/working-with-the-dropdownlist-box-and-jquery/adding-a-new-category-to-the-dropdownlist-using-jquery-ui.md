---
uid: mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
title: Aggiunta di una nuova categoria a DropDownList mediante l'interfaccia utente di jQuery | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 01/12/2012
ms.assetid: 44aa1ac4-6ea2-48a2-972d-52710c48eae5
msc.legacyurl: /mvc/overview/older-versions/working-with-the-dropdownlist-box-and-jquery/adding-a-new-category-to-the-dropdownlist-using-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: 3207079ee468232e5f75b081421241c232936baf
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77455724"
---
# <a name="adding-a-new-category-to-the-dropdownlist-using-jquery-ui"></a>Aggiunta di una nuova categoria a DropDownList tramite l'interfaccia utente di jQuery

di [Rick Anderson](https://twitter.com/RickAndMSFT)

Il tag HTML `Select` è ideale per presentare un elenco di dati di categoria fissi, ma spesso è necessario aggiungere una nuova categoria. Si supponga di voler aggiungere il genere "opera" alle categorie nel database? In questa sezione verrà usata l'interfaccia utente di jQuery per aggiungere una finestra di dialogo che è possibile usare per aggiungere una nuova categoria. Nell'immagine seguente viene illustrato il modo in cui l'interfaccia utente verrà visualizzata nel browser.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image1.png)

Quando un utente seleziona il collegamento **Aggiungi nuovo genere** , viene visualizzata una finestra di dialogo popup in cui viene richiesto all'utente un nuovo nome del genere (e, facoltativamente, una descrizione). Nell'immagine seguente viene mostrata la finestra di dialogo **Aggiungi genere** .

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image2.png)

Quando viene immesso un nuovo nome di genere e viene effettuato il push del pulsante **Salva** , si verifica quanto segue:

1. Una chiamata AJAX invia i dati al metodo create del controller di genere, che salva il nuovo genere nel database e restituisce le nuove informazioni sul genere (nome e ID del genere) come JSON.
2. JavaScript aggiunge i nuovi dati di genere all'elenco di selezione.
3. JavaScript rende il nuovo genere l'elemento selezionato.

   Nell'immagine seguente, **opera** è stato aggiunto al database e selezionato nell'elenco a discesa **genere** . 

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image3.png)

Aprire il file *Views\StoreManager\Create.cshtml* e sostituire il markup del genere con il codice seguente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample1.cshtml)]

Il `_ChooseGenre` visualizzazione parziale conterrà tutta la logica per collegare JavaScript e jQuery usati per implementare la funzionalità Aggiungi nuovo genere. Una volta completato il codice, sarà semplice eseguire la stessa operazione con l'interfaccia utente dell'artista.

In Esplora soluzioni, fare clic con il pulsante destro del mouse sulla cartella *Views\StoreManager* e selezionare **Aggiungi**, quindi **Visualizza**. Nell'input **nome visualizzazione** immettere `_ChooseGenre` quindi selezionare **Aggiungi**. Sostituire il markup nel file *Views\StoreManager\\_ChooseGenre. cshtml* con il codice seguente:

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample2.cshtml)]

La prima riga dichiara che viene passato un `Album` come modello, esattamente la stessa istruzione del modello trovata nella visualizzazione create. Le righe successive sono il markup dell'helper **Label** . La riga successiva è la chiamata di helper **DropDownList** , esattamente come nella visualizzazione di creazione originale. La riga successiva aggiunge un collegamento con il nome `Add New Genre`e lo stili come un pulsante. La riga contenente `ValidationMessageFor` viene copiata direttamente dalla vista Create. Le righe seguenti:

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample3.html)]

Crea un div nascosto, con l'ID del `genreDialog`. Verrà usato jQuery per collegare la finestra di dialogo **Aggiungi genere** con l'ID `genreDialog` in questo div. Gli ultimi due tag script contengono i collegamenti ai file JavaScript che si utilizzeranno per implementare la funzionalità Aggiungi nuovo genere. Il file */Scripts/chooseGenre.js* viene fornito nel progetto, che verrà esaminato più avanti nell'esercitazione.

Eseguire l'applicazione e fare clic sul pulsante **Aggiungi nuovo genere** . Nella finestra di dialogo **Aggiungi genere** immettere **opera** nella casella **nome** input.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image4.png)

Fare clic su **Salva**. Una chiamata AJAX crea la categoria opera e quindi compila l'elenco a discesa con opera e imposta opera come genere selezionato.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image5.png)

Immettere un artista, un titolo e un prezzo, quindi selezionare il pulsante **Crea** . Se si immette un prezzo inferiore a $8,99, il nuovo album verrà visualizzato nella parte superiore della visualizzazione dell'indice. Verificare che la voce del nuovo album sia stata salvata nel database.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image6.png)

Provare a creare un nuovo genere con una sola lettera. Il codice seguente nel file *Models\Genre.cs* imposta la lunghezza minima e massima del nome del genere.

[!code-csharp[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample4.cs)]

Report di convalida lato client è necessario immettere una stringa di lunghezza compresa tra 2 e 20 caratteri.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image7.png)

### <a name="examining-how-a-new-genre-is-added-to-the-database-and-the-select-list"></a>Esame del modo in cui un nuovo genere viene aggiunto al database e all'elenco di selezione.

Aprire il file *Scripts\chooseGenre.js* ed esaminare il codice.

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample5.js)]

La seconda riga usa l'ID `genreDialog` per creare una finestra di dialogo nel tag div nel file *Views\StoreManager\\_ChooseGenre. cshtml* . La maggior parte dei parametri denominati è intuitiva. Il parametro `autoOpen` è impostato su false. Se si seleziona il pulsante **Crea genere** , il dialogo verrà aperto in modo esplicito (come descritto in questo secondo caso). Nella finestra di dialogo sono presenti due pulsanti, **Save** e **Cancel**. Il pulsante **Annulla** chiude la finestra di dialogo. Nel codice seguente viene illustrata la funzione del pulsante **Salva** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample6.js)]

Il `var createGenreForm` viene selezionato dall'ID del `createGenreForm`. L'ID `createGenreForm` è stato impostato nel codice seguente trovato nel file *Views\Genre\\_CreateGenre. cshtml* .

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample7.cshtml)]

L'overload dell'helper [HTML. BeginForm](https://msdn.microsoft.com/library/dd492714.aspx) usato nel file *Views\Genre\\_CreateGenre. cshtml* genera il codice HTML con un attributo di azione contenente l'URL per l'invio del modulo. Per visualizzarlo, è possibile visualizzare la pagina Crea album in un browser e selezionare Mostra origine nel browser. Il markup seguente illustra il codice HTML generato contenente il tag form.

[!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample8.html)]

La riga di `$.post` jQuery esegue una chiamata AJAX all'attributo Action (`/StoreManager/Create`) e passa i dati dalla finestra di dialogo **Crea genere** . I dati sono costituiti dal nome del nuovo genere e da una descrizione facoltativa. Se la chiamata AJAX ha esito positivo, il nuovo nome e il valore del genere vengono aggiunti al markup Select e il nuovo genere viene impostato sul valore selezionato. Poiché si tratta di un markup generato in modo dinamico, non è possibile visualizzare la nuova opzione Select visualizzando l'origine nel browser. È possibile visualizzare il nuovo codice HTML con gli strumenti di sviluppo F12 di IE 9. Per visualizzare la nuova opzione Seleziona, in Internet Explorer 9 Premere il tasto F12 per avviare gli strumenti di sviluppo F12. Passare alla pagina Create (Crea) e aggiungere un nuovo genere, in modo che il nuovo genere sia selezionato nell'elenco di selezione di genere. Negli strumenti di sviluppo F12:

1. Selezionare la scheda HTML.
2. Premere l'icona di aggiornamento.  
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image8.png)
3. Nella casella di ricerca immettere GenreID.
4. Utilizzando l'icona avanti,   
    ![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image9.png)  
   passare al tag SELECT seguente:

    [!code-html[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample9.html)]
5. Espandere l'ultimo valore dell'opzione.

![](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/_static/image10.png)

Il codice seguente nel file *Scripts\chooseGenre.js* Mostra come il pulsante **Aggiungi nuovo genere** viene connesso all'evento click e come viene creata la finestra di dialogo **Aggiungi nuovo tipo** .

[!code-javascript[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample10.js)]

La prima riga crea una funzione click collegata al pulsante **Aggiungi nuovo genere** . Il markup seguente dal file Views\StoreManager\\_ChooseGenre. cshtml Mostra come viene creato il pulsante **Aggiungi nuovo genere** :

[!code-cshtml[Main](adding-a-new-category-to-the-dropdownlist-using-jquery-ui/samples/sample11.cshtml)]

Il metodo Load crea e apre la finestra di dialogo Aggiungi genere e chiama il metodo jQuery `parse` in modo che la convalida del client venga eseguita sui dati immessi nella finestra di dialogo.

In questa sezione si è appreso come creare una finestra di dialogo che può essere usata per aggiungere nuovi dati di categoria a un elenco di selezione. È possibile seguire la stessa procedura per creare un'interfaccia utente per aggiungere un nuovo artista all'elenco di selezione dell'artista. In questa esercitazione è stata fornita una panoramica dell'uso del **DropDownList**helper HTML ASP.NET MVC. Per ulteriori informazioni sull'utilizzo del **DropDownList**, vedere la sezione relativa ai riferimenti di addizione riportata di seguito. Indica se questa esercitazione è stata utile.

Rick.Anderson[at]Microsoft.com

### <a name="additional-references"></a>Altre informazioni di riferimento

- [ASP.NET MVC – elenco a discesa a cascata-esercitazione](https://weblogs.asp.net/raduenuca/archive/2011/03/06/asp-net-mvc-cascading-dropdown-lists-tutorial-part-1-defining-the-problem-and-the-context.aspx) di [Radu Enuca](https://weblogs.asp.net/raduenuca/default.aspx)
- [Scelta](https://harvesthq.github.com/chosen/) Un plug-in JavaScript che supporta la selezione e il filtraggio.

### <a name="contributors"></a>Collaboratori

- [Enuca Radu](https://weblogs.asp.net/raduenuca/default.aspx)
- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)

### <a name="reviewers"></a>Revisori

- Jean-Sébastien Goupil
- [Brad Wilson](http://bradwilson.typepad.com/)
- Mike Pope
- Tom Dykstra

> [!div class="step-by-step"]
> [Precedente](examining-how-aspnet-mvc-scaffolds-the-dropdownlist-helper.md)
