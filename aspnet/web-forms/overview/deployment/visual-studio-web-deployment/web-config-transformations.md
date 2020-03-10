---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Distribuzione Web ASP.NET con Visual Studio: trasformazioni di file Web. config | Microsoft Docs'
author: tdykstra
description: Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET per app Azure servizio app Web o un provider di hosting di terze parti, da usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632832"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Distribuzione Web ASP.NET con Visual Studio: trasformazioni di file Web. config

di [Tom Dykstra](https://github.com/tdykstra)

[Scarica progetto Starter](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Questa serie di esercitazioni illustra come distribuire (pubblicare) un'applicazione Web ASP.NET in app Web di servizio app Azure o in un provider di hosting di terze parti, usando Visual Studio 2012 o Visual Studio 2010. Per informazioni sulla serie, vedere [la prima esercitazione della serie](introduction.md).

## <a name="overview"></a>Panoramica

In questa esercitazione viene illustrato come automatizzare il processo di modifica del file *Web. config* quando lo si distribuisce in ambienti di destinazione diversi. Per la maggior parte delle applicazioni sono presenti impostazioni nel file *Web. config* che devono essere diverse quando si distribuisce l'applicazione. L'automazione del processo di modifica di queste modifiche impedisce di eseguire manualmente le modifiche ogni volta che si distribuisce, il che sarebbe noioso e soggetto a errori.

Promemoria: se si riceve un messaggio di errore o un elemento non funziona durante l'esercitazione, assicurarsi di controllare la pagina relativa alla [risoluzione dei problemi](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Trasformazioni di Web. config rispetto ai parametri di Distribuzione Web

Esistono due modi per automatizzare il processo di modifica delle impostazioni del file *Web. config* : le [trasformazioni di Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e i [parametri del distribuzione Web](https://msdn.microsoft.com/library/ff398068.aspx). Un file di trasformazione *Web. config* contiene markup XML che specifica come modificare il file *Web. config* quando viene distribuito. È possibile specificare modifiche diverse per configurazioni di compilazione specifiche e per profili di pubblicazione specifici. Le configurazioni di compilazione predefinite sono debug e release ed è possibile creare configurazioni di compilazione personalizzate. Un profilo di pubblicazione corrisponde in genere a un ambiente di destinazione. Per ulteriori informazioni sui profili di pubblicazione, vedere l'esercitazione [distribuzione in IIS come ambiente di test](deploying-to-iis.md) .

È possibile utilizzare i parametri Distribuzione Web per specificare molti tipi diversi di impostazioni che devono essere configurate durante la distribuzione, incluse le impostazioni presenti nei file *Web. config* . Quando viene utilizzato per specificare le modifiche al file *Web. config* , i parametri distribuzione Web sono più complessi da configurare, ma sono utili quando non si conosce il valore da impostare fino a quando non si esegue la distribuzione. In un ambiente aziendale, ad esempio, è possibile creare un *pacchetto di distribuzione* e assegnarlo a una persona del reparto IT per l'installazione in produzione e tale utente deve essere in grado di immettere le stringhe di connessione o le password che non si conoscono.

Per lo scenario descritto in questa serie di esercitazioni, si sa in anticipo tutto ciò che è necessario eseguire nel file *Web. config* , pertanto non è necessario usare distribuzione Web parametri. Verranno configurate alcune trasformazioni che variano a seconda della configurazione di compilazione utilizzata e altre che variano a seconda del profilo di pubblicazione usato.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Specifica delle impostazioni di Web. config in Azure

Se le impostazioni del file *Web. config* che si desidera modificare si trovano nell'elemento `<connectionStrings>` o `<appSettings>` e se si esegue la distribuzione in app Web nel servizio app Azure, è possibile automatizzare le modifiche durante la distribuzione. È possibile immettere le impostazioni che si desidera rendere effettive in Azure nella scheda **Configura** della pagina del portale di gestione per l'app Web (scorrere verso il basso fino alle sezioni **impostazioni app** e **stringhe di connessione** ). Quando si distribuisce il progetto, Azure applica automaticamente le modifiche. Per ulteriori informazioni, vedere [siti Web di Microsoft Azure: funzionamento delle stringhe delle applicazioni e delle stringhe di connessione](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>File di trasformazione predefiniti

In **Esplora soluzioni**espandere *Web. config* per visualizzare i file di trasformazione *Web. debug. config* e *Web. Release. config* che vengono creati per impostazione predefinita per le due configurazioni di compilazione predefinite.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

È possibile creare file di trasformazione per le configurazioni di compilazione personalizzate facendo clic con il pulsante destro del mouse sul file Web. config e scegliendo **Aggiungi trasformazioni di configurazione** dal menu di scelta rapida. Per questa esercitazione non è necessario eseguire questa operazione e l'opzione di menu è disabilitata perché non sono state create configurazioni di compilazione personalizzate.

Successivamente verranno creati altri tre file di trasformazione, uno per i profili di pubblicazione di test, di staging e di produzione. Un esempio tipico di un'impostazione che è possibile gestire in un file di trasformazione del profilo di pubblicazione perché dipende dall'ambiente di destinazione è un endpoint WCF diverso per i test e la produzione. Verranno creati file di trasformazione del profilo di pubblicazione nelle esercitazioni successive dopo aver creato i profili di pubblicazione con cui vengono creati.

## <a name="disable-debug-mode"></a>Disabilitare la modalità di debug

Un esempio di impostazione che dipende dalla configurazione della build anziché dall'ambiente di destinazione è l'attributo `debug`. Per una build di rilascio, in genere si vuole disabilitare il debug indipendentemente dall'ambiente in cui si esegue la distribuzione. Per impostazione predefinita, i modelli di progetto di Visual Studio creano quindi i file di trasformazione *Web. Release. config* con il codice che rimuove l'attributo `debug` dall'elemento `compilation`. Di seguito è riportato il *file Web. Release. config*predefinito: oltre ad alcuni esempi di codice di trasformazione impostati come commento, include il codice nell'elemento `compilation` che rimuove l'attributo `debug`:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

L'attributo `xdt:Transform="RemoveAttributes(debug)"` specifica che si desidera che l'attributo `debug` venga rimosso dall'elemento `system.web/compilation` nel file *Web. config* distribuito. Questa operazione verrà eseguita ogni volta che si distribuisce una build di rilascio.

## <a name="limit-error-log-access-to-administrators"></a>Limita l'accesso ai log degli errori agli amministratori

Se si verifica un errore durante l'esecuzione dell'applicazione, l'applicazione visualizza una pagina di errore generica al posto della pagina di errore generata dal sistema e usa il [pacchetto NuGet ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) per la registrazione e la creazione di report degli errori. L'elemento `customErrors` nel file *Web. config* dell'applicazione specifica la pagina di errore:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Per visualizzare la pagina di errore, modificare temporaneamente l'attributo `mode` dell'elemento `customErrors` da "RemoteOnly" a "on" ed eseguire l'applicazione da Visual Studio. Generare un errore richiedendo un URL non valido, ad esempio *Studentsxxx. aspx*. Anziché una pagina di errore "Impossibile trovare la risorsa", viene visualizzata la pagina *pagina GenericErrorPage. aspx* .

![Pagina di errore](web-config-transformations/_static/image2.png)

Per visualizzare il log degli errori, sostituire tutti gli elementi nell'URL dopo il numero di porta con *ELMAH. axd* (ad esempio, `http://localhost:51130/elmah.axd`) e premere INVIO:

![Pagina ELMAH](web-config-transformations/_static/image3.png)

Al termine, non dimenticare di impostare nuovamente l'elemento `customErrors` sulla modalità "RemoteOnly".

Nel computer di sviluppo è consigliabile consentire l'accesso gratuito alla pagina log degli errori, ma in produzione si tratta di un rischio per la sicurezza. Per il sito di produzione, è necessario aggiungere una regola di autorizzazione che limita l'accesso ai log degli errori agli amministratori e per assicurarsi che la restrizione funzioni anche in test e staging. Si tratta quindi di un'altra modifica che si desidera implementare ogni volta che si distribuisce una build di rilascio e che quindi appartiene al file *Web. Release. config* .

Aprire *Web. Release. config* e aggiungere un nuovo elemento `location` immediatamente prima del tag di chiusura `configuration`, come illustrato di seguito.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

Il valore dell'attributo `Transform` "Insert" comporta l'aggiunta di questo elemento `location` come elemento di pari livello a tutti gli elementi `location` esistenti nel file *Web. config* . Esiste già un elemento `location` che specifica le regole di autorizzazione per la pagina di **aggiornamento crediti** .

A questo punto è possibile visualizzare in anteprima la trasformazione per assicurarsi che sia stata codificata correttamente.

In **Esplora soluzioni**fare clic con il pulsante destro del mouse su *Web. Release. config* e scegliere **Anteprima trasformazione**.

![Menu trasforma anteprima](web-config-transformations/_static/image4.png)

Verrà visualizzata una pagina che mostra il file *Web. config* di sviluppo a sinistra e il file *Web. config* distribuito, con le modifiche evidenziate.

![Anteprima della trasformazione debug](web-config-transformations/_static/image5.png)

![Anteprima della trasformazione percorso](web-config-transformations/_static/image6.png)

(Nell'anteprima è possibile notare alcune modifiche aggiuntive per le quali non sono state scritte le trasformazioni): questi comportano in genere la rimozione di spazi vuoti che non influiscono sulla funzionalità.

Quando si esegue il test del sito dopo la distribuzione, verrà testato anche per verificare che la regola di autorizzazione sia valida.

> [!NOTE] 
> 
> **Nota sulla sicurezza** Non visualizzare mai i dettagli dell'errore al pubblico in un'applicazione di produzione oppure archiviare le informazioni in un percorso pubblico. Gli utenti malintenzionati possono usare le informazioni sugli errori per individuare le vulnerabilità in un sito. Se si usa ELMAH nella propria applicazione, configurare ELMAH per ridurre al minimo i rischi per la sicurezza. L'esempio ELMAH in questa esercitazione non deve essere considerato una configurazione consigliata. Si tratta di un esempio che è stato scelto per illustrare come gestire una cartella in cui l'applicazione deve essere in grado di creare file. Per ulteriori informazioni, vedere [protezione dell'endpoint ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Impostazione che verrà gestita nei file di trasformazione del profilo di pubblicazione

Uno scenario comune consiste nel disporre di impostazioni del file *Web. config* che devono essere diverse in ogni ambiente in cui si esegue la distribuzione. Ad esempio, un'applicazione che chiama un servizio WCF potrebbe avere bisogno di un endpoint diverso negli ambienti di test e di produzione. L'applicazione Contoso University include anche un'impostazione di questo tipo. Questa impostazione controlla un indicatore visibile sulle pagine di un sito che indica l'ambiente in cui si trova, ad esempio sviluppo, test o produzione. Il valore dell'impostazione determina se l'applicazione aggiungerà "(dev)" o "(test)" all'intestazione principale della pagina master del *sito. master* :

![Indicatore ambiente](web-config-transformations/_static/image7.png)

L'indicatore di ambiente viene omesso quando l'applicazione è in esecuzione in gestione temporanea o in produzione.

Le pagine Web di Contoso University leggono un valore impostato in `appSettings` nel file *Web. config* per determinare l'ambiente in cui l'applicazione è in esecuzione:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

Il valore deve essere "test" nell'ambiente di test e "prod" per la gestione temporanea e la produzione.

Il codice seguente in un file di trasformazione implementerà questa trasformazione:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

Il valore dell'attributo `xdt:Transform` "SetAttributes" indica che lo scopo di questa trasformazione è modificare i valori di attributo di un elemento esistente nel file *Web. config* . Il valore dell'attributo `xdt:Locator` "Match (Key)" indica che l'elemento da modificare è quello il cui attributo `key` corrisponde all'attributo `key` specificato qui. L'unico altro attributo dell'elemento `add` è `value`ed è ciò che verrà modificato nel file *Web. config* distribuito. Il codice riportato di seguito fa in modo che l'attributo `value` dell'elemento `Environment` `appSettings` sia impostato su "test" nel file *Web. config* distribuito.

Questa trasformazione appartiene ai file di trasformazione del profilo di pubblicazione, che non sono stati ancora creati. Verranno creati e aggiornati i file di trasformazione che implementano questa modifica quando si creano i profili di pubblicazione per gli ambienti di test, di gestione temporanea e di produzione. Questa operazione verrà eseguita nelle esercitazioni [per la distribuzione in IIS](deploying-to-iis.md) e la [distribuzione in produzione](deploying-to-production.md) .

> [!NOTE]
> Poiché questa impostazione si trova nell'elemento `<appSettings>`, è possibile specificare un'altra alternativa per specificare la trasformazione durante la distribuzione in app Web nel servizio app Azure vedere [specifica delle impostazioni di Web. config in Azure](#watransforms) in precedenza in questo argomento.

## <a name="setting-connection-strings"></a>Impostazione delle stringhe di connessione

Sebbene il file di trasformazione predefinito contenga un esempio in cui viene illustrato come aggiornare una stringa di connessione, nella maggior parte dei casi non è necessario impostare le trasformazioni delle stringhe di connessione, in quanto è possibile specificare le stringhe di connessione nel profilo di pubblicazione. Questa operazione verrà eseguita nelle esercitazioni [per la distribuzione in IIS](deploying-to-iis.md) e la [distribuzione in produzione](deploying-to-production.md) .

## <a name="summary"></a>Riepilogo

A questo punto, è possibile eseguire le operazioni necessarie per le trasformazioni di *Web. config* prima di creare i profili di pubblicazione e si è verificata un'anteprima di ciò che verrà visualizzato nel file Web. config distribuito.

![Anteprima della trasformazione percorso](web-config-transformations/_static/image8.png)

Nell'esercitazione seguente verranno gestite le attività di configurazione della distribuzione che richiedono l'impostazione delle proprietà del progetto.

## <a name="more-information"></a>Altre informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, vedere [utilizzo delle trasformazioni di Web. config per modificare le impostazioni nel file Web. config o nel file app. config di destinazione durante la distribuzione](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) nella mappa del contenuto della distribuzione Web per Visual Studio e ASP.NET.

> [!div class="step-by-step"]
> [Precedente](preparing-databases.md)
> [Successivo](project-properties.md)
