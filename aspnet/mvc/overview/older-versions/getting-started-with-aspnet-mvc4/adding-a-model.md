---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: 'Nota: una versione aggiornata di questa esercitazione è disponibile qui che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e demo...'
ms.author: riande
ms.date: 08/28/2012
ms.assetid: 53db72da-e0b9-44d9-b60b-6e6988c00b28
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: a9851d93dde495814f67fa0c807d3534f5f0d8ef
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457804"
---
# <a name="adding-a-model"></a>Aggiunta di un modello

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> > [!NOTE]
> > Una versione aggiornata di questa esercitazione è disponibile [qui](../../getting-started/introduction/getting-started.md) che usa ASP.NET MVC 5 e Visual Studio 2013. È più sicuro, molto più semplice da seguire e illustra altre funzionalità.

In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database. Queste classi saranno il modello di &quot;&quot; parte dell'applicazione MVC ASP.NET.

Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come [Entity Framework](https://msdn.microsoft.com/library/bb399572(VS.110).aspx) per definire e utilizzare queste classi di modelli. Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*. Code First consente di creare oggetti modello scrivendo classi semplici. Sono note anche come classi POCO, da oggetti CLR &quot;Plain Old.&quot;) È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.

## <a name="adding-model-classes"></a>Aggiunta di classi di modelli

In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.

![](adding-a-model/_static/image1.png)

Immettere il nome della *classe* &quot;Movie&quot;.

Aggiungere le cinque proprietà seguenti alla classe `Movie`:

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

Si userà la classe `Movie` per rappresentare i film in un database. Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.

Nello stesso file aggiungere la classe `MovieDBContext` seguente:

[!code-csharp[Main](adding-a-model/samples/sample2.cs)]

La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database. Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework.

Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `using` seguente all'inizio del file:

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

Di seguito è riportato il file *Movie.cs* completo. Sono state rimosse diverse istruzioni using non necessarie.

[!code-csharp[Main](adding-a-model/samples/sample4.cs)]

## <a name="creating-a-connection-string-and-working-with-sql-server-localdb"></a>Creazione di una stringa di connessione e uso di SQL Server LocalDB

La classe `MovieDBContext` creata gestisce l'attività di connessione al database e di mapping `Movie` oggetti ai record del database. Una domanda che è possibile porre, tuttavia, è come specificare il database a cui si connetterà. A tale scopo, aggiungere le informazioni di connessione nel file *Web. config* dell'applicazione.

Aprire il file *Web. config* radice dell'applicazione. (Non il file *Web. config* nella cartella *views* ). Aprire il file *Web. config* indicato in rosso.

![](adding-a-model/_static/image2.png)

Aggiungere la stringa di connessione seguente all'elemento `<connectionStrings>` nel file *Web. config* .

[!code-xml[Main](adding-a-model/samples/sample5.xml)]

Nell'esempio seguente viene illustrata una parte del file *Web. config* con la nuova stringa di connessione aggiunta:

[!code-xml[Main](adding-a-model/samples/sample6.xml?highlight=6-9)]

Questa piccola quantità di codice e XML è tutto ciò che è necessario scrivere per rappresentare e archiviare i dati dei film in un database.

Successivamente, verrà compilata una nuova classe di `MoviesController` che è possibile usare per visualizzare i dati dei film e consentire agli utenti di creare nuovi elenchi di film.

> [!div class="step-by-step"]
> [Precedente](adding-a-view.md)
> [Successivo](accessing-your-models-data-from-a-controller.md)
