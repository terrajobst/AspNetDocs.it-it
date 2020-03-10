---
uid: whitepapers/aspnet4/breaking-changes
title: ASP.NET 4 modifiche di rilievo | Microsoft Docs
author: rick-anderson
description: In questo documento vengono descritte le modifiche apportate alla versione .NET Framework versione 4 che possono potenzialmente influire sulle applicazioni create con...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: d601c540-f86b-4feb-890c-20c806b3da6c
msc.legacyurl: /whitepapers/aspnet4/breaking-changes
msc.type: content
ms.openlocfilehash: 8ccad3b40a723c92a3164de082e1f94577141008
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546249"
---
# <a name="aspnet-4-breaking-changes"></a>Modifiche importanti in ASP.NET 4

> Questo documento descrive le modifiche apportate alla versione .NET Framework versione 4 che possono potenzialmente influire sulle applicazioni create con versioni precedenti, incluse le versioni ASP.NET 4 beta 1 e beta 2.
> 
> [Scarica questo white paper](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_Breaking_Changes.pdf)

<a id="0.1__Toc256768952"></a><a id="0.1__Toc256770056"></a>

## <a name="contents"></a>Contenuto

[Impostazione ControlRenderingCompatibilityVersion nel file Web. config](#0.1__Toc256770141 "_Toc256770141")  
[Modifiche a ClientIDMode](#0.1__Toc256770142 "_Toc256770142")  
[HtmlEncode e UrlEncode ora codificano le virgolette singole](#0.1__Toc256770143 "_Toc256770143")  
[Il parser di pagina (. aspx) ASP.NET è più restrittivo](#0.1__Toc256770144 "_Toc256770144")  
[File di definizione del browser aggiornati](#0.1__Toc256770145 "_Toc256770145")  
[System. Web. mobile. dll rimosso dal file di configurazione Web radice](#0.1__Toc256770146 "_Toc256770146")  
[Convalida richiesta ASP.NET](#0.1__Toc256770147 "_Toc256770147")  
[L'algoritmo hash predefinito è ora HMACSHA256](#0.1__Toc256770148 "_Toc256770148")  
[Errori di configurazione correlati alla nuova configurazione radice di ASP.NET 4](#0.1__Toc256770149 "_Toc256770149")  
[ASP.NET 4 le applicazioni figlio non vengono avviate quando si usa ASP.NET 2,0 o ASP.NET 3,5](#0.1__Toc256770150 "_Toc256770150")  
[Impossibile avviare i siti Web di ASP.NET 4 nei computer in cui è installato SharePoint](#0.1__Toc256770151 "_Toc256770151")  
[La proprietà HttpRequest. FilePath non include più valori PathInfo](#0.1__Toc256770152 "_Toc256770152")  
[Le applicazioni ASP.NET 2,0 possono generare errori HttpException che fanno riferimento a eurl. axd](#0.1__Toc256770153 "_Toc256770153")  
[I gestori eventi potrebbero non essere generati in un documento predefinito in modalità integrata IIS 7 o IIS 7,5](#0.1__Toc256770154 "_Toc256770154")  
[Modifiche all'implementazione di sicurezza dall'accesso di codice (CAS) ASP.NET](#0.1__Toc256770155 "_Toc256770155")  
[MembershipUser e altri tipi nello spazio dei nomi System. Web. Security sono stati spostati](#0.1__Toc256770156 "_Toc256770156")  
[Modifiche della cache di output per variare \* intestazione HTTP](#0.1__Toc256770157 "_Toc256770157")  
[I tipi System. Web. Security per Passport sono obsoleti](#0.1__Toc256770158 "_Toc256770158")  
[La proprietà MenuItem. PopOutImageUrl non è in grado di eseguire il rendering di un'immagine in ASP.NET 4](#0.1__Toc256770159 "_Toc256770159")  
[Menu. StaticPopOutImageUrl e menu. DynamicPopOutImageUrl non riescono a eseguire il rendering delle immagini quando i percorsi contengono barre rovesciate](#0.1__Toc256770160 "_Toc256770160")  
[Non responsabilità](#0.1__Toc256770161 "_Toc256770161")

<a id="0.1__ControlRenderingCompatibilityVersio"></a><a id="0.1__Toc245724853"></a><a id="0.1__Toc255587630"></a><a id="0.1__Toc256770141"></a>

## <a name="controlrenderingcompatibilityversion-setting-in-the-webconfig-file"></a>Impostazione ControlRenderingCompatibilityVersion nel file Web. config

I controlli ASP.NET sono stati modificati nella .NET Framework versione 4 per consentire di specificare più precisamente il modo in cui eseguono il rendering del markup. Nelle versioni precedenti del .NET Framework, alcuni controlli generavano markup che non era possibile disabilitare. Per impostazione predefinita, ASP.NET 4 questo tipo di markup non viene più generato.

Se si usa Visual Studio 2010 per aggiornare l'applicazione da ASP.NET 2,0 o ASP.NET 3,5, lo strumento aggiunge automaticamente un'impostazione al file `Web.config` che conserva il rendering legacy. Se tuttavia si aggiorna un'applicazione modificando il pool di applicazioni in IIS per impostare come destinazione .NET Framework 4, ASP.NET usa la nuova modalità di rendering per impostazione predefinita. Per disabilitare la nuova modalità di rendering, aggiungere la seguente impostazione nel file di `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample1.xml)]

Di seguito sono riportate le principali modifiche di rendering introdotte dal nuovo comportamento:

- I controlli **Image** e **ImageButton** non eseguono più il rendering di un attributo `border="0"`.
- La classe **BaseValidator** e i controlli di convalida che derivano da esso non eseguono più il rendering del testo rosso per impostazione predefinita.
- Il controllo **HtmlForm** non esegue il rendering di un attributo del **nome** .
- Il controllo **tabella** non esegue più il rendering di un attributo `border="0"`.
- I controlli che non sono progettati per l'input dell'utente (ad esempio, il controllo **etichetta** ) non eseguono più il rendering dell'attributo `disabled="disabled"` se la relativa proprietà **Enabled** è impostata su **false** (o se ereditano questa impostazione da un controllo contenitore).

<a id="0.1__Toc245724854"></a><a id="0.1__Toc255587631"></a><a id="0.1__Toc256770142"></a>

## <a name="clientidmode-changes"></a>Modifiche a ClientIDMode

L'impostazione **ClientIDMode** in ASP.NET 4 consente di specificare il modo in cui ASP.NET genera l'attributo **ID** per gli elementi HTML. Nelle versioni precedenti di ASP.NET, il comportamento predefinito era equivalente all'impostazione **AutoID** di **ClientIDMode**. Tuttavia, l'impostazione predefinita è ora **stimabile**.

Se si usa Visual Studio 2010 per aggiornare l'applicazione da ASP.NET 2,0 o ASP.NET 3,5, lo strumento aggiunge automaticamente un'impostazione al file `Web.config` che conserva il comportamento delle versioni precedenti del .NET Framework. Se tuttavia si aggiorna un'applicazione modificando il pool di applicazioni in IIS per impostare come destinazione .NET Framework 4, ASP.NET usa la nuova modalità per impostazione predefinita. Per disabilitare la modalità nuovo ID client, aggiungere la seguente impostazione nel file di `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample2.xml)]

<a id="0.1__Toc245724855"></a><a id="0.1__Toc255587632"></a><a id="0.1__Toc256770143"></a>

## <a name="htmlencode-and-urlencode-now-encode-single-quotation-marks"></a>HtmlEncode e UrlEncode ora codificano le virgolette singole

In ASP.NET 4, i metodi **HtmlEncode** e **UrlEncode** delle classi **HttpUtility** e **HttpServerUtility** sono stati aggiornati per codificare il carattere virgoletta singola (') come indicato di seguito:

- Il metodo **HtmlEncode** codifica le istanze delle virgolette singole come '.
- Il metodo **UrlEncode** codifica le istanze delle virgolette singole come %27.

<a id="0.1__Toc255587633"></a><a id="0.1__Toc256770144"></a><a id="0.1__Toc245724856"></a>

## <a name="aspnet-page-aspx-parser-is-stricter"></a>Il parser di pagina (. aspx) ASP.NET è più restrittivo

Il parser di pagina per le pagine ASP.NET (file`.aspx`) e i controlli utente (file`.ascx`) è più restrittivo in ASP.NET 4 e rifiuterà più istanze di markup non valido. Ad esempio, i due frammenti di codice seguenti analizzano correttamente le versioni precedenti di ASP.NET, ma ora generano errori del parser in ASP.NET 4.

[!code-aspx[Main](breaking-changes/samples/sample3.aspx)]

Si noti il punto e virgola non valido alla fine del tag **HiddenField** .

[!code-aspx[Main](breaking-changes/samples/sample4.aspx)]

Si noti l'attributo di **stile** non chiuso che viene eseguito nell'attributo **CssClass** .

<a id="0.1__Toc255587634"></a><a id="0.1__Toc256770145"></a>

## <a name="browser-definition-files-updated"></a>File di definizione del browser aggiornati

I file di definizione del browser sono stati aggiornati per includere informazioni su browser e dispositivi nuovi e aggiornati. Sono stati rimossi browser e dispositivi precedenti come Netscape Navigator e sono stati aggiunti browser e dispositivi più recenti come Google Chrome e Apple iPhone.

Se l'applicazione in uso contiene definizioni del browser personalizzate che derivano da una delle definizioni del browser rimosse, viene visualizzato un errore. Se, ad esempio, la cartella `App_Browsers` contiene una definizione del browser che eredita dalla definizione del browser IE2, verrà visualizzato il messaggio di errore di configurazione seguente:

- Impossibile trovare l'elemento browser o gateway con ID ' IE2'.

> [!NOTE]
> L'oggetto **HttpBrowserCapabilities** , esposto dalla proprietà **Request. browser** della pagina, è determinato dai file delle definizioni del browser. Pertanto, le informazioni restituite tramite l'accesso a una proprietà di questo oggetto in ASP.NET 4 potrebbero essere diverse dalle informazioni restituite in una versione precedente di ASP.NET.

È possibile ripristinare i file di definizione del browser precedenti copiando i file di definizione del browser dalla seguente cartella:

[!code-console[Main](breaking-changes/samples/sample5.cmd)]

Copiare i file nella cartella `\CONFIG\Browsers` corrispondente per ASP.NET 4. Dopo aver copiato i file, eseguire lo strumento da riga di comando ASPNET\_RegBrowsers. exe.

<a id="0.1__Toc255587635"></a><a id="0.1__Toc256770146"></a>

## <a name="systemwebmobiledll-removed-from-root-web-configuration-file"></a>System. Web. mobile. dll rimosso dal file di configurazione Web radice

Nelle versioni precedenti di ASP.NET, un riferimento all'assembly System. Web. mobile. dll era incluso nel file di `Web.config` radice nella sezione degli **assembly** in. Per migliorare le prestazioni, il riferimento a questo assembly è stato rimosso.

L'assembly System. Web. mobile. dll è incluso in ASP.NET 4, ma è deprecato. Se si desidera utilizzare i tipi dall'assembly System. Web. mobile. dll, aggiungere un riferimento a questo assembly nel file di `Web.config` radice o in un file di `Web.config` dell'applicazione. Se ad esempio si vuole usare uno dei controlli per dispositivi mobili ASP.NET (deprecati), è necessario aggiungere un riferimento all'assembly System. Web. mobile. dll al file di `Web.config`.

<a id="0.1__Toc245724857"></a><a id="0.1__Toc255587636"></a><a id="0.1__Toc256770147"></a>

## <a name="aspnet-request-validation"></a>Convalida richiesta ASP.NET

La funzionalità di convalida delle richieste in ASP.NET fornisce un certo livello di protezione predefinita contro gli attacchi di scripting (XSS) tra siti. Nelle versioni precedenti di ASP.NET, la convalida delle richieste era abilitata per impostazione predefinita. Tuttavia, si applica solo alle pagine ASP.NET (`.aspx` file e ai relativi file di classe) e solo quando tali pagine sono in esecuzione.

In ASP.NET 4, per impostazione predefinita, la convalida delle richieste è abilitata per tutte le richieste, perché è abilitata prima della fase **BeginRequest** di una richiesta HTTP. Di conseguenza, la convalida della richiesta si applica alle richieste per tutte le risorse ASP.NET, non solo alle richieste di pagine aspx. Sono incluse richieste quali chiamate al servizio Web e gestori HTTP personalizzati. La convalida della richiesta è attiva anche quando i moduli HTTP personalizzati leggono il contenuto di una richiesta HTTP.

Di conseguenza, è possibile che si verifichino errori di convalida delle richieste per le richieste che in precedenza non hanno attivato errori. Per ripristinare il comportamento della funzionalità di convalida delle richieste ASP.NET 2,0, aggiungere la seguente impostazione nel file di `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample6.xml)]

Tuttavia, si consiglia di analizzare gli eventuali errori di convalida delle richieste per determinare se i gestori, i moduli o altro codice personalizzato accedano a input HTTP potenzialmente non sicuri che potrebbero essere vettori di attacco XSS.

<a id="0.1__Toc245724858"></a><a id="0.1__Toc255587637"></a><a id="0.1__Toc256770148"></a>

## <a name="default-hashing-algorithm-is-now-hmacsha256"></a>L'algoritmo hash predefinito è ora HMACSHA256

Per la protezione di dati come i cookie di autenticazione modulo e lo stato di visualizzazione, ASP.NET usa sia la crittografia che gli algoritmi hash. Per impostazione predefinita, ASP.NET 4 USA ora l'algoritmo HMACSHA256 per le operazioni hash sui cookie e sullo stato di visualizzazione. Nelle versioni precedenti di ASP.NET è stato utilizzato l'algoritmo HMACSHA1 meno recente.

Le applicazioni potrebbero essere interessate se si eseguono ambienti misti ASP.NET 2.0/ASP. NET 4, in cui i dati, ad esempio i cookie di autenticazione basata su form, devono usare le versioni di across.NET Framework. Per configurare un'applicazione Web ASP.NET 4 in modo che usi l'algoritmo HMACSHA1 precedente, aggiungere la seguente impostazione nel file di `Web.config`:

[!code-xml[Main](breaking-changes/samples/sample7.xml)]

<a id="0.1__Toc245724859"></a><a id="0.1__Toc255587638"></a><a id="0.1__Toc256770149"></a>

## <a name="configuration-errors-related-to-new-aspnet-4-root-configuration"></a>Errori di configurazione correlati alla nuova configurazione radice di ASP.NET 4

I file di configurazione radice (il file `machine.config` e il file `Web.config` radice) per il .NET Framework 4 (e quindi ASP.NET 4) sono stati aggiornati in modo da includere la maggior parte delle informazioni di configurazione standard che in ASP.NET 3,5 sono state trovate nei file dell'applicazione `Web.config`. A causa della complessità dei sistemi di configurazione gestiti IIS 7 e IIS 7,5, l'esecuzione di applicazioni ASP.NET 3,5 in ASP.NET 4 e in IIS 7 e IIS 7,5 può comportare errori di configurazione di ASP.NET o IIS.

È consigliabile aggiornare le applicazioni ASP.NET 3,5 a ASP.NET 4 usando gli strumenti di aggiornamento del progetto in Visual Studio 2010, se possibile. Visual Studio 2010 modifica automaticamente il file di `Web.config` dell'applicazione ASP.NET 3,5 in modo da contenere le impostazioni appropriate per ASP.NET 4.

Tuttavia, è uno scenario supportato per l'esecuzione di applicazioni ASP.NET 3,5 usando il .NET Framework 4 senza ricompilazione. In tal caso, potrebbe essere necessario modificare manualmente il file di `Web.config` dell'applicazione prima di eseguire l'applicazione in .NET Framework 4 e in IIS 7 o IIS 7,5.

Nelle due sezioni successive vengono descritte le modifiche che potrebbero essere necessarie per diverse combinazioni di software.

**Windows Vista SP1 o Windows Server 2008 SP1, in cui non sono installati gli hotfix KB958854 né SP2.** In questa configurazione, il sistema di configurazione di IIS 7 unisce erroneamente la configurazione gestita di un'applicazione confrontando il file di `Web.config` a livello di applicazione con i file `machine.config` ASP.NET 2,0. Per questo motivo, i file di `Web.config` a livello di applicazione della .NET Framework 3,5 o versioni successive devono disporre di una definizione della sezione di configurazione **System. Web. Extensions** (l'elemento) per non causare un errore di convalida di IIS 7.

Tuttavia, le voci di file `Web.config` a livello di applicazione che non corrispondono esattamente alle definizioni originali della sezione di configurazione introdotte con Visual Studio 2008 provocheranno errori di configurazione ASP.NET. (Le voci di configurazione predefinite generate da Visual Studio 2008 funzionano correttamente). Un problema comune è che i file di `Web.config` modificati manualmente lasciano gli attributi di configurazione **allowDefinition** e **RequirePermission** che si trovano in varie definizioni di sezione di configurazione. Ciò provoca una mancata corrispondenza tra la sezione di configurazione abbreviata nei file di `Web.config` a livello di applicazione e la definizione completa nel file ASP.NET 4 `machine.config`. Di conseguenza, in fase di esecuzione, il sistema di configurazione ASP.NET 4 genera un errore di configurazione.

**Windows Vista SP2, Windows Server 2008 SP2, Windows 7, Windows Server 2008 R2 e Windows Vista SP1 e Windows Server 2008 SP1 in cui è installato l'hotfix KB958854.**

In questo scenario, il sistema di configurazione nativa IIS 7 e IIS 7,5 restituisce un errore di configurazione perché esegue un confronto di testo sull'attributo di **tipo** definito per un gestore della sezione di configurazione gestita. Poiché tutti i file di `Web.config` generati da Visual Studio 2008 e Visual Studio 2008 SP1 hanno "3,5" nella stringa di tipo per il sistema. i gestori delle sezioni di configurazione **Web. Extensions** (e related) e, poiché il file ASP.NET 4 `machine.config` ha "4,0" nell'attributo **Type** per gli stessi gestori delle sezioni di configurazione, le applicazioni generate in Visual Studio 2008 o Visual Studio 2008 SP1 hanno sempre esito negativo per la convalida della configurazione in iis 7 e IIS 7,5.

<a id="0.1__Toc251910248"></a>

### <a name="resolving-these-issues"></a>Risoluzione di questi problemi

La soluzione alternativa per il primo scenario consiste nell'aggiornare il file di `Web.config` a livello di applicazione includendo il testo di configurazione standard da un file `Web.config` generato automaticamente da Visual Studio 2008.

Una soluzione alternativa per il primo scenario consiste nell'installare il Service Pack 2 per Vista o Windows Server 2008 nel computer in uso o per installare l'hotfix KB958854 ([https://support.microsoft.com/kb/958854](https://support.microsoft.com/kb/958854)) per correggere il comportamento di Unione di configurazione errato del sistema di configurazione IIS. Tuttavia, dopo aver eseguito una di queste azioni, si verificherà probabilmente un errore di configurazione dell'applicazione a causa del problema descritto per il secondo scenario.

La soluzione alternativa per il secondo scenario consiste nell'eliminare o impostare come commento tutte le definizioni della sezione di configurazione **System. Web. Extensions** e le definizioni dei gruppi di sezioni di configurazione dal file di `Web.config` a livello di applicazione. Queste definizioni si trovano in genere nella parte superiore del file di `Web.config` a livello di applicazione e possono essere identificate dall'elemento **configSections** e dai relativi elementi figlio.

Per entrambi gli scenari, è consigliabile eliminare manualmente anche la sezione **System. CodeDom** , sebbene questa operazione non sia obbligatoria.

<a id="0.1__Toc252995490"></a><a id="0.1__Toc255587639"></a><a id="0.1__Toc256770150"></a><a id="0.1__Toc245724860"></a>

## <a name="aspnet-4-child-applications-fail-to-start-when-under-aspnet-20-or-aspnet-35-applications"></a>ASP.NET 4 le applicazioni figlio non vengono avviate quando si usa ASP.NET 2,0 o ASP.NET 3,5

Le applicazioni ASP.NET 4 configurate come elementi figlio di applicazioni che eseguono versioni precedenti di ASP.NET potrebbero non avviarsi a causa di errori di compilazione o di configurazione. Nell'esempio seguente viene illustrata una struttura di directory per un'applicazione interessata.

`/parentwebapp` (configurato per l'uso di ASP.NET 2,0 o ASP.NET 3,5)  
`/childwebapp` (configurato per l'uso di ASP.NET 4)

L'applicazione nella cartella `childwebapp` non verrà avviata in IIS 7 o IIS 7,5 e segnalerà un errore di configurazione. Nel testo dell'errore verrà incluso un messaggio simile al seguente:

- `The requested page cannot be accessed because the related configuration data for the page is invalid.`

- `The configuration section 'configSections' cannot be read because it is missing a section declaration.`

In IIS 6 non sarà possibile avviare anche l'applicazione nella cartella `childwebapp`, ma verrà segnalato un errore diverso. Ad esempio, il testo dell'errore potrebbe indicare quanto segue:

- `The value for the 'compilerVersion' attribute in the provider options must be 'v4.0' or later if you are compiling for version 4.0 or later of the .NET Framework. To compile this Web application for version 3.5 or earlier of the .NET Framework, remove the 'targetFramework' attribute from the element of the Web.config file`

Questi scenari si verificano perché le informazioni di configurazione dell'applicazione padre nella cartella `parentwebapp` fanno parte della gerarchia delle informazioni di configurazione che determinano le impostazioni di configurazione finale Unite che vengono utilizzate dall'applicazione Web figlio nella cartella `childwebapp`. A seconda che l'applicazione Web ASP.NET 4 sia in esecuzione in IIS 7 (o IIS 7,5) o in IIS 6, il sistema di configurazione di IIS o il sistema di compilazione ASP.NET 4 restituirà un errore.

I passaggi da eseguire per risolvere il problema e per far funzionare l'applicazione ASP.NET 4 figlio variano a seconda che l'applicazione ASP.NET 4 venga eseguita in IIS 6 o IIS 7 (o IIS 7,5).

### <a name="step-1-iis-7-or-iis-75-only"></a>Passaggio 1 (solo IIS 7 o IIS 7,5)

Questo passaggio è necessario solo per i sistemi operativi che eseguono IIS 7 o IIS 7,5, che include Windows Vista, Windows Server 2008, Windows 7 e Windows Server 2008 R2.

Spostare la definizione **configSections** nel file di `Web.config` dell'applicazione padre, ovvero l'applicazione che esegue ASP.NET 2,0 o ASP.NET 3,5, nel file `Web.config` radice per the.NET Framework 2,0. Il sistema di configurazione nativa IIS 7 e IIS 7,5 analizza l'elemento **configSections** quando unisce la gerarchia dei file di configurazione. Se si trasferisce la definizione **configSections** dal file di `Web.config` dell'applicazione Web padre al file `Web.config` radice, l'elemento viene nascosto dal processo di merge della configurazione che si verifica per l'applicazione ASP.NET 4 figlio.

In un sistema operativo a 32 bit o per i pool di applicazioni a 32 bit, il file radice `Web.config` per ASP.NET 2,0 e ASP.NET 3,5 si trova in genere nella cartella seguente:

`C:\Windows\Microsoft.NET\Framework\v2.0.50727\CONFIG`

In un sistema operativo a 64 bit o per i pool di applicazioni a 64 bit, il file radice `Web.config` per ASP.NET 2,0 e ASP.NET 3,5 si trova in genere nella cartella seguente:

`C:\Windows\Microsoft.NET\Framework64\v2.0.50727\CONFIG`

Se si eseguono applicazioni Web a 32 bit e a 64 bit in un computer a 64 bit, è necessario spostare l'elemento **configSections** in file radice `Web.config` sia per i sistemi a 32 bit che per quelli a 64 bit.

Quando si inserisce l'elemento **configSections** nel file di `Web.config` radice, incollare la sezione immediatamente dopo l'elemento di **configurazione** . Nell'esempio seguente viene illustrata la parte superiore del file di `Web.config` radice dovrebbe essere simile al termine dello stato di trasferimento degli elementi.

> [!NOTE]
> Nell'esempio seguente, le righe sono state incapsulate per migliorare la leggibilità.

[!code-xml[Main](breaking-changes/samples/sample8.xml)]

### <a name="step-2-all-versions-of-iis"></a>Passaggio 2 (tutte le versioni di IIS)

Questo passaggio è necessario se l'applicazione Web ASP.NET 4 figlio è in esecuzione in IIS 6 o IIS 7 (o IIS 7,5).

Nel file di `Web.config` dell'applicazione Web padre che esegue ASP.NET 2 o ASP.NET 3,5, aggiungere un tag **location** che specifichi in modo esplicito (per entrambi i sistemi di configurazione IIS e ASP.NET) che le voci di configurazione si applicano solo all'applicazione Web padre. Nell'esempio seguente viene illustrata la sintassi dell'elemento **location** da aggiungere:

[!code-xml[Main](breaking-changes/samples/sample9.xml)]

Nell'esempio seguente viene illustrato il modo in cui viene utilizzato il tag **location** per eseguire il wrapping di tutte le sezioni di configurazione, iniziando dalla sezione **appSettings** e terminando con **System. webserver** .

[!code-xml[Main](breaking-changes/samples/sample10.xml)]

Una volta completati i passaggi 1 e 2, le applicazioni Web ASP.NET 4 vengono avviate senza errori.

<a id="0.1__Toc252995491"></a><a id="0.1__Toc255587640"></a><a id="0.1__Toc256770151"></a>

## <a name="aspnet-4-web-sites-fail-to-start-on-computers-where-sharepoint-is-installed"></a>Impossibile avviare i siti Web di ASP.NET 4 nei computer in cui è installato SharePoint

I server Web che eseguono SharePoint dispongono di un file di `Web.config` distribuito alla radice di un sito Web di SharePoint, ad esempio `c:\inetpub\wwwroot\web.config` per il sito Web predefinito. In questo `Web.config` file SharePoint imposta un livello di attendibilità parziale personalizzato denominato WSS\_minimo.

Se si tenta di eseguire un sito Web ASP.NET 4 distribuito come figlio di questo tipo di sito Web di SharePoint, verrà visualizzato l'errore seguente:

`Could not find permission set named 'ASP.NET'.`

Questo errore si verifica perché l'infrastruttura ASP.NET 4 Code Access Security (CAS) Cerca un set di autorizzazioni denominato ASP.NET. Tuttavia, il file di configurazione con attendibilità parziale a cui viene fatto riferimento da WSS\_minimo non contiene alcun set di autorizzazioni con tale nome.

Attualmente non è disponibile una versione di SharePoint compatibile con ASP.NET. Di conseguenza, è consigliabile non tentare di eseguire un sito Web ASP.NET 4 come sito figlio sotto siti Web di SharePoint.

<a id="0.1__Toc255587641"></a><a id="0.1__Toc256770152"></a>

## <a name="the-httprequestfilepath-property-no-longer-includes-pathinfo-values"></a>La proprietà HttpRequest. FilePath non include più valori PathInfo

Le versioni precedenti di ASP.NET includevano un valore **PathInfo** nel valore restituito da diverse proprietà correlate al percorso di file, tra cui **HttpRequest. FilePath**, **HttpRequest. AppRelativeCurrentExecutionFilePath**e **HttpRequest. CurrentExecutionFilePath**. ASP.NET 4 non include più il valore **PathInfo** nei valori restituiti da queste proprietà. Le informazioni **PathInfo** sono invece disponibili in **HttpRequest. PathInfo**. Considerare ad esempio il frammento di URL seguente:

`/testapp/Action.mvc/SomeAction`

Nelle versioni precedenti di ASP.NET, le proprietà **HttpRequest** presentano i valori seguenti:

**HttpRequest. FilePath**: `/testapp/Action.mvc/SomeAction`

**HttpRequest. PathInfo**: (vuota)

In ASP.NET 4, le proprietà **HttpRequest** hanno invece i valori seguenti:

**HttpRequest. FilePath**: `/testapp/Action.mvc`

**HttpRequest. PathInfo**: `SomeAction`

<a id="0.1__Toc252995493"></a><a id="0.1__Toc255587642"></a><a id="0.1__Toc256770153"></a><a id="0.1__Toc245724861"></a>

## <a name="aspnet-20-applications-might-generate-httpexception-errors-that-reference-eurlaxd"></a>Le applicazioni ASP.NET 2,0 possono generare errori HttpException che fanno riferimento a eurl. axd

Dopo l'abilitazione di ASP.NET 4 in IIS 6, le applicazioni ASP.NET 2.0 in esecuzione su IIS 6 (in Windows Server 2003 o Windows Server 2003 R2) possono generare errori come il seguente:

`System.Web.HttpException: Path '/[yourApplicationRoot]/eurl.axd/[Value]' was not found.`

Questo errore si verifica perché quando ASP.NET rileva che un sito Web è configurato per l'uso di ASP.NET 4, un componente nativo di ASP.NET 4 passa un URL con estensione alla parte gestita di ASP.NET per un'ulteriore elaborazione. Tuttavia, se le directory virtuali che si trovano al di sotto di un sito Web ASP.NET 4 sono configurate per l'uso di ASP.NET 2,0, l'elaborazione dell'URL senza estensione in questo modo restituisce un URL modificato che contiene la stringa "eurl. axd". Questo URL modificato viene quindi inviato all'applicazione ASP.NET 2,0. ASP.NET 2,0 non è in grado di riconoscere il formato "eurl. axd". ASP.NET 2,0 tenta quindi di trovare un file denominato `eurl.axd` ed eseguirlo. Poiché non esiste alcun file di questo tipo, la richiesta ha esito negativo con un'eccezione **HttpException** .

Per risolvere questo problema, è possibile usare una delle opzioni seguenti.

### <a name="option-1"></a>Opzione 1

Se ASP.NET 4 non è necessario per eseguire il sito Web, modificare il mapping del sito in modo da usare ASP.NET 2,0.

### <a name="option-2"></a>Opzione 2

Se è necessario ASP.NET 4 per eseguire il sito Web, spostare le directory virtuali ASP.NET 2,0 figlio in un sito Web diverso mappato a ASP.NET 2,0.

### <a name="option-3"></a>Opzione 3

Se non è pratico eseguire il mapping del sito Web a ASP.NET 2,0 o modificare il percorso di una directory virtuale, disabilitare in modo esplicito l'elaborazione degli URL senza estensione in ASP.NET 4. Utilizzare la procedura seguente:

1. Nel registro di sistema di Windows aprire il nodo seguente:

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ASP.NET\4.0.30319.0`

1. Creare un nuovo valore **DWORD** denominato **EnableExtensionlessUrls**.
2. Impostare **EnableExtensionlessUrls** su 0. In questo modo viene disabilitato il comportamento degli URL con estensione.
3. Salvare il valore del registro di sistema e chiudere l'editor del registro di sistema.
4. Eseguire lo strumento da riga di comando **iisreset** , che fa in modo che IIS legga il nuovo valore del registro di sistema.

> [!NOTE]
> L'impostazione di **EnableExtensionlessUrls** su 1 Abilita il comportamento degli URL con estensione. Questa è l'impostazione predefinita se non viene specificato alcun valore.

<a id="0.1__Toc252995494"></a><a id="0.1__Toc255587643"></a><a id="0.1__Toc256770154"></a><a id="0.1__Toc245724862"></a>

## <a name="event-handlers-might-not-be-not-raised-in-a-default-document-in-iis-7-or-iis-75-integrated-mode"></a>I gestori eventi potrebbero non essere generati in un documento predefinito in modalità integrata IIS 7 o IIS 7,5

ASP.NET 4 include modifiche che consentono di modificare la modalità di rendering dell'attributo **azione** dell'elemento **modulo** HTML quando un URL senza estensione viene risolto in un documento predefinito. Un esempio di URL senza estensione che si risolve in un documento predefinito è [http://contoso.com/](http://contoso.com/), ottenendo una richiesta di [http://contoso.com/Default.aspx](http://contoso.com/Default.aspx).

ASP.NET 4 ora esegue il rendering del valore dell'attributo **azione** dell'elemento **modulo** HTML come stringa vuota quando viene effettuata una richiesta a un URL senza estensione a cui è stato eseguito il mapping di un documento predefinito. Nelle versioni precedenti di ASP.NET, ad esempio, una richiesta di [http://contoso.com](http://contoso.com) comporterebbe una richiesta di `Default.aspx`. In tale documento verrà eseguito il rendering del tag di apertura del **modulo** come nell'esempio seguente:

`<form action="Default.aspx" />`

In ASP.NET 4, una richiesta di [http://contoso.com](http://contoso.com) inoltre comporta una richiesta di `Default.aspx`. Tuttavia, ASP.NET ora esegue il rendering del tag **form** di apertura HTML come nell'esempio seguente:

`<form action="" />`

Questa differenza nel modo in cui viene eseguito il rendering dell'attributo **azione** può causare modifiche minime all'elaborazione di un post del modulo da parte di IIS e ASP.NET. Quando l'attributo dell' **azione** è una stringa vuota, l'oggetto **DefaultDocumentModule** di IIS creerà una richiesta figlio per `Default.aspx`. Nella maggior parte dei casi, questa richiesta figlio è trasparente per il codice dell'applicazione e la pagina `Default.aspx` viene eseguita normalmente.

Tuttavia una potenziale interazione tra il codice gestito e la modalità integrata di IIS 7 o IIS 7.5 può interrompere il funzionamento corretto delle pagine gestite con estensione aspx durante la richiesta figlio. Se si verificano le seguenti condizioni, la richiesta figlio a un documento `Default.aspx` comporterà un errore o un comportamento imprevisto:

1. Una pagina aspx viene inviata al browser con l'attributo **azione** dell'elemento **modulo** impostato su "".
2. Viene eseguito il postback del modulo a ASP.NET.
3. Un modulo HTTP gestito legge una parte del corpo dell'entità. Un modulo, ad esempio, legge **Request. Form** o **Request. params**. Di conseguenza il corpo dell'entità della richiesta POST viene letto nella memoria gestita. Pertanto il corpo dell'entità non è più disponibile per i moduli di codice nativo che sono in esecuzione in modalità integrata IIS 7 o IIS 7.5.
4. L'oggetto **DefaultDocumentModule** di IIS viene infine eseguito e crea una richiesta figlio al documento `Default.aspx`. Tuttavia poiché il corpo dell'entità è già stato letto da un frammento di codice gestito, non è disponibile alcun corpo di entità da inviare alla richiesta figlio.
5. Quando la pipeline HTTP viene eseguita per la richiesta figlio, il gestore per `.aspx` file viene eseguito durante la fase di esecuzione del gestore.
6. Poiché non è presente alcun corpo di entità, non sono presenti variabili di form e lo stato di visualizzazione e pertanto non sono disponibili informazioni per il gestore della pagina aspx per determinare quale evento, se presente, deve essere generato. Pertanto non viene eseguito nessun gestore dell'evento di postback per la pagina con estensione aspx interessata.

È possibile aggirare questo comportamento nei modi seguenti:

- Identificare il modulo HTTP che accede al corpo dell'entità della richiesta durante le richieste di documenti predefinite e determinare se può essere configurato per l'esecuzione solo per le richieste gestite. In modalità integrata per IIS 7 e IIS 7,5, i moduli HTTP possono essere contrassegnati per l'esecuzione solo per le richieste gestite aggiungendo l'attributo seguente alla voce **System. webserver/Modules** del modulo:

- `precondition="managedHandler"`

- Questa impostazione Disabilita il modulo per le richieste che IIS 7 e IIS 7,5 determinano come richieste non gestite. Per le richieste di documenti predefinite, la prima richiesta è un URL senza estensione. IIS non esegue pertanto alcun modulo gestito contrassegnato con una precondizione del gestore gestito durante l'elaborazione della richiesta iniziale. Di conseguenza, i moduli gestiti non leggono accidentalmente il corpo dell'entità e quindi il corpo dell'entità è ancora disponibile e viene passato alla richiesta figlio e al documento predefinito.

- Se i moduli HTTP problematici devono essere eseguiti per tutte le richieste (per i file statici, per gli URL senza estensione che si risolvono nell'oggetto **DefaultDocumentModule** , per le richieste gestite e così via), modificare le pagine aspx interessate impostando in modo esplicito la proprietà **Action** del controllo **System. Web. UI. HtmlControls. HtmlForm** della pagina su una stringa non vuota. Se, ad esempio, il documento predefinito è `Default.aspx`, modificare il codice della pagina per impostare in modo esplicito la proprietà **Action** del controllo **HtmlForm** su "default. aspx".

<a id="0.1__Toc255587644"></a><a id="0.1__Toc256770155"></a>

## <a name="changes-to-the-aspnet-code-access-security-cas-implementation"></a>Modifiche all'implementazione di sicurezza dall'accesso di codice (CAS) ASP.NET

ASP.NET 2,0 e, per estensione, le funzionalità di ASP.NET aggiunte in 3,5, usare il modello di sicurezza dall'accesso di codice .NET Framework 1,1 e 2,0. Tuttavia l'implementazione di CAS in ASP.NET 4 è stata modificata in modo sostanziale. Di conseguenza, le applicazioni ASP.NET con attendibilità parziale basate su codice attendibile in esecuzione nel Global Assembly Cache (GAC) potrebbero non riuscire con diverse eccezioni di sicurezza. Anche le applicazioni parzialmente attendibili che si basano su modifiche estese ai criteri CAS del computer potrebbero avere esito negativo con eccezioni di sicurezza.

È possibile ripristinare il comportamento di ASP.NET 1,1 e 2,0 usando il nuovo attributo **legacyCasModel** nell'elemento di configurazione **trust** , come illustrato nell'esempio seguente:

`<trust level= "Medium" legacyCasModel="true" />`

Quando si ripristina il modello CAS legacy, sono abilitati i seguenti comportamenti CAS precedenti:

- Il criterio CAS del computer viene rispettato.
- Sono consentiti più set di autorizzazioni diversi in un singolo dominio applicazione.
- Le asserzioni di autorizzazioni esplicite non sono necessarie per gli assembly nella GAC che vengono richiamati quando nello stack è presente solo ASP.NET o altro codice .NET Framework.

Non è possibile ripristinare uno scenario nel .NET Framework 4: le applicazioni con attendibilità parziale non Web non possono più chiamare determinate API in System. Web. dll e System. Web. Extensions. dll. Nelle versioni precedenti del .NET Framework era possibile concedere in modo esplicito le autorizzazioni **AspNetHostingPermission** alle applicazioni con attendibilità parziale non Web. Queste applicazioni possono quindi utilizzare **System. Web. HttpUtility**, i tipi negli spazi dei nomi **System. Web. ClientServices.\*** e i tipi correlati a appartenenza, ruoli e profili. La chiamata di questi tipi da applicazioni con attendibilità parziale non Web non è più supportata nel .NET Framework 4.

> [!NOTE]
> La funzionalità **HtmlEncode** e **HtmlDecode** della classe **System. Web. HttpUtility** è stata spostata nella nuova classe .NET Framework 4 **System .NET. WebUtility** . Se questa è l'unica funzionalità ASP.NET in uso, modificare il codice dell'applicazione per usare la nuova classe **WebUtility** .

Di seguito è riportato un riepilogo generale delle modifiche apportate all'implementazione CAS predefinita in ASP.NET 4:

- I domini applicazione ASP.NET sono ora domini applicazione omogenei. In un dominio applicazione sono disponibili solo i set di concessioni parzialmente attendibili e con attendibilità totale.
- I set di concessioni con attendibilità parziale ASP.NET sono indipendenti da qualsiasi criterio CAS a livello di organizzazione, a livello di computer o a livello di utente.
- Gli assembly ASP.NET forniti in 3,5 e 3,5 SP1 sono stati convertiti in modo da usare il modello di trasparenza .NET Framework 4.
- L'uso dell'attributo **AspNetHostingPermission** di ASP.NET è stato notevolmente ridotto. La maggior parte delle istanze di questo attributo è stata rimossa dalle API ASP.NET pubbliche.
- Gli assembly compilati in modo dinamico creati dai provider di compilazione ASP.NET sono stati aggiornati per contrassegnare in modo esplicito gli assembly come trasparenti.
- Tutti gli assembly ASP.NET sono ora contrassegnati in modo tale che l'attributo APTCA viene rispettato solo negli ambienti di hosting Web. Gli ambienti di hosting non Web parzialmente attendibili come ClickOnce non saranno in grado di effettuare chiamate negli assembly ASP.NET.

Per ulteriori informazioni sul nuovo modello di sicurezza dall'accesso di codice ASP.NET 4, vedere [utilizzo della sicurezza dall'accesso di codice in applicazioni ASP.NET](https://msdn.microsoft.com/library/dd984947%28VS.100%29.aspx) sul sito Web MSDN.

<a id="0.1__Toc256770156"></a><a id="0.1__Toc245724863"></a><a id="0.1__Toc252995496"></a><a id="0.1__Toc255587645"></a><a id="0.1__Toc245724864"></a>

## <a name="membershipuser-and-other-types-in-the-systemwebsecurity-namespace-have-been-moved"></a>MembershipUser e altri tipi nello spazio dei nomi System. Web. Security sono stati spostati

Alcuni tipi utilizzati nell'appartenenza a ASP.NET sono stati spostati da `System.Web.dll` al nuovo assembly System. Web. ApplicationServices. dll. I tipi sono stati spostati per risolvere dipendenze dei livelli di architettura tra i tipi nel client e negli SKU estesi di .NET Framework.

I progetti di siti Web non presentano problemi in seguito allo spostamento di questi tipi, perché System. Web. ApplicationServices. dll è stato aggiunto all'elenco degli assembly a cui si fa riferimento usato per impostazione predefinita dal sistema di compilazione ASP.NET. Se si aggiorna un progetto di sito Web creato con una versione precedente di ASP.NET a ASP.NET 4 aprendolo in Visual Studio 2010, il progetto verrà compilato senza errori.

Analogamente, se si aggiorna un progetto di applicazione Web creato in una versione precedente di ASP.NET a ASP.NET 4 aprendolo in Visual Studio 2010, il processo di aggiornamento aggiunge un riferimento a System. Web. ApplicationServices. dll al progetto. I progetti di applicazione Web aggiornati vengono pertanto compilati anche senza errori.

I file compilati (binari) creati con versioni precedenti di ASP.NET vengono eseguiti anche senza errori in ASP.NET 4, anche se i tipi di appartenenza sono stati spostati in un assembly diverso. Le informazioni di invio dei tipi sono state aggiunte alla versione ASP.NET 4 di `System.Web.dll` che indirizza automaticamente i riferimenti di run-time per questi tipi al nuovo percorso per i tipi.

Tuttavia, le librerie di classi che usano tipi di appartenenza specifici e aggiornate da versioni precedenti di ASP.NET non riusciranno a compilare se usate in un progetto ASP.NET 4. Un progetto di libreria di classi, ad esempio, potrebbe non riuscire a compilare e segnalare un errore come il seguente:

- `The type 'System.Web.Security.MembershipUser' is defined in an assembly that is not referenced. You must add a reference to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`

- `The type name 'MembershipUser' could not be found. This type has been forwarded to assembly 'System.Web.ApplicationServices, Version=4.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'. Consider adding a reference to that assembly.`

Per aggirare questo problema, è possibile aggiungere un riferimento nel progetto di libreria di classi a System. Web. ApplicationServices. dll.

Nell'elenco seguente sono illustrati i tipi *System. Web. Security* spostati da `System.Web.dll` a System. Web. ApplicationServices. dll:

- *System. Web. Security. MembershipCreateStatus*
- *System. Web. Security. Membership. CreateUserException*
- *System. Web. Security. MembershipPasswordException*
- *System. Web. Security. dell'attributo MembershipPasswordFormat*
- *System. Web. Security. MembershipProvider*
- *System. Web. Security. MembershipProviderCollection*
- *System. Web. Security. MembershipUser*
- *System. Web. Security. MembershipUsercollection*
- *System. Web. Security. MembershipValidatePasswordEventHandler*
- *System. Web. Security. ValidatePasswordEventArgs*
- *System. Web. Security. RoleProvider*
- <a id="0.1_a"></a>*System. Web. Configuration. MembershipPasswordCompatibilityMode*

<a id="0.1__Toc256770157"></a>

## <a name="output-caching-changes-to-vary--http-header"></a>Modifiche della cache di output per variare \* intestazione HTTP

In ASP.NET 1,0, un bug provocava la memorizzazione nella cache di pagine che specificavano `Location="ServerAndClient"` come impostazione della cache di output per emettere un'intestazione HTTP `Vary:*` nella risposta. Questa intestazione comunicava ai browser client di non memorizzare mai la pagina nella cache in locale.

In ASP.NET 1,1 è stato aggiunto il metodo **System. Web. HttpCachePolicy. SetOmitVaryStar** , che è possibile chiamare per disattivare l'intestazione `Vary:*`. Questo metodo è stato scelto perché la modifica dell'intestazione HTTP emessa era considerata una modifica potenzialmente sostanziale al momento. Tuttavia, gli sviluppatori hanno confuso il comportamento in ASP.NET e i report sui bug indicano che gli sviluppatori non sono a conoscenza del comportamento **SetOmitVaryStar** esistente.

In ASP.NET 4 è stata presa la decisione di correggere il problema principale. L'intestazione HTTP `Vary:*` non viene più emessa dalle risposte che specificano la direttiva seguente:

`<%@OutputCache Location="ServerAndClient" %>`

Di conseguenza, **SetOmitVaryStar** non è più necessario per evitare l'intestazione `Vary:*`.

Nelle applicazioni che specificano `Location="ServerAndClient"` nella direttiva **@ OutputCache** in una pagina, verrà ora visualizzato il comportamento implicito dal nome del valore dell'attributo **location** , ovvero le pagine saranno memorizzabili nel browser senza che sia necessario chiamare il metodo **SetOmitVaryStar** .

Se le pagine dell'applicazione devono emettere `Vary:*`, chiamare il metodo **AppendHeader** , come nell'esempio seguente:

`HttpResponse.AppendHeader("Vary","*");`

In alternativa, è possibile modificare il valore dell'attributo **location** Caching output in "Server".

<a id="0.1__Toc255587646"></a><a id="0.1__Toc256770158"></a>

## <a name="systemwebsecurity-types-for-passport-are-obsolete"></a>I tipi System. Web. Security per Passport sono obsoleti

Il supporto Passport integrato in ASP.NET 2,0 è obsoleto e non è supportato per alcuni anni a causa delle modifiche apportate a Passport (ora LiveID). Di conseguenza, i cinque tipi correlati a Passport in **System. Web. Security** sono ora contrassegnati con l'attributo **ObsoleteAttribute** .

<a id="0.1__The_MenuItem.PopOutImageUrl_Propert"></a><a id="0.1__Toc256770159"></a>

## <a name="the-menuitempopoutimageurl-property-fails-to-render-an-image-in-aspnet-4"></a>La proprietà MenuItem. PopOutImageUrl non è in grado di eseguire il rendering di un'immagine in ASP.NET 4

In ASP.NET 3,5 la proprietà *MenuItem. PopOutImageUrl* consente di specificare l'URL di un'immagine visualizzata in una voce di menu per indicare che la voce di menu dispone di un sottomenu dinamico. Nell'esempio seguente viene illustrato come specificare questa proprietà nel markup in ASP.NET 3,5.

[!code-aspx[Main](breaking-changes/samples/sample11.aspx)]

In seguito a una modifica della progettazione in ASP.NET 4, non viene eseguito il rendering dell'output per *PopOutImageUrl* se la proprietà è impostata per la classe *MenuItem* . È invece necessario specificare un URL di immagine direttamente nel controllo *menu* usando la proprietà *StaticPopOutImageUrl* o la proprietà *DynamicPopOutImageUrl* . Quando si utilizza un menu statico, la proprietà *menu. StaticPopOutImageUrl* specifica l'URL di un'immagine visualizzata per indicare che la voce di menu statico dispone di un sottomenu, come illustrato nell'esempio seguente:

[!code-aspx[Main](breaking-changes/samples/sample12.aspx)]

Se si utilizza un menu dinamico, utilizzare la proprietà *menu. DynamicPopOutImageUrl* per specificare l'URL di un'immagine che indica che una voce di menu dinamico dispone di un sottomenu. L'esempio seguente è simile a quello precedente, ma Mostra come impostare la proprietà *DynamicPopOutImageUrl* per un menu dinamico.

[!code-aspx[Main](breaking-changes/samples/sample13.aspx)]

Se la proprietà *menu. DynamicPopOutImageUrl* non è impostata e la proprietà *menu. DynamicEnableDefaultPopOutImage* è impostata su *false*, non viene visualizzata alcuna immagine. Analogamente, se la proprietà *StaticPopOutImageUrl* non è impostata e la proprietà *StaticEnableDefaultPopOutImage* è impostata su *false*, non viene visualizzata alcuna immagine.

Quando si impostano i percorsi per queste proprietà, utilizzare una barra (/) anziché una barra rovesciata (\). Per altre informazioni, vedere [menu. StaticPopOutImageUrl e menu. DynamicPopOutImageUrl non riescono a eseguire il rendering delle immagini quando i percorsi contengono barre rovesciate](#0.1__Menu.StaticPopOutImageUrl_and_Menu. "_Menu. StaticPopOutImageUrl_and_Menu.") altrove in questo documento.

<a id="0.1__Menu.StaticPopOutImageUrl_and_Menu."></a><a id="0.1__Toc256770160"></a>

## <a name="menustaticpopoutimageurl-and-menudynamicpopoutimageurl-fail-to-render-images-when-paths-contain-backslashes"></a>Menu. StaticPopOutImageUrl e menu. DynamicPopOutImageUrl non riescono a eseguire il rendering delle immagini quando i percorsi contengono barre rovesciate

In ASP.NET 4, le immagini specificate usando le proprietà *menu. StaticPopOutImageUrl* e *menu. DynamicPopOutImageUrl* non vengono sottoposte a rendering se il percorso contiene le ciglia (\). Si tratta di una modifica rispetto alle versioni precedenti di ASP.NET.

Nell'esempio seguente di markup del controllo *menu* viene mostrato il set di proprietà *StaticPopOutImageUrl* usando un percorso che contiene una barra rovesciata. In ASP.NET 4 non viene eseguito il rendering dell'immagine specificata nella proprietà.

[!code-aspx[Main](breaking-changes/samples/sample14.aspx)]

Per correggere questo problema, modificare i valori dei percorsi specificati nelle proprietà *StaticPopOutImageUrl* e *DynamicPopOutImageUrl* per utilizzare le barre (/). Nell'esempio seguente viene illustrata questa modifica:

[!code-aspx[Main](breaking-changes/samples/sample15.aspx)]

Si noti che potrebbero essere interessate anche le applicazioni di cui è stata eseguita la migrazione da versioni precedenti di ASP.NET a ASP.NET 4, perché la proprietà *MenuItem. PopOutImageUrl* è stata modificata. Per ulteriori informazioni, vedere [la proprietà MenuItem. PopOutImageUrl non è in grado di eseguire il rendering di un'immagine in ASP.NET 4](#0.1__The_MenuItem.PopOutImageUrl_Propert "_The_MenuItem. PopOutImageUrl_Propert") altrove in questo documento.

<a id="0.1__Toc224729061"></a><a id="0.1__Toc255587647"></a><a id="0.1__Toc256770161"></a>

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Il presente documento è una versione preliminare e può essere modificato in modo sostanziale prima della versione finale commerciale del software qui descritto.

Le informazioni contenute nel presente documento rappresentano l'attuale opinione di Microsoft Corporation circa le problematiche discusse alla data della pubblicazione. Poiché Microsoft deve rispondere ai cambiamenti delle condizioni di mercato, il presente documento non deve essere interpretato quale un impegno da parte di Microsoft e Microsoft non può garantire l'accuratezza delle informazioni presentate dopo la data di pubblicazione.

Il presente white paper è fornito solo a scopi informativi. MICROSOFT NON RILASCIA ALCUNA GARANZIA, ESPRESSA, IMPLICITA O LEGALE, IN MERITO ALLE INFORMAZIONI DEL PRESENTE DOCUMENTO.

Il rispetto di tutte le leggi applicabili in materia di copyright è a esclusivo carico dell'utente. Senza limitare i diritti sanciti dal copyright, nessuna parte di questo documento può essere riprodotta, memorizzata o inserita in un sistema di ricerca o trasmessa in qualsiasi forma o mezzo (elettronico o meccanico, mediante fotocopia, registrazione o altro), per alcuno scopo, senza l'autorizzazione scritta di Microsoft Corporation.

Microsoft può essere titolare di brevetti, domande di brevetto, marchi, copyright o altri diritti di proprietà intellettuale relativi all'oggetto del presente documento. Salvo quanto espressamente previsto in un contratto scritto di licenza Microsoft, la consegna del presente documento non implica la concessione di alcuna licenza su tali brevetti, marchi, copyright o altra proprietà intellettuale.

Se non specificato diversamente, le aziende, le organizzazioni, i prodotti, i nomi di dominio, gli indirizzi di posta elettronica, i logo, le persone, le località e gli eventi riportati in questo documento sono fittizi e nessuna associazione con nessuna società, organizzazione, prodotto, nome di dominio, indirizzo di posta elettronica Indirizzo, logo, persona, luogo o evento è intenzionale o può essere dedotto.

© 2010 Microsoft Corporation. Tutti i diritti sono riservati.

Microsoft e Windows sono marchi registrati o marchi di Microsoft Corporation negli Stati Uniti e/o in altri paesi.

I nomi di società e prodotti reali citati nel presente documento possono essere marchi dei rispettivi proprietari.
