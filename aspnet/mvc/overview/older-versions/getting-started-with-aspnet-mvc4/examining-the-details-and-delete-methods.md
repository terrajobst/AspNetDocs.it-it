---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
title: Analisi dei dettagli e dei metodi di eliminazione | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 11425ff3-09fc-4efa-be9a-b53bce503460
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/examining-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 37787a26f37473b9d36792a9f7715982cb274074
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457557"
---
# <a name="examining-the-details-and-delete-methods"></a>Analisi dei dettagli e dei metodi di eliminazione

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa parte dell'esercitazione verranno esaminati i metodi di `Details` e `Delete` generati automaticamente.

## <a name="examining-the-details-and-delete-methods"></a>Analisi dei dettagli e dei metodi di eliminazione

Aprire il controller di `Movie` ed esaminare il metodo di `Details`.

![](examining-the-details-and-delete-methods/_static/image1.png)

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample1.cs)]

Il motore di impalcatura MVC che ha creato questo metodo di azione aggiunge un commento che mostra una richiesta HTTP che richiama il metodo. In questo caso si tratta di una richiesta di `GET` con tre segmenti URL, il controller `Movies`, il metodo `Details` e un valore `ID`.

Code First semplifica la ricerca dei dati tramite il metodo `Find`. Un'importante funzionalità di sicurezza integrata nel metodo è che il codice verifica che il metodo `Find` abbia rilevato un film prima che il codice tenti di eseguire qualsiasi operazione. Un pirata informatico può, ad esempio, introdurre errori nel sito modificando l'URL creato dai collegamenti da `http://localhost:xxxx/Movies/Details/1` a un elemento come `http://localhost:xxxx/Movies/Details/12345` (o un altro valore che non rappresenta un film effettivo). Se non è stata verificata la presenza di un film null, un film null genera un errore di database.

Esaminare i metodi `Delete` e `DeleteConfirmed`.

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample2.cs?highlight=17)]

Si noti che il metodo `HTTP Get``Delete` non elimina il film specificato, restituisce una visualizzazione del film in cui è possibile inviare (`HttpPost`) l'eliminazione. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza. Per altre informazioni, vedere il post di Blog di Stephen Walther [ASP.NET MVC Tip #46-non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Il metodo `HttpPost` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample3.cs)]

Il Common Language Runtime (CLR) richiede metodi in rapporto di overload per disporre di una firma di parametro univoca, ovvero lo stesso nome di metodo ma un elenco diverso di parametri. Tuttavia, in questo caso sono necessari due metodi Delete, uno per GET e uno per POST, che hanno entrambe la stessa firma di parametro. Entrambi devono accettare un singolo intero come parametro.

Per risolvere il problema, è possibile eseguire alcune operazioni. Uno è quello di assegnare i nomi ai metodi diversi. Questa operazione è stata eseguita dal meccanismo di scaffolding nell'esempio precedente. Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. In questo modo viene eseguito il mapping per il sistema di routing in modo che un URL che includa <em>/Delete/</em>per una richiesta post trovi il metodo `DeleteConfirmed`.

Un altro modo comune per evitare un problema con i metodi che hanno nomi e firme identici consiste nel modificare artificialmente la firma del metodo POST per includere un parametro non usato. Ad esempio, alcuni sviluppatori aggiungono un tipo di parametro `FormCollection` passato al metodo POST e quindi semplicemente non usano il parametro:

[!code-csharp[Main](examining-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="summary"></a>Riepilogo

A questo punto è disponibile un'applicazione MVC ASP.NET completa che archivia i dati in un database locale. È possibile creare, leggere, aggiornare, eliminare e cercare i film.

![](examining-the-details-and-delete-methods/_static/image2.png)

## <a name="next-steps"></a>Passaggi successivi

Una volta compilata e testata un'applicazione Web, il passaggio successivo consiste nel renderlo disponibile ad altri utenti per l'utilizzo su Internet. A tale scopo, è necessario distribuirlo a un provider di hosting Web. Microsoft offre hosting web gratuito per un massimo di 10 siti Web in un [account di valutazione gratuito di Windows Azure](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604). Suggerisco di seguire l'esercitazione [distribuire un'app ASP.NET MVC sicura con appartenenza, OAuth e database SQL in un sito Web di Microsoft Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un'esercitazione eccellente è la [creazione di un modello di dati di Entity Framework per un'applicazione MVC ASP.NET](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)di Tom Dykstra. [StackOverflow](http://stackoverflow.com/help) e i [Forum ASP.NET MVC](https://forums.asp.net/1146.aspx) sono un'ottima soluzione per porre domande. [Seguimi su Twitter per poter](https://twitter.com/RickAndMSFT) ottenere gli aggiornamenti sulle mie ultime esercitazioni.

I commenti sono benvenuti.

\- [Rick Anderson](https://blogs.msdn.com/rickAndy) twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT)  
\- [Scott hanseln](http://www.hanselman.com/blog/) twitter: [@shanselman](https://twitter.com/shanselman)

> [!div class="step-by-step"]
> [Precedente](adding-validation-to-the-model.md)
