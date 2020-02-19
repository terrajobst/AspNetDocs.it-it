---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
title: Strategie di partizionamento dei dati (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 513837a7-cfea-4568-a4e9-1f5901245d24
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/data-partitioning-strategies
msc.type: authoredcontent
ms.openlocfilehash: efc3fa0255aa765e515412c5fa4098303a9d9234
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457024"
---
# <a name="data-partitioning-strategies-building-real-world-cloud-apps-with-azure"></a>Strategie di partizionamento dei dati (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sulla serie, vedere [il primo capitolo](introduction.md).

In precedenza abbiamo visto quanto sia semplice ridimensionare il livello Web di un'applicazione cloud, aggiungendo e rimuovendo i server Web. Ma se tutti raggiungono lo stesso archivio dati, il collo di bottiglia dell'applicazione passa dal front-end al back-end e il livello dati è il più difficile da scalare. In questo capitolo viene illustrato come rendere scalabile il livello dati tramite il partizionamento dei dati in più database relazionali o mediante la combinazione di archiviazione di database relazionali con altre opzioni di archiviazione dati.

La configurazione di uno schema di partizionamento viene eseguita meglio per lo stesso motivo indicato in precedenza: è molto difficile modificare la strategia di archiviazione dei dati dopo che un'app è in produzione. Se si pensa di affrontare i diversi approcci, è possibile evitare di avere un "momento di Twitter" quando l'app si arresta in modo anomalo o si arresta per molto tempo durante la riorganizzazione dei dati e del codice di accesso ai dati dell'app.

## <a name="the-three-vs-of-data-storage"></a>Tre rispetto all'archiviazione dei dati

Per determinare se è necessaria una strategia di partizionamento e cosa dovrebbe essere, prendere in considerazione tre domande sui dati:

- Volume: quanti dati vengono archiviati in definitiva? Un paio di Gigabyte? Un centinaio di Gigabyte? Terabyte? Petabyte?
- Velocità: qual è la velocità con cui i dati aumenteranno? Si tratta di un'app interna che non genera una grande quantità di dati? Un'app esterna in cui verranno caricati i video e le immagini?
- Varietà: specificare il tipo di dati che si archivierà. Relazionali, immagini, coppie chiave-valore, grafici sociali?

Se si ritiene di avere una grande quantità di volume, velocità o varietà, è necessario valutare con attenzione il tipo di schema di partizionamento che consentirà di ottimizzare la scalabilità dell'app in modo efficiente ed efficace Man mano che si aumenta e di evitare eventuali colli di bottiglia.

Esistono fondamentalmente tre approcci al partizionamento:

- Il partizionamento verticale
- Partizionamento orizzontale
- Partizionamento ibrido

## <a name="vertical-partitioning"></a>Il partizionamento verticale

Le porzioni verticali sono come la suddivisione di una tabella in base alle colonne: un set di colonne passa in un archivio dati e un altro set di colonne entra in un archivio dati diverso.

Si supponga, ad esempio, che l'app memorizzi i dati sulle persone, incluse le immagini:

![Tabella dati](data-partitioning-strategies/_static/image1.png)

Quando si rappresentano questi dati come una tabella e si esaminano le diverse varietà di dati, è possibile notare che le tre colonne a sinistra contengono dati di stringa che possono essere archiviati in modo efficiente da un database relazionale, mentre le due colonne a destra sono essenzialmente matrici di byte che c ome dai file di immagine. È possibile archiviare i dati dei file di immagine in un database relazionale e molte persone lo eseguono perché non desiderano salvare i dati nel file system. È possibile che non dispongano di un file system in grado di archiviare i volumi di dati necessari o che non vogliono gestire un sistema di backup e ripristino separato. Questo approccio funziona bene per i database locali e per piccole quantità di dati nei database cloud. Nell'ambiente locale, potrebbe essere più semplice consentire all'amministratore di database di occuparsi di tutto.

Tuttavia, in un database cloud, l'archiviazione è relativamente costosa e un volume elevato di immagini potrebbe far aumentare le dimensioni del database oltre i limiti in cui è possibile operare in modo efficiente. Per risolvere questi problemi, è possibile partizionare verticalmente i dati, il che significa che è possibile scegliere l'archivio dati più appropriato per ogni colonna della tabella di dati. Ciò che potrebbe funzionare meglio per questo esempio è inserire i dati stringa in un database relazionale e le immagini nell'archivio BLOB.

![Tabella dati partizionata verticalmente](data-partitioning-strategies/_static/image2.png)

L'archiviazione di immagini nell'archiviazione BLOB anziché in un database è più pratica nel cloud rispetto a un ambiente locale, perché non è necessario preoccuparsi della configurazione di file server o della gestione del backup e del ripristino dei dati archiviati all'esterno del database relazionale: tutti gestito automaticamente dal servizio di archiviazione BLOB.

Questo è l'approccio di partizionamento implementato nell'app Correggi it, che verrà esaminato nel [capitolo archiviazione BLOB](unstructured-blob-storage.md). Senza questo schema di partizionamento e supponendo una dimensione media dell'immagine di 3 megabyte, l'app Correggi it sarà in grado di archiviare solo circa 40.000 attività prima di raggiungere le dimensioni massime del database di 150 gigabyte. Dopo aver rimosso le immagini, il database può archiviare 10 volte il numero di attività. è possibile passare molto più tempo prima che sia necessario considerare l'implementazione di uno schema di partizionamento orizzontale. E quando l'app viene ridimensionata, le spese crescono più lentamente, perché la maggior parte delle esigenze di archiviazione entra in un archivio BLOB molto economico.

## <a name="horizontal-partitioning-sharding"></a>Partizionamento orizzontale (sharding)

La suddivisione orizzontale è come la suddivisione di una tabella in base alle righe: un set di righe viene inserito in un archivio dati e un altro set di righe entra in un archivio dati diverso.

Dato lo stesso set di dati, un'altra opzione consiste nell'archiviare diversi intervalli di nomi di clienti in database diversi.

![Tabella dati partizionata orizzontalmente](data-partitioning-strategies/_static/image3.png)

Si vuole prestare molta attenzione allo schema di partizionamento orizzontale per assicurarsi che i dati vengano distribuiti in modo uniforme per evitare aree sensibili. Questo semplice esempio che usa la prima lettera del cognome non soddisfa tale requisito, perché molte persone hanno i cognomi che iniziano con determinate lettere comuni. Si raggiungevano i limiti delle dimensioni della tabella prima di quanto ci si potrebbe aspettare perché alcuni database avrebbero avuto dimensioni molto elevate, ma la maggior parte rimarrebbe ridotta.

Un lato inferiore del partizionamento orizzontale è che potrebbe essere difficile eseguire query su tutti i dati. In questo esempio, una query dovrebbe trarre da un massimo di 26 database diversi per ottenere tutti i dati archiviati dall'app.

## <a name="hybrid-partitioning"></a>Partizionamento ibrido

È possibile combinare il partizionamento verticale e orizzontale. Ad esempio, nei dati di esempio è possibile archiviare le immagini nell'archivio BLOB e partizionare orizzontalmente i dati stringa.

![Tabella dati con partizionamento ibrido](data-partitioning-strategies/_static/image4.png)

## <a name="partitioning-a-production-application"></a>Partizionamento di un'applicazione di produzione

Concettualmente è facile vedere come funziona uno schema di partizionamento, ma qualsiasi schema di partizionamento aumenta la complessità del codice e introduce molte nuove complicazioni da gestire. Se si stanno muovendo immagini nell'archivio BLOB, cosa si può fare quando il servizio di archiviazione è inattivo? Modalità di gestione della sicurezza BLOB Cosa accade se il database e l'archiviazione BLOB non sono sincronizzati? Se si esegue il partizionamento orizzontale, come si gestiranno le query in tutti i database?

Le complicazioni sono gestibili a condizione che vengano pianificate prima di passare alla produzione. Molti utenti che non hanno fatto questo desiderio avrebbero avuto più tardi. In media, il team di consulenza clienti (CAT) prende il panico per una volta al mese dai clienti le cui app sono in un modo molto grande e non hanno eseguito questa pianificazione. E dicono qualcosa di simile a: "Help! Ho inserito tutto in un singolo archivio dati e, in 45 giorni, lo spazio su di esso verrà esaurito. Se si dispone di una grande logica di business incorporata nel modo in cui si accede all'archivio dati e si hanno clienti che usano l'app, non c'è molto tempo per passare a un giorno durante la migrazione. Ci stiamo impegnando per il tentativo di aiutare i clienti a partizionare i dati in tempo reale senza tempi di inattività. Si tratta di un'operazione molto interessante e inquietante e non è un elemento di cui si vuole essere a rischio se è possibile evitarlo! La valutazione di questo approccio e l'integrazione nell'app rende molto più semplice se l'app si espande in un secondo momento.

## <a name="summary"></a>Riepilogo

Uno schema di partizionamento efficace può consentire all'app cloud di ridimensionarsi in petabyte di dati nel cloud senza colli di bottiglia. E non devi pagare prima di tutto per computer di grandi dimensioni o un'infrastruttura completa, come potresti se Esegui l'app in un data center locale. Nel cloud è possibile aggiungere in modo incrementale la capacità in base alle esigenze e si paga solo per quanto riguarda l'utilizzo.

Nel [prossimo capitolo](unstructured-blob-storage.md) verrà illustrato come l'app Fix it implementi il partizionamento verticale archiviando immagini nell'archivio BLOB.

## <a name="resources"></a>Risorse

Per ulteriori informazioni sulle strategie di partizionamento, vedere le risorse seguenti.

Documentazione:

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi cloud di Microsoft Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper di Mark Simms e Michael Thomassy.
- [Modelli e procedure Microsoft: modelli di progettazione cloud](https://msdn.microsoft.com/library/dn568099.aspx). Vedere linee guida per il partizionamento dei dati, modello di partizionamento orizzontale.

Video

- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Vedere la discussione sul partizionamento nell'episodio 7.
- [Creazione di grandi dimensioni: lezioni apprese dai clienti di Microsoft Azure-parte i](https://channel9.msdn.com/Events/Build/2012/3-029). Mark Simms illustra gli schemi di partizionamento, le strategie di partizionamento orizzontale, come implementare il partizionamento orizzontale e le federazioni del database SQL, a partire da 19:49. In modo analogo alla serie failsafe, ma passa ad altre procedure dettagliate.

Codice di esempio:

- [Nozioni fondamentali sui servizi cloud in Windows Azure](https://code.msdn.microsoft.com/Cloud-Service-Fundamentals-4ca72649). Applicazione di esempio che include un database partizionato. Per una descrizione dello schema di partizionamento orizzontale implementato, vedere [dal-partizionamento orizzontale di RDBMS](https://blogs.msdn.com/b/windowsazure/archive/2013/09/05/dal-sharding-of-rdbms.aspx) nel Blog di Windows Azure.

> [!div class="step-by-step"]
> [Precedente](data-storage-options.md)
> [Successivo](unstructured-blob-storage.md)
