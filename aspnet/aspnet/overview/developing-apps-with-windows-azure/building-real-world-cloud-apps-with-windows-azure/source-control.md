---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: Controllo del codice sorgente (compilazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5a1e0d7cd3c396d4be79c8958422602055eb3db1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457102"
---
# <a name="source-control-building-real-world-cloud-apps-with-azure"></a>Controllo del codice sorgente (compilazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Il controllo del codice sorgente è essenziale per tutti i progetti di sviluppo cloud, non solo per gli ambienti team. Non è possibile modificare il codice sorgente o persino un documento di Word senza una funzione di annullamento e backup automatici e il controllo del codice sorgente offre tali funzioni a livello di progetto, in cui è possibile risparmiare ancora più tempo quando si verifica un errore. Con i servizi di controllo del codice sorgente cloud, non è più necessario preoccuparsi della configurazione complicata ed è possibile usare Azure Repos controllo del codice sorgente gratuitamente per un massimo di 5 utenti.

La prima parte di questo capitolo illustra tre procedure consigliate principali da tenere presente:

- [Considerare gli script di automazione come codice sorgente](#scripts) e la relativa versione insieme al codice dell'applicazione.
- [Non archiviare mai i segreti](#secrets) (dati sensibili, ad esempio le credenziali) in un repository del codice sorgente.
- [Configurare i rami di origine](#devops) per abilitare il flusso di lavoro DevOps.

Il resto del capitolo fornisce alcune implementazioni di esempio di questi modelli in Visual Studio, Azure e Azure Repos:

- [Aggiungere script al controllo del codice sorgente in Visual Studio](#vsscripts)
- [Archiviare dati sensibili in Azure](#appsettings)
- [Usare git in Visual Studio e Azure Repos](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Considera gli script di automazione come codice sorgente

Quando si lavora su un progetto cloud, si modificano spesso le cose e si vuole essere in grado di reagire rapidamente ai problemi segnalati dai clienti. Rispondere rapidamente comporta l'uso di script di automazione, come illustrato nel capitolo [automatizzare tutti gli elementi](automate-everything.md) . Tutti gli script usati per creare l'ambiente, distribuirli, ridimensionarli e così via, devono essere sincronizzati con il codice sorgente dell'applicazione.

Per conservare gli script sincronizzati con il codice, archiviarli nel sistema di controllo del codice sorgente. Quindi, se è necessario eseguire il rollback delle modifiche o apportare una correzione rapida al codice di produzione, che è diverso dal codice di sviluppo, non è necessario sprecare tempo per individuare le impostazioni modificate o i membri del team che hanno copie della versione necessaria. Si ha la certezza che gli script necessari siano sincronizzati con la codebase necessaria per e si ha la certezza che tutti i membri del team stiano lavorando con gli stessi script. Se è necessario automatizzare i test e la distribuzione di una correzione a caldo per la produzione o per lo sviluppo di nuove funzionalità, sarà necessario lo script corretto per il codice che deve essere aggiornato.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>Non archiviare segreti

Un repository di codice sorgente è generalmente accessibile a un numero eccessivo di persone affinché sia un luogo protetto in modo appropriato per i dati sensibili, ad esempio le password. Se gli script si basano su segreti come le password, parametrizzare tali impostazioni in modo che non vengano salvate nel codice sorgente e memorizzino i segreti altrove.

Azure, ad esempio, consente di scaricare i file che contengono le impostazioni di pubblicazione per automatizzare la creazione dei profili di pubblicazione. Questi file includono i nomi utente e le password autorizzati a gestire i servizi di Azure. Se si usa questo metodo per creare profili di pubblicazione e se si archiviano questi file nel controllo del codice sorgente, chiunque abbia accesso al repository potrà visualizzare tali nomi utente e password. È possibile archiviare in modo sicuro la password nel profilo di pubblicazione perché è crittografata e si trova in un file con *estensione pubxml. User* che, per impostazione predefinita, non è incluso nel controllo del codice sorgente.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Strutturare rami di origine per semplificare il flusso di lavoro DevOps

La modalità di implementazione dei rami nel repository influiscono sulla possibilità di sviluppare nuove funzionalità e risolvere i problemi nell'ambiente di produzione. Di seguito è riportato un modello utilizzato da numerosi team di medie dimensioni:

![Struttura del ramo di origine](source-control/_static/image1.png)

Il ramo master corrisponde sempre al codice in produzione. I rami al di sotto del Master corrispondono a fasi diverse del ciclo di vita dello sviluppo. Il ramo Development è il punto in cui vengono implementate nuove funzionalità. Per un piccolo team è possibile avere solo master e sviluppo, ma spesso è consigliabile che gli utenti abbiano un ramo di staging tra sviluppo e master. È possibile usare la gestione temporanea per i test di integrazione finali prima che un aggiornamento venga spostato in produzione.

Per i Big team possono essere presenti rami separati per ogni nuova funzionalità. per un team più piccolo, è possibile che tutti gli utenti possano archiviare il ramo di sviluppo.

Se si dispone di un ramo per ogni funzionalità, quando la funzionalità A è pronta, le modifiche al codice sorgente vengono unite nel ramo di sviluppo e negli altri rami della funzionalità. Questo processo di Unione del codice sorgente può richiedere molto tempo e, per evitare tale problema, mantenendo le funzionalità separate, alcuni team implementano un'alternativa denominata funzioni di *[alternanza](http://en.wikipedia.org/wiki/Feature_toggle)* (note anche come *flag funzionalità*). Ciò significa che tutto il codice per tutte le funzionalità si trova nello stesso ramo, ma si Abilita o disabilita ogni funzionalità usando le opzioni nel codice. Si supponga, ad esempio, che la funzionalità A sia un nuovo campo per la correzione delle attività dell'app e che la funzionalità B aggiunga la funzionalità di memorizzazione nella cache. Il codice per entrambe le funzionalità può essere nel ramo di sviluppo, ma l'app visualizzerà il nuovo campo solo quando una variabile è impostata su true e utilizzerà la memorizzazione nella cache solo quando una variabile diversa è impostata su true. Se la funzionalità A non è pronta per essere innalzata di livello, ma la funzionalità B è pronta, è possibile alzare di livello tutto il codice nell'ambiente di produzione con la funzionalità A disattivazione e funzionalità B attivata. È quindi possibile completare la funzionalità A e innalzarla di livello in un secondo momento, senza unire codice sorgente.

Indipendentemente dal fatto che si usino o meno rami per le funzionalità, una struttura di branching come questa consente di passare il codice dallo sviluppo alla produzione in modo agile e ripetibile.

Questa struttura consente inoltre di reagire rapidamente ai commenti dei clienti. Se è necessario eseguire una correzione rapida per la produzione, è anche possibile eseguire questa operazione in modo efficiente in modo agile. È possibile creare un ramo fuori dal master o dalla gestione temporanea e, quando è pronto, eseguirne il merge nel database master e nei rami di sviluppo e funzionalità.

![ramo hotfix](source-control/_static/image2.png)

Senza una struttura di branching come questa, con la separazione dei rami di produzione e di sviluppo, un problema di produzione può comportare la necessità di innalzare di livello il nuovo codice della funzionalità insieme alla correzione di produzione. Il nuovo codice della funzionalità potrebbe non essere completamente testato e pronto per la produzione e potrebbe essere necessario eseguire numerose operazioni di backup delle modifiche che non sono pronte. In alternativa, potrebbe essere necessario ritardare la correzione per testare le modifiche e prepararle per la distribuzione.

Verranno ora illustrati alcuni esempi di come implementare questi tre modelli in Visual Studio, Azure e Azure Repos. Si tratta di esempi piuttosto che istruzioni dettagliate per la procedura da eseguire. per istruzioni dettagliate che forniscono tutto il contesto necessario, vedere la sezione relativa alle [risorse](#resources) alla fine del capitolo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Aggiungere script al controllo del codice sorgente in Visual Studio

È possibile aggiungere script al controllo del codice sorgente in Visual Studio, inserendoli in una cartella della soluzione di Visual Studio (presupponendo che il progetto sia nel controllo del codice sorgente). Ecco un modo per eseguire questa operazione.

Creare una cartella per gli script nella cartella della soluzione (la stessa cartella che contiene il file con *estensione sln* ).

![Cartella di automazione](source-control/_static/image3.png)

Copiare i file di script nella cartella.

![Contenuto della cartella di automazione](source-control/_static/image4.png)

In Visual Studio aggiungere una cartella della soluzione al progetto.

![Selezione menu nuova cartella soluzione](source-control/_static/image5.png)

E aggiungere i file di script alla cartella della soluzione.

![Aggiungi selezione menu elemento esistente](source-control/_static/image6.png)

![Aggiungi elemento esistente - finestra di dialogo](source-control/_static/image7.png)

I file script sono ora inclusi nel progetto e il controllo del codice sorgente tiene traccia delle modifiche della versione insieme alle modifiche del codice sorgente corrispondenti.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Archiviare dati sensibili in Azure

Se si esegue l'applicazione in un sito Web di Azure, un modo per evitare di archiviare le credenziali nel controllo del codice sorgente consiste nell'archiviarli in Azure.

Ad esempio, l'applicazione Fix it archivia nel file Web. config due stringhe di connessione che avranno password in produzione e una chiave che consente l'accesso all'account di archiviazione di Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Se si inseriscono i valori di produzione effettivi per queste impostazioni nel file *Web. config* o si inseriscono tali valori nel file *Web. Release. config* per configurare una trasformazione Web. config in modo da inserirli durante la distribuzione, questi verranno archiviati nel repository di origine. Se si immettono le stringhe di connessione al database nel profilo di pubblicazione di produzione, la password sarà presente nel file con *estensione pubxml* . È possibile escludere il file con *estensione pubxml* dal controllo del codice sorgente, ma si perde il vantaggio di condividere tutte le altre impostazioni di distribuzione.

Azure offre un'alternativa per le sezioni **appSettings** e stringhe di connessione del file *Web. config* . Di seguito è illustrata la parte pertinente della scheda **configurazione** per un sito Web nel portale di gestione di Azure:

![appSettings e connectionStrings nel portale](source-control/_static/image8.png)

Quando si distribuisce un progetto in questo sito Web e l'applicazione viene eseguita, tutti i valori archiviati in Azure sostituiscono tutti i valori presenti nel file Web. config.

È possibile impostare questi valori in Azure usando il portale di gestione o gli script. Lo script di automazione per la creazione dell'ambiente visualizzato nel capitolo [automatizzare tutto](automate-everything.md) crea un database SQL di Azure, ottiene le stringhe di connessione del database SQL e di archiviazione e archivia questi segreti nelle impostazioni per il sito Web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Si noti che gli script sono parametrizzati in modo che i valori effettivi non vengano salvati in modo permanente nel repository di origine.

Quando si esegue in locale nell'ambiente di sviluppo, l'app legge il file Web. config locale e la stringa di connessione fa riferimento a un database SQL Server database locale nella cartella *app\_data* del progetto Web. Quando si esegue l'app in Azure e l'app tenta di leggere questi valori dal file Web. config, cosa si ottiene e USA sono i valori archiviati per il sito Web, non il file Web. config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Usare git in Visual Studio e Azure DevOps

È possibile usare qualsiasi ambiente del controllo del codice sorgente per implementare la struttura di diramazione DevOps presentata in precedenza. Per i team distribuiti, un [sistema di controllo della versione distribuito](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) potrebbe funzionare meglio; per gli altri team un [sistema centralizzato](http://en.wikipedia.org/wiki/Revision_control) potrebbe funzionare meglio.

[Git](http://git-scm.com/) è un sistema di controllo della versione distribuito diffuso. Quando si usa Git per il controllo del codice sorgente, si ha una copia completa del repository con tutta la relativa cronologia nel computer locale. Molti utenti preferiscono questo perché è più semplice continuare a lavorare quando non si è connessi alla rete: è possibile continuare a eseguire commit e rollback, creare e cambiare rami e così via. Anche quando si è connessi alla rete, è più facile e veloce creare rami e cambiare ramo quando tutto è locale. È anche possibile eseguire commit e rollback locali senza avere un effetto su altri sviluppatori. È possibile eseguire il commit in batch prima di inviarli al server.

[Azure Repos](/azure/devops/repos/index?view=vsts) offre sia [git](/azure/devops/repos/git/?view=vsts) che [controllo della versione di Team Foundation](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; controllo del codice sorgente centralizzato). Per iniziare a usare Azure DevOps, vedere [qui](https://app.vsaex.visualstudio.com/signup).

Visual Studio 2017 include il [supporto Git](https://msdn.microsoft.com/library/hh850437.aspx)integrato di prima classe. Ecco una rapida dimostrazione del funzionamento.

Con un progetto aperto in Visual Studio, fare clic con il pulsante destro del mouse sulla soluzione in **Esplora soluzioni**, quindi scegliere **Aggiungi soluzione al controllo del codice sorgente**.

![Aggiungi soluzione al controllo del codice sorgente](source-control/_static/image9.png)

Visual Studio chiede se si vuole usare TFVC (controllo della versione centralizzato) o Git.

![Scegliere il controllo del codice sorgente](source-control/_static/image10.png)

Quando si seleziona git e si fa clic su **OK**, in Visual Studio viene creato un nuovo repository git locale nella cartella della soluzione. Il nuovo repository non contiene ancora file; è necessario aggiungerli al repository eseguendo un commit Git. Fare clic con il pulsante destro del mouse sulla soluzione in **Esplora soluzioni**, quindi scegliere Esegui **commit**.

![Commit](source-control/_static/image11.png)

Visual Studio crea automaticamente il commit di tutti i file di progetto per il commit e li elenca in **Team Explorer** nel riquadro **modifiche incluse** . Se sono presenti alcuni elementi che non si desidera includere nel commit, è possibile selezionarli, fare clic con il pulsante destro del mouse e fare clic su **Escludi**.

![Team Explorer](source-control/_static/image12.png)

Immettere un commento di commit e fare clic su **commit**. in Visual Studio viene eseguito il commit e viene visualizzato l'ID commit.

![Team Explorer modifiche](source-control/_static/image13.png)

A questo punto, se si modifica il codice in modo che sia diverso da quello presente nel repository, è possibile visualizzare facilmente le differenze. Fare clic con il pulsante destro del mouse su un file che è stato modificato, scegliere **Confronta con non modificato**e ottenere una visualizzazione del confronto che mostra la modifica di cui non è stato eseguito il commit.

![Confronta con non modificato](source-control/_static/image14.png)

![Differenze nella visualizzazione delle modifiche](source-control/_static/image15.png)

È possibile visualizzare facilmente le modifiche che vengono apportate e archiviarle.

Si supponga di dover creare un ramo: è possibile farlo anche in Visual Studio. In **Team Explorer**fare clic su **nuovo ramo**.

![Team Explorer nuovo ramo](source-control/_static/image16.png)

Immettere un nome di ramo, fare clic su **Crea ramo**. se è stato selezionato **Estrai ramo**, Visual Studio estrae automaticamente il nuovo ramo.

![Team Explorer nuovo ramo](source-control/_static/image17.png)

È ora possibile apportare modifiche ai file e archiviarli in questo branch. È possibile passare facilmente tra i rami e Visual Studio sincronizza automaticamente i file in qualsiasi ramo Estratto. In questo esempio il titolo della pagina Web in *\_layout. cshtml* è stato modificato in "Hot Fix 1" nel ramo Hotfix1.

![Ramo Hotfix1](source-control/_static/image18.png)

Se si passa di nuovo al ramo master, il contenuto del file *\_layout. cshtml* ripristina automaticamente il contenuto del ramo master.

![Ramo master](source-control/_static/image19.png)

Questo è un semplice esempio di come è possibile creare rapidamente un ramo e capovolgersi tra i rami. Questa funzionalità consente un flusso di lavoro altamente agile usando la struttura del ramo e gli script di automazione presentati nel capitolo [automatizzare tutti gli elementi](automate-everything.md) . È possibile, ad esempio, lavorare nel ramo di sviluppo, creare un ramo di correzione a caldo fuori dal master, passare al nuovo ramo, apportare le modifiche ed eseguirne il commit, quindi tornare al ramo di sviluppo e continuare l'operazione.

In questo articolo viene illustrato come usare un repository git locale in Visual Studio. In un ambiente team si esegue in genere anche il push delle modifiche in un repository comune. Gli strumenti di Visual Studio consentono inoltre di puntare a un repository GIT remoto. È possibile usare GitHub.com a tale scopo oppure è possibile usare [git e Azure Repos](/azure/devops/repos/git/overview?view=vsts) integrati con tutte le altre funzionalità di DevOps di Azure, ad esempio il rilevamento di elementi di lavoro e bug.

Questo non è l'unico modo per implementare una strategia di branching agile, ovviamente. È possibile abilitare lo stesso flusso di lavoro Agile usando un repository centralizzato del controllo del codice sorgente.

## <a name="summary"></a>Riepilogo

Misurare il successo del sistema di controllo del codice sorgente in base alla velocità con cui è possibile apportare una modifica e renderla disponibile in modo sicuro e prevedibile. Se ci si accorge di aver apportato una modifica perché è necessario eseguire un giorno o due di test manuali, è possibile chiedersi quali siano i requisiti necessari per eseguire il processo o il testing in modo che sia possibile apportare la modifica in pochi minuti o non più di un'ora. Una strategia per eseguire questa operazione consiste nell'implementare l'integrazione continua e il recapito continuo, che verranno illustrati nel [capitolo successivo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per ulteriori informazioni sulle strategie di diramazione, vedere le risorse seguenti:

- [Compilazione di una pipeline di versione con Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentazione di Microsoft Patterns and Practices. Vedere il capitolo 6 per una discussione sulle strategie di branching. La funzionalità Advocates Visualizza i rami delle funzionalità e, in caso di utilizzo di rami per le funzionalità, sostiene di mantenerli di breve durata (ore o giorni al massimo).
- [Guida al controllo della versione](https://aka.ms/vsarsolutions). Guida alle strategie di diramazione da ALM Rangers. Vedere branching Strategies. pdf nella scheda Downloads.
- [Funzionalità per lo sviluppo di software con funzionalità](https://msdn.microsoft.com/magazine/dn683796.aspx). Articolo di MSDN Magazine.
- [Interruttore della funzionalità](http://martinfowler.com/bliki/FeatureToggle.html). Introduzione ai flag di funzionalità/Attiva/disattivazione nel Blog di Martin Fowler.
- [Funzionalità](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx)consente di disabilitare i rami delle funzionalità di Visual Studio. Un altro post di Blog sugli interruttori delle funzionalità, di Dylan Smith.

Per ulteriori informazioni sulla gestione di informazioni riservate che non devono essere mantenute nei repository del controllo del codice sorgente, vedere le risorse seguenti:

- [Procedure consigliate per la distribuzione di password e altri dati sensibili a ASP.NET e al servizio app Azure](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Siti Web di Azure: funzionamento delle stringhe delle applicazioni e delle stringhe di connessione](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Viene illustrata la funzionalità di Azure che sostituisce `appSettings` e `connectionStrings` i dati nel file *Web. config* .
- [Impostazioni di configurazione e applicazione personalizzate in siti Web di Azure-con Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Per informazioni su altri metodi per mantenere le informazioni riservate fuori dal controllo del codice sorgente, vedere [ASP.NET MVC: Mantieni le impostazioni private dal controllo del codice sorgente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Precedente](automate-everything.md)
> [Successivo](continuous-integration-and-continuous-delivery.md)
