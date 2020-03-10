---
uid: web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
title: Ordinamento, paging e filtro dei dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 266e7866-e327-4687-b29d-627a0925e87d
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/sorting-paging-and-filtering-data
msc.type: authoredcontent
ms.openlocfilehash: f8e64392af6110f36c6af98c4e4e9481c94a0d82
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78548062"
---
# <a name="sorting-paging-and-filtering-data-with-model-binding-and-web-forms"></a>Ordinamento, paging e filtro dei dati con associazione di modelli e Web Form

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource. Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come aggiungere ordinamento, paging e filtro dei dati tramite l'associazione di modelli.
> 
> Questa esercitazione si basa sul progetto creato nella prima [parte](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.

## <a name="what-youll-build"></a>Elementi da compilare

In questa esercitazione si apprenderà come:

1. Abilitare l'ordinamento e il paging dei dati
2. Consente di filtrare i dati in base a una selezione effettuata dall'utente

## <a name="add-sorting"></a>Aggiungere l'ordinamento

L'abilitazione dell'ordinamento in GridView è molto semplice. Nel file Student. aspx è sufficiente impostare **AllowSorting** su **true** in GridView. Non è necessario impostare un valore **SortExpression** per ogni colonna perché il campo DataField viene usato automaticamente. GridView modifica la query per includere l'ordinamento dei dati in base al valore selezionato. Il codice evidenziato seguente illustra l'aggiunta che è necessario eseguire per abilitare l'ordinamento.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample1.aspx?highlight=5)]

Eseguire l'applicazione Web e testare i record degli studenti di ordinamento in base ai valori in colonne diverse.

![Ordina studenti](sorting-paging-and-filtering-data/_static/image2.png)

## <a name="add-paging"></a>Aggiungere la suddivisione in pagine

Anche l'abilitazione del paging è molto semplice. In GridView impostare la proprietà **AllowPaging** su **true** e impostare la proprietà **pageSize** sul numero di record che si desidera visualizzare in ogni pagina. In questa esercitazione è possibile impostarla su 4.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample2.aspx?highlight=5)]

Eseguire l'applicazione Web. si noti che ora i record sono divisi in più pagine con non più di 4 record visualizzati in una singola pagina.

![Aggiungi paging](sorting-paging-and-filtering-data/_static/image4.png)

L'esecuzione posticipata delle query migliora l'efficienza dell'applicazione. Anziché recuperare l'intero set di dati, GridView modifica la query per recuperare solo i record per la pagina corrente.

## <a name="filter-records-by-user-selection"></a>Filtrare i record in base alla selezione dell'utente

L'associazione di modelli aggiunge diversi attributi che consentono di definire come impostare il valore per un parametro in un metodo di associazione di modelli. Questi attributi si trovano nello spazio dei nomi **System. Web. ModelBinding** . e comprendono:

- Control
- Cookie
- Form
- Profilo
- QueryString
- RouteData
- Sessione
- UserProfile
- ViewState

In questa esercitazione verrà usato il valore di un controllo per filtrare i record che vengono visualizzati in GridView. L'attributo **Control** viene aggiunto al metodo di query creato in precedenza. In un'esercitazione [successiva](using-query-string-values-to-retrieve-data.md) verrà applicato l'attributo **QueryString** a un parametro per specificare che il valore del parametro deriva da un valore della stringa di query.

Innanzitutto, sopra il ValidationSummary, aggiungere un elenco a discesa per filtrare gli studenti visualizzati.

[!code-aspx[Main](sorting-paging-and-filtering-data/samples/sample3.aspx?highlight=3-11)]

Nel file code-behind modificare il metodo Select per ricevere un valore dal controllo e impostare il nome del parametro sul nome del controllo che fornisce il valore.

È necessario aggiungere un'istruzione **using** per lo spazio dei nomi **System. Web. ModelBinding** per risolvere l'attributo del controllo.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample4.cs)]

Il codice seguente mostra che il metodo Select è stato rielaborato per filtrare i dati restituiti in base al valore dell'elenco a discesa. L'aggiunta di un attributo di controllo prima di un parametro specifica che il valore di questo parametro deriva da un controllo con lo stesso nome.

[!code-csharp[Main](sorting-paging-and-filtering-data/samples/sample5.cs)]

Eseguire l'applicazione Web e selezionare valori diversi nell'elenco a discesa per filtrare l'elenco di studenti.

![Filtra studenti](sorting-paging-and-filtering-data/_static/image6.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato abilitato l'ordinamento e il paging dei dati. È stato inoltre abilitato il filtraggio dei dati in base al valore di un controllo.

Nell' [esercitazione](integrating-jquery-ui.md) successiva verrà migliorata l'interfaccia utente integrando un widget dell'interfaccia utente jQuery nel modello Dynamic Data.

> [!div class="step-by-step"]
> [Precedente](updating-deleting-and-creating-data.md)
> [Successivo](integrating-jquery-ui.md)
