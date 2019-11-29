---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: Differenze di configurazione comuni tra sviluppo e produzione (VB) | Microsoft Docs
author: rick-anderson
description: Nelle esercitazioni precedenti è stato distribuito il sito Web copiando tutti i file pertinenti dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia...
ms.author: riande
ms.date: 04/01/2009
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: cc65af6eb4fca8b3b805e11e26da468a958a4221
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74619948"
---
# <a name="common-configuration-differences-between-development-and-production-vb"></a>Differenze di configurazione più comuni tra sviluppo e produzione (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare PDF](https://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> Nelle esercitazioni precedenti è stato distribuito il sito Web copiando tutti i file pertinenti dall'ambiente di sviluppo all'ambiente di produzione. Tuttavia, non è insolito che siano presenti differenze di configurazione tra gli ambienti, per cui è necessario che ogni ambiente disponga di un file Web. config univoco. In questa esercitazione vengono esaminate le differenze di configurazione tipiche e vengono esaminate le strategie per la gestione di informazioni di configurazione separate.

## <a name="introduction"></a>Introduzione

Le ultime due esercitazioni hanno illustrato come distribuire una semplice applicazione Web. Nell'esercitazione [*distribuzione del sito tramite un client FTP*](deploying-your-site-using-an-ftp-client-vb.md) è stato illustrato come utilizzare un client FTP autonomo per copiare i file necessari dall'ambiente di sviluppo fino alla produzione. Nell'esercitazione precedente, [*distribuzione del sito con Visual Studio*](deploying-your-site-using-visual-studio-vb.md), è stata esaminata la distribuzione usando lo strumento Copia sito Web di Visual Studio e l'opzione pubblica. In entrambe le esercitazioni ogni file nell'ambiente di produzione era una copia di un file nell'ambiente di sviluppo. Tuttavia, non è insolito che i file di configurazione nell'ambiente di produzione siano diversi da quelli nell'ambiente di sviluppo. La configurazione di un'applicazione Web viene archiviata nel file di `Web.config` e in genere include informazioni sulle risorse esterne, ad esempio i server di database, Web e di posta elettronica. Viene inoltre digitato il comportamento dell'applicazione in determinate situazioni, ad esempio il corso di azione da intraprendere quando si verifica un'eccezione non gestita.

Quando si distribuisce un'applicazione Web, è importante che le informazioni di configurazione corrette finiscano nell'ambiente di produzione. Nella maggior parte dei casi il file di `Web.config` nell'ambiente di sviluppo non può essere copiato nell'ambiente di produzione così com'è. Al contrario, una versione personalizzata di `Web.config` deve essere caricata nell'ambiente di produzione. Questa esercitazione esamina brevemente alcune delle differenze di configurazione più comuni; riepiloga inoltre alcune tecniche per la gestione di informazioni di configurazione diverse tra gli ambienti.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Differenze di configurazione tipiche tra gli ambienti di sviluppo e di produzione

Il file `Web.config` include un'ampia gamma di informazioni di configurazione per un'applicazione ASP.NET. Alcune di queste informazioni di configurazione sono le stesse indipendentemente dall'ambiente. Ad esempio, le impostazioni di autenticazione e le regole di autorizzazione URL digitate negli elementi `<authentication>` e `<authorization>` del file `Web.config` sono in genere le stesse indipendentemente dall'ambiente. Tuttavia, altre informazioni di configurazione, ad esempio le informazioni sulle risorse esterne, variano in genere a seconda dell'ambiente.

Le stringhe di connessione al database sono un ottimo esempio di informazioni di configurazione che variano in base all'ambiente. Quando un'applicazione Web comunica con un server di database, deve prima stabilire una connessione e tale risultato viene eseguito tramite una [stringa di connessione](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Sebbene sia possibile impostare come hardcoded la stringa di connessione al database direttamente nelle pagine Web o nel codice che si connette al database, è preferibile posizionarlo `Web.config`[elemento`<connectionStrings>`](https://msdn.microsoft.com/library/bf7sd233.aspx) in modo che le informazioni della stringa di connessione si trovino in un'unica posizione centralizzata. Spesso un database diverso viene usato durante lo sviluppo rispetto a quello usato nell'ambiente di produzione; di conseguenza, le informazioni della stringa di connessione devono essere univoche per ogni ambiente.

> [!NOTE]
> Esercitazioni future esplorare la distribuzione di applicazioni basate sui dati. a questo punto verranno illustrate le specifiche relative alla modalità di archiviazione delle stringhe di connessione del database nel file di configurazione.

Il comportamento previsto degli ambienti di sviluppo e produzione è sostanzialmente diverso. Un'applicazione Web nell'ambiente di sviluppo viene creata, testata e sottoposta a debug da un piccolo gruppo di sviluppatori. Nell'ambiente di produzione la stessa applicazione viene visitata da molti utenti simultanei diversi. ASP.NET include una serie di funzionalità che consentono agli sviluppatori di eseguire il test e il debug di un'applicazione, ma queste funzionalità devono essere disabilitate per motivi di sicurezza e prestazioni nell'ambiente di produzione. Esaminiamo alcune impostazioni di configurazione.

### <a name="configuration-settings-that-impact-performance"></a>Impostazioni di configurazione che influiscano sulle prestazioni

Quando una pagina ASP.NET viene visitata per la prima volta (o la prima volta dopo che è stata modificata), il markup dichiarativo deve essere convertito in una classe e la classe deve essere compilata. Se l'applicazione Web usa la compilazione automatica, è necessario compilare anche la classe code-behind della pagina. È possibile configurare un assortimento di opzioni di compilazione tramite l' [elemento`<compilation>`](https://msdn.microsoft.com/library/s10awwz0.aspx)del file `Web.config`.

L'attributo debug è uno degli attributi più importanti nell'elemento `<compilation>`. Se l'attributo `debug` è impostato su "true", gli assembly compilati includono i simboli di debug, necessari per il debug di un'applicazione in Visual Studio. Tuttavia, i simboli di debug aumentano le dimensioni dell'assembly e impongono requisiti di memoria aggiuntivi durante l'esecuzione del codice. Inoltre, quando l'attributo `debug` è impostato su "true", tutti i contenuti restituiti da `WebResource.axd` non vengono memorizzati nella cache, ovvero ogni volta che un utente visita una pagina, sarà necessario scaricare di nuovo il contenuto statico restituito da `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` è un gestore HTTP incorporato introdotto in ASP.NET 2,0 che i controlli server usano per recuperare le risorse incorporate, ad esempio file di script, immagini, file CSS e altro contenuto. Per altre informazioni sul funzionamento di `WebResource.axd` e su come usarlo per accedere alle risorse incorporate dai controlli server personalizzati, vedere [accesso alle risorse incorporate tramite un URL con `WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).

L'attributo `debug` dell'elemento `<compilation>` viene in genere impostato su "true" nell'ambiente di sviluppo. Infatti, questo attributo deve essere impostato su "true" per eseguire il debug di un'applicazione Web; Se si tenta di eseguire il debug di un'applicazione ASP.NET da Visual Studio e l'attributo `debug` è impostato su "false", in Visual Studio verrà visualizzato un messaggio che informa che non è possibile eseguire il debug dell'applicazione finché l'attributo `debug` non è impostato su "true" e offrirà per apportare questa modifica.

L'attributo `debug` **non deve mai** essere impostato su "true" in un ambiente di produzione a causa dell'impatto sulle prestazioni. Per una discussione più approfondita su questo argomento, vedere il post di Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/), [non eseguire applicazioni ASP.NET di produzione con `debug="true"` abilitata](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Errori e traccia personalizzati

Quando si verifica un'eccezione non gestita in un'applicazione ASP.NET, si esegue il bubbling fino al runtime, in cui si verifica una delle tre situazioni seguenti:

- Viene visualizzato un messaggio di errore di runtime generico. Questa pagina informa l'utente che si è verificato un errore di runtime, ma non fornisce informazioni dettagliate sull'errore.
- Viene visualizzato un messaggio Dettagli eccezione, che include informazioni sull'eccezione appena generata.
- Viene visualizzata una pagina di errore personalizzata, ovvero una pagina ASP.NET creata che Visualizza tutti i messaggi desiderati.

Ciò che accade in caso di eccezione non gestita dipende dalla [sezione`<customErrors>`](https://msdn.microsoft.com/library/h0hfz6fc.aspx)del file di `Web.config`.

Durante lo sviluppo e il test di un'applicazione, consente di visualizzare i dettagli di qualsiasi eccezione nel browser. Tuttavia, la visualizzazione dei dettagli delle eccezioni in un'applicazione in produzione costituisce un potenziale rischio per la sicurezza. Inoltre, non è più lusinghiero e rende l'aspetto non professionale del tuo sito Web. Idealmente, in caso di eccezione non gestita, un'applicazione Web nell'ambiente di sviluppo visualizzerà i dettagli dell'eccezione mentre nella stessa applicazione in produzione verrà visualizzata una pagina di errore personalizzata.

> [!NOTE]
> L'impostazione predefinita `<customErrors>` sezione Mostra il messaggio Dettagli eccezione solo quando la pagina viene visitata tramite localhost e Mostra la pagina di errore di runtime generica in caso contrario. Questo non è ideale, ma garantisce che il comportamento predefinito non riveli i dettagli dell'eccezione a visitatori non locali. Un'esercitazione futura esamina la sezione `<customErrors>` in modo più dettagliato e Mostra come avere una pagina di errore personalizzata visualizzata quando si verifica un errore nell'ambiente di produzione.

Un'altra funzionalità di ASP.NET utile durante lo sviluppo è la traccia. La traccia, se abilitata, registra le informazioni relative a ogni richiesta in ingresso e fornisce una pagina Web speciale, `Trace.axd`, per visualizzare i dettagli delle richieste recenti. È possibile attivare e configurare la traccia tramite l' [elemento`<trace>`](https://msdn.microsoft.com/library/6915t83k.aspx) in `Web.config`.

Se si Abilita la traccia, verificare che sia disabilitata nell'ambiente di produzione. Poiché le informazioni di traccia includono cookie, dati della sessione e altre informazioni potenzialmente riservate, è importante disabilitare la traccia nell'ambiente di produzione. L'aspetto positivo è che, per impostazione predefinita, la traccia è disabilitata e il file `Trace.axd` è accessibile solo tramite localhost. Se si modificano queste impostazioni predefinite in fase di sviluppo, assicurarsi che siano riattivate nell'ambiente di produzione.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Tecniche per la gestione di informazioni di configurazione separate

La presenza di impostazioni di configurazione diverse negli ambienti di sviluppo e di produzione complica il processo di distribuzione. Nelle due esercitazioni precedenti il processo di distribuzione ha comportato la copia di tutti i file necessari dallo sviluppo alla produzione, ma questo approccio funziona solo se le informazioni di configurazione sono le stesse in entrambi gli ambienti. Sono disponibili diverse tecniche per la distribuzione di un'applicazione con informazioni di configurazione diverse. Di seguito sono riportate alcune opzioni per le applicazioni Web ospitate.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Distribuzione manuale del file di configurazione dell'ambiente di produzione

L'approccio più semplice consiste nel mantenere due versioni del file di `Web.config`: una per l'ambiente di sviluppo e una per l'ambiente di produzione. La distribuzione di un sito in produzione comporta la copia di tutti i file nel server di produzione nell'ambiente di sviluppo, *ad eccezione* del file di `Web.config`. Al contrario, il file di `Web.config` specifico dell'ambiente di produzione verrà copiato in produzione.

Questo approccio non è molto sofisticato, ma è facile da implementare perché le informazioni di configurazione cambiano raramente. Funziona meglio per le applicazioni con un team di sviluppo di piccole dimensioni ospitato in un singolo server Web e le cui informazioni di configurazione non vengono modificate di frequente. È più sostenibile quando si distribuiscono manualmente i file dell'applicazione usando un client FTP autonomo. Quando si usa lo strumento Copia sito Web di Visual Studio o l'opzione pubblica è necessario prima di tutto scambiare il file di `Web.config` specifico della distribuzione con quello specifico per la produzione prima della distribuzione e quindi scambiarli nuovamente al termine della distribuzione.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Modificare la configurazione durante il processo di compilazione o distribuzione

Le discussioni finora hanno assunto un processo di compilazione e distribuzione ad hoc. Molti progetti software di grandi dimensioni hanno processi più formalizzati che usano strumenti open source, a casa o di terze parti. Per i progetti di questo tipo è possibile personalizzare il processo di compilazione o distribuzione per modificare in modo appropriato le informazioni di configurazione prima di effettuarne il push in produzione. Se si compila l'applicazione Web utilizzando [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [Nant](http://nant.sourceforge.net/)o un altro strumento di compilazione, è possibile aggiungere un'istruzione di compilazione per modificare il file di `Web.config` in modo da includere le impostazioni specifiche per la produzione. In alternativa, il flusso di lavoro di distribuzione potrebbe connettersi a livello di codice al server del controllo del codice sorgente e recuperare il file di `Web.config` appropriato.

L'approccio effettivo per ottenere le informazioni di configurazione appropriate per la produzione varia notevolmente a seconda degli strumenti e del flusso di lavoro. Di conseguenza, non verrà approfondito questo argomento. Se si usa uno strumento di compilazione comune come MSBuild o NAnt, è possibile trovare articoli ed esercitazioni sulla distribuzione specifici di questi strumenti tramite una ricerca Web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Gestione delle differenze di configurazione tramite il componente aggiuntivo del progetto di distribuzione Web

In 2006 Microsoft ha rilasciato il componente aggiuntivo progetto di sviluppo Web per Visual Studio 2005. Un componente aggiuntivo per Visual Studio 2008 è stato rilasciato in 2008. Questo componente aggiuntivo consente agli sviluppatori ASP.NET di creare un progetto di distribuzione Web separato insieme al progetto di applicazione Web che, quando compilato, compila in modo esplicito l'applicazione Web e copia i file per la distribuzione in una directory di output locale. Il progetto di applicazione Web utilizza MSBuild dietro le quinte.

Per impostazione predefinita, il file di `Web.config` dell'ambiente di sviluppo viene copiato nella directory di output, ma è possibile configurare il progetto di distribuzione Web per personalizzare il

informazioni di configurazione che vengono copiate in questa directory nei modi seguenti:

- Tramite `Web.config` sostituzione della sezione del file, in cui si specifica la sezione da sostituire e un file XML che contiene il testo di sostituzione.
- Specificando un percorso a un file di origine della configurazione esterna. Se questa opzione è selezionata, il progetto di distribuzione Web copia un file di `Web.config` specifico nella directory di output (piuttosto che nel file di `Web.config` utilizzato nell'ambiente di sviluppo).
- Aggiungendo regole personalizzate al file MSBuild utilizzato dal progetto di distribuzione Web.

Per distribuire l'applicazione Web, compilare il progetto di distribuzione Web, quindi copiare i file dalla cartella di output del progetto nell'ambiente di produzione.

Per ulteriori informazioni sull'utilizzo del progetto di distribuzione Web, vedere l'articolo relativo ai [progetti di distribuzione Web](https://msdn.microsoft.com/magazine/cc163448.aspx) dalla pagina di aprile 2007 relativa a [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)oppure consultare i collegamenti nella sezione ulteriori informazioni alla fine di questa esercitazione.

> [!NOTE]
> Non è possibile utilizzare il progetto di distribuzione Web con Visual Web Developer perché il progetto di distribuzione Web viene implementato come componente aggiuntivo di Visual Studio e le edizioni Visual Studio Express (incluso Visual Web Developer) non supportano i componenti aggiuntivi.

## <a name="summary"></a>Riepilogo

Le risorse esterne e il comportamento di un'applicazione Web in fase di sviluppo sono in genere diversi rispetto a quando la stessa applicazione è in produzione. Ad esempio, le stringhe di connessione del database, le opzioni di compilazione e il comportamento quando si verifica un'eccezione non gestita si differenziano generalmente tra gli ambienti. Il processo di distribuzione deve soddisfare queste differenze. Come illustrato in questa esercitazione, l'approccio più semplice consiste nel copiare manualmente un file di configurazione alternativo nell'ambiente di produzione. Quando si usa il componente aggiuntivo progetto di distribuzione Web o con un processo di compilazione o distribuzione più formale che può supportare tali personalizzazioni, sono possibili soluzioni più eleganti.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Descrizione delle stringhe di connessione](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [Stringhe di connessione al database @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Non eseguire applicazioni ASP.NET di produzione con `debug="true"` abilitato](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Risposta normale a eccezioni non gestite-visualizzazione di pagine di errore semplici da usare](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Procedura: utilizzo di un progetto di distribuzione Web di Visual Studio 2008](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Impostazioni di configurazione delle chiavi durante la distribuzione di un database](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- Scarica i [progetti di distribuzione Web di Visual studio 2008](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [scaricare i progetti di distribuzione Web di Visual Studio 2005](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Progetti di distribuzione Web di Visual studio 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [supporto del progetto di distribuzione web di vs 2008 rilasciato](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Progetti di distribuzione Web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Precedente](deploying-your-site-using-visual-studio-vb.md)
> [Successivo](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
