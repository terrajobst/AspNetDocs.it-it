---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Archiviazione BLOB non strutturata (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: f48b2be755b84dff9b2672bd348c73107602c6dd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617313"
---
# <a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Archiviazione BLOB non strutturata (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Nel capitolo precedente abbiamo esaminato gli schemi di partizionamento e illustrato come l'app Fix it archivia le immagini nel servizio BLOB del servizio di archiviazione di Azure e altri dati delle attività nel database SQL di Azure. In questo capitolo viene approfondito il servizio BLOB e viene illustrato come viene implementato nel codice del progetto Correggi it.

## <a name="what-is-blob-storage"></a>Informazioni sull'archiviazione BLOB

Il servizio BLOB del servizio di archiviazione di Azure fornisce un modo per archiviare i file nel cloud. Il servizio BLOB presenta diversi vantaggi rispetto all'archiviazione dei file in una rete locale file system:

- È altamente scalabile. Un singolo account di archiviazione può archiviare [centinaia di terabyte](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx)ed è possibile avere più account di archiviazione. Alcuni dei principali clienti di Azure archiviano centinaia di petabyte. Microsoft SkyDrive usa l'archivio BLOB.
- È durevole. Viene eseguito il backup automatico di tutti i file archiviati nel servizio BLOB.
- Fornisce disponibilità elevata. Il [contratto di contratto per l'archiviazione](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promette il tempo di incirca del 99,9% o del 99,99%, a seconda dell'opzione di ridondanza geografica scelta.
- Si tratta di una funzionalità di piattaforma distribuita come servizio (PaaS) di Azure, che significa che è sufficiente archiviare e recuperare i file, pagando solo per la quantità effettiva di archiviazione usata e Azure gestisce automaticamente la configurazione e la gestione di tutte le macchine virtuali e delle unità disco necessarie per il servizio.
- È possibile accedere al servizio BLOB usando un'API REST o un'API del linguaggio di programmazione. Gli SDK sono disponibili per .NET, Java, Ruby e altri.
- Quando si archivia un file nel servizio BLOB, è possibile renderlo facilmente disponibile pubblicamente tramite Internet.
- È possibile proteggere i file nel servizio BLOB in modo che possano accedervi solo da utenti autorizzati oppure è possibile fornire token di accesso temporanei che li rendano disponibili solo a un utente per un periodo di tempo limitato.

Ogni volta che si crea un'app per Azure e si vuole archiviare una grande quantità di dati in un ambiente locale, i file, ad esempio immagini, video, file PDF, fogli di calcolo e così via, si consideri il servizio BLOB.

## <a name="creating-a-storage-account"></a>Creazione di un account di archiviazione

Per iniziare a usare il servizio BLOB, creare un account di archiviazione in Azure. Nel portale fare clic su **nuovo** -- **servizi dati** -- **archiviazione** -- **creazione rapida**, quindi immettere un URL e un percorso Data Center. Il percorso di data center deve essere lo stesso dell'app Web.

![Creare un acct di archiviazione](unstructured-blob-storage/_static/image1.png)

Si sceglie l'area primaria in cui si vuole archiviare il contenuto e se si sceglie l'opzione di [replica geografica](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) , Azure crea le repliche di tutti i dati in un data center diverso in un'altra area del paese. Se, ad esempio, si sceglie il data center Stati Uniti occidentali, quando si archivia un file viene indirizzato agli Stati Uniti occidentali data center, ma in background Azure lo copia anche in uno degli altri Data Center statunitensi. Se si verifica un'emergenza in un'area del paese, i dati sono ancora sicuri.

Azure non replica i dati attraverso i confini geografici: se la località primaria è negli Stati Uniti, i file vengono replicati solo in un'altra area all'interno degli Stati Uniti; Se la località primaria è l'Australia, i file vengono replicati solo in un altro data center in Australia.

Naturalmente, è anche possibile creare un account di archiviazione eseguendo i comandi da uno script, come illustrato in precedenza. Ecco un comando di Windows PowerShell per creare un account di archiviazione:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Dopo aver creato un account di archiviazione, è possibile iniziare immediatamente a archiviare i file nel servizio BLOB.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Uso dell'archiviazione BLOB nell'app Correggi it

Il Fix it app consente di caricare foto.

![Creare un'attività Correggi it](unstructured-blob-storage/_static/image2.png)

Quando si fa clic su **Crea Fixit**, l'applicazione carica il file di immagine specificato e lo archivia nel servizio BLOB.

### <a name="set-up-the-blob-container"></a>Configurare il contenitore BLOB

Per archiviare un file nel servizio BLOB, è necessario un *contenitore* in cui archiviarlo. Un contenitore del servizio BLOB corrisponde a una cartella file system. Gli script di creazione dell'ambiente rivisti nel [capitolo automatizzare tutti gli elementi](automate-everything.md) creano l'account di archiviazione, ma non creano un contenitore. Lo scopo del metodo `CreateAndConfigure` della classe `PhotoService` consiste nel creare un contenitore, se non esiste già. Questo metodo viene chiamato dal metodo `Application_Start` in *Global. asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

Il nome e la chiave di accesso dell'account di archiviazione vengono archiviati nella raccolta `appSettings` del file *Web. config* e il codice nel metodo `StorageUtils.StorageAccount` utilizza tali valori per compilare una stringa di connessione e stabilire una connessione:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

Il metodo `CreateAndConfigureAsync` crea quindi un oggetto che rappresenta il servizio BLOB e un oggetto che rappresenta un contenitore (cartella) denominato "images" nel servizio BLOB:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Se un contenitore denominato "images" non esiste ancora, che sarà true la prima volta che si esegue l'app in un nuovo account di archiviazione, il codice crea il contenitore e imposta le autorizzazioni per renderlo pubblico. Per impostazione predefinita, i nuovi contenitori BLOB sono privati e sono accessibili solo agli utenti che dispongono dell'autorizzazione per accedere all'account di archiviazione.

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Archiviare la foto caricata nell'archivio BLOB

Per caricare e salvare il file di immagine, l'app usa un'interfaccia `IPhotoService` e un'implementazione dell'interfaccia nella classe `PhotoService`. Il file *Photoservice.cs* contiene tutto il codice presente nell'app Fix it che comunica con il servizio BLOB.

Il metodo del controller MVC seguente viene chiamato quando l'utente fa clic su **Crea Fixit**. In questo codice `photoService` fa riferimento a un'istanza della classe `PhotoService` e `fixittask` fa riferimento a un'istanza della classe di entità `FixItTask` che archivia i dati per una nuova attività.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

Il metodo `UploadPhotoAsync` nella classe `PhotoService` archivia il file caricato nel servizio BLOB e restituisce un URL che punta al nuovo BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Come nel metodo `CreateAndConfigure`, il codice si connette all'account di archiviazione e crea un oggetto che rappresenta il contenitore BLOB "images", ad eccezione del fatto che presuppone che il contenitore esista già.

Crea quindi un identificatore univoco per l'immagine che sta per essere caricata, concatenando un nuovo valore GUID con l'estensione di file:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

Il codice usa quindi l'oggetto contenitore BLOB e il nuovo identificatore univoco per creare un oggetto BLOB, imposta un attributo su tale oggetto che indica il tipo di file e quindi usa l'oggetto BLOB per archiviare il file nell'archivio BLOB.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Infine, ottiene un URL che fa riferimento al BLOB. Questo URL viene archiviato nel database e può essere utilizzato in Correggi pagine Web per visualizzare l'immagine caricata.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Questo URL viene archiviato nel database come una delle colonne della tabella FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Con solo l'URL nel database e le immagini nell'archivio BLOB, l'app per la correzione consente di mantenere il database piccolo, scalabile ed economico, mentre le immagini vengono archiviate in cui l'archiviazione è economica e in grado di gestire terabyte o petabyte. Un account di archiviazione può archiviare centinaia di terabyte di foto di correzione e si paga solo per le risorse usate. È quindi possibile iniziare con un numero ridotto di 9 centesimi per il primo gigabyte e aggiungere altre immagini per le penne per ogni gigabyte aggiuntivo.

### <a name="display-the-uploaded-file"></a>Visualizza il file caricato

Per correggere l'applicazione, viene visualizzato il file di immagine caricato quando vengono visualizzati i dettagli per un'attività.

![Correggi dettagli attività IT con la foto](unstructured-blob-storage/_static/image3.png)

Per visualizzare l'immagine, è necessario che tutte le visualizzazioni MVC includano il valore `PhotoUrl` nel codice HTML inviato al browser. Il server Web e il database non usano cicli per visualizzare l'immagine, ma solo pochi byte per l'URL dell'immagine. Nel codice Razor seguente `Model` fa riferimento a un'istanza della classe `FixItTask` Entity.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Se si esamina il codice HTML della pagina visualizzata, viene visualizzato l'URL che punta direttamente all'immagine nell'archivio BLOB, simile alla seguente:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Riepilogo

Si è visto come l'app Fix it archivia le immagini nel servizio BLOB e solo gli URL di immagine nel database SQL. L'uso del servizio BLOB consente di mantenere il database SQL molto più piccolo rispetto a quanto altrimenti sarebbe possibile scalare fino a un numero quasi illimitato di attività e può essere eseguito senza scrivere molto codice.

È possibile disporre di centinaia di terabyte in un account di archiviazione e il costo di archiviazione è molto meno costoso rispetto all'archiviazione del database SQL, a partire da circa 3 centesimi per Gigabyte al mese più una piccola tariffa per le transazioni. Tenere presente che non si paga per la capacità massima, ma solo per la quantità effettivamente archiviata, quindi l'app è pronta per la scalabilità, ma non si paga per tutta la capacità aggiuntiva.

Nel [prossimo capitolo](design-to-survive-failures.md) parleremo dell'importanza di rendere un'app cloud in grado di gestire correttamente gli errori.

## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le risorse seguenti:

- [Introduzione all'archiviazione BLOB di Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog di Mike Wood.
- [Come usare il servizio di archiviazione BLOB di Azure in .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentazione ufficiale sul sito MicrosoftAzure.com. Una breve introduzione all'archiviazione BLOB seguita da esempi di codice che illustrano come connettersi all'archiviazione BLOB, creare contenitori, caricare e scaricare BLOB e così via.
- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Presenta concetti di alto livello e principi architetturali in modo molto accessibile e interessante, con storie tratte dall'esperienza del team di consulenza clienti Microsoft (CAT) con i clienti effettivi. Per informazioni sul servizio di archiviazione di Azure e sui BLOB, vedere Episode 5 a partire da 35:13.
- [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere modello di chiave del posteggiatore.

> [!div class="step-by-step"]
> [Precedente](data-partitioning-strategies.md)
> [Successivo](design-to-survive-failures.md)
