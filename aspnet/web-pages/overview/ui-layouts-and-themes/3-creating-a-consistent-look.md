---
uid: web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
title: Creazione di un layout coerente nei siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Per rendere più efficiente la creazione di pagine Web per il sito, è possibile creare blocchi di contenuto riutilizzabili (ad esempio intestazioni e piè di pagina) per il sito Web e c...
ms.author: riande
ms.date: 03/10/2014
ms.assetid: d7bd001b-6db2-4422-9b78-f3d08b743b00
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/3-creating-a-consistent-look
msc.type: authoredcontent
ms.openlocfilehash: 3f63ce68ae4c13970ac0df196167ace0b22b592c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78627449"
---
# <a name="creating-a-consistent-layout-in-aspnet-web-pages-razor-sites"></a>Creazione di un layout coerente nei siti Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come usare le pagine di layout in un sito Web di Pagine Web ASP.NET (Razor) per creare blocchi di contenuto riutilizzabili, ad esempio intestazioni e piè di pagina, e per creare un aspetto coerente per tutte le pagine del sito.
> 
> **Cosa si apprenderà:** 
> 
> - Come creare blocchi riutilizzabili di contenuto, ad esempio intestazioni e piè di pagina.
> - Come creare un aspetto coerente per tutte le pagine del sito usando un layout.
> - Come passare i dati in fase di esecuzione a una pagina di layout.
> 
> Queste sono le funzionalità di ASP.NET introdotte nell'articolo:
> 
> - I blocchi di contenuto, ovvero file che contengono contenuto in formato HTML da inserire in più pagine.
> - Pagine di layout, ovvero pagine che contengono contenuto in formato HTML che può essere condiviso da pagine del sito Web.
> - I metodi `RenderPage`, `RenderBody`e `RenderSection`, che indicano a ASP.NET dove inserire gli elementi della pagina.
> - Dizionario `PageData` che consente di condividere i dati tra i blocchi di contenuto e le pagine di layout.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

## <a name="about-layout-pages"></a>Informazioni sulle pagine di layout

Molti siti Web hanno contenuto visualizzato in ogni pagina, ad esempio un'intestazione e un piè di pagina, o una casella che indica agli utenti che sono connessi. ASP.NET consente di creare un file separato con un blocco di contenuto che può contenere testo, markup e codice, proprio come una normale pagina Web. È quindi possibile inserire il blocco di contenuto in altre pagine del sito in cui si desidera visualizzare le informazioni. In questo modo non è necessario copiare e incollare lo stesso contenuto in ogni pagina. La creazione di contenuto comune come questo rende anche più semplice l'aggiornamento del sito. Se è necessario modificare il contenuto, è possibile aggiornare solo un singolo file e le modifiche vengono quindi riflesse ovunque sia stato inserito il contenuto.

Il diagramma seguente illustra come funzionano i blocchi di contenuto. Quando un browser richiede una pagina dal server Web, ASP.NET inserisce i blocchi di contenuto nel punto in cui viene chiamato il metodo `RenderPage` nella pagina principale. La pagina terminata (unita) viene quindi inviata al browser.

![Diagramma concettuale che Mostra come il Metodo RenderPage inserisce una pagina a cui viene fatto riferimento nella pagina corrente.](3-creating-a-consistent-look/_static/image1.jpg)

In questa procedura verrà creata una pagina che fa riferimento a due blocchi di contenuto, ovvero un'intestazione e un piè di pagina, che si trovano in file distinti. È possibile usare questi stessi blocchi di contenuto in qualsiasi pagina del sito. Al termine, si otterrà una pagina simile alla seguente:

![Screenshot che mostra una pagina nel browser risultante dall'esecuzione di una pagina che include chiamate al Metodo RenderPage.](3-creating-a-consistent-look/_static/image2.png)

1. Nella cartella radice del sito Web creare un file denominato *index. cshtml*.
2. Sostituire il markup esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample1.html)]
3. Nella cartella radice creare una cartella denominata *Shared*.

    > [!NOTE]
    > È prassi comune archiviare i file condivisi tra le pagine Web in una cartella denominata *Shared*.
4. Nella cartella *condivisa* creare un file denominato *\_header. cshtml*.
5. Sostituire il contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample2.html)]

    Si noti che il nome del file è *\_header. cshtml*, con un carattere di sottolineatura (\_) come prefisso. ASP.NET non invierà una pagina al browser se il nome inizia con un carattere di sottolineatura. In questo modo si impedisce agli utenti di richiedere (inavvertitamente o diversamente) queste pagine direttamente. È consigliabile usare un carattere di sottolineatura per assegnare un nome alle pagine che contengono blocchi di contenuto, perché non si vuole che gli utenti siano in grado di richiedere &#8212; che queste pagine esistano esclusivamente per essere inserite in altre pagine.
6. Nella cartella *condivisa* creare un file denominato *\_footer. cshtml* e sostituire il contenuto con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample3.html)]
7. Nella pagina *index. cshtml* aggiungere due chiamate al metodo `RenderPage`, come illustrato di seguito:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample4.html)]

    Viene illustrato come inserire un blocco di contenuto in una pagina Web. Chiamare il metodo `RenderPage` e passargli il nome del file di cui si vuole inserire il contenuto in quel momento. Qui si inserisce il contenuto dei file *\_header. cshtml* e *\_footer. cshtml* nel file *index. cshtml* .
8. Eseguire la pagina *index. cshtml* in un browser. In WebMatrix, nell'area di lavoro **file** , fare clic con il pulsante destro del mouse sul file e scegliere **Avvia nel browser**.
9. Nel browser visualizzare l'origine della pagina. Ad esempio, in Internet Explorer fare clic con il pulsante destro del mouse sulla pagina, quindi scegliere **Visualizza origine**.

    In questo modo è possibile visualizzare il markup della pagina Web inviato al browser, che combina il markup della pagina di indice con i blocchi di contenuto. Nell'esempio seguente viene illustrata l'origine della pagina di cui viene eseguito il rendering per *index. cshtml*. Le chiamate a `RenderPage` inserite in *index. cshtml* sono state sostituite dal contenuto effettivo dei file di intestazione e piè di pagina.

    [!code-html[Main](3-creating-a-consistent-look/samples/sample5.html)]

## <a name="creating-a-consistent-look-using-layout-pages"></a>Creazione di un aspetto coerente utilizzando le pagine di layout

Finora si è visto che è facile includere lo stesso contenuto su più pagine. Un approccio più strutturato per creare un aspetto coerente per un sito consiste nell'usare le pagine di layout. Una pagina di layout definisce la struttura di una pagina Web, ma non contiene alcun contenuto effettivo. Dopo aver creato una pagina di layout, è possibile creare pagine Web che contengono il contenuto e quindi collegarle alla pagina di layout. Quando queste pagine vengono visualizzate, verranno formattate in base alla pagina di layout. In questo senso, una pagina di layout funge da tipo di modello per il contenuto definito in altre pagine.

La pagina layout è analoga a qualsiasi pagina HTML, ad eccezione del fatto che contiene una chiamata al metodo `RenderBody`. La posizione del metodo `RenderBody` nella pagina di layout determina la posizione in cui verranno incluse le informazioni contenute nella pagina contenuto.

Il diagramma seguente mostra in che modo le pagine di contenuto e le pagine di layout vengono combinate in fase di esecuzione per produrre la pagina Web completata. Il browser richiede una pagina di contenuto. Nella pagina contenuto è presente codice che specifica la pagina di layout da utilizzare per la struttura della pagina. Nella pagina layout il contenuto viene inserito nel punto in cui viene chiamato il metodo `RenderBody`. I blocchi di contenuto possono anche essere inseriti nella pagina di layout chiamando il metodo `RenderPage`, come nella sezione precedente. Quando la pagina Web è completa, viene inviata al browser.

![Screenshot che mostra una pagina nel browser risultante dall'esecuzione di una pagina che include chiamate al metodo RenderBody.](3-creating-a-consistent-look/_static/image3.jpg)

La procedura seguente illustra come creare una pagina di layout e collegarvi le pagine di contenuto.

1. Nella cartella *condivisa* del sito Web creare un file denominato *\_Layout1. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample6.html)]

    Usare il metodo `RenderPage` in una pagina di layout per inserire i blocchi di contenuto. Una pagina di layout può contenere una sola chiamata al metodo `RenderBody`.
3. Nella cartella *condivisa* creare un file denominato *\_header2. cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample7.html)]
4. Nella cartella radice creare una nuova cartella e denominarla *stili*.
5. Nella cartella *stili* creare un file denominato *site. CSS* e aggiungere le seguenti definizioni di stile:

    [!code-css[Main](3-creating-a-consistent-look/samples/sample8.css)]

    Queste definizioni di stile sono disponibili solo per mostrare come i fogli di stile possono essere usati con le pagine di layout. Se lo si desidera, è possibile definire stili personalizzati per questi elementi.
6. Nella cartella radice creare un file denominato *Content1. cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample9.cshtml)]

    Si tratta di una pagina che utilizzerà una pagina di layout. Il blocco di codice nella parte superiore della pagina indica la pagina di layout da usare per formattare il contenuto.
7. Eseguire *Content1. cshtml* in un browser. La pagina di cui è stato eseguito il rendering usa il formato e il foglio di stile definito in *\_Layout1. cshtml* e il testo (contenuto) definito in *Content1. cshtml*.

    ![[immagine]](3-creating-a-consistent-look/_static/image4.png)

    È possibile ripetere il passaggio 6 per creare pagine di contenuto aggiuntive che possono condividere la stessa pagina di layout.

    > [!NOTE]
    > È possibile configurare il sito in modo da poter utilizzare automaticamente la stessa pagina di layout per tutte le pagine di contenuto di una cartella. Per informazioni dettagliate, vedere [personalizzazione del comportamento a livello di sito per pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906).

## <a name="designing-layout-pages-that-have-multiple-content-sections"></a>Progettazione di pagine di layout con più sezioni di contenuto

Una pagina contenuto può avere più sezioni, che risulta utile se si desidera utilizzare layout con più aree con contenuto sostituibile. Nella pagina contenuto assegnare a ogni sezione un nome univoco. La sezione predefinita viene lasciata senza nome. Nella pagina layout aggiungere un metodo di `RenderBody` per specificare dove deve essere visualizzata la sezione senza nome (impostazione predefinita). Si aggiungono quindi metodi di `RenderSection` distinti per eseguire singolarmente il rendering di sezioni denominate.

Il diagramma seguente illustra il modo in cui ASP.NET gestisce il contenuto suddiviso in più sezioni. Ogni sezione denominata è contenuta in un blocco di sezione nella pagina contenuto. Sono denominati `Header` e `List` nell'esempio. Il framework inserisce la sezione Content nella pagina di layout nel punto in cui viene chiamato il metodo `RenderSection`. La sezione senza nome (impostazione predefinita) viene inserita nel punto in cui viene chiamato il metodo `RenderBody`, come illustrato in precedenza.

![Diagramma concettuale che mostra il modo in cui il Metodo RenderSection inserisce i riferimenti alle sezioni nella pagina corrente.](3-creating-a-consistent-look/_static/image5.jpg)

Questa procedura illustra come creare una pagina di contenuto con più sezioni di contenuto e come eseguirne il rendering usando una pagina di layout che supporta più sezioni di contenuto.

1. Nella cartella *condivisa* creare un file denominato *\_Layout2. cshtml*.
2. Sostituire il contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample10.html)]

    Usare il metodo `RenderSection` per eseguire il rendering delle sezioni di intestazione e di elenco.
3. Nella cartella radice creare un file denominato *Content2. cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample11.cshtml)]

    Questa pagina contenuto contiene un blocco di codice nella parte superiore della pagina. Ogni sezione denominata è contenuta in un blocco di sezione. Il resto della pagina contiene la sezione del contenuto predefinita (senza nome).
4. Eseguire *Content2. cshtml* in un browser.

    ![Screenshot che mostra una pagina nel browser risultante dall'esecuzione di una pagina che include chiamate al Metodo RenderSection.](3-creating-a-consistent-look/_static/image6.png)

## <a name="making-content-sections-optional"></a>Creazione di sezioni di contenuto facoltative

In genere, le sezioni create in una pagina di contenuto devono corrispondere alle sezioni definite nella pagina di layout. È possibile ottenere errori se si verifica una delle condizioni seguenti:

- La pagina contenuto contiene una sezione senza sezione corrispondente nella pagina layout.
- La pagina layout contiene una sezione per la quale non sono presenti contenuti.
- Nella pagina layout sono incluse chiamate al metodo che tentano di eseguire il rendering della stessa sezione più di una volta.

Tuttavia, è possibile eseguire l'override di questo comportamento per una sezione denominata dichiarando la sezione come facoltativa nella pagina di layout. In questo modo è possibile definire più pagine di contenuto che possono condividere una pagina di layout ma che potrebbero o meno contenere contenuto per una sezione specifica.

1. Aprire *Content2. cshtml* e rimuovere la sezione seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample12.cshtml)]
2. Salvare la pagina e quindi eseguirla in un browser. Viene visualizzato un messaggio di errore, perché la pagina contenuto non fornisce contenuto per una sezione definita nella pagina di layout, ovvero la sezione dell'intestazione.

    ![Screenshot che mostra l'errore che si verifica se si esegue una pagina che chiama il Metodo RenderSection, ma non è stata specificata la sezione corrispondente.](3-creating-a-consistent-look/_static/image7.png)
3. Nella cartella *condivisa* aprire la pagina *\_Layout2. cshtml* e sostituire questa riga:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample13.js)]

    con il codice seguente:

    [!code-javascript[Main](3-creating-a-consistent-look/samples/sample14.js)]

    In alternativa, è possibile sostituire la riga di codice precedente con il blocco di codice seguente, che produce gli stessi risultati:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample15.cshtml)]
4. Eseguire di nuovo la pagina *Content2. cshtml* in un browser. Se questa pagina è ancora aperta nel browser, è possibile aggiornarla semplicemente. Questa volta la pagina viene visualizzata senza errori, anche se non è presente alcuna intestazione.

## <a name="passing-data-to-layout-pages"></a>Passaggio di dati a pagine di layout

È possibile che siano presenti dati definiti nella pagina contenuto a cui è necessario fare riferimento in una pagina di layout. In tal caso, è necessario passare i dati dalla pagina contenuto alla pagina layout. Ad esempio, potrebbe essere necessario visualizzare lo stato di accesso di un utente oppure è possibile mostrare o nascondere le aree di contenuto in base all'input dell'utente.

Per passare dati da una pagina di contenuto a una pagina di layout, è possibile inserire i valori nella proprietà `PageData` della pagina contenuto. La proprietà `PageData` è una raccolta di coppie nome/valore che contengono i dati che si desidera passare tra le pagine. Nella pagina layout è quindi possibile leggere i valori fuori dalla proprietà `PageData`.

Ecco un altro diagramma. Questo Mostra come ASP.NET può usare la proprietà `PageData` per passare i valori da una pagina di contenuto alla pagina di layout. Quando ASP.NET inizia a creare la pagina Web, crea la raccolta di `PageData`. Nella pagina contenuto è possibile scrivere il codice per inserire i dati nella raccolta `PageData`. È possibile accedere ai valori della raccolta `PageData` anche da altre sezioni della pagina contenuto o da blocchi di contenuto aggiuntivi.

![Diagramma concettuale che Mostra come una pagina di contenuto può popolare un dizionario PageData e passare tali informazioni alla pagina di layout.](3-creating-a-consistent-look/_static/image8.jpg)

Nella procedura seguente viene illustrato come passare i dati da una pagina di contenuto a una pagina di layout. Quando la pagina viene eseguita, viene visualizzato un pulsante che consente all'utente di nascondere o visualizzare un elenco definito nella pagina di layout. Quando gli utenti fanno clic sul pulsante, viene impostato un valore true/false (booleano) nella proprietà `PageData`. La pagina layout legge tale valore e, se è false, nasconde l'elenco. Il valore viene usato anche nella pagina contenuto per determinare se visualizzare il pulsante **Nascondi elenco** o il pulsante **Mostra elenco** .

![[immagine]](3-creating-a-consistent-look/_static/image9.jpg)

1. Nella cartella radice creare un file denominato *Content3. cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample16.cshtml)]

    Il codice archivia due tipi di dati nella proprietà &#8212; `PageData` il titolo della pagina Web e true o false per specificare se visualizzare un elenco.

    Si noti che ASP.NET consente di inserire il markup HTML nella pagina in modo condizionale usando un blocco di codice. Ad esempio, il blocco `if/else` nel corpo della pagina determina quale modulo visualizzare a seconda che `PageData["ShowList"]` sia impostato su true.
2. Nella cartella *condivisa* creare un file denominato *\_layout3. cshtml* e sostituire qualsiasi contenuto esistente con il codice seguente:

    [!code-cshtml[Main](3-creating-a-consistent-look/samples/sample17.cshtml)]

    Nella pagina layout è inclusa un'espressione nell'elemento `<title>` che ottiene il valore del titolo dalla proprietà `PageData`. USA anche il valore `ShowList` della proprietà `PageData` per determinare se visualizzare il blocco di contenuto dell'elenco.
3. Nella cartella *condivisa* creare un file denominato *\_List. cshtml* e sostituire il contenuto esistente con il codice seguente:

    [!code-html[Main](3-creating-a-consistent-look/samples/sample18.html)]
4. Eseguire la pagina *Content3. cshtml* in un browser. La pagina viene visualizzata con l'elenco visibile sul lato sinistro della pagina e un pulsante **Nascondi elenco** in basso.

    ![Screenshot che mostra la pagina che include l'elenco e un pulsante con il testo "Hide list".](3-creating-a-consistent-look/_static/image10.png)
5. Fare clic su **Nascondi elenco**. L'elenco scompare e il pulsante cambia in **Mostra elenco**.

    ![Screenshot che mostra la pagina che non include l'elenco e un pulsante con il testo "show list".](3-creating-a-consistent-look/_static/image11.png)
6. Fare clic sul pulsante **Mostra elenco** per visualizzare di nuovo l'elenco.

## <a name="additional-resources"></a>Risorse aggiuntive

[Personalizzazione del comportamento a livello di sito per Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
