---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-4
title: Gestione delle relazioni tra entità | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/16/2014
ms.assetid: d2f5710c-23c7-40a5-9cd9-5d0516570cba
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-4
msc.type: authoredcontent
ms.openlocfilehash: be4948e5443a5eb4e1824c63dd0c445a7ee1928e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557463"
---
# <a name="handling-entity-relations"></a>Gestione delle relazioni tra entità

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://github.com/MikeWasson/BookService)

In questa sezione vengono descritti alcuni dettagli sul modo in cui EF carica le entità correlate e come gestire le proprietà di navigazione circolari nelle classi del modello. Questa sezione fornisce informazioni di base e non è necessaria per completare l'esercitazione. Se preferisci, vai alla [parte 5.](part-5.md).)

## <a name="eager-loading-versus-lazy-loading"></a>Caricamento eager rispetto al caricamento lazy

Quando si usa EF con un database relazionale, è importante comprendere in che modo EF carica i dati correlati.

È anche utile vedere le query SQL generate da EF. Per tracciare l'oggetto SQL, aggiungere la seguente riga di codice al costruttore `BookServiceContext`:

[!code-csharp[Main](part-4/samples/sample1.cs)]

Se si invia una richiesta GET a/API/Books, viene restituito JSON come il seguente:

[!code-console[Main](part-4/samples/sample2.cmd)]

Si noterà che la proprietà Author è null, anche se il libro contiene una valida autorizzazione. Questo perché EF non carica le entità Author correlate. Il log di traccia della query SQL conferma quanto segue:

[!code-console[Main](part-4/samples/sample3.sql)]

L'istruzione SELECT prende dalla tabella books e non fa riferimento alla tabella author.

Per riferimento, di seguito è riportato il metodo nella classe `BooksController` che restituisce l'elenco di libri.

[!code-csharp[Main](part-4/samples/sample4.cs)]

Vediamo come possiamo restituire l'autore come parte dei dati JSON. Sono disponibili tre modi per caricare i dati correlati in Entity Framework: caricamento eager, caricamento lazy e caricamento esplicito. Esistono compromessi per ciascuna tecnica, quindi è importante comprenderne il funzionamento.

### <a name="eager-loading"></a>Caricamento eager

Con il *caricamento eager*, EF carica le entità correlate come parte della query di database iniziale. Per eseguire il caricamento eager, usare il metodo di estensione **System. Data. Entity. include** .

[!code-csharp[Main](part-4/samples/sample5.cs)]

Ciò indica a EF di includere i dati dell'autore nella query. Se si apportano queste modifiche ed è stata eseguita l'app, ora i dati JSON sono simili ai seguenti:

[!code-console[Main](part-4/samples/sample6.cmd)]

Il log di traccia Mostra che EF ha eseguito un join sulle tabelle Book e Author.

[!code-console[Main](part-4/samples/sample7.cmd)]

### <a name="lazy-loading"></a>Caricamento lazy

Con il caricamento lazy, EF carica automaticamente un'entità correlata quando viene dereferenziata la proprietà di navigazione per l'entità. Per abilitare il caricamento lazy, rendere virtuale la proprietà di navigazione. Ad esempio, nella classe Book:

[!code-csharp[Main](part-4/samples/sample8.cs?highlight=6)]

Si consideri ora il codice seguente:

[!code-csharp[Main](part-4/samples/sample9.cs)]

Quando il caricamento lazy è abilitato, l'accesso alla proprietà `Author` in `books[0]` causa l'esecuzione di una query sul database per l'autore da parte di EF.

Il caricamento lazy richiede più percorsi di database, perché EF invia una query ogni volta che viene recuperata un'entità correlata. In genere, è necessario disabilitare il caricamento lazy per gli oggetti serializzati. Il serializzatore deve leggere tutte le proprietà del modello, che attiva il caricamento delle entità correlate. Ad esempio, di seguito sono riportate le query SQL quando EF serializza l'elenco di libri in cui è abilitato il caricamento lazy. Come si può notare, EF esegue tre query separate per i tre autori.

[!code-console[Main](part-4/samples/sample10.sql)]

In alcuni casi è possibile utilizzare il caricamento lazy. Il caricamento eager può causare la generazione di un join molto complesso da parte di EF. In alternativa, potrebbero essere necessarie entità correlate per un piccolo subset di dati e il caricamento lazy risulta più efficiente.

Un modo per evitare problemi di serializzazione consiste nel serializzare gli oggetti DTO (Data Transfer Objects) anziché gli oggetti entità. Questo approccio verrà illustrato più avanti in questo articolo.

### <a name="explicit-loading"></a>Caricamento esplicito

Il caricamento esplicito è simile al caricamento lazy, tranne per il fatto che si ottengono in modo esplicito i dati correlati nel codice; non viene eseguita automaticamente quando si accede a una proprietà di navigazione. Il caricamento esplicito offre un maggiore controllo sul momento in cui caricare i dati correlati, ma richiede codice aggiuntivo. Per ulteriori informazioni sul caricamento esplicito, vedere [caricamento di entità correlate](https://msdn.microsoft.com/data/jj574232#explicit).

## <a name="navigation-properties-and-circular-references"></a>Proprietà di navigazione e riferimenti circolari

Quando sono stati definiti i modelli Book e Author, è stata definita una proprietà di navigazione nella classe `Book` per la relazione book-author, ma non è stata definita una proprietà di navigazione nell'altra direzione.

Cosa accade se si aggiunge la proprietà di navigazione corrispondente alla classe `Author`?

[!code-csharp[Main](part-4/samples/sample11.cs?highlight=7)]

Sfortunatamente, questo crea un problema quando si serializzano i modelli. Se si caricano i dati correlati, viene creato un oggetto grafico circolare.

![](part-4/_static/image1.png)

Quando il formattatore JSON o XML tenta di serializzare il grafo, verrà generata un'eccezione. I due formattatori generano messaggi di eccezione diversi. Di seguito è riportato un esempio per il formattatore JSON:

[!code-console[Main](part-4/samples/sample12.cmd)]

Il formattatore XML è il seguente:

[!code-xml[Main](part-4/samples/sample13.xml)]

Una soluzione consiste nell'usare dto, descritto nella sezione successiva. In alternativa, è possibile configurare i formattatori JSON e XML per gestire i cicli Graph. Per ulteriori informazioni, vedere [gestione di riferimenti a oggetti circolari](../../formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references).

Per questa esercitazione non è necessaria la proprietà di navigazione `Author.Book`, quindi è possibile lasciarla invariata.

> [!div class="step-by-step"]
> [Precedente](part-3.md)
> [Successivo](part-5.md)
