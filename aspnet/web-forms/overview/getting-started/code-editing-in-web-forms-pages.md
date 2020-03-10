---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: Modifica del codice ASP.NET Web Forms in Visual Studio 2013 | Microsoft Docs
author: Erikre
description: ''
ms.author: riande
ms.date: 03/03/2014
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 3473ad476fbbebc58e12586334b4600f57cf17ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632748"
---
# <a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Modifica del codice di Web Forms ASP.NET in Visual Studio 2013

di [Erik Reitan](https://github.com/Erikre)

In molte pagine Web Form ASP.NET è possibile scrivere codice in Visual Basic, C#o in un altro linguaggio. L'editor di codice in Visual Studio consente di scrivere codice rapidamente e di evitare errori. Inoltre, l'editor fornisce modi per creare codice riutilizzabile per ridurre la quantità di lavoro che è necessario eseguire.

Questa procedura dettagliata illustra le varie funzionalità dell'editor di codice di Visual Studio.

Durante questa procedura dettagliata, si apprenderà come:

- Correggere gli errori di codifica inline.
- Effettuare il refactoring e rinominare il codice.
- Rinominare variabili e oggetti.
- Inserire frammenti di codice.

## <a name="prerequisites"></a>Prerequisiti

Per completare questa procedura dettagliata, è necessario:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 per il Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). Il .NET Framework viene installato automaticamente. 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 e Microsoft Visual Studio Express 2013 per il Web verranno spesso denominati Visual Studio in questa serie di esercitazioni.  
    >   
    > Se si usa Visual Studio, in questa procedura dettagliata si presuppone che sia stata selezionata la raccolta di impostazioni di **sviluppo Web** per la prima volta che si avvia Visual Studio. Per altre informazioni, vedere [procedura: selezionare le impostazioni dell'ambiente di sviluppo Web](https://msdn.microsoft.com/library/ff521558.aspx).

## <a name="creating-a-web-application-project-and-a-page"></a>Creazione di un progetto di applicazione Web e di una pagina

<a id="sectionToggle0"></a>

In questa parte della procedura dettagliata si creerà un progetto di applicazione Web e si aggiungerà una nuova pagina.

### <a name="to-create-a-web-application-project"></a>Per creare un progetto di applicazione Web

1. Aprire Microsoft Visual Studio.
2. Nel menu **File** selezionare **Nuovo progetto**.  
    Menu file ![](code-editing-in-web-forms-pages/_static/image1.png)

    Verrà visualizzata la finestra di dialogo **Nuovo progetto**.
3. Selezionare i **modelli** -&gt; **Visual C#**  -&gt; gruppo di modelli **Web** a sinistra.
4. Scegliere il modello **Applicazione Web ASP.NET** nella colonna centrale.
5. Assegnare al progetto il nome ***BasicWebApp*** e fare clic sul pulsante **OK** .   
![Finestra di dialogo Nuovo progetto](code-editing-in-web-forms-pages/_static/image2.png)
6. Selezionare quindi il modello **Web Form** e fare clic sul pulsante **OK** per creare il progetto.  
![Finestra di dialogo Nuovo progetto ASP.NET](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio crea un nuovo progetto che include funzionalità predefinite basate sul modello Web Form.

## <a name="creating-a-new-aspnet-web-forms-page"></a>Creazione di una nuova pagina Web Form ASP.NET

Quando si crea una nuova applicazione Web form usando il modello di progetto **applicazione web ASP.NET** , Visual Studio aggiunge una pagina ASP.NET (pagina Web Form) denominata *default. aspx*, oltre a diversi altri file e cartelle. È possibile utilizzare la pagina *default. aspx* come Home page per l'applicazione Web. Tuttavia, per questa procedura dettagliata, verrà creata e utilizzata una nuova pagina.

### <a name="to-add-a-page-to-the-web-application"></a>Per aggiungere una pagina all'applicazione Web

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul nome dell'applicazione Web (in questa esercitazione il nome dell'applicazione è **BasicWebSite**), quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **gruppo C# Visual** -&gt; modelli **Web** a sinistra. Quindi, selezionare **Web Form** nell'elenco centrale e denominarlo *FirstWebPage. aspx*.   
    ![Finestra di dialogo Aggiungi nuovo elemento](code-editing-in-web-forms-pages/_static/image4.png)
3. Fare clic su **Aggiungi** per aggiungere la pagina Web Form al progetto.  
 Visual Studio crea la nuova pagina e la apre.
4. Impostare quindi questa nuova pagina come pagina iniziale predefinita. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla nuova pagina denominata *FirstWebPage. aspx* e selezionare **Imposta come pagina iniziale**. Alla successiva esecuzione dell'applicazione per testare lo stato di avanzamento, la nuova pagina verrà visualizzata automaticamente nel browser.

## <a name="correcting-inline-coding-errors"></a>Correzione degli errori di codifica inline

L'editor di codice in Visual Studio consente di evitare errori durante la scrittura del codice e, in caso di errore, l'editor di codice consente di correggere l'errore. In questa parte della procedura dettagliata verrà scritta una riga di codice che illustra le funzionalità di correzione degli errori dell'editor.

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Per correggere gli errori di codifica semplici in Visual Studio

1. In visualizzazione **progettazione** fare doppio clic sulla pagina vuota per creare un gestore per l'evento di **caricamento** per la pagina.   
   Si usa il gestore eventi solo come posizione per scrivere codice.
2. All'interno del gestore digitare la riga seguente che contiene un errore e premere **invio**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   Quando si preme **invio**, l'editor del codice posiziona le sottolineature verdi e rosse (in genere chiama &quot;linee&quot; ondulate) in aree del codice che presentano problemi. Una sottolineatura verde indica un avviso. Una sottolineatura rossa indica un errore che è necessario correggere. 

    Mantenere il puntatore del mouse su `myStr` per visualizzare una descrizione comando che indica l'avviso. Inoltre, mantenere il puntatore del mouse sulla sottolineatura rossa per visualizzare il messaggio di errore.

    Nell'immagine seguente viene illustrato il codice con le sottolineature.

    ![Testo di benvenuto nella visualizzazione progettazione](code-editing-in-web-forms-pages/_static/image5.png "Testo di benvenuto nella visualizzazione Progettazione")  
   Per correggere l'errore, è necessario aggiungere un punto e virgola `;` alla fine della riga. L'avviso informa semplicemente che non è ancora stata usata la variabile `myStr`.  

    > [!NOTE] 
    > 
    > Per visualizzare le impostazioni di formattazione del codice correnti in Visual Studio, selezionare **strumenti** -**opzioni** &gt; -&gt; **tipi di carattere e colori**.

## <a name="refactoring-and-renaming"></a>Refactoring e ridenominazione

Il refactoring è una metodologia software che prevede la ristrutturazione del codice per semplificare la comprensione e la manutenzione, conservando al tempo stesso le funzionalità. Un semplice esempio potrebbe essere la scrittura di codice in un gestore eventi per ottenere dati da un database. Durante lo sviluppo della pagina, si scopre che è necessario accedere ai dati di diversi gestori. Pertanto, è necessario effettuare il refactoring del codice della pagina creando un metodo di accesso ai dati nella pagina e inserendo le chiamate al metodo nei gestori.

Nell'editor di codice sono inclusi strumenti che consentono di eseguire diverse attività di refactoring. In questa procedura dettagliata si utilizzeranno due tecniche di refactoring: ridenominazione di variabili ed estrazione di metodi. Altre opzioni di refactoring includono l'incapsulamento di campi, la promozione di variabili locali a parametri di metodo e la gestione dei parametri del metodo. La disponibilità di queste opzioni di refactoring dipende dalla posizione nel codice.

### <a name="refactoring-code"></a>Refactoring del codice

Uno scenario di refactoring comune consiste nel creare (estrarre) un metodo dal codice che si trova all'interno di un altro membro, ad esempio un metodo. In questo modo si riduce la dimensione del membro originale e si rende il codice Estratto riutilizzabile.

In questa parte della procedura dettagliata verrà scritto un codice semplice e quindi verrà estratto un metodo. Il refactoring è supportato C#per, quindi verrà creata una pagina che utilizza C# come linguaggio di programmazione.

### <a name="to-extract-a-method-in-a-c-page"></a>Per estrarre un metodo in una C# pagina

1. Passa alla visualizzazione **progettazione** .
2. Dalla scheda **standard** della **casella degli strumenti**trascinare un controllo [Button](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx) nella pagina.
3. Fare doppio clic sul controllo **Button** per creare un gestore per l'evento [Click](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx) , quindi aggiungere il codice evidenziato seguente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   Il codice crea un oggetto **ArrayList** , usa un ciclo per caricarlo con valori, quindi usa un altro ciclo per visualizzare il contenuto dell'oggetto **ArrayList** .
4. Premere **CTRL + F5** per eseguire la pagina e quindi fare clic sul **pulsante** per verificare che venga visualizzato il seguente output:   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. Tornare all'editor di codice e quindi selezionare le righe seguenti nel gestore eventi.   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. Fare clic con il pulsante destro del mouse sulla selezione, scegliere **refactoring**, quindi **Estrai metodo**. 

    Verrà visualizzata la finestra di dialogo **Estrai metodo** .
7. Nella casella **nuovo nome metodo** digitare **DisplayArray**e quindi fare clic su **OK**. 

    L'editor di codice crea un nuovo metodo denominato `DisplayArray`e inserisce una chiamata al nuovo metodo nel gestore di **clic** in cui il ciclo era originariamente.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. Premere **CTRL + F5** per eseguire di nuovo la pagina e fare clic sul **pulsante**.

    La pagina funziona come prima. Il metodo `DisplayArray` ora può essere chiamato da qualsiasi punto della classe Page.

## <a name="renaming-variables"></a>Ridenominazione di variabili

Quando si utilizzano le variabili, nonché gli oggetti, è possibile rinominarli dopo che è già stato fatto riferimento nel codice. Tuttavia, la ridenominazione di variabili e oggetti può causare interruzioni del codice in caso di mancata ridenominazione di uno dei riferimenti. Pertanto, è possibile utilizzare il refactoring per eseguire la ridenominazione.

### <a name="to-use-refactoring-to-rename-a-variable"></a>Per usare il refactoring per rinominare una variabile

1. Nel gestore dell'evento **Click** individuare la riga seguente:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. Fare clic con il pulsante destro del mouse sul nome della variabile `alist`, scegliere **refactoring**, quindi scegliere **Rinomina**.

    Verrà visualizzata la finestra di dialogo **Rinomina** .
3. Nella casella **nuovo nome** digitare **ArrayList1** e verificare che la casella di controllo **Anteprima modifiche riferimenti** sia stata selezionata. Fare quindi clic su **OK**.

    Viene visualizzata la finestra di dialogo **Anteprima modifiche** e viene visualizzato un albero che contiene tutti i riferimenti alla variabile che si sta rinominando.
4. Fare clic su **applica** per chiudere la finestra di dialogo **Anteprima modifiche** .

    Le variabili che fanno riferimento in modo specifico all'istanza selezionata vengono rinominate. Si noti, tuttavia, che la variabile `alist` nella riga seguente non è stata rinominata.

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    La variabile `alist` in questa riga non è stata rinominata perché non rappresenta lo stesso valore della variabile `alist` rinominata. La variabile `alist` nella dichiarazione `DisplayArray` è una variabile locale per il metodo. In questo modo viene illustrato che l'utilizzo del refactoring per rinominare le variabili è diverso rispetto all'esecuzione di un'azione trova e Sostituisci nell'editor. il refactoring Rinomina le variabili con una conoscenza della semantica della variabile utilizzata.

## <a name="inserting-snippets"></a>Inserimento di frammenti

Poiché sono presenti molte attività di codifica che gli sviluppatori di Web Form hanno spesso la necessità di eseguire, l'editor di codice fornisce una raccolta di frammenti di codice o blocchi di codice prescritto. È possibile inserire i frammenti di codice nella pagina.

Ogni lingua utilizzata in Visual Studio presenta piccole differenze nel modo in cui si inseriscono i frammenti di codice. Per informazioni sull'inserimento di frammenti di codice, vedere [Visual Basic frammenti di codice IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx). Per informazioni sull'inserimento di frammenti di codice in C#visuale, vedere [frammenti di codice visivi C# ](https://msdn.microsoft.com/library/z41h7fat.aspx).

## <a name="next-steps"></a>Passaggi successivi

In questa procedura dettagliata sono state illustrate le funzionalità di base dell'editor di codice di Visual Studio 2010 per correggere gli errori nel codice, il refactoring del codice, la ridenominazione delle variabili e l'inserimento di frammenti di codice nel codice. Funzionalità aggiuntive nell'editor possono semplificare e velocizzare lo sviluppo di applicazioni. Può, ad esempio, essere necessario:

- Ulteriori informazioni sulle funzionalità di IntelliSense, ad esempio la modifica delle opzioni IntelliSense, la gestione dei frammenti di codice e la ricerca di frammenti di codice online. Per altre informazioni, vedere [Using IntelliSense](https://msdn.microsoft.com/library/hcw1s69b.aspx).
- Informazioni su come creare frammenti di codice personalizzati. Per altre informazioni, vedere [creazione e uso dei frammenti di codice IntelliSense](https://msdn.microsoft.com/library/ms165392.aspx)
- Altre informazioni sulle funzionalità specifiche di Visual Basic dei frammenti di codice IntelliSense, ad esempio la personalizzazione dei frammenti di codice e la risoluzione dei problemi. Per ulteriori informazioni, vedere [Visual Basic frammenti di codice IntelliSense](https://msdn.microsoft.com/library/18yz4be4.aspx)
- Altre informazioni sulle funzionalità C#specifiche di IntelliSense, ad esempio il refactoring e i frammenti di codice. Per ulteriori informazioni, vedere [Visual C# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx).
