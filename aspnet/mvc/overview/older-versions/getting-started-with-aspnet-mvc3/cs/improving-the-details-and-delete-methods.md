---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
title: Miglioramento dei dettagli e dei metodi diC#eliminazione () | Microsoft Docs
author: Rick-Anderson
description: In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, ovvero...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 3f42edd9-c5b8-4712-9055-970f7d38e350
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/improving-the-details-and-delete-methods
msc.type: authoredcontent
ms.openlocfilehash: 54d7be8fe1bff604ae9c9e9914d7c6426ab85c1c
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78615311"
---
# <a name="improving-the-details-and-delete-methods-c"></a>Miglioramento dei dettagli e dei metodi di eliminazione (C#)

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.
> 
> 
> In questa esercitazione vengono illustrate le nozioni di base della creazione di un'applicazione Web MVC ASP.NET utilizzando Microsoft Visual Web Developer 2010 Express Service Pack 1, una versione gratuita di Microsoft Visual Studio. Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente i prerequisiti usando i collegamenti seguenti:
> 
> - [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)
> 
> Se si usa Visual Studio 2010 anziché Visual Web Developer 2010, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Per accompagnare questo argomento, C# è disponibile un progetto Visual Web Developer con codice sorgente. [Scaricare la C# versione](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Se si preferisce Visual Basic, passare alla [versione Visual Basic](../vb/intro-to-aspnet-mvc-3.md) di questa esercitazione.

In questa parte dell'esercitazione verranno apportati alcuni miglioramenti ai metodi di `Details` e `Delete` generati automaticamente. Queste modifiche non sono necessarie, ma con pochi piccoli frammenti di codice, è possibile migliorare facilmente l'applicazione.

## <a name="improving-the-details-and-delete-methods"></a>Miglioramento dei dettagli e dei metodi di eliminazione

Quando si esegue il ponteggi del controller `Movie`, il codice generato da MVC ASP.NET che funzionava benissimo, ma che può essere reso più solido con alcune piccole modifiche.

Aprire il controller di `Movie` e modificare il metodo di `Details` restituendo `HttpNotFound` quando non viene trovato un film. È anche necessario modificare il metodo `Details` per impostare un valore predefinito per l'ID passato. Sono state apportate modifiche simili al metodo `Edit` nella [parte 6](examining-the-edit-methods-and-edit-view.md) di questa esercitazione. Tuttavia, è necessario modificare il tipo restituito del metodo `Details` da `ViewResult` a `ActionResult`, perché il metodo `HttpNotFound` non restituisce un oggetto `ViewResult`. Nell'esempio seguente viene illustrato il metodo `Details` modificato.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample1.cs)]

Code First semplifica la ricerca dei dati tramite il metodo `Find`. Un'importante funzionalità di sicurezza integrata nel metodo è che il codice verifica che il metodo `Find` abbia rilevato un film prima che il codice tenti di eseguire qualsiasi operazione. Un pirata informatico può, ad esempio, introdurre errori nel sito modificando l'URL creato dai collegamenti da `http://localhost:xxxx/Movies/Details/1` a un elemento come `http://localhost:xxxx/Movies/Details/12345` (o un altro valore che non rappresenta un film effettivo). Se non si verifica un film null, potrebbe verificarsi un errore di database.

Analogamente, modificare i metodi `Delete` e `DeleteConfirmed` per specificare un valore predefinito per il parametro ID e restituire `HttpNotFound` quando un film non viene trovato. Di seguito sono riportati i metodi di `Delete` aggiornati nel controller `Movie`.

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample2.cs)]

Si noti che il metodo `Delete` non elimina i dati. L'esecuzione di un'operazione di eliminazione in risposta a una richiesta GET (o l'esecuzione di un'operazione di modifica, di creazione o di qualsiasi altra azione che modifica i dati) introduce un problema di sicurezza. Per altre informazioni, vedere il post di Blog di Stephen Walther [ASP.NET MVC Tip #46-non usare i collegamenti Delete perché creano buchi di sicurezza](http://stephenwalther.com/blog/archive/2009/01/21/asp.net-mvc-tip-46-ndash-donrsquot-use-delete-links-because.aspx).

Il metodo `HttpPost` che elimina i dati è denominato `DeleteConfirmed` per fornire al metodo HTTP POST un nome o una firma univoca. Le firme dei due metodi sono illustrate di seguito:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample3.cs)]

Il Common Language Runtime (CLR) richiede che i metodi di overload abbiano una firma univoca (lo stesso nome, un elenco diverso di parametri). Tuttavia, in questo caso sono necessari due metodi Delete, uno per GET e uno per POST, che richiedono entrambi la stessa firma. Entrambi devono accettare un singolo intero come parametro.

Per risolvere il problema, è possibile eseguire alcune operazioni. Uno è quello di assegnare i nomi ai metodi diversi. Questo è ciò che è stato fatto nell'esempio precedente. Tuttavia in questo modo si introduce un piccolo problema: ASP.NET esegue il mapping dei segmenti di un URL ai metodi di azione in base al nome e, se si rinomina un metodo, generalmente il routing non è in grado di trovare questo metodo. La soluzione è mostrata nell'esempio e consiste nell'aggiungere l'attributo `ActionName("Delete")` al metodo `DeleteConfirmed`. In questo modo viene eseguito il mapping per il sistema di routing in modo che un URL che includa <em>/Delete/</em>per una richiesta post trovi il metodo `DeleteConfirmed`.

Un altro modo per evitare un problema con i metodi che hanno nomi e firme identici consiste nel modificare artificialmente la firma del metodo POST per includere un parametro non usato. Ad esempio, alcuni sviluppatori aggiungono un tipo di parametro `FormCollection` passato al metodo POST e quindi semplicemente non usano il parametro:

[!code-csharp[Main](improving-the-details-and-delete-methods/samples/sample4.cs)]

## <a name="wrapping-up"></a>Wrapping

A questo punto è disponibile un'applicazione MVC ASP.NET completa che archivia i dati in un database SQL Server Compact. È possibile creare, leggere, aggiornare, eliminare e cercare i film.

![](improving-the-details-and-delete-methods/_static/image1.png)

Questa esercitazione di base ha iniziato a creare i controller, ad associarli alle visualizzazioni e a passare i dati hardcoded. È stato quindi creato e progettato un modello di dati. Entity Framework Code First creato un database dal modello di dati in tempo reale e il sistema di impalcatura MVC ASP.NET ha generato automaticamente i metodi di azione e le visualizzazioni per le operazioni CRUD di base. È stato quindi aggiunto un modulo di ricerca che consente agli utenti di eseguire ricerche nel database. Il database è stato modificato in modo da includere una nuova colonna di dati, quindi sono state aggiornate due pagine per creare e visualizzare questi nuovi dati. È stata aggiunta la convalida contrassegnando il modello di dati con attributi dallo spazio dei nomi `DataAnnotations`. La convalida risultante viene eseguita nel client e nel server.

Se si vuole distribuire l'applicazione, è utile testare prima l'applicazione nel server IIS 7 locale. È possibile utilizzare il collegamento [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=ASPNET;) per abilitare l'impostazione IIS per le applicazioni ASP.NET. Vedere i collegamenti di distribuzione seguenti:

- [Mappa del contenuto della distribuzione di ASP.NET](https://msdn.microsoft.com/library/dd394698.aspx)
- [Abilitazione di IIS 7. x](https://blogs.msdn.com/b/rickandy/archive/2011/03/14/enabling-iis-7-x-on-windows-7-vista-sp1-windows-2008-windows-2008-r2.aspx)
- [Distribuzione di progetti di applicazione Web](https://msdn.microsoft.com/library/dd394698.aspx)

A questo punto, si consiglia di passare al nostro livello intermedio [creando un modello di dati Entity Framework per un'applicazione mvc ASP.NET](../../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) e le esercitazioni su [MVC Music Store](../../mvc-music-store/mvc-music-store-part-1.md) , per esplorare gli [articoli ASP.NET su MSDN](https://msdn.microsoft.com/library/gg416514(VS.98).aspx)e [https://asp.net/mvc](https://asp.net/mvc) per scoprire di più su ASP.NET MVC. I [Forum di ASP.NET MVC](https://forums.asp.net/1146.aspx) sono un ottimo punto di domanda.

Buona lettura.

-Scott Hansel ([http://hanselman.com](http://hanselman.com) e [@shanselman](http://twitter.com/shanselman) su Twitter) e Rick Anderson [Blogs.msdn.com/rickAndy](https://blogs.msdn.com/rickAndy)

> [!div class="step-by-step"]
> [Precedente](adding-validation-to-the-model.md)
