---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
title: Ricerca per categorie usare il controllo ComboBox? (VB) | Microsoft Docs
author: microsoft
description: ComboBox è un controllo AJAX ASP.NET che combina la flessibilità di una casella di testo con un elenco di opzioni da cui gli utenti possono scegliere.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: e887e7b2-a6e7-4a28-a134-ba334494badb
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 468063a72253cce55a02bfaef1219bff03d06418
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78554488"
---
# <a name="how-do-i-use-the-combobox-control-vb"></a>Ricerca per categorie usare il controllo ComboBox? (VB)

[Microsoft](https://github.com/microsoft)

> ComboBox è un controllo AJAX ASP.NET che combina la flessibilità di una casella di testo con un elenco di opzioni da cui gli utenti possono scegliere.

L'obiettivo di questa esercitazione è spiegare il controllo ComboBox di AJAX Control Toolkit. ComboBox funziona come una combinazione tra un controllo DropDownList ASP.NET standard e un controllo TextBox. È possibile effettuare una selezione da un elenco preesistente di elementi o immettere un nuovo elemento.

ComboBox è simile all'Extender del controllo di completamento automatico, ma i controlli vengono usati in scenari diversi. Il dispositivo Extender AutoComplete esegue una query su un servizio Web per ottenere le voci corrispondenti. Il controllo ComboBox, al contrario, viene inizializzato con un set di elementi. L'uso del dispositivo Extender AutoComplete è utile quando si lavora con un set di dati di grandi dimensioni (milioni di parti dell'auto) quando si usa il controllo ComboBox, quando si lavora con un piccolo set di dati (dozzine di parti dell'auto).

## <a name="selecting-from-a-static-list-of-items"></a>Selezione da un elenco statico di elementi

Inizia con un semplice esempio di utilizzo del controllo ComboBox. Si supponga di voler visualizzare un elenco statico di elementi in un elenco a discesa. Tuttavia, si desidera lasciare aperto il rischio che l'elenco non sia completo. Si desidera consentire a un utente di immettere un valore personalizzato nell'elenco.

Viene creata una nuova pagina Web Form ASP.NET e viene utilizzato il controllo ComboBox nella pagina. Aggiungere la nuova pagina ASP.NET al progetto e passare alla visualizzazione progettazione.

Se si desidera utilizzare il controllo ComboBox nella pagina, è necessario aggiungere un controllo ScriptManager alla pagina. Trascinare il controllo ScriptManager da sotto la scheda Estensioni AJAX nell'area di progettazione. È necessario aggiungere il controllo ScriptManager nella parte superiore della pagina; è possibile aggiungerlo immediatamente sotto il modulo di apertura &lt;lato server&gt; tag.

Trascinare quindi il controllo ComboBox nella pagina. È possibile trovare il controllo ComboBox nella casella degli strumenti con gli altri controlli e Extender di controllo AJAX Control Toolkit (vedere Figura 1).

[![forma semplice per la creazione di un biglietto da lavoro](how-do-i-use-the-combobox-control-vb/_static/image1.jpg)](how-do-i-use-the-combobox-control-vb/_static/image1.png)

**Figura 01**: selezione del controllo ComboBox dalla casella degli strumenti ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image2.png))

Si userà il controllo ComboBox per visualizzare un elenco statico di opzioni. L'utente può selezionare un particolare livello di piccante per il proprio cibo da un elenco di tre opzioni: lieve, media e attiva (vedere la figura 2).

[![la selezione da un elenco statico di elementi](how-do-i-use-the-combobox-control-vb/_static/image2.jpg)](how-do-i-use-the-combobox-control-vb/_static/image3.png)

**Figura 02**: selezione da un elenco statico di elementi ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image4.png))

È possibile aggiungere queste opzioni al controllo ComboBox in due modi. In primo luogo, si seleziona l'opzione modifica opzioni attività quando si posiziona il puntatore del mouse sul controllo nella visualizzazione progettazione e si apre l'editor di elementi (vedere la figura 3).

[![modifica elementi ComboBox](how-do-i-use-the-combobox-control-vb/_static/image3.jpg)](how-do-i-use-the-combobox-control-vb/_static/image5.png)

**Figura 03**: modifica di elementi ComboBox ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image6.png))

La seconda opzione consiste nell'aggiungere l'elenco di elementi tra i tag di apertura e chiusura &lt;ASP: ComboBox&gt; nella visualizzazione origine. La pagina nel listato 1 contiene la casella combinata aggiornata che include l'elenco di elementi.

**Listato 1-static. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample1.aspx)]

Quando si apre la pagina nel listato 1, è possibile selezionare una delle opzioni preesistenti dalla casella combinata. In altre parole, la ComboBox funziona esattamente come un controllo DropDownList.

Tuttavia, è anche possibile immettere una nuova scelta, ad esempio Super piccante, che non è presente nell'elenco esistente. Quindi, la ComboBox funziona anche come un controllo TextBox.

Indipendentemente dal fatto che si scelga un elemento preesistente o si immetta un elemento personalizzato, quando si invia il modulo, la scelta verrà visualizzata nel controllo etichetta. Quando si invia il modulo, il btnSubmit\_fare clic su gestore esegue e aggiorna l'etichetta (vedere la figura 4).

[![la visualizzazione dell'elemento selezionato](how-do-i-use-the-combobox-control-vb/_static/image4.jpg)](how-do-i-use-the-combobox-control-vb/_static/image7.png)

**Figura 04**: visualizzazione dell'elemento selezionato ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image8.png))

ComboBox supporta le stesse proprietà del controllo DropDownList per il recupero dell'elemento selezionato dopo l'invio di un modulo:

- SelectedItem. Text-Visualizza il valore della proprietà Text dell'elemento selezionato.
- SelectedItem. Value: Visualizza il valore della proprietà Value dell'elemento selezionato oppure Visualizza il testo digitato nella casella combinata.
- SelectedValue: uguale a SelectedItem. Value con la differenza che questa proprietà consente di specificare l'elemento selezionato predefinito (iniziale).

Se si digita una scelta personalizzata nella casella combinata, la scelta personalizzata viene assegnata alle proprietà SelectedItem. Text e SelectedItem. Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Selezione dell'elenco di elementi dal database

È possibile recuperare l'elenco di elementi visualizzati dalla casella combinata da un database. Ad esempio, è possibile associare ComboBox a un controllo SqlDataSource, un controllo ObjectDataSource, un LinqDataSource o un oggetto EntityDataSource.

Si supponga di voler visualizzare un elenco di filmati in una casella combinata. Si desidera recuperare l'elenco dei film dalla tabella del database Movies. Attenersi ai passaggi riportati di seguito.

1. Creare una pagina denominata Movies. aspx
2. Aggiungere un controllo ScriptManager alla pagina trascinando ScriptManager dalla scheda Estensioni AJAX della casella degli strumenti nella pagina.
3. Aggiungere un controllo ComboBox alla pagina trascinando la casella combinata nella pagina.
4. Nella visualizzazione della struttura posizionare il puntatore del mouse sul controllo ComboBox e selezionare l'opzione per l'attività **Scegli origine dati** (vedere la figura 5). Viene avviata la configurazione guidata origine dati.
5. Nel passaggio **Scegli un'origine dati** selezionare l'opzione &lt;nuova origine dati&gt;.
6. Nel passaggio **scegliere un tipo di origine dati** selezionare database.
7. Nel passaggio **scegliere la connessione dati** selezionare il database, ad esempio MoviesDB. MDF.
8. Nel passaggio **Salva la stringa di connessione nel file di configurazione dell'applicazione** selezionare l'opzione per salvare la stringa di connessione.
9. Nel passaggio **Configura istruzione SELECT** selezionare la tabella del database Movies e selezionare tutte le colonne.
10. Nel passaggio **Test query** fare clic sul pulsante fine.
11. Tornare al passaggio **Scegli origine dati** , selezionare la colonna titolo per il campo da visualizzare e la colonna ID per il campo dati (vedere la figura).
12. Fare clic sul pulsante OK per chiudere la procedura guidata.

[![la scelta di un'origine dati](how-do-i-use-the-combobox-control-vb/_static/image5.jpg)](how-do-i-use-the-combobox-control-vb/_static/image9.png)

**Figura 05**: scelta di un'origine dati ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image10.png))

[![la scelta dei campi del valore e del testo dei dati](how-do-i-use-the-combobox-control-vb/_static/image6.jpg)](how-do-i-use-the-combobox-control-vb/_static/image11.png)

**Figura 06**: scelta dei campi del valore e del testo dei dati ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image12.png))

Dopo aver completato i passaggi precedenti, la ComboBox viene associata a un controllo SqlDataSource che rappresenta i film della tabella del database Movies. L'origine della pagina è simile all'elenco 2 (la formattazione è stata rimossa un po').

**Listato 2-Movies. aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-vb/samples/sample2.aspx)]

Si noti che il controllo ComboBox ha una proprietà DataSourceID che punta al controllo SqlDataSource. Quando si apre la pagina in un browser, viene visualizzato l'elenco di filmati dal database (vedere la figura 7). È possibile selezionare un film dall'elenco o immettere un nuovo film digitando il filmato nella casella combinata.

[![visualizzazione di un elenco di filmati](how-do-i-use-the-combobox-control-vb/_static/image7.jpg)](how-do-i-use-the-combobox-control-vb/_static/image13.png)

**Figura 07**: visualizzazione di un elenco di filmati ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image14.png))

## <a name="setting-the-dropdownstyle"></a>Impostazione di DropDownStyle

È possibile utilizzare la proprietà ComboBox DropDownStyle per modificare il comportamento della casella combinata. Questa proprietà accetta i valori possibili:

- Elenco a discesa: (valore predefinito) la casella combinata Visualizza un elenco a discesa quando si fa clic sulla freccia ed è possibile immettere un valore personalizzato.
- Semplice: la casella combinata Visualizza automaticamente un elenco a discesa ed è possibile immettere un valore personalizzato.
- DropDownList: la casella combinata funziona esattamente come un controllo DropDownList.

Il diverso elenco a discesa tra e semplice è quando viene visualizzato l'elenco degli elementi. Nel caso di Simple, l'elenco viene visualizzato immediatamente quando si sposta lo stato attivo sulla casella combinata. Nel caso dell'elenco a discesa, è necessario fare clic sulla freccia per visualizzare l'elenco di elementi.

Il valore DropDownList fa sì che il controllo ComboBox funzioni esattamente come un controllo DropDownList standard. Tuttavia, c'è una differenza importante. Nelle versioni precedenti di Internet Explorer viene visualizzato un controllo DropDownList con un indice z infinito, in modo che il controllo venga visualizzato davanti a qualsiasi controllo inserito davanti a esso. Poiché ComboBox esegue il rendering di un tag HTML &lt;div&gt; invece di un HTML &lt;selezionare&gt; tag, la ComboBox rispetta correttamente l'ordinamento z.

## <a name="setting-the-autocompletemode"></a>Impostazione di AutoCompleteMode

Usare la proprietà ComboBox AutoCompleteMode per specificare cosa accade quando un utente digita il testo nella casella combinata. Questa proprietà accetta i valori possibili seguenti:

- None: (valore predefinito) ComboBox non fornisce alcun comportamento di completamento automatico.
- Suggerisci: la casella combinata Visualizza l'elenco ed evidenzia l'elemento corrispondente nell'elenco (vedere la figura 8).
- Append: ComboBox non Visualizza l'elenco e aggiunge l'elemento corrispondente dall'elenco a quello digitato (vedere la figura 9).
- SuggestAppend-ComboBox Visualizza l'elenco e aggiunge l'elemento corrispondente dall'elenco a quello digitato (vedere la figura 10).

[![ComboBox emette un suggerimento](how-do-i-use-the-combobox-control-vb/_static/image8.jpg)](how-do-i-use-the-combobox-control-vb/_static/image15.png)

**Figura 08**: ComboBox esegue un suggerimento ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image16.png))

[ComboBox ![aggiunge il testo corrispondente](how-do-i-use-the-combobox-control-vb/_static/image9.jpg)](how-do-i-use-the-combobox-control-vb/_static/image17.png)

**Figura 09**: ComboBox aggiunge il testo corrispondente ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image18.png))

[![ComboBox suggerisce e aggiunge](how-do-i-use-the-combobox-control-vb/_static/image10.jpg)](how-do-i-use-the-combobox-control-vb/_static/image19.png)

**Figura 10**: la casella combinata suggerisce e accoda ([fare clic per visualizzare l'immagine con dimensioni complete](how-do-i-use-the-combobox-control-vb/_static/image20.png))

## <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come usare il controllo ComboBox per visualizzare un set fisso di elementi. Il controllo ComboBox è stato associato a un set statico di elementi e a una tabella di database. Infine, si è appreso come modificare il comportamento della casella combinata impostando le relative proprietà DropDownStyle e AutoCompleteMode.

> [!div class="step-by-step"]
> [Precedente](how-do-i-use-the-combobox-control-cs.md)
