---
uid: web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
title: Integrazione di DatePicker interfaccia utente di JQuery con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 3cbab37b-fb0f-4751-9ec4-74e068c3f380
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/integrating-jquery-ui
msc.type: authoredcontent
ms.openlocfilehash: c8d711dd44950116f3a3e09d5d12c507918c543f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78642478"
---
# <a name="integrating-jquery-ui-datepicker-with-model-binding-and-web-forms"></a>Integrazione di DatePicker interfaccia utente JQuery con associazione di modelli e Web Form

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource. Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come aggiungere il [widget DATEPICKER UI DatePicker](http://jqueryui.com/datepicker/) a un Web Form e usare l'associazione di modelli per aggiornare il database con il valore selezionato.
> 
> Questa esercitazione si basa sul progetto creato nella [prima](retrieving-data.md) e nella [seconda](updating-deleting-and-creating-data.md) parte della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.

## <a name="what-youll-build"></a>Elementi da compilare

In questa esercitazione si apprenderà come:

1. Aggiungere una proprietà al modello per registrare la data di registrazione dello studente
2. Consentire all'utente di selezionare la data di registrazione tramite il widget DatePicker interfaccia utente di JQuery
3. Applicare le regole di convalida per la data di registrazione

Il widget DatePicker interfaccia utente di JQuery consente agli utenti di selezionare facilmente una data da un calendario visualizzato quando l'utente interagisce con il campo. L'uso di questo widget può essere più utile per gli utenti rispetto alla digitazione manuale di una data. L'integrazione del widget DatePicker in una pagina che usa l'associazione di modelli per le operazioni sui dati richiede solo una piccola quantità di lavoro aggiuntivo.

## <a name="add-a-new-property-to-the-model"></a>Aggiungere una nuova proprietà al modello

Innanzitutto, si aggiungerà una proprietà **DateTime** al modello Student e si eseguirà la migrazione della modifica al database. Aprire **UniversityModels.cs**e aggiungere il codice evidenziato al modello Student.

[!code-csharp[Main](integrating-jquery-ui/samples/sample1.cs?highlight=16-18)]

**RangeAttribute** è incluso per applicare le regole di convalida per la proprietà. Per questa esercitazione si presuppone che Contoso University sia stato creato il 1 ° gennaio 2013 e pertanto le date di registrazione precedenti non sono valide.

Nella finestra Gestione pacchetti aggiungere una migrazione eseguendo il comando **Add-Migration AddEnrollmentDate**. Si noti che il codice di migrazione aggiunge la nuova colonna DateTime alla tabella Student. Per trovare la corrispondenza con il valore specificato in RangeAttribute, aggiungere un valore predefinito per la nuova colonna, come illustrato nel codice evidenziato di seguito.

[!code-csharp[Main](integrating-jquery-ui/samples/sample2.cs?highlight=11)]

Salvare le modifiche apportate al file di migrazione.

Non è necessario eseguire nuovamente il seeding dei dati. Quindi, aprire **Configuration.cs** nella cartella Migrations e rimuovere o impostare come commento il codice nel metodo **Seed** . Salvare e chiudere il file.

A questo punto, eseguire il comando **Update-database**. Si noti che la colonna è ora presente nel database e tutti i record esistenti hanno il valore predefinito per EnrollmentDate.

## <a name="add-dynamic-controls-for-enrollment-date"></a>Aggiungere controlli dinamici per la data di registrazione

Verranno ora aggiunti i controlli per la visualizzazione e la modifica della data di registrazione. A questo punto, il valore viene modificato tramite una casella di testo. Più avanti nell'esercitazione si modificherà la casella di testo nel widget JQuery DatePicker.

Prima di tutto, è importante tenere presente che non è necessario apportare alcuna modifica al file **AddStudent. aspx** . Il controllo DynamicEntity eseguirà automaticamente il rendering della nuova proprietà.

Aprire **students. aspx**e aggiungere il codice evidenziato seguente.

[!code-aspx[Main](integrating-jquery-ui/samples/sample3.aspx?highlight=13)]

Eseguire l'applicazione e notare che è possibile impostare il valore della data di registrazione digitando una data. Quando si aggiunge un nuovo studente:

![data impostazione](integrating-jquery-ui/_static/image1.png)

In alternativa, modificare un valore esistente:

![Data di modifica](integrating-jquery-ui/_static/image2.png)

Digitare la data funziona, ma potrebbe non essere l'esperienza del cliente che si desidera fornire. Nella sezione successiva verrà abilitata la selezione di una data tramite un calendario.

## <a name="install-nuget-package-to-work-with-jquery-ui"></a>Installare il pacchetto NuGet per lavorare con l'interfaccia utente di JQuery

Il pacchetto NuGet dell' **interfaccia utente Juice** consente l'integrazione semplificata dei widget dell'interfaccia utente di jQuery nell'applicazione Web. Per usare questo pacchetto, installarlo tramite NuGet.

![Aggiungi interfaccia utente Juice](integrating-jquery-ui/_static/image3.png)

La versione dell'interfaccia utente di Juice installata potrebbe entrare in conflitto con la versione di JQuery nell'applicazione. Prima di procedere con questa esercitazione, provare a eseguire l'applicazione. Se si verifica un errore JavaScript, è necessario riconciliare la versione di JQuery. È possibile aggiungere la versione prevista di JQuery alla cartella degli script (versione 1.8.2 al momento della stesura di questa esercitazione) o in site. master specificare il percorso del file JQuery.

[!code-aspx[Main](integrating-jquery-ui/samples/sample4.aspx)]

## <a name="customize-datetime-template-to-include-datepicker-widget"></a>Personalizzare il modello DateTime per includere il widget DatePicker

Il widget DatePicker viene aggiunto al modello Dynamic Data per la modifica di un valore DateTime. Aggiungendo il widget al modello, il rendering viene eseguito automaticamente in entrambi i form per l'aggiunta di un nuovo studente e nella visualizzazione griglia per la modifica degli studenti. Aprire **DateTime\_Edit. ascx**e aggiungere il codice evidenziato seguente.

[!code-aspx[Main](integrating-jquery-ui/samples/sample5.aspx?highlight=3)]

Nel file code-behind si imposteranno le date minime e massime per il DatePicker. Impostando questi valori, si impedirà agli utenti di spostarsi a date non valide. Si recupereranno i valori minimo e massimo da **RangeAttribute** sulla proprietà DateTime, se ne viene specificato uno. Aprire **DateTime\_Edit.ascx.cs**e aggiungere il codice evidenziato seguente alla pagina\_metodo Load.

[!code-csharp[Main](integrating-jquery-ui/samples/sample6.cs?highlight=9-14)]

Eseguire l'applicazione Web e passare alla pagina AddStudent. Specificare i valori per i campi. si noti che quando si fa clic sulla casella di testo per la data di registrazione, viene visualizzato il calendario.

![selezione data](integrating-jquery-ui/_static/image4.png)

Selezionare una data e fare clic su **Inserisci**. RangeAttribute impone la convalida nel server. Impostando la proprietà minDe nell'oggetto DatePicker, viene applicata anche la convalida nel client. Il calendario non consente all'utente di passare a una data prima del valore di minDe.

Quando si modifica un record nella visualizzazione griglia, viene visualizzato anche il calendario.

![DatePicker in GridView](integrating-jquery-ui/_static/image5.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione si è appreso come incorporare un widget JQuery in un Web Form che usa l'associazione di modelli.

Nell' [esercitazione](using-query-string-values-to-retrieve-data.md)successiva si userà un valore della stringa di query quando si selezionano i dati.

> [!div class="step-by-step"]
> [Precedente](sorting-paging-and-filtering-data.md)
> [Successivo](using-query-string-values-to-retrieve-data.md)
