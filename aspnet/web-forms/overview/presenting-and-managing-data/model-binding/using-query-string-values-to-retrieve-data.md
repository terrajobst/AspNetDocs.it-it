---
uid: web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
title: Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e Web Form | Microsoft Docs
author: Rick-Anderson
description: Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati-...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: b90978bd-795d-4871-9ade-1671caff5730
msc.legacyurl: /web-forms/overview/presenting-and-managing-data/model-binding/using-query-string-values-to-retrieve-data
msc.type: authoredcontent
ms.openlocfilehash: 143ddcb40b576a3129e659b90bfc8321c061a547
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78639097"
---
# <a name="using-query-string-values-to-filter-data-with-model-binding-and-web-forms"></a>Utilizzo di valori di stringa di query per filtrare i dati con l'associazione di modelli e Web Form

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa serie di esercitazioni illustra gli aspetti di base dell'uso dell'associazione di modelli con un progetto Web Form ASP.NET. L'associazione di modelli rende più semplice l'interazione dei dati rispetto alla gestione di oggetti origine dati, ad esempio ObjectDataSource o SqlDataSource. Questa serie inizia con materiale introduttivo e passa a concetti più avanzati nelle esercitazioni successive.
> 
> Questa esercitazione illustra come passare un valore nella stringa di query e usare tale valore per recuperare i dati tramite l'associazione di modelli.
> 
> Questa esercitazione si basa sul progetto creato nelle parti [precedenti](retrieving-data.md) della serie.
> 
> È possibile [scaricare](https://go.microsoft.com/fwlink/?LinkId=286116) il progetto completo in C# o VB. Il codice scaricabile funziona con Visual Studio 2012 o Visual Studio 2013. Usa il modello di Visual Studio 2012, che è leggermente diverso rispetto al modello di Visual Studio 2013 illustrato in questa esercitazione.

## <a name="what-youll-build"></a>Elementi da compilare

In questa esercitazione si apprenderà come:

1. Aggiungere una nuova pagina per mostrare i corsi registrati per uno studente
2. Recupera i corsi registrati per lo studente selezionato in base a un valore nella stringa di query
3. Aggiungere un collegamento ipertestuale con un valore della stringa di query dalla visualizzazione griglia alla nuova pagina

I passaggi descritti in questa esercitazione sono piuttosto simili a quelli dell' [esercitazione](sorting-paging-and-filtering-data.md) precedente per filtrare gli studenti visualizzati in base alla selezione dell'utente in un elenco a discesa. In questa esercitazione è stato usato l'attributo **Control** nel metodo Select per specificare che il valore del parametro deriva da un controllo. In questa esercitazione si userà l'attributo **QueryString** nel metodo Select per specificare che il valore del parametro deriva dalla stringa di query.

## <a name="add-new-page-for-displaying-a-students-courses"></a>Aggiungi nuova pagina per la visualizzazione dei corsi di uno studente

Aggiungere un nuovo Web Form che usa la pagina master site. master e denominare i **corsi**della pagina.

Nel file **courses. aspx** aggiungere una visualizzazione griglia per visualizzare i corsi per lo studente selezionato.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample1.aspx)]

## <a name="define-the-select-method"></a>Definire il metodo Select

In **courses.aspx.cs**, il metodo Select viene aggiunto con il nome specificato nella proprietà **SelectMethod** della visualizzazione griglia. In questo metodo si definirà la query per il recupero dei corsi di uno studente e si specificherà che il parametro deriva da un valore della stringa di query con lo stesso nome del parametro.

In primo luogo, è necessario aggiungere le istruzioni **using** seguenti.

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample2.cs)]

Aggiungere quindi il codice seguente a Courses.aspx.cs:

[!code-csharp[Main](using-query-string-values-to-retrieve-data/samples/sample3.cs)]

L'attributo QueryString indica che un valore di stringa di query denominato StudentID viene assegnato automaticamente al parametro in questo metodo.

## <a name="add-hyperlink-with-query-string-value"></a>Aggiungi collegamento ipertestuale con valore stringa di query

Nella visualizzazione griglia di students. aspx si aggiungerà un campo collegamento ipertestuale che collega alla pagina nuovi corsi. Il collegamento ipertestuale includerà un valore della stringa di query con l'ID dello studente.

In students. aspx aggiungere il campo seguente alle colonne della visualizzazione griglia immediatamente sotto il campo per i crediti totali.

[!code-aspx[Main](using-query-string-values-to-retrieve-data/samples/sample4.aspx?highlight=7-8)]

Eseguire l'applicazione e notare che la visualizzazione griglia include ora il collegamento Courses (corsi).

![Aggiungi collegamento ipertestuale](using-query-string-values-to-retrieve-data/_static/image1.png)

Quando si fa clic su uno dei collegamenti, verranno visualizzati i corsi registrati per gli studenti.

![Mostra corsi](using-query-string-values-to-retrieve-data/_static/image2.png)

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato aggiunto un collegamento con un valore della stringa di query. È stato usato il valore della stringa di query per il valore del parametro nel metodo Select.

Nell' [esercitazione](adding-business-logic-layer.md)successiva verrà spostato il codice dai file code-behind in un livello di logica di business e un livello di accesso ai dati.

> [!div class="step-by-step"]
> [Precedente](integrating-jquery-ui.md)
> [Successivo](adding-business-logic-layer.md)
