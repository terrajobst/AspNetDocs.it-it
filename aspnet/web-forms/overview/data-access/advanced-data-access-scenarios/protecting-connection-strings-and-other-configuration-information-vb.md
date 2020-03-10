---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
title: Protezione delle stringhe di connessione e di altre informazioni di configurazione (VB) | Microsoft Docs
author: rick-anderson
description: Un'applicazione ASP.NET in genere archivia le informazioni di configurazione in un file Web. config. Alcune di queste informazioni sono riservate e garantiscono la protezione. Per def...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: cd17dbe1-c5e1-4be8-ad3d-57233d52cef1
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-vb
msc.type: authoredcontent
ms.openlocfilehash: 070e1dccb80ef9af21ea621357c5b23e2ada6f9f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78524654"
---
# <a name="protecting-connection-strings-and-other-configuration-information-vb"></a>Protezione delle stringhe di connessione e di altre informazioni di configurazione (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_VB.zip) o [Scarica PDF](protecting-connection-strings-and-other-configuration-information-vb/_static/datatutorial73vb1.pdf)

> Un'applicazione ASP.NET in genere archivia le informazioni di configurazione in un file Web. config. Alcune di queste informazioni sono riservate e garantiscono la protezione. Per impostazione predefinita, il file non verrà servito al visitatore di un sito Web, ma un amministratore o un pirata informatico può ottenere l'accesso al file system del server Web e visualizzare il contenuto del file. In questa esercitazione si apprenderà come ASP.NET 2,0 consenta di proteggere le informazioni riservate mediante la crittografia delle sezioni del file Web. config.

## <a name="introduction"></a>Introduzione

Le informazioni di configurazione per le applicazioni ASP.NET vengono in genere archiviate in un file XML denominato `Web.config`. Nel corso di queste esercitazioni è stato aggiornato il `Web.config` alcune volte. Quando si crea il `Northwind` DataSet tipizzato nella [prima esercitazione](../introduction/creating-a-data-access-layer-vb.md), ad esempio, le informazioni sulla stringa di connessione sono state aggiunte automaticamente ai `Web.config` nella sezione `<connectionStrings>`. Successivamente, nell'esercitazione sulle [pagine master e sulla navigazione nel sito](../introduction/master-pages-and-site-navigation-vb.md) , è stato aggiornato manualmente `Web.config`, aggiungendo un elemento `<pages>` che indica che tutte le pagine ASP.NET del progetto devono usare il tema `DataWebControls`.

Poiché `Web.config` possono contenere dati sensibili, ad esempio stringhe di connessione, è importante che il contenuto di `Web.config` venga mantenuto protetto e nascosto dai visualizzatori non autorizzati. Per impostazione predefinita, qualsiasi richiesta HTTP a un file con l'estensione `.config` viene gestita dal motore ASP.NET, che restituisce il messaggio *questo tipo di pagina non viene servito* come indicato nella figura 1. Ciò significa che i visitatori non possono visualizzare il contenuto del file `Web.config` semplicemente inserendo http://www.YourServer.com/Web.config nella barra degli indirizzi del browser.

[![visitare Web. config tramite un browser restituisce un messaggio di questo tipo non viene servito](protecting-connection-strings-and-other-configuration-information-vb/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image1.png)

**Figura 1**: la visita di `Web.config` tramite un browser restituisce un messaggio di questo tipo di pagina non viene servito ([fare clic per visualizzare l'immagine con dimensioni complete](protecting-connection-strings-and-other-configuration-information-vb/_static/image3.png))

Ma cosa accade se un utente malintenzionato riesce a trovare un altro exploit che consente di visualizzare il contenuto del file `Web.config`? Cosa può fare un utente malintenzionato con queste informazioni e quali operazioni è possibile eseguire per proteggere ulteriormente le informazioni riservate all'interno `Web.config`? Fortunatamente, la maggior parte delle sezioni in `Web.config` non contengono informazioni riservate. Qual è il danno che un utente malintenzionato può perpetrare se conosce il nome del tema predefinito usato dalle pagine di ASP.NET?

Alcune `Web.config` sezioni, tuttavia, contengono informazioni riservate che possono includere stringhe di connessione, nomi utente, password, nomi di server, chiavi di crittografia e così via. Queste informazioni si trovano in genere nelle sezioni `Web.config` seguenti:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

In questa esercitazione vengono esaminate le tecniche per la protezione di tali informazioni di configurazione riservate. Come si vedrà, il .NET Framework versione 2,0 include un sistema di configurazioni protette che consente di crittografare e decrittografare le sezioni di configurazione selezionate a livello di codice.

> [!NOTE]
> Questa esercitazione conclude con i suggerimenti di Microsoft per la connessione a un database da un'applicazione ASP.NET. Oltre a crittografare le stringhe di connessione, è possibile contribuire alla protezione avanzata del sistema garantendo la connessione al database in modo sicuro.

## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Passaggio 1: esplorazione delle opzioni di configurazione protette di ASP.NET 2,0 s

ASP.NET 2,0 include un sistema di configurazione protetto per la crittografia e la decrittografia delle informazioni di configurazione. Sono inclusi i metodi nel .NET Framework che possono essere usati per crittografare o decrittografare le informazioni di configurazione a livello di codice. Il sistema di configurazione protetta utilizza il [modello di provider](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), che consente agli sviluppatori di scegliere quale implementazione di crittografia viene utilizzata.

Il .NET Framework viene fornito con due provider di configurazione protetti:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) : usa l' [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) asimmetrico per la crittografia e la decrittografia.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) : usa Windows [Data Protection API (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) per la crittografia e la decrittografia.

Poiché il sistema di configurazione protetta implementa il modello di progettazione del provider, è possibile creare il proprio provider di configurazione protetta e inserirlo nell'applicazione. Per ulteriori informazioni su questo processo, vedere [implementazione di un provider di configurazione protetta](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) .

I provider RSA e DPAPI utilizzano le chiavi per le routine di crittografia e decrittografia e possono essere archiviate a livello di computer o utente. Le chiavi a livello di computer sono ideali per gli scenari in cui l'applicazione Web viene eseguita in un server dedicato o in presenza di più applicazioni in un server che devono condividere informazioni crittografate. Le chiavi a livello di utente sono un'opzione più sicura negli ambienti host condivisi in cui altre applicazioni nello stesso server non devono essere in grado di decrittografare le sezioni di configurazione protetta dell'applicazione.

In questa esercitazione gli esempi utilizzeranno il provider DPAPI e le chiavi a livello di computer. In particolare, si esaminerà la crittografia della sezione `<connectionStrings>` in `Web.config`, sebbene il sistema di configurazione protetta possa essere usato per crittografare la maggior parte delle `Web.config` sezione. Per informazioni sull'uso delle chiavi a livello di utente o sul provider RSA, vedere le risorse nella sezione ulteriori letture alla fine di questa esercitazione.

> [!NOTE]
> I provider di `RSAProtectedConfigurationProvider` e di `DPAPIProtectedConfigurationProvider` vengono registrati nel file `machine.config` con i nomi di provider `RsaProtectedConfigurationProvider` e `DataProtectionConfigurationProvider`, rispettivamente. Quando si esegue la crittografia o la decrittografia delle informazioni di configurazione, è necessario fornire il nome del provider appropriato (`RsaProtectedConfigurationProvider` o `DataProtectionConfigurationProvider`) anziché il nome effettivo del tipo (`RSAProtectedConfigurationProvider` e `DPAPIProtectedConfigurationProvider`). È possibile trovare il file di `machine.config` nella cartella `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG`.

## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Passaggio 2: crittografare e decrittografare le sezioni di configurazione a livello di codice

Con poche righe di codice è possibile crittografare o decrittografare una particolare sezione di configurazione usando un provider specificato. Il codice, come si vedrà a breve, è sufficiente fare riferimento a livello di codice alla sezione di configurazione appropriata, chiamare il metodo `ProtectSection` o `UnprotectSection` e quindi chiamare il metodo `Save` per salvare in modo permanente le modifiche. Il .NET Framework include inoltre un'utilità da riga di comando utile che consente di crittografare e decrittografare le informazioni di configurazione. Questa utilità della riga di comando verrà esaminata nel passaggio 3.

Per illustrare la protezione a livello di codice delle informazioni di configurazione, è possibile creare una pagina ASP.NET che includa i pulsanti per la crittografia e la decrittografia della sezione `<connectionStrings>` in `Web.config`.

Per iniziare, aprire la pagina `EncryptingConfigSections.aspx` nella cartella `AdvancedDAL`. Trascinare un controllo TextBox dalla casella degli strumenti nella finestra di progettazione, impostando la relativa proprietà `ID` su `WebConfigContents`, la relativa proprietà `TextMode` su `MultiLine`e le relative proprietà `Width` e `Rows` rispettivamente su 95% e 15. Questo controllo TextBox visualizzerà il contenuto di `Web.config` che ci consente di verificare rapidamente se il contenuto è crittografato o meno. Naturalmente, in un'applicazione reale non si vuole mai visualizzare il contenuto del `Web.config`.

Sotto la casella di testo aggiungere due controlli Button denominati `EncryptConnStrings` e `DecryptConnStrings`. Impostare le proprietà di testo per crittografare le stringhe di connessione e decrittografare le stringhe di connessione.

A questo punto la schermata dovrebbe essere simile alla figura 2.

[![aggiungere una casella di testo e i controlli Web di due pulsanti alla pagina](protecting-connection-strings-and-other-configuration-information-vb/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image4.png)

**Figura 2**: aggiungere una casella di testo e i controlli Web di due pulsanti alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](protecting-connection-strings-and-other-configuration-information-vb/_static/image6.png))

Successivamente, è necessario scrivere il codice che carica e visualizza il contenuto di `Web.config` nella casella di testo `WebConfigContents` quando la pagina viene caricata per la prima volta. Aggiungere il codice seguente alla classe code-behind della pagina. Questo codice aggiunge un metodo denominato `DisplayWebConfig` e lo chiama dal gestore eventi `Page_Load` quando `Page.IsPostBack` è `False`:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample1.vb)]

Il metodo `DisplayWebConfig` usa la [classe`File`](https://msdn.microsoft.com/library/system.io.file.aspx) per aprire il file `Web.config` dell'applicazione, la [classe`StreamReader`](https://msdn.microsoft.com/library/system.io.streamreader.aspx) per leggerne il contenuto in una stringa e la [classe`Path`](https://msdn.microsoft.com/library/system.io.path.aspx) per generare il percorso fisico del file di `Web.config`. Queste tre classi sono tutte disponibili nello [spazio dei nomi`System.IO`](https://msdn.microsoft.com/library/system.io.aspx). Di conseguenza, è necessario aggiungere un'istruzione `Imports``System.IO` all'inizio della classe code-behind o, in alternativa, anteporre a questi nomi di classe `System.IO.`

Successivamente, è necessario aggiungere gestori di eventi per i controlli di due pulsanti `Click` eventi e aggiungere il codice necessario per crittografare e decrittografare la sezione `<connectionStrings>` utilizzando una chiave a livello di computer con il provider DPAPI. Nella finestra di progettazione fare doppio clic su ognuno dei pulsanti per aggiungere un gestore dell'evento `Click` nella classe code-behind, quindi aggiungere il codice seguente:

[!code-vb[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample2.vb)]

Il codice usato nei due gestori di eventi è quasi identico. Entrambi iniziano ottenendo informazioni sul file di `Web.config` dell'applicazione corrente tramite il [metodo`OpenWebConfiguration`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx)della [classe`WebConfigurationManager`](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) . Questo metodo restituisce il file di configurazione Web per il percorso virtuale specificato. A questo punto, è possibile accedere alla sezione `<connectionStrings>` `Web.config` file tramite il [metodo`GetSection(sectionName)`](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx)della [classe`Configuration`](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) , che restituisce un oggetto [`ConfigurationSection`](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) .

L'oggetto `ConfigurationSection` include una [proprietà`SectionInformation`](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) che fornisce informazioni e funzionalità aggiuntive relative alla sezione di configurazione. Come illustrato nel codice precedente, è possibile determinare se la sezione di configurazione è crittografata controllando la proprietà `SectionInformation` proprietà `IsProtected`. Inoltre, la sezione può essere crittografata o decrittografata tramite la proprietà `SectionInformation` `ProtectSection(provider)` e i metodi di `UnprotectSection`.

Il metodo `ProtectSection(provider)` accetta come input una stringa che specifica il nome del provider di configurazione protetta da usare per la crittografia. Nel gestore eventi del pulsante `EncryptConnString` viene passato DataProtectionConfigurationProvider al metodo `ProtectSection(provider)` in modo che venga utilizzato il provider DPAPI. Il metodo `UnprotectSection` può determinare il provider utilizzato per crittografare la sezione di configurazione e pertanto non richiede alcun parametro di input.

Dopo aver chiamato il metodo `ProtectSection(provider)` o `UnprotectSection`, è necessario chiamare il metodo dell'oggetto `Configuration` s [`Save`](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) per salvare in modo permanente le modifiche. Una volta che le informazioni di configurazione sono state crittografate o decrittografate e le modifiche sono state salvate, viene chiamato `DisplayWebConfig` per caricare il contenuto del `Web.config` aggiornato nel controllo TextBox.

Dopo aver immesso il codice precedente, testarlo visitando la pagina `EncryptingConfigSections.aspx` tramite un browser. Inizialmente, viene visualizzata una pagina che elenca il contenuto di `Web.config` con la sezione `<connectionStrings>` visualizzata in testo normale (vedere la figura 3).

[![aggiungere una casella di testo e i controlli Web di due pulsanti alla pagina](protecting-connection-strings-and-other-configuration-information-vb/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image7.png)

**Figura 3**: aggiungere una casella di testo e i controlli Web di due pulsanti alla pagina ([fare clic per visualizzare l'immagine con dimensioni complete](protecting-connection-strings-and-other-configuration-information-vb/_static/image9.png))

A questo punto fare clic sul pulsante Crittografa stringhe di connessione. Se la convalida della richiesta è abilitata, il markup inviato dalla casella di testo `WebConfigContents` produrrà un `HttpRequestValidationException`, che Visualizza il messaggio, dal client è stato rilevato un valore di `Request.Form` potenzialmente pericoloso. La convalida delle richieste, abilitata per impostazione predefinita in ASP.NET 2,0, impedisce i postback che includono codice HTML non codificato ed è progettato per impedire attacchi di script injection. Questo controllo può essere disabilitato a livello di pagina o di applicazione. Per disattivarla per questa pagina, impostare l'impostazione `ValidateRequest` su `False` nella direttiva `@Page`. La direttiva `@Page` si trova nella parte superiore del markup dichiarativo della pagina.

[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample3.aspx)]

Per altre informazioni sulla convalida della richiesta, il suo scopo, su come disabilitarlo a livello di pagina e a livello di applicazione, nonché come codificare il markup HTML, vedere [richieste di convalida-prevenzione degli attacchi di script](../../../../whitepapers/request-validation.md).

Dopo aver disabilitato la convalida della richiesta per la pagina, provare a fare nuovamente clic sul pulsante Crittografa stringhe di connessione. Durante il postback, viene eseguito l'accesso al file di configurazione e la relativa sezione `<connectionStrings>` crittografata utilizzando il provider DPAPI. La casella di testo viene quindi aggiornata per visualizzare il nuovo contenuto del `Web.config`. Come illustrato nella figura 4, le informazioni `<connectionStrings>` sono ora crittografate.

[![facendo clic sul pulsante Crittografa stringhe di connessione viene crittografata la sezione&gt; &lt;connectionString](protecting-connection-strings-and-other-configuration-information-vb/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image10.png)

**Figura 4**: facendo clic sul pulsante Crittografa stringhe di connessione viene crittografata la sezione `<connectionString>` ([fare clic per visualizzare l'immagine con dimensioni complete](protecting-connection-strings-and-other-configuration-information-vb/_static/image12.png))

Di seguito è riportata la sezione `<connectionStrings>` crittografata generata nel computer, sebbene parte del contenuto nell'elemento `<CipherData>` sia stata rimossa per brevità:

[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample4.xml)]

> [!NOTE]
> L'elemento `<connectionStrings>` specifica il provider utilizzato per eseguire la crittografia (`DataProtectionConfigurationProvider`). Queste informazioni vengono utilizzate dal metodo `UnprotectSection` quando si fa clic sul pulsante Decrypt Connection Strings.

Quando si accede alle informazioni della stringa di connessione da `Web.config`, dal codice scritto, da un controllo SqlDataSource o dal codice generato automaticamente dagli oggetti TableAdapter nei set di dati tipizzati, questo viene decrittografato automaticamente. In breve, non è necessario aggiungere codice o logica aggiuntiva per decrittografare la sezione `<connectionString>` crittografata. Per illustrare questo problema, vedere una delle esercitazioni precedenti, ad esempio la semplice esercitazione sulla visualizzazione della sezione report di base (`~/BasicReporting/SimpleDisplay.aspx`). Come illustrato nella figura 5, l'esercitazione funziona esattamente come previsto, a indicare che le informazioni crittografate della stringa di connessione vengono decrittografate automaticamente dalla pagina ASP.NET.

[![il livello di accesso ai dati decrittografa automaticamente le informazioni sulla stringa di connessione](protecting-connection-strings-and-other-configuration-information-vb/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-vb/_static/image13.png)

**Figura 5**: il livello di accesso ai dati decrittografa automaticamente le informazioni sulla stringa[di connessione (fare clic per visualizzare l'immagine con dimensioni complete](protecting-connection-strings-and-other-configuration-information-vb/_static/image15.png))

Per ripristinare la sezione del `<connectionStrings>` alla relativa rappresentazione in testo normale, fare clic sul pulsante Decrypt Connection Strings. Durante il postback dovrebbero essere visualizzate le stringhe di connessione in `Web.config` testo normale. A questo punto, la schermata dovrebbe apparire come se fosse stata visualizzata per la prima volta in questa pagina (vedere la figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnet_regiisexe"></a>Passaggio 3: crittografia delle sezioni di configurazione con`aspnet_regiis.exe`

Il .NET Framework include un'ampia gamma di strumenti da riga di comando nella cartella `$WINDOWS$\Microsoft.NET\Framework\version\`. Nell'esercitazione [utilizzo delle dipendenze della cache SQL](../caching-data/using-sql-cache-dependencies-vb.md) , ad esempio, è stato esaminato l'utilizzo dello strumento da riga di comando `aspnet_regsql.exe` per aggiungere l'infrastruttura necessaria per le dipendenze della cache SQL. Un altro strumento da riga di comando utile in questa cartella è lo [strumento di registrazione ASP.NET IIS (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Come suggerisce il nome, lo strumento di registrazione IIS ASP.NET viene usato principalmente per registrare un'applicazione ASP.NET 2,0 con server Web di livello professionale Microsoft, IIS. Oltre alle funzionalità correlate a IIS, lo strumento di registrazione IIS ASP.NET può essere usato anche per crittografare o decrittografare le sezioni di configurazione specificate in `Web.config`.

Nell'istruzione seguente viene illustrata la sintassi generale utilizzata per crittografare una sezione di configurazione con lo strumento da riga di comando `aspnet_regiis.exe`:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample5.cmd)]

la *sezione di* configurazione da crittografare (ad esempio connectionStrings), la *directory fisica\_* è il percorso fisico completo della directory radice dell'applicazione Web e il *provider* è il nome del provider di configurazione protetta da usare (ad esempio DataProtectionConfigurationProvider). In alternativa, se l'applicazione Web è registrata in IIS, è possibile immettere il percorso virtuale anziché il percorso fisico usando la sintassi seguente:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample6.cmd)]

Nell'esempio di `aspnet_regiis.exe` seguente viene crittografata la sezione `<connectionStrings>` utilizzando il provider DPAPI con una chiave a livello di computer:

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample7.cmd)]

Analogamente, è possibile usare lo strumento da riga di comando `aspnet_regiis.exe` per decrittografare le sezioni di configurazione. Anziché utilizzare l'opzione `-pef`, utilizzare `-pdf` (o invece di `-pe`utilizzare `-pd`). Si noti inoltre che il nome del provider non è necessario per la decrittografia.

[!code-console[Main](protecting-connection-strings-and-other-configuration-information-vb/samples/sample8.cmd)]

> [!NOTE]
> Poiché si utilizza il provider DPAPI, che utilizza chiavi specifiche del computer, è necessario eseguire `aspnet_regiis.exe` dallo stesso computer da cui vengono gestite le pagine Web. Se, ad esempio, si esegue questo programma da riga di comando dal computer di sviluppo locale e quindi si carica il file Web. config crittografato nel server di produzione, il server di produzione non sarà in grado di decrittografare le informazioni della stringa di connessione perché è stato crittografato uso delle chiavi specifiche del computer di sviluppo. Il provider RSA non presenta questa limitazione perché è possibile esportare le chiavi RSA in un altro computer.

## <a name="understanding-database-authentication-options"></a>Informazioni sulle opzioni di autenticazione del database

Prima che un'applicazione possa emettere `SELECT`, `INSERT`, `UPDATE`o `DELETE` query a un database Microsoft SQL Server, il database deve innanzitutto identificare il richiedente. Questo processo è noto come *autenticazione* e SQL Server fornisce due metodi di autenticazione:

- **Autenticazione di Windows** : il processo con cui viene eseguita l'applicazione viene utilizzato per comunicare con il database. Quando si esegue un'applicazione ASP.NET con Visual Studio 2005 s Server di sviluppo ASP.NET, l'applicazione ASP.NET presuppone l'identità dell'utente attualmente connesso. Per le applicazioni ASP.NET in Microsoft Internet Information Server (IIS), le applicazioni ASP.NET in genere assumono l'identità di `domainName``\MachineName` o `domainName``\NETWORK SERVICE`, anche se possono essere personalizzate.
- **Autenticazione SQL** : i valori di ID utente e password vengono specificati come credenziali per l'autenticazione. Con l'autenticazione SQL, l'ID utente e la password vengono specificati nella stringa di connessione.

L'autenticazione di Windows è preferibile rispetto all'autenticazione SQL perché è più sicura. Con l'autenticazione di Windows la stringa di connessione è priva di nome utente e password e, se il server Web e il server di database si trovano in due computer diversi, le credenziali non vengono inviate in rete come testo normale. Con l'autenticazione SQL, tuttavia, le credenziali di autenticazione sono hardcoded nella stringa di connessione e vengono trasmesse dal server Web al server di database in testo normale.

In queste esercitazioni è stata utilizzata l'autenticazione di Windows. È possibile stabilire quale modalità di autenticazione viene utilizzata controllando la stringa di connessione. La stringa di connessione in `Web.config` per le esercitazioni è stata:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

Il valore Integrated Security = true e la mancanza di un nome utente e una password indicano che è in uso l'autenticazione di Windows. In alcune stringhe di connessione viene utilizzato il termine connessione trusted = Sì o Integrated Security = SSPI anziché Integrated Security = true, ma tutti e tre indicano l'utilizzo dell'autenticazione di Windows.

Nell'esempio seguente viene illustrata una stringa di connessione che utilizza l'autenticazione SQL. Si notino le credenziali incorporate nella stringa di connessione:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Si supponga che un utente malintenzionato sia in grado di visualizzare il file di `Web.config` dell'applicazione. Se si usa l'autenticazione SQL per connettersi a un database accessibile tramite Internet, l'utente malintenzionato può usare questa stringa di connessione per connettersi al database tramite SQL Management Studio o da pagine ASP.NET nel proprio sito Web. Per attenuare questa minaccia, crittografare le informazioni della stringa di connessione in `Web.config` usando il sistema di configurazione protetto.

> [!NOTE]
> Per altre informazioni sui diversi tipi di autenticazione disponibili in SQL Server, vedere [compilazione di applicazioni ASP.NET sicure: autenticazione, autorizzazione e comunicazione sicura](https://msdn.microsoft.com/library/aa302392.aspx). Per ulteriori esempi di stringhe di connessione che illustrano le differenze tra la sintassi di autenticazione di Windows e SQL, vedere [connectionStrings.com](http://www.connectionstrings.com/).

## <a name="summary"></a>Riepilogo

Per impostazione predefinita, non è possibile accedere ai file con estensione `.config` in un'applicazione ASP.NET tramite un browser. Questi tipi di file non vengono restituiti perché possono contenere informazioni riservate, ad esempio stringhe di connessione del database, nomi utente e password e così via. Il sistema di configurazione protetta in .NET 2,0 consente di proteggere ulteriormente le informazioni riservate consentendo la crittografia delle sezioni di configurazione specificate. Sono disponibili due provider di configurazione protetta predefiniti: uno che usa l'algoritmo RSA e uno che usa la Data Protection API di Windows (DPAPI).

In questa esercitazione è stato esaminato come crittografare e decrittografare le impostazioni di configurazione usando il provider DPAPI. Questa operazione può essere eseguita sia a livello di codice, come illustrato nel passaggio 2, sia tramite lo strumento da riga di comando `aspnet_regiis.exe`, descritto nel passaggio 3. Per altre informazioni sull'uso delle chiavi a livello di utente o sul provider RSA, vedere le risorse nella sezione ulteriori letture.

Buona programmazione!

## <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Creazione di applicazioni ASP.NET sicure: autenticazione, autorizzazione e comunicazione sicura](https://msdn.microsoft.com/library/aa302392.aspx)
- [Crittografia delle informazioni di configurazione nelle applicazioni ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Crittografia dei valori di `Web.config` in ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Procedura: crittografare le sezioni di configurazione in ASP.NET 2,0 usando DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Procedura: crittografare le sezioni di configurazione in ASP.NET 2,0 usando RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [API di configurazione in .NET 2,0](http://www.odetocode.com/Articles/418.aspx)
- [Protezione dei dati di Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. I revisori del lead per questa esercitazione erano Teresa Murphy e Randy Schmidt. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb.md)
> [Successivo](debugging-stored-procedures-vb.md)
