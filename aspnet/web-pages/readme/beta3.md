---
uid: web-pages/readme/beta3
title: File Leggimi della versione beta 3 di Web Matrix e Pagine Web ASP.NET (Razor) | Microsoft Docs
author: rick-anderson
description: File Leggimi di WebMatrix e pagine Web ASP.NET (Razor) Beta 3
ms.author: riande
ms.date: 01/10/2011
ms.assetid: ffa3d5c9-91e5-4da3-b409-560b0c7fbbf0
msc.legacyurl: /web-pages/readme/beta3
msc.type: content
ms.openlocfilehash: dc1d9237c04a7fcdbf4db6ccc8c36d255f6de003
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78628898"
---
# <a name="web-matrix-and-aspnet-web-pages-razor-beta-3-release-readme"></a>File Leggimi di WebMatrix e pagine Web ASP.NET (Razor) Beta 3

> File Leggimi di WebMatrix e pagine Web ASP.NET (Razor) Beta 3

9 novembre 2010

## <a name="contents"></a>Contenuto

- [Panoramica](#Overview)
- [Installazione](#Installation_Notes)
- [Nuove funzionalità, modifiche e problemi noti nella versione beta 3](#Known_Issues)

    - [Problemi di installazione di WebMatrix](#Known_Issues_Installation)
    - [ASP.NET Web Pages](#Known_Issues_ASPNET)
    - [SQL Server Compact](#Known_Issues_SQL_Server_Compact)
    - [Installazione di applicazioni](#Known_Issues_Installing_Applications)
    - [Pubblicazione di applicazioni](#Known_Issues_Publishing_Applications)
    - [Altri problemi](#Known_Issues_Other_Issues)
- [Ulteriori informazioni](#More_Info)

<a id="Overview"></a>

## <a name="overview"></a>Panoramica

> Microsoft WebMatrix beta è uno stack di sviluppo web gratuito che viene installato in pochi minuti. Integra un server Web con Framework di programmazione e database per creare un'unica esperienza integrata. È possibile usare WebMatrix beta per semplificare il codice, testare e pubblicare il proprio sito Web ASP.NET o PHP oppure è possibile usare WebMatrix beta per avviare un nuovo sito Web usando le app open source più diffuse, ad esempio DotNetNuke, Umbraco, WordPress o Joomla. WebMatrix beta usa lo stesso ambiente di server Web, motore di database e Framework che eseguirà il sito Web su Internet, il che rende la transizione dallo sviluppo alla produzione senza problemi.

<a id="Installation_Notes"></a>

## <a name="installation"></a>Installazione

> Per installare WebMatrix Beta 3, usare [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638). Dopo aver installato l'installazione guidata piattaforma Web, è possibile usarla per installare WebMatrix Beta 3.
> 
> Se si verificano problemi durante l'installazione, vedere [risoluzione dei problemi con installazione guidata piattaforma Web Microsoft](https://go.microsoft.com/fwlink/?LinkId=196212).

<a id="Installation_Notes0"></a>

## <a name="instructions-for-publishing-applications"></a>Istruzioni per la pubblicazione di applicazioni

> Vedere [le istruzioni dettagliate per la pubblicazione di applicazioni](https://go.microsoft.com/fwlink/?LinkID=196149)

<a id="Known_Issues"></a>

## <a name="new-features-changes-andknown-issues"></a>Nuove funzionalità, modifiche, problemi di andKnown

<a id="Known_Issues_Installation"></a>

### <a name="webmatrix-beta-3-installation"></a>Installazione di WebMatrix Beta 3

#### <a name="issue-webmatrix-beta-3-is-only-available-on-platforms-that-support-microsoft-net-framework-4"></a>Problema: WebMatrix Beta 3 è disponibile solo su piattaforme che supportano Microsoft .NET Framework 4

> Per la versione beta di WebMatrix è necessario il .NET Framework versione 4. In alcuni casi, il programma di installazione di WebMatrix beta consente di provare a eseguire l'installazione in una piattaforma che non fa parte del set di configurazione supportato. In particolare, Windows Vista senza l'aggiornamento di SP1 consentirà di iniziare l'installazione di WebMatrix beta, ma il componente .NET Framework 4 avrà esito negativo e bloccherà l'installazione.
> 
> **Soluzione alternativa**  
> Installare in una piattaforma supportata, che include:
> 
> - Windows 7
> - Windows Server 2008
> - Windows Server 2008 R2
> - Windows Vista SP1 o versione successiva
> - Windows XP SP3
> - Windows Server 2003 SP2

#### <a name="issue-cannot-install-webmatrix-beta-3-if-microsoft-visual-studio-2008-is-installed-without-microsoft-visual-studio-2008-sp1"></a>Problema: non è possibile installare WebMatrix Beta 3 Se Microsoft Visual Studio 2008 è installato senza Microsoft Visual Studio 2008 SP1

> **Soluzione alternativa**  
> Installare [Microsoft Visual Studio 2008 SP1](https://www.microsoft.com/downloads/details.aspx?FamilyId=FBEE1648-7106-44A7-9649-6D9F6D58056E&amp;displaylang=en) dall'area download Microsoft.

#### <a name="issue-some-assemblies-for-sql-server-compact-40-are-not-installed-in-the-gac"></a>Problema: alcuni assembly per SQL Server Compact 4,0 non sono installati nella GAC

> Gli assembly gestiti per SQL Server Compact 4,0 non vengono inseriti nella Global Assembly Cache (GAC) quando si installa SQL Server Compact 4,0 in un computer a 64 bit e il computer dispone solo del profilo client .NET Framework 3,5 SP1 installato. Gli assembly gestiti che non sono installati nella GAC sono:
> 
> - *System. Data. SqlServerCe. dll* (provider ADO.NET)
> - *System. Data. SqlServerCe. Entity. dll* (ADO.NET Entity Framework)
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4,0. Scaricare e installare la versione completa di .NET Framework 3,5 SP1 dal percorso seguente:  
>   
> [Microsoft .NET Framework 3,5 Service Pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
>   
> Reinstallare SQL Server Compact 4,0.

#### <a name="issue-cannot-uninstall-sql-server-compact-using-the-command-line"></a>Problema: Impossibile disinstallare SQL Server Compact tramite la riga di comando

> La disinstallazione di SQL Server Compact mediante opzioni della riga di comando non funziona in questa versione.
> 
> **Soluzione alternativa**  
> Usare *programmi e funzionalità* nel pannello di controllo di Windows per disinstallare Microsoft SQL Server Compact 4,0.

<a id="Known_Issues_ASPNET"></a>

### <a name="aspnet-web-pages"></a>Pagine Web ASP.NET

In questa sezione del documento vengono descritte le nuove funzionalità, le modifiche e i problemi noti relativi alla versione beta 3 di Pagine Web ASP.NET con sintassi Razor.

- [Nuove funzionalità](#NewFeatures)
- [Modifiche](#Changes)
- [Problemi](#Issues)

<a id="NewFeatures"></a>

#### <a name="new-features-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Nuove funzionalità della versione beta 3 per Pagine Web ASP.NET con sintassi Razor

#### <a name="new-htmlraw-method-renders-unencoded-markup"></a>Nuovo: il metodo "HTML. Raw" esegue il rendering di markup non codificato

> Il nuovo metodo di `Html.Raw` consente di eseguire il rendering del markup HTML come markup anziché eseguire il rendering dell'output codificato. Per impostazione predefinita, ASP.NET Razor codifica le stringhe prima di eseguirne il rendering. La sintassi è:
> 
> `Html.Raw(value)`
> 
> Nell'esempio riportato di seguito viene illustrato come usare `Html.Raw`:
> 
> [!code-cshtml[Main](beta3/samples/sample1.cshtml)]

<a id="Changes"></a>

#### <a name="changes-in-beta-3-for-aspnet-web-pages-with-razor-syntax"></a>Modifiche della versione beta 3 per Pagine Web ASP.NET con sintassi Razor

#### <a name="change-hrefattribute-method-removed"></a>Modifica: metodo "HrefAttribute" rimosso

> Il metodo `HrefAttribute` della classe `WebPage` è stato rimosso. Questo helper è stato usato per codificare i caratteri non sicuri negli URL. Non è più necessario perché ASP.NET Razor codifica automaticamente le stringhe. Utilizzare il nuovo metodo `Html.Raw` per eseguire il rendering di stringhe non codificate.

#### <a name="change-syntax-for-declarative-helper-helpers-changed"></a>Modifica: sintassi per gli helper "@helper" dichiarativi modificati

> Nella versione beta 3, ASP.NET modifica il modo in cui analizza gli helper creati usando la sintassi `@helper`. In sostanza, la sintassi `@helper` viene ora analizzata come un blocco di codice anziché come un blocco di markup che può includere codice. Di conseguenza, non è necessario che il codice all'interno dell'helper sia racchiuso tra `@{ }` blocchi. Viceversa, il markup all'interno dell'helper deve essere incluso in modo esplicito negli elementi HTML o nei tag `<text></text>` Razor di ASP.NET.
> 
> Ad esempio, la sintassi di `@helper` seguente funziona nella versione beta 3:
> 
> [!code-cshtml[Main](beta3/samples/sample2.cshtml)]
> 
> Nella versione beta 3, questo helper deve essere modificato in modo simile all'esempio seguente:
> 
> [!code-cshtml[Main](beta3/samples/sample3.cshtml)]
> 
> Si noti che il `@{ }` caratteri intorno al codice iniziale nell'helper non viene più utilizzato. Questo perché il contenuto degli helper viene considerato come un blocco di codice per impostazione predefinita. L'helper esegue il rendering del markup, che inizia con il tag di apertura `<a>`. Se l'helper deve eseguire il rendering di testo normale o tag che non includono un tag di chiusura (ad esempio, `<meta>` Tag), il contenuto di cui eseguire il rendering deve trovarsi in `<text></text>` tag.

#### <a name="change-webpagecontexthttpcontext-removed"></a>Change: "WebPageContext.HttpContext" removed

> La proprietà `WebPageContext.HttpContext` è stata rimossa. In alternativa, utilizzare `HttpContext.Current`. (La proprietà `WebPageContext.HttpContext` è semplicemente avvolta in questo oggetto).

#### <a name="change-facebook-helper-moved-to-new-package"></a>Modifica: l'helper "Facebook" è stato spostato in un nuovo pacchetto

> Il `Facebook` Helper è stato spostato nella libreria *Facebook. helper* , che include l'helper `Facebook` e la funzionalità aggiuntiva. È necessario installare questa libreria come pacchetto separato, come descritto in "installazione di helper con gestione pacchetti" nell'esercitazione [Introduzione con le pagine di ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202889).

#### <a name="change-membership-role-and-security-types-moves-to-new-assembly"></a>Modifica: l'appartenenza, il ruolo e i tipi di sicurezza vengono spostati in un nuovo assembly

> I tipi seguenti sono stati spostati nell'assembly `WebMatrix.WebData`:
> 
> - `ExtendedMembershipProvider`
> - `SimpleMembershipProvider`
> - `SimpleRoleProvider`
> - `WebSecurity`

#### <a name="change-tagbuilder-class-moved-to-systemwebwebpagesdll-assembly"></a>Modifica: la classe "TagBuilder" è stata spostata nell'assembly System. Web. WebPages. dll

> La classe `TagBuilde` r è stata spostata nell'assembly System. Web. WebPages. dll. In precedenza, si trovava in un assembly che faceva parte di ASP.NET MVC. Questa modifica significa che non è necessario installare ASP.NET MVC per poter usare la classe `TagBuilder`.
> 
> Tuttavia, la classe si trova ancora nello spazio dei nomi `System.Web.Mvc`. Per usare la classe `TagBuilder` (ad esempio, in un helper Razor di ASP.NET personalizzato), è necessario fare riferimento allo spazio dei nomi (ad esempio, aggiungendo `@using System.Web.Mvc` al codice).

#### <a name="change-request-validation-syntax-changed-validation-class-removed"></a>Modifica: sintassi della convalida della richiesta modificata; Classe "Validation" rimossa

> Nella versione beta 3, per disabilitare la convalida per un singolo campo o set di campi, è possibile chiamare il metodo `Validation.Exclude`, passando il nome o i nomi dei campi da escludere dalla convalida. Una nuova sintassi è disponibile nella versione beta 3 per ignorare la convalida. Il metodo `Validation` usato nella versione beta 3 è stato rimosso.
> 
> > [!NOTE]
> > Se non si disabilita la convalida della richiesta, se gli utenti provano a caricare il markup HTML, ad esempio usando un editor di testo RTF in una pagina, il sito Web segnalerà un errore come *una richiesta potenzialmente pericolosa. il valore del modulo è stato rilevato dal client* e l'input dell'utente non è accettato. Se si disabilita la convalida delle richieste, è necessario controllare manualmente l'input dell'utente per assicurarsi che non contenga un markup o uno script potenzialmente pericoloso utilizzando un elemento simile a [Microsoft Anti-Cross Site Scripting Library v 4.0](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=f4cd231b-7e06-445b-bec7-343e5884e651).
> 
> 
> Per disabilitare la convalida automatica delle richieste, chiamare il metodo `Request.Unvalidated`, passandogli il nome del campo o di un altro oggetto post per il quale si desidera ignorare la convalida della richiesta. È possibile utilizzare questo metodo per ignorare la convalida di tutti gli elementi nelle raccolte `Form`, `QueryString`, `Cookies`e `ServerVariables`. Negli esempi seguenti viene illustrato come utilizzare il metodo `Unvalidated`:
> 
> [!code-csharp[Main](beta3/samples/sample4.cs)]

<a id="Issues"></a>

#### <a name="known-issues-for-aspnet-web-pages-with-razor-syntax"></a>Problemi noti per Pagine Web ASP.NET con sintassi Razor

#### <a name="issue-unexpected-behavior-when-using-a-custom-user-table-for-membership"></a>Problema: comportamento imprevisto quando si usa una tabella utente personalizzata per l'appartenenza

> Per inizializzare il provider di appartenenze per un sito Web Razor di ASP.NET, chiamare il metodo `WebSecurity.InitializeDatabaseConnection`. (In WebMatrix, il modello di sito Starter include una chiamata a questo metodo nel file *\_AppStart. cshtml* ). Se il parametro `autoCreateTables` di questo metodo è impostato su true (per impostazione predefinita, è impostato su true nel modello di sito Starter) e se un nome di tabella non riconosciuto viene passato al metodo (il secondo parametro), il metodo non genera alcun errore. Viene invece creata automaticamente la tabella.
> 
> Questo può costituire un problema se si prevede di usare una tabella utente personalizzata per l'appartenenza, ma si passa il nome di tabella errato al metodo `WebSecurity.InitializeDatabaseConnection`. Poiché per impostazione predefinita il metodo non genera un errore se la tabella specificata non esiste e viene invece creata una nuova tabella, l'applicazione può sembrare funzionante. Tuttavia, il codice dell'applicazione che si basa sulla tabella utente personalizzata (e sui relativi campi) può avere esito negativo con errori imprevisti.
> 
> **Soluzione alternativa**  
> Verificare che il nome passato nel metodo `InitializeDatabaseConnection` corrisponda alla tabella dei profili utente nel database delle appartenenze o verificare che il parametro `autoCreateTables` sia impostato su false.

#### <a name="issue-failed-to-generate-a-user-instance-of-sql-server-error"></a>Problema: "Impossibile generare un'istanza utente di SQL Server" errore

> Se un'applicazione Web WebMatrix usa SQL Server Express ed esegue IIS 7,5 in Windows 7 o Windows Server 2008 R2, è possibile che venga visualizzato un errore che indica che SQL Server non è in grado di recuperare il percorso dell'applicazione locale dell'utente in fase di esecuzione.
> 
> **Soluzione alternativa** Verificare che l'account di Windows con cui viene eseguita l'applicazione (in genere servizio di rete) disponga delle autorizzazioni di lettura/scrittura per le cartelle radice dell'applicazione e per le sottocartelle, ad esempio *i dati\_app*. Informazioni più dettagliate sono disponibili nell'articolo della Knowledge base [problemi relativi a SQL Server Express istanze utente e ai progetti di applicazione Web ASP.NET](https://support.microsoft.com/kb/2002980).

#### <a name="issue-in-visual-studio-namespaces-for-custom-assemblies-dlls-are-not-imported-automatically"></a>Problema: in Visual Studio gli spazi dei nomi per gli assembly personalizzati (dll) non vengono importati automaticamente

> Se si utilizzano assembly personalizzati in un progetto in Visual Studio, gli spazi dei nomi dichiarati in tali assembly non vengono importati automaticamente in fase di progettazione. Di conseguenza, i riferimenti ai tipi personalizzati potrebbero non essere riconosciuti in fase di progettazione e contrassegnati come non riconosciuti in Visual Studio (usando "zigzag"). Questo problema si verifica solo in fase di progettazione in Visual Studio. l'applicazione stessa viene eseguita correttamente.
> 
> **Soluzione alternativa**  
> Includere un'istruzione `using` (`imports` in Visual Basic) che fa riferimento alle entità non riconosciute in fase di progettazione.

#### <a name="issue-visual-studio-intellisense-and-project-templates-available-only-in-aspnet-mvc-version-3"></a>Problema: i modelli di progetto e IntelliSense di Visual Studio sono disponibili solo in ASP.NET MVC versione 3

> L'installazione di Pagine Web ASP.NET non installa anche strumenti per Visual Studio, ad esempio IntelliSense e modelli di progetto per le applicazioni Pagine Web ASP.NET.
> 
> **Soluzione alternativa** Per usare IntelliSense e i modelli di progetto per applicazioni Pagine Web ASP.NET in Visual Studio, installare ASP.NET MVC 3 RC tramite l'installazione guidata piattaforma Web o il [programma di installazione autonomo](https://go.microsoft.com/fwlink/?LinkID=191797).

#### <a name="issue-lthelpergt-class-cannot-be-found-error"></a>Problema: errore "Impossibile trovare la classe&gt; Helper&lt;

> Dopo l'aggiornamento a Beta 3, potrebbe essere visualizzato un errore che segnala che una classe helper (ad esempio, la classe `Facebook`) non è stata trovata. A partire da Beta 2 e continuando nella versione beta 3, gli helper sono stati spostati in pacchetti che è necessario installare in modo esplicito. I siti esistenti non vengono aggiornati per includere questi pacchetti; sono inclusi i siti nelle cartelle *siti Web* *\My Documents\IISExpress* o \My. In particolare, questo errore viene visualizzato se si usa il sito predefinito in *siti personali* (WebSite1), che include un riferimento all'helper `Twitter`.
> 
> **Soluzione alternativa**  
> Impostare come commento le chiamate a qualsiasi helper nel sito, eseguire la pagina di *amministrazione\_* e installare il pacchetto o i pacchetti che includono gli helper che si desidera utilizzare. Dopo aver installato il pacchetto, è possibile rimuovere il commento dalle righe che fanno riferimento agli helper.

#### <a name="issue-deploying-beta-3-aspnet-razor-assemblies-to-the-bin-folder-might-not-work-on-hosting-sites"></a>Problema: la distribuzione degli assembly ASP.NET Razor Beta 3 nella cartella bin potrebbe non funzionare nei siti di hosting

> Se si distribuisce un sito Web Pagine Web ASP.NET in un sito di hosting e si distribuiscono gli assembly ASP.NET Razor Beta 3 nella cartella *bin* del sito, è possibile che si verifichino errori, inclusi i seguenti:
> 
> `Could not load type 'Microsoft.Web.Infrastructure.DynamicModuleHelper.DynamicModuleUtility' from assembly 'Microsoft.Web.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35'.`
> 
> Questo problema può verificarsi se il provider di hosting ha installato gli assembly Pagine Web ASP.NET Beta 1 nella Global Application Cache (GAC) del server. Gli assembly nella GAC ottengono la precedenza sugli assembly installati localmente nella cartella *bin* .
> 
> **Soluzione alternativa** Contattare il provider di hosting per verificare che gli errori visualizzati siano dovuti a un conflitto tra le versioni del provider degli assembly e l'utente. In tal caso, richiedere al provider di hosting di aggiornare gli assembly nella GAC del server.

#### <a name="issue-reading-feeds-or-other-external-data-via-a-proxy-server"></a>Problema: lettura di feed o altri dati esterni tramite un server proxy

> Se il server in cui è in esecuzione il sito si trova dietro un server proxy, potrebbe essere necessario configurare le informazioni sul proxy nel file *Web. config* per poter leggere le informazioni che provengono dall'esterno del sito. Se ad esempio si usa l'helper `ReCaptcha`, l'helper comunica con il servizio reCAPTCHA, ma potrebbe essere bloccato dal server proxy. Analogamente, i feed usati in Pagine Web ASP.NET, ad esempio il feed usato da Gestione pacchetti, potrebbero richiedere la configurazione del proxy.
> 
> Se si verificano problemi durante l'uso di un servizio esterno o l'uso del feed di pacchetti, inserire gli elementi seguenti nel file *Web. config* radice dell'applicazione:
> 
> [!code-xml[Main](beta3/samples/sample5.xml)]
> 
> Per ulteriori informazioni sulla configurazione di un server proxy, vedere [&lt;elemento proxy&gt; (impostazioni di rete)](https://msdn.microsoft.com/library/sa91de1e.aspx) nel sito Web MSDN.

#### <a name="issue-microsoftwebinfrastructuredll-cannot-be-loaded-error"></a>Problema: errore "Impossibile caricare Microsoft. Web. Infrastructure. dll"

> Se in precedenza è stata installata la versione beta 1 di Pagine Web ASP.NET con sintassi Razor e quindi si installa la versione beta 3, tutti gli assembly appropriati vengono installati nella GAC, ad eccezione di *Microsoft. Web. Infrastructure. dll*. Di conseguenza, quando si esegue ASP.NET Razor Pages, viene visualizzato un errore che indica che non è stato possibile caricare *Microsoft. Web. Infrastructure. dll* .
> 
> Questo problema non si verifica se è stata caricata la versione beta 3 in un computer pulito.
> 
> **Soluzione alternativa**  
> Nel pannello di controllo disinstallare Pagine Web ASP.NET. Reinstallare quindi la versione beta 3.

#### <a name="issue-uninstalling-the-net-framework-version-4-disables-aspnet-web-pages-with-razor-syntax"></a>Problema: la disinstallazione di .NET Framework versione 4 Disabilita Pagine Web ASP.NET con la sintassi Razor

> Se si disinstalla il .NET Framework versione 4 e quindi lo si reinstalla, Pagine Web ASP.NET con sintassi Razor è disabilitato. Le pagine con estensione *cshtml* non vengono eseguite correttamente. Pagine Web ASP.NET registra un assembly nel file *Web. config* radice del computer e rimuovendo il .NET Framework rimuove il file. Con la reinstallazione del .NET Framework viene installata una nuova versione del file di configurazione, ma non viene aggiunto il riferimento per l'assembly di Pagine Web ASP.NET.
> 
> **Soluzione alternativa** Dopo aver reinstallato il .NET Framework, reinstallare Pagine Web ASP.NET con sintassi Razor. In questo modo viene aggiunto il seguente elemento al file *Web. config* nella radice del computer, che in genere si trova nel percorso seguente:  
> 
> `C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config (32-bit)`  
> 
> `C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config (64-bit)`
> 
> [!code-xml[Main](beta3/samples/sample6.xml)]

#### <a name="issue-applications-previously-deployed-with-aspnet-assemblies-in-the-bin-folder-experience-errors"></a>Problema: le applicazioni distribuite in precedenza con gli assembly ASP.NET nella cartella bin hanno riscontrato errori

> Durante la distribuzione, copie degli assembly di Pagine Web ASP.NET (ad esempio, *Microsoft. WebPages. dll*) nella cartella *bin* del sito Web sul server. Questa operazione potrebbe essere avvenuta automaticamente durante la distribuzione o perché gli assembly sono stati copiati in modo esplicito dallo sviluppatore. Tuttavia, quando viene installata la versione beta 3, si verificano errori, ad esempio errori che non sono stati trovati determinati tipi. Questo problema si verifica perché un numero di tipi di Pagine Web ASP.NET è stato spostato in spazi dei nomi diversi per la versione beta 3.
> 
> **Soluzione alternativa**   
> Deselezionare la cartella *bin* dell'applicazione distribuita, copiare i nuovi assembly nella cartella (o ridistribuire l'applicazione), quindi riavviare l'applicazione.

#### <a name="issue-extensionless-urls-do-not-find-cshtmlvbhtml-files-on-iis-7-or-iis-75"></a>Problema: gli URL senza estensione non trovano i file con estensione cshtml/vbhtml in IIS 7 o IIS 7,5

> In IIS 7 o IIS 7,5, le richieste con un URL simile al seguente non sono in grado di trovare le pagine con estensione *cshtml* o *vbhtml* :  
> 
> `http://www.example.com/ExampleSite/ExampleFile`  
> 
> Il problema si verifica perché la riscrittura degli URL non è abilitata per impostazione predefinita per IIS 7 o IIS 7,5. Lo scenario probabile è che il problema non viene visualizzato durante i test in locale usando IIS Express, ma si verifica quando si distribuisce il sito Web in un sito Web di hosting.
> 
> **Soluzione alternativa**
> 
> - Se si ha il controllo sul computer server, nel computer server installare l'aggiornamento descritto in [un aggiornamento è disponibile che consente a determinati gestori iis 7,0 o iis 7,5 di gestire le richieste i cui URL non terminano con un punto](https://support.microsoft.com/kb/980368).
> - Se non si ha il controllo sul computer server (ad esempio, si distribuisce in un sito Web di hosting), aggiungere quanto segue al file *Web. config* del sito Web:
> 
> 
> [!code-xml[Main](beta3/samples/sample7.xml)]

#### <a name="issue-using-web-application-project-or-aspnet-mvc-and-aspnet-web-pages-in-the-same-application"></a>Problema: uso di un progetto di applicazione Web o di pagine Web ASP.NET MVC e ASP.NET nella stessa applicazione

> Se si usa Pagine Web ASP.NET in un progetto di applicazione Web o in un'applicazione MVC ASP.NET, è possibile che venga visualizzato un errore che segnala che *WebPageHttpApplication* non è stato trovato.
> 
> **Soluzione alternativa**  
> Se viene ricevuto questo errore, modificare la classe di base da cui deriva l'applicazione. Nel file *Global. asax* modificare la riga seguente:
> 
> [!code-csharp[Main](beta3/samples/sample8.cs)]
> 
> A questo scopo:
> 
> [!code-csharp[Main](beta3/samples/sample9.cs)]
> 
> Questo effetto inverte una modifica introdotta per la versione beta 1 di Pagine Web ASP.NET con sintassi Razor.

#### <a name="issue-deploying-an-application-to-a-computer-that-does-not-have-sql-server-compact-installed"></a>Problema: distribuzione di un'applicazione in un computer in cui non è installato SQL Server Compact

> Le applicazioni che includono SQL Server Compact database possono essere eseguite in un computer in cui SQL Server Compact non è installato. Microsoft WebMatrix Beta 3 copia automaticamente questi file binari ed esegue le trasformazioni appropriate del file *Web. config* .
> 
> **Soluzione alternativa** Se è necessario copiare questi file e modificare manualmente il file *Web. config* , eseguire le operazioni seguenti:
> 
> 1. Copiare gli assembly del motore di database nella cartella *bin* (e nelle sottocartelle) dell'applicazione nel computer di destinazione: 
> 
>     - Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Desktop\System.Data.SqlServerCe.dll* **a** *\bin*
>     - Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private\x86\\* * **in** *\Bin\x86*
>     - Copia *c:\programmi\microsoft SQL Server Compact Edition\v4.0\Private\amd64\\* * **in** *\Bin\amd64*
> 2. Nella cartella radice del sito Web creare o aprire un file *Web. config* . (In WebMatrix Beta 3 questo tipo di file è disponibile se si fa clic su **tutto** nella finestra di dialogo **scegliere un tipo di file** ).
> 3. Aggiungere l'elemento seguente come figlio dell'elemento **&lt;configuration&gt;** (non all'interno dell'elemento **&lt;system. Web&gt;** ):
> 
> 
> [!code-xml[Main](beta3/samples/sample10.xml)]

#### <a name="issue-database-and-webgrid-helpers-do-not-work-in-medium-trust-in-visual-basic"></a>Problema: gli helper di database e WebGrid non funzionano con attendibilità media in Visual Basic

> Se si usa Visual Basic (creazione di file con *estensione vbhtml* ), gli helper `Database` e `WebGrid` non funzioneranno se l'applicazione è impostata per usare l'attendibilità media.
> 
> **Soluzione alternativa**  
> Impostare temporaneamente l'applicazione in modo che utilizzi l'attendibilità totale.

<a id="Known_Issues_SQL_Server_Compact"></a>
### <a name="sql-server-compact"></a>SQL Server Compact

#### <a name="issue-encrypt-property-is-not-recognized"></a>Problema: la proprietà "Encrypt" non è riconosciuta

> SQL Server Compact 4,0 non riconosce la proprietà `Encrypt` della classe `SqlCeConnection`. Questa proprietà non deve essere utilizzata per crittografare i file di database. La proprietà `Encrypt` è stata deprecata nella versione SQL Server Compact 3,5 ed è stata mantenuta solo per compatibilità con le versioni precedenti. 
> 
> **Soluzione alternativa**  
> Usare la proprietà `Encryption Mode` della classe `SqlCeConnection` per crittografare i file di database SQL Server Compact 4,0. Nell'esempio seguente viene illustrato come creare un database SQL Server Compact 4,0 crittografato utilizzando la proprietà `Encryption Mode`:
> 
> [!code-csharp[Main](beta3/samples/sample11.cs)]
> 
> [!code-vb[Main](beta3/samples/sample12.vb)]
> 
> Per modificare la modalità di crittografia di un database SQL Server Compact 4,0 esistente, procedere come segue:
> 
> [!code-csharp[Main](beta3/samples/sample13.cs)]
> 
> [!code-vb[Main](beta3/samples/sample14.vb)]
> 
> Per crittografare un database SQL Server Compact 4,0 non crittografato, effettuare le operazioni seguenti:
> 
> [!code-csharp[Main](beta3/samples/sample15.cs)]
> 
> [!code-vb[Main](beta3/samples/sample16.vb)]

#### <a name="issue-microsoft-visual-c-2008-runtime-libraries-are-required"></a>Problema: le librerie C++ di runtime di Microsoft Visual 2008 sono obbligatorie

> Per le DLL native di SQL Server Compact 4,0 sono necessarie le C++ librerie di runtime di Microsoft Visual 2008 (x86, ia64 e x64), Service Pack 1.
> 
> **Soluzione alternativa**  
> Installare la .NET Framework 3,5 SP1. Vengono inoltre installate le librerie C++ di runtime di Visual 2008 SP1. È possibile scaricare le librerie dal percorso seguente:   
>   
> [Aggiornamento della C++ sicurezza ATL di Microsoft Visual 2008 Service Pack 1 Redistributable Package](https://go.microsoft.com/fwlink/?LinkId=194827)
> 
> [!NOTE]
> Si noti che l'installazione di .NET Framework 2,0, 3,0 o 4 *non comporta l'* installazione C++ di Visual 2008 Runtime Libraries SP1.

#### <a name="issue-if-sql-server-compact-is-installed-prior-to-installing-net-framework-on-the-computer-its-provider-invariant-name-is-not-registered-in-the-net-framework-machineconfig-file"></a>Problema: se SQL Server Compact viene installato prima di installare .NET Framework nel computer, il nome invariante del provider non è registrato nel file .NET Framework Machine. config

> SQL Server Compact possibile installare in un computer in cui non è installato .NET Framework perché SQL Server Compact richiede .NET Framework. Se non viene installata nessuna .NET Framework versione 3,5 né 4 prima di installare SQL Server Compact, il programma di installazione di SQL Server Compact non registra il nome invariante del provider nel file *Machine. config* . Eventuali applicazioni che si basano sulla voce SQL Server Compact nel file *Machine. config* avranno esito negativo. La voce di registrazione del nome invariante in *Machine. config* è simile all'esempio seguente:
> 
> [!code-xml[Main](beta3/samples/sample17.xml)]
> 
> **Soluzione alternativa**  
> Disinstallare SQL Server Compact 4,0 CTP1. Scaricare e installare le versioni complete del .NET Framework dal percorso seguente:
> 
> [Microsoft .NET Framework 3,5 Service Pack 1 (pacchetto completo)](https://go.microsoft.com/fwlink/?LinkId=194828)  
> [Versione di Microsoft .NET Framework 4,0 (pacchetto completo)](https://www.microsoft.com/downloads/details.aspx?FamilyID=9cfb2d51-5ff4-4491-b0e5-b386f32c0992&amp;displaylang=en)
> 
> Reinstallare [SQL Server Compact 4,0 CTP1](https://www.microsoft.com/downloads/details.aspx?FamilyID=0d2357ea-324f-46fd-88fc-7364c80e4fdb&amp;displaylang=en).

<a id="Known_Issues_Installing_Applications"></a>

### <a name="installing-applications"></a>Installazione di applicazioni

#### <a name="issue-installing-an-application-can-take-a-long-time-if-the-users-my-documents-folder-is-redirected-to-a-network-share"></a>Problema: l'installazione di un'applicazione può richiedere molto tempo se la cartella documenti dell'utente viene reindirizzata a una condivisione di rete

> **Soluzione alternativa**  
> Nessuna. L'installazione dell'applicazione potrebbe richiedere un po' di tempo, ma verrà installata correttamente.

<a id="Known_Issues_Publishing_Applications"></a>

### <a name="publishing-applications"></a>Pubblicazione di applicazioni

#### <a name="issue-site-might-not-work-after-publishing-if-the-destination-url-field-is-not-prefixed-with-http-or-https"></a>Problema: il sito potrebbe non funzionare dopo la pubblicazione se il campo "URL di destinazione" non è preceduto da http://o https://

> Nella finestra di dialogo **impostazioni di pubblicazione** , se l'URL di destinazione non inizia con `http://` o `https://`, il sito potrebbe non funzionare dopo la distribuzione.
> 
> **Soluzione alternativa**  
> Assicurarsi che prima di pubblicare un sito, l'URL di destinazione nella finestra di dialogo **impostazioni di pubblicazione** inizi con `http://` o `https://`.

#### <a name="issue-publishing-a-mysql-database-fails-with-the-error-failed-to-publish-the-database-this-can-happen-if-the-remote-database-cannot-run-the-script"></a>Problema: la pubblicazione di un database MySQL ha esito negativo con l'errore "Impossibile pubblicare il database. Questo problema può verificarsi se il database remoto non è in grado di eseguire lo script ".

> L'errore può verificarsi per diversi motivi. Uno dei motivi per cui è possibile visualizzare questo errore è che lo script del database contiene una virgoletta singola (') e il set di caratteri predefinito del database MySQL di destinazione non è UTF-8.
> 
> **Soluzione alternativa**  
> Impostare il set di caratteri predefinito per il database MySQL remoto su UTF-8.

<a id="Known_Issues_Other_Issues"></a>

### <a name="other-issues"></a>Altri problemi

#### <a name="issue-searchfilter-does-not-work-in-reports-for-group-by-issue-type"></a>Problema: la ricerca e il filtro non funzionano nei report per Group by: tipo di problema

> Quando si esegue un report per un sito, se si immette testo nella casella *Filtra per URL* e si fa clic su *Cerca*, non viene eseguita alcuna operazione. Questo è dovuto al fatto che questo controllo non è funzionante mentre lo stato *Group by* del report è impostato sul *tipo di problema*, che corrisponde all'impostazione predefinita.
> 
> **Soluzione alternativa** Nella scheda *Raggruppa per* della barra multifunzione fare clic su *URL* per raggruppare le voci in base all'URL di origine. La casella di testo e il pulsante per filtrare le voci sono funzionali in questo stato.

#### <a name="issue-wcf-applications-fail-to-run-with-iis-express"></a>Problema: non è possibile eseguire le applicazioni WCF con IIS Express

> Se si passa a un'applicazione WCF, viene restituito un errore simile al seguente:
> 
> *Impossibile caricare il file o l'assembly ' Microsoft. Web. Administration, Version = 7.0.0.0, Culture = neutral, PublicKeyToken = 31bf3856ad364e35' o una delle relative dipendenze. Il sistema non è in grado di trovare il file specificato.*
> 
> Questo problema si verifica perché IIS Express versione beta non supporta WCF per impostazione predefinita.
> 
> **Soluzione alternativa** Utilizzare una delle soluzioni alternative seguenti (soluzione alternativa #2 richiede Microsoft Windows Vista o versione successiva):
> 
> 
> 1. Copiare gli assembly *Microsoft. Web. dll* e *Microsoft. Web. Administration. dll* dal percorso di installazione di WebMatrix alla directory *bin* dell'applicazione WCF. Per impostazione predefinita, WebMatrix viene installato nella sottocartella *Microsoft WebMatrix* nella cartella *programmi* del sistema.
> 2. In Microsoft Windows Vista o versione successiva, creare un collegamento simbolico agli assembly nella directory *bin* usando i comandi seguenti. Questo approccio ha il vantaggio di non creare una copia degli assembly.
> 
>     [!code-console[Main](beta3/samples/sample18.cmd)]
> 3. Installare i due assembly nella global assembly cache (GAC). Da un prompt con privilegi elevati, eseguire i comandi seguenti:
> 
>     [!code-console[Main](beta3/samples/sample19.cmd)]

#### <a name="issue-webmatrix-beta-3-is-unable-to-perform-certain-tasks-that-require-elevation"></a>Problema: WebMatrix Beta 3 non è in grado di eseguire determinate attività per cui è richiesta l'elevazione dei privilegi

> WebMatrix Beta 3 non è in grado di eseguire determinate attività che richiedono l'elevazione dei privilegi, ad esempio l'installazione di componenti aggiuntivi nelle situazioni seguenti:
> 
> - In Windows Vista o Windows 7 si è connessi con un account che non dispone di privilegi amministrativi e il controllo dell'account utente è disabilitato.
> - Si sta utilizzando Microsoft Windows XP o Microsoft Windows Server 2003.
> 
> **Soluzione alternativa**  
> La maggior parte delle attività in WebMatrix Beta 3 non richiede autorizzazioni amministrative. Per tali operazioni, è possibile eseguire l'operazione come amministratore o attenersi alla procedura seguente:
> 
> - In Windows Vista o Windows 7 abilitare UAC.
> - In Windows XP aggiungere l'utente al gruppo di sicurezza Administrators.

#### <a name="issue-site-from-web-gallery-is-disabled"></a>Problema: "sito da raccolta web" disabilitato

> Se l'installazione guidata piattaforma Web 3,0 non è installata, l'opzione **sito da raccolta web** è disabilitata.
> 
> **Soluzione alternativa**  
> Installare il [Installazione guidata piattaforma Web Microsoft 3,0](https://go.microsoft.com/fwlink/?LinkID=194638).

#### <a name="issue-on-windows-server-2003-iis-express-does-not-start-for-a-non-administrative-user"></a>Problema: in Windows Server 2003, IIS Express non viene avviato per un utente non amministratore

> In Windows Server 2003, quando si avvia una pagina o si avvia IIS Express, IIS Express non viene avviato. Per le pagine Web viene visualizzato un errore che indica che l'applicazione è stata avviata da un utente non amministratore.
> 
> **Soluzione alternativa**  
> Avviare WebMatrix Beta 3 come utente amministratore. Per ulteriori informazioni, vedere l'articolo della Knowledge Base seguente:  
>   
> [Un'applicazione avviata da un utente non amministratore non può restare in ascolto del traffico HTTP del computer in cui è in esecuzione l'applicazione in Windows Vista, Windows Server 2003 o Windows XP.](https://support.microsoft.com/kb/939786)

#### <a name="issue-google-chrome-is-not-available-as-a-run-option"></a>Problema: Google Chrome non è disponibile come opzione di esecuzione

> Google Chrome non viene visualizzato nell'elenco dei browser in **Esegui** nella scheda **Home** .
> 
> **Soluzione alternativa**  
> Alcune versioni di Google Chrome non si registrano correttamente con la funzionalità programmi predefiniti di Windows. Come soluzione alternativa, avviare Google Chrome, fare clic sul menu *Personalizza e controlla Google Chrome* , scegliere *Opzioni*e quindi fare clic su *crea Google Chrome come browser predefinito*.

#### <a name="issue-the-foreign-key-dialog-box-doesnt-allow-entering-a-primary-key"></a>Problema: la finestra di dialogo "chiave esterna" non consente l'immissione di una chiave primaria

> La finestra di dialogo **chiave esterna** non consente di immettere il nome della chiave primaria dalla tabella della chiave primaria.
> 
> **Soluzione alternativa**  
> Questo comportamento è intenzionale. Non è necessario immettere il nome della chiave primaria dalla tabella della chiave primaria.

#### <a name="issue-the-relationships-button-is-disabled"></a>Problema: il pulsante "relazioni" è disabilitato

> Il pulsante **relazioni** nella scheda **tabella** dell'area di lavoro **database** è disabilitato per SQL Server Compact database.
> 
> **Soluzione alternativa**  
> Nessuna. SQL Server Compact non supporta le relazioni tra tabelle.

#### <a name="issue-parameterized-sql-queries-throw-exceptions"></a>Problema: le query SQL con parametri generano eccezioni

> In SQL Server Compact 4,0, se non si specifica un tipo di dati come `SqlDbType` o `DbType` per i parametri nelle query con parametri, viene generata un'eccezione durante l'esecuzione della query.
> 
> **Soluzione alternativa**  
> Impostare in modo esplicito il tipo di dati per i parametri, ad esempio `SqlDbType` o `DbType`. Si tratta di un problema critico nel caso dei tipi di dati BLOB (`image` e `ntext`). Usare un codice simile al seguente:
> 
> [!code-sql[Main](beta3/samples/sample20.sql)]
> 
> [!code-vb[Main](beta3/samples/sample21.vb)]

<a id="More_Info"></a>

## <a name="for-more-information"></a>Ulteriori informazioni

Per ulteriori informazioni su WebMatrix Beta 3, vedere i siti Web seguenti:

- [IIS.net](http://iis.net/)
- [ASP.NET 2.0](https://asp.net/webmatrix)
- [Microsoft.com/web](https://www.microsoft.com/web)

---

© 2010 Microsoft Corporation. Tutti i diritti riservati. [Condizioni](https://msdn.microsoft.cos/cc300389.aspx)per l'utilizzo.
