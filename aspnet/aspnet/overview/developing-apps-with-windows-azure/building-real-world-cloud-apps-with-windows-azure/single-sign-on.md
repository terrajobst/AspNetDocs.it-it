---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
title: Single Sign-on (creazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7d82d5e9-0619-4f22-9e03-32a6d52940a5
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/single-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 1ca93cce22487295a24aae95437b3e69dfc5b504
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457141"
---
# <a name="single-sign-on-building-real-world-cloud-apps-with-azure"></a>Single Sign-on (creazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

Ci sono molti problemi di sicurezza da considerare quando si sviluppa un'app Cloud, ma per questa serie ci si concentrerà solo su uno di essi: Single Sign-On. Una domanda frequente è la seguente: "Sto creando principalmente app per i dipendenti della mia azienda; in che modo è possibile ospitare queste app nel cloud e continuare a usare lo stesso modello di sicurezza che i dipendenti conoscono e usano nell'ambiente locale quando eseguono App ospitate all'interno del firewall? " Uno dei modi in cui si Abilita questo scenario è denominato Azure Active Directory (Azure AD). Azure AD consente di rendere disponibili app LOB (line-of-business) aziendali tramite Internet e di renderle disponibili anche ai partner commerciali.

## <a name="introduction-to-azure-ad"></a>Introduzione a Azure AD

[Azure ad](https://docs.microsoft.com/azure/active-directory/) fornisce [Active Directory](https://msdn.microsoft.com/library/windows/desktop/aa746492.aspx) nel cloud. Di seguito sono riportate alcune funzionalità principali:

- Si integra con Active Directory locali.
- Consente Single Sign-On con le app.
- Supporta standard aperti, ad esempio [SAML](http://en.wikipedia.org/wiki/SAML_2.0), [WS-Fed](http://en.wikipedia.org/wiki/WS-Federation)e [OAuth 2,0](http://oauth.net/2/).
- Supporta l' [API REST](https://msdn.microsoft.com/library/hh974476.aspx)di Enterprise Graph.

Si supponga di avere un ambiente Windows Server Active Directory locale da usare per consentire ai dipendenti di accedere alle app Intranet:

![](single-sign-on/_static/image1.png)

La Azure AD consente di creare una directory nel cloud. Si tratta di una funzionalità gratuita e facile da configurare.

Può essere completamente indipendente dalla Active Directory locale; è possibile inserire tutti gli elementi desiderati e autenticarli nelle app Internet.

![Active Directory di Windows Azure](single-sign-on/_static/image2.png)

In alternativa, è possibile integrarlo con Active Directory locale.

![Integrazione di AD e WAAD](single-sign-on/_static/image3.png)

Ora tutti i dipendenti che possono autenticarsi in locale possono anche eseguire l'autenticazione su Internet, senza dover aprire un firewall o distribuire nuovi server nel data center. È possibile continuare a sfruttare tutto l'ambiente di Active Directory esistente che si conosce e si utilizza oggi per fornire alle app interne funzionalità Single Sign-on.

Una volta stabilita la connessione tra AD e Azure AD, è anche possibile abilitare le app Web e i dispositivi mobili per autenticare i dipendenti nel cloud ed è possibile abilitare app di terze parti, ad esempio Office 365, SalesForce.com o Google Apps, per accettare il credenziali dei dipendenti. Se si usa Office 365, si è già configurati con Azure AD perché Office 365 USA Azure AD per l'autenticazione e l'autorizzazione.

![app di terze parti](single-sign-on/_static/image4.png)

La bellezza di questo approccio è che ogni volta che l'organizzazione aggiunge o Elimina un utente o un utente modifica una password, si usa lo stesso processo usato oggi nell'ambiente locale. Tutte le modifiche AD locali vengono propagate automaticamente nell'ambiente cloud.

Se la società USA o passa a Office 365, è opportuno che la Azure AD configurata automaticamente perché Office 365 USA Azure AD per l'autenticazione. Quindi, è possibile usare facilmente nelle proprie app la stessa autenticazione utilizzata da Office 365.

## <a name="set-up-an-azure-ad-tenant"></a>Configurare un tenant di Azure AD

una directory Azure AD viene chiamata [tenant](https://technet.microsoft.com/library/jj573650.aspx)Azure ad e la configurazione di un tenant è piuttosto semplice. Verranno illustrate le modalità di esecuzione nel portale di gestione di Azure per illustrare i concetti, ma naturalmente, come le altre funzioni del portale, è possibile eseguire questa operazione anche usando uno script o un'API di gestione.

Nel portale di gestione fare clic sulla scheda Active Directory.

![WAAD nel portale](single-sign-on/_static/image5.png)

Si dispone automaticamente di un tenant Azure AD per l'account Azure ed è possibile fare clic sul pulsante **Aggiungi** nella parte inferiore della pagina per creare directory aggiuntive. Potrebbe essere necessario un ambiente di testing e uno per la produzione, ad esempio. Valutare con attenzione il nome di una nuova directory. Se si usa il nome per la directory e si usa di nuovo il nome per uno degli utenti, questa operazione può generare confusione.

![Aggiungere un'istanza di Active Directory](single-sign-on/_static/image6.png)

Il portale offre supporto completo per la creazione, l'eliminazione e la gestione di utenti in questo ambiente. Per aggiungere un utente, ad esempio, passare alla scheda **utenti** e fare clic sul pulsante **Aggiungi utente** .

![Pulsante Aggiungi utente](single-sign-on/_static/image7.png)

![Finestra di dialogo Aggiungi utente](single-sign-on/_static/image8.png)

È possibile creare un nuovo utente che esiste solo in questa directory oppure è possibile registrare un account Microsoft come utente in questa directory oppure eseguire la registrazione o un utente da un'altra Azure AD directory come utente in questa directory. In una directory reale, il dominio predefinito è ContosoTest.onmicrosoft.com. È anche possibile usare un dominio di propria scelta, ad esempio contoso.com.

![Tipi di utente](single-sign-on/_static/image9.png)

![Finestra di dialogo Aggiungi utente](single-sign-on/_static/image10.png)

È possibile assegnare l'utente a un ruolo.

![Profilo utente](single-sign-on/_static/image11.png)

E l'account viene creato con una password temporanea.

![Password temporanea](single-sign-on/_static/image12.png)

Gli utenti creati in questo modo possono accedere immediatamente alle app Web usando questa directory cloud.

La soluzione ideale per Single Sign-On aziendali, tuttavia, è la scheda **integrazione directory** :

![Scheda integrazione directory](single-sign-on/_static/image13.png)

Se si Abilita l'integrazione della directory e si [Scarica uno strumento](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx), è possibile sincronizzare questa directory cloud con la Active Directory locale esistente già in uso all'interno dell'organizzazione. Tutti gli utenti archiviati nella directory vengono quindi visualizzati in questa directory cloud. Le app Cloud possono ora autenticare tutti i dipendenti usando le credenziali di Active Directory esistenti. Tutto questo è gratuito, sia lo strumento di sincronizzazione che Azure AD stesso.

Lo strumento è una procedura guidata che è facile da usare, come si può notare da queste schermate. Queste istruzioni non sono complete, ma solo un esempio che illustra il processo di base. Per informazioni dettagliate sulle procedure, vedere i collegamenti nella sezione [Resources](#resources) alla fine del capitolo.

![Configurazione guidata dello strumento di sincronizzazione WAAD](single-sign-on/_static/image14.png)

Fare clic su **Avanti**, quindi immettere le credenziali Azure Active Directory.

![Configurazione guidata dello strumento di sincronizzazione WAAD](single-sign-on/_static/image15.png)

Fare clic su **Avanti**, quindi immettere le credenziali di ad locali.

![Configurazione guidata dello strumento di sincronizzazione WAAD](single-sign-on/_static/image16.png)

Fare clic su **Avanti**e indicare se si desidera archiviare un hash delle password di Active Directory nel cloud.

![Configurazione guidata dello strumento di sincronizzazione WAAD](single-sign-on/_static/image17.png)

L'hash della password che è possibile archiviare nel cloud è un hash unidirezionale. le password effettive non vengono mai archiviate in Azure AD. Se si decide di non archiviare gli hash nel cloud, sarà necessario utilizzare [Active Directory Federation Services](https://technet.microsoft.com/library/hh831502.aspx) (ADFS). Esistono anche [altri fattori da considerare quando si sceglie di usare ADFS](https://technet.microsoft.com/library/jj573653.aspx). L'opzione ADFS richiede alcuni passaggi di configurazione aggiuntivi.

Se si sceglie di archiviare gli hash nel cloud, l'operazione viene eseguita e lo strumento avvia la sincronizzazione delle directory quando si fa clic su **Avanti**.

![Configurazione guidata dello strumento di sincronizzazione WAAD](single-sign-on/_static/image18.png)

E in pochi minuti.

![Configurazione guidata dello strumento di sincronizzazione WAAD](single-sign-on/_static/image19.png)

È necessario eseguire questa operazione solo in un controller di dominio dell'organizzazione, in Windows 2003 o versione successiva. E non è necessario riavviare il computer. Al termine, tutti gli utenti si trovano nel cloud ed è possibile eseguire Single Sign-On da qualsiasi applicazione Web o per dispositivi mobili, usando SAML, OAuth o WS-Fed.

A volte viene chiesto come proteggerlo: Microsoft lo usa per i propri dati aziendali riservati? E la risposta è sì. Ad esempio, se si passa al sito di Microsoft SharePoint interno in [https://microsoft.sharepoint.com/](https://microsoft.sharepoint.com/), viene richiesto di effettuare l'accesso.

![Pagina di accesso di Office 365](single-sign-on/_static/image20.png)

Microsoft ha abilitato ADFS. Pertanto, quando si immette un ID Microsoft, si viene reindirizzati a una pagina di accesso ad ADFS.

![Accesso ad ADFS](single-sign-on/_static/image21.png)

Dopo aver immesso le credenziali archiviate in un account Microsoft AD interno, è possibile accedere a questa applicazione interna.

![Sito di MS SharePoint](single-sign-on/_static/image22.png)

Si sta usando un server di accesso AD principalmente perché ADFS è già stato configurato prima che Azure AD diventi disponibile, ma il processo di accesso passa attraverso una directory Azure AD nel cloud. Microsoft ha inserito i documenti, il controllo del codice sorgente, i file di gestione delle prestazioni, i report sulle vendite e altro ancora nel cloud e sta usando questa identica soluzione per proteggerli.

## <a name="create-an-aspnet-app-that-uses-azure-ad-for-single-sign-on"></a>Creare un'app ASP.NET che usa Azure AD per Single Sign-On

Visual Studio semplifica la creazione di un'app che usa Azure AD per Single Sign-On, come si può notare da alcune schermate.

Quando si crea una nuova applicazione ASP.NET, ovvero MVC o Web Form, il metodo di autenticazione predefinito è ASP.NET Identity. Per modificare tale valore in Azure AD, fare clic sul pulsante **Modifica autenticazione** .

![Modifica autenticazione](single-sign-on/_static/image23.png)

Selezionare account aziendali, immettere il nome di dominio e quindi selezionare Single Sign-on.

![Finestra di dialogo Configura autenticazione](single-sign-on/_static/image24.png)

È anche possibile concedere all'app l'autorizzazione di lettura o di lettura/scrittura per i dati di directory. In tal caso, è possibile usare l' [API REST di Azure Graph](https://msdn.microsoft.com/library/windowsazure/hh974476.aspx) per cercare il numero di telefono degli utenti, scoprire se sono in ufficio, quando hanno eseguito l'ultimo accesso e così via.

A questo punto, Visual Studio richiede le credenziali di un amministratore per il tenant di Azure AD e quindi configura sia il progetto sia il tenant Azure AD per la nuova applicazione.

Quando si esegue il progetto, viene visualizzata una pagina di accesso ed è possibile accedere con le credenziali di un utente nella directory Azure AD.

![Accesso all'account dell'organizzazione](single-sign-on/_static/image25.png)

![Accesso eseguito](single-sign-on/_static/image26.png)

Quando si distribuisce l'app in Azure, è sufficiente selezionare una casella di controllo **Abilita autenticazione dell'organizzazione** e, ancora una volta, Visual Studio gestisce automaticamente tutta la configurazione.

![Pubblicazione del sito Web](single-sign-on/_static/image27.png)

Queste schermate provengono da un'esercitazione dettagliata completa che illustra come compilare un'app che usa l'autenticazione Azure AD: [sviluppo di app ASP.NET con Azure Active Directory](../../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md).

## <a name="summary"></a>Riepilogo

In questo capitolo si è visto che Azure Active Directory, Visual Studio e ASP.NET, semplificano la configurazione Single Sign-On nelle applicazioni Internet per gli utenti dell'organizzazione. Gli utenti possono accedere alle app Internet usando le stesse credenziali usate per accedere usando Active Directory nella rete interna.

Il [capitolo successivo](data-storage-options.md) esamina le opzioni di archiviazione dei dati disponibili per un'app cloud.

<a id="resources"></a>
## <a name="resources"></a>Risorse

Per ulteriori informazioni, vedere le seguenti risorse:

- [Documentazione di Azure Active Directory](https://docs.microsoft.com/azure/active-directory/). Pagina del portale per Azure AD documentazione nel sito di windowsazure.com. Per esercitazioni dettagliate, vedere la sezione **sviluppare** .
- [Multi-factor authentication di Azure](https://docs.microsoft.com/azure/multi-factor-authentication/). Pagina del portale per la documentazione sull'autenticazione a più fattori in Azure.
- [Opzioni di autenticazione dell'account aziendale](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#orgauthoptions). Spiegazione delle opzioni di autenticazione Azure AD nella finestra di dialogo Visual Studio 2013 nuovo progetto.
- [Modelli e procedure Microsoft-modello di identità federata](https://msdn.microsoft.com/library/dn589790.aspx).
- [HOWTO: installare lo strumento di sincronizzazione Azure Active Directory](https://social.technet.microsoft.com/wiki/contents/articles/19098.howto-install-the-windows-azure-active-directory-sync-tool-now-with-pictures.aspx).
- [Mappa del contenuto Active Directory Federation Services 2,0](https://social.technet.microsoft.com/wiki/contents/articles/2735.ad-fs-2-0-content-map.aspx). Collegamenti alla documentazione relativa ad ADFS 2,0.
- [Autorizzazione basata su ruoli e ACL in un'applicazione Windows Azure ad](https://code.msdn.microsoft.com/Role-Based-and-ACL-Based-86ad71a1). Applicazione di esempio.
- [Azure Active Directory API Graph Blog](https://blogs.msdn.com/b/aadgraphteam/).
- [Controllo degli accessi in BYOD e integrazione di directory in un'infrastruttura di identità ibrida](https://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/PCIT-B213#fbid=). Video della sessione tech ed 2014 di Gayana benivegna.

> [!div class="step-by-step"]
> [Precedente](web-development-best-practices.md)
> [Successivo](data-storage-options.md)
