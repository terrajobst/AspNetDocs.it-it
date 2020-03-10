---
uid: web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
title: Aggiornamento, eliminazione e creazione di dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 602baa94-5a4f-46eb-a717-7a9e539c1db4
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/updating-deleting-and-creating-data
msc.type: authoredcontent
ms.openlocfilehash: 11dc52ec79ca91119b37ea60e08164eb1b1d0e2b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78586646"
---
# <a name="updating-deleting-and-creating-data-with-model-binding-and-web-forms"></a>Aggiornamento, eliminazione e creazione di dati con l'associazione di modelli e Web Form

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource. Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come creare, aggiornare ed eliminare dati con l'associazione di modelli. Vengono impostate le proprietà seguenti:
> 
> - DeleteMethod
> - InsertMethod
> - UpdateMethod
> 
> Queste proprietà ricevono il nome del metodo che gestisce l'operazione corrispondente. All'interno di questo metodo, viene fornita la logica per interagire con i dati.
> 
> Questa esercitazione si basa sul progetto creato nella prima [parte](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.

## <a name="what-youll-build"></a>Elementi da compilare

In questa esercitazione si apprenderà come:

1. Aggiungere modelli di dati dinamici
2. Abilitare l'aggiornamento e l'eliminazione di dati tramite metodi di associazione di modelli
3. Applicare le regole di convalida dei dati-abilitare la creazione di un nuovo record nel database

## <a name="add-dynamic-data-templates"></a>Aggiungere modelli di dati dinamici

Per offrire la migliore esperienza utente e ridurre al minimo la ripetizione del codice, si utilizzeranno i modelli di dati dinamici. È possibile integrare facilmente i modelli di dati dinamici predefiniti nel sito esistente installando un pacchetto NuGet.

Da **Gestisci pacchetti NuGet**installare **DynamicDataTemplatesCS**.

![modelli di dati dinamici](updating-deleting-and-creating-data/_static/image1.png)

Si noti che il progetto include ora una cartella denominata **DynamicData**. In tale cartella si troveranno i modelli che vengono applicati automaticamente ai controlli dinamici nei Web Form.

![cartella Dynamic Data](updating-deleting-and-creating-data/_static/image2.png)

## <a name="enable-updating-and-deleting"></a>Abilitare l'aggiornamento e l'eliminazione

Consentire agli utenti di aggiornare ed eliminare i record nel database è molto simile al processo di recupero dei dati. Nelle proprietà **UpdateMethod** e **DeleteMethod** è possibile specificare i nomi dei metodi che eseguono tali operazioni. Con un controllo GridView è inoltre possibile specificare la generazione automatica dei pulsanti modifica ed Elimina. Il codice evidenziato seguente mostra le aggiunte al codice GridView.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample1.aspx?highlight=4-5)]

Nel file code-behind aggiungere un'istruzione using per **System. Data. Entity. Infrastructure**.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample2.cs)]

Aggiungere quindi i metodi di aggiornamento ed eliminazione seguenti.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample3.cs)]

Il metodo **TryUpdateModel** applica i valori associati a dati corrispondenti dal Web Form all'elemento di dati. L'elemento dati viene recuperato in base al valore del parametro ID.

## <a name="enforce-validation-requirements"></a>Applicare i requisiti di convalida

Gli attributi di convalida applicati alle proprietà FirstName, LastName e Year della classe Student vengono applicati automaticamente quando si aggiornano i dati. I controlli DynamicField aggiungono validator client e server in base agli attributi di convalida. Entrambe le proprietà FirstName e LastName sono obbligatorie. FirstName non può superare i 20 caratteri di lunghezza e LastName non può superare i 40 caratteri. L'anno deve essere un valore valido per l'enumerazione AcademicYear.

Se l'utente viola uno dei requisiti di convalida, l'aggiornamento non procede. Per visualizzare il messaggio di errore, aggiungere un controllo ValidationSummary sopra GridView. Per visualizzare gli errori di convalida dall'associazione di modelli, impostare la proprietà **ShowModelStateErrors** su **true**. 

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample4.aspx)]

Eseguire l'applicazione Web e aggiornare ed eliminare i record.

![aggiornare i dati](updating-deleting-and-creating-data/_static/image3.png)

Si noti che quando in modalità di modifica il valore della proprietà Year viene automaticamente visualizzato come elenco a discesa. La proprietà Year è un valore di enumerazione e il modello di dati dinamici per un valore di enumerazione specifica un elenco a discesa per la modifica. È possibile trovare il modello aprendo il file di **enumerazione\_Edit. ascx** nella cartella **DynamicData**/**FieldTemplates** .

Se vengono forniti valori validi, l'aggiornamento viene completato correttamente. Se si viola uno dei requisiti di convalida, l'aggiornamento non procede e viene visualizzato un messaggio di errore sopra la griglia.

![messaggio di errore](updating-deleting-and-creating-data/_static/image4.png)

## <a name="add-new-records"></a>Aggiungi nuovi record

Il controllo GridView non include la proprietà **InsertMethod** e pertanto non può essere usato per aggiungere un nuovo record con l'associazione di modelli. È possibile trovare la proprietà InsertMethod nei controlli **FormView**, **DetailsView**o **ListView** . In questa esercitazione si userà un controllo FormView per aggiungere un nuovo record.

In primo luogo, aggiungere un collegamento alla nuova pagina che si creerà per l'aggiunta di un nuovo record. Sopra il ValidationSummary aggiungere:

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample5.aspx)]

Il nuovo collegamento verrà visualizzato nella parte superiore del contenuto della pagina Students (studenti).

![nuovo collegamento](updating-deleting-and-creating-data/_static/image5.png)

Aggiungere quindi un nuovo Web form usando una pagina master e denominarlo **AddStudent**. Selezionare site. master come pagina master.

Si eseguirà il rendering dei campi per l'aggiunta di un nuovo studente usando un controllo **DynamicEntity** . Il controllo DynamicEntity esegue il rendering delle proprietà modificabili nella classe specificata nella proprietà ItemType. La proprietà StudentID è stata contrassegnata con l'attributo **[ScaffoldColumn (false)]** in modo che non venga sottoposta a rendering. Nel segnaposto MainContent della pagina AddStudent aggiungere il codice seguente.

[!code-aspx[Main](updating-deleting-and-creating-data/samples/sample6.aspx)]

Nel file code-behind (AddStudent.aspx.cs) aggiungere un'istruzione **using** per lo spazio dei nomi **ContosoUniversityModelBinding. Models** .

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample7.cs)]

Aggiungere quindi i metodi seguenti per specificare come inserire un nuovo record e un gestore eventi per il pulsante Annulla.

[!code-csharp[Main](updating-deleting-and-creating-data/samples/sample8.cs)]

Salvare tutte le modifiche.

Eseguire l'applicazione Web e creare un nuovo studente.

![Aggiungi nuovo studente](updating-deleting-and-creating-data/_static/image6.png)

Fare clic su **Inserisci** . si noti che il nuovo studente è stato creato.

![Visualizza nuovo studente](updating-deleting-and-creating-data/_static/image7.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione sono stati abilitati l'aggiornamento, l'eliminazione e la creazione di dati. Le regole di convalida vengono applicate quando si interagisce con i dati.

Nell' [esercitazione](sorting-paging-and-filtering-data.md) successiva di questa serie sarà possibile abilitare l'ordinamento, il paging e il filtro dei dati.

> [!div class="step-by-step"]
> [Precedente](retrieving-data.md)
> [Successivo](sorting-paging-and-filtering-data.md)
