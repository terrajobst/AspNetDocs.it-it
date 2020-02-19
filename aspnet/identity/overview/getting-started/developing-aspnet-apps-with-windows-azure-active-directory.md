---
uid: identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
title: Sviluppo di app ASP.NET con Azure Active Directory-ASP.NET 4. x
author: Rick-Anderson
description: Microsoft ASP.NET Tools per Azure Active Directory semplifica l'abilitazione dell'autenticazione per le applicazioni Web ospitate in Azure. È possibile usare Azure Preautenti...
ms.author: riande
ms.date: 08/14/2014
ms.assetid: 457d7eaf-ee76-4ceb-9082-c7c1721435ad
ms.custom: seoapril2019
msc.legacyurl: /identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory
msc.type: authoredcontent
ms.openlocfilehash: 28425ea8d1312dfc6e14df9677396f2cbcf6f16d
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77456725"
---
# <a name="developing-aspnet-apps-with-azure-active-directory"></a>Sviluppo di app ASP.NET con Azure Active Directory

di [Rick Anderson](https://twitter.com/RickAndMSFT)

Microsoft ASP.NET Tools per Azure Active Directory semplifica l'abilitazione dell'autenticazione per le app Web ospitate in [Azure](https://www.windowsazure.com/home/features/web-sites/). È possibile usare l'autenticazione di Azure per autenticare gli utenti di Office 365 dall'organizzazione, gli account aziendali sincronizzati dal Active Directory locale o dagli utenti creati nel dominio Azure Active Directory personalizzato. Con l'abilitazione dell'autenticazione di Windows Azure, l'applicazione viene configurata per autenticare gli utenti tramite un singolo tenant [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) .

In questa esercitazione viene illustrato come creare un'applicazione ASP.NET configurata per l'accesso con [Azure Active Directory](https://msdn.microsoft.com/library/azure/mt168838.aspx) (Azure ad). Si apprenderà anche come chiamare il API Graph per ottenere informazioni sull'utente attualmente connesso e su come distribuire l'applicazione in Azure.

## <a name="prerequisites"></a>Prerequisiti

1. [Visual Studio Express 2013 per il Web](https://my.visualstudio.com/Downloads?q=visual%20studio%202013#d-2013-express) o [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013).
2. È necessario [Visual Studio 2013 Update 4](https://www.microsoft.com/download/details.aspx?id=44921) aggiornamento 3 o versione successiva.
3. Un account Azure. Per una versione di valutazione gratuita, [fare clic qui](https://azure.microsoft.com/pricing/free-trial/) se non si ha già un account.

## <a name="add-a-global-administrator-to-your-active-directory"></a>Aggiungere un amministratore globale al Active Directory

1. Accedere al [portale di gestione di Azure](https://manage.windowsazure.com/).
2. Tutti gli account Azure contengono una **directory predefinita** , fare clic su di essa, quindi fare clic sulla scheda **utenti** nella parte superiore della pagina (vedere l'immagine seguente).
3. Fare clic su Add User.
    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image1.png)
4. Creare un nuovo utente con il ruolo di **amministratore globale** . Fare clic su **utenti** nel menu in alto, quindi fare clic sul pulsante **Aggiungi utente** sulla barra dei comandi.
5. Nella finestra di dialogo **Aggiungi utente** immettere un nome per il nuovo utente e quindi fare clic sulla freccia a destra.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image2.png)
6. Immettere il nome utente e impostare il ruolo su **amministratore globale**. Per gli amministratori globali è necessario un indirizzo di posta elettronica alternativo per il ripristino della password. Al termine, fare clic sulla freccia a destra.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image3.png)
7. Nella pagina successiva della finestra di dialogo fare clic su **Crea**. Verrà creata una password temporanea per il nuovo utente e visualizzata nella finestra di dialogo.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image4.png)

   Salvare la password. verrà richiesto di modificare la password dopo il primo accesso. La figura seguente mostra il nuovo account amministratore. È necessario usare la Azure Active Directory per accedere all'app, non l'account Microsoft visualizzata anche in questa pagina.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image5.png)

## <a name="create-an-aspnet-application"></a>Creare un'applicazione ASP.NET

La procedura seguente usa [Visual Studio Express 2013 per il Web](https://www.microsoft.com/download/details.aspx?id=40747)e richiede [Visual Studio 2013 aggiornamento 3](https://www.microsoft.com/download/details.aspx?id=43721).

1. In Visual Studio fare clic su **file** , quindi su **nuovo progetto**. Nella finestra di dialogo **nuovo progetto** selezionare il progetto C# Web visuale dal menu a sinistra e fare clic su **OK**. Se non si vuole usare la funzionalità per l'applicazione, è anche possibile deselezionare la casella **di controllo aggiungi Application Insights al progetto** .
2. Nella finestra di dialogo **nuovo progetto ASP.NET** selezionare **MVC**, quindi fare clic su **Modifica autenticazione**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image6.png)
3. Nella finestra di dialogo **Cambia autenticazione** selezionare **account aziendali**. Queste opzioni possono essere usate per registrare automaticamente l'applicazione con Azure AD e configurare automaticamente l'applicazione per l'integrazione con Azure AD. Non è necessario usare la finestra di dialogo **Modifica autenticazione** per registrare e configurare l'applicazione, ma ciò rende molto più semplice. Se si usa Visual Studio 2012, ad esempio, è comunque possibile registrare manualmente l'applicazione nel portale di gestione di Azure e aggiornarne la configurazione per l'integrazione con Azure AD.
   Nei menu a discesa selezionare **cloud-singola organizzazione** e **Single Sign-on, lettura dati directory**. Immettere il dominio per la directory Azure AD, ad esempio (nelle immagini seguenti) *aricka0yahoo.onmicrosoft.com*, quindi fare clic su **OK**. È possibile ottenere il nome di dominio dalla scheda domini per la directory predefinita nel portale di Azure (vedere l'immagine successiva in basso).

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image7.png)

   Nell'immagine seguente viene illustrato il nome di dominio del portale di Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image8.png)

    > [!NOTE]
    > Facoltativamente, è possibile configurare l'URI dell'ID applicazione che verrà registrato in Azure AD facendo clic su **altre opzioni**. L'URI ID app è l'identificatore univoco per un'applicazione, registrato in Azure AD e usato dall'applicazione per identificare se stesso durante la comunicazione con Azure AD. Per ulteriori informazioni sull'URI ID app e altre proprietà delle applicazioni registrate, vedere [questo argomento](https://msdn.microsoft.com/library/azure/dn499820.aspx#BKMK_Registering). Facendo clic sulla casella di controllo sotto il campo URI ID app, è anche possibile scegliere di sovrascrivere una registrazione esistente in Azure AD che usa lo stesso URI ID app.
4. Dopo aver fatto clic su **OK**, verrà visualizzata una finestra di dialogo di accesso con un account amministratore globale (non l'account Microsoft associato alla sottoscrizione). Se in precedenza è stato creato un nuovo account amministratore, sarà necessario modificare la password e quindi eseguire di nuovo l'accesso con la nuova password.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image9.png)
5. Una volta eseguita l'autenticazione, nella finestra di dialogo **nuovo progetto ASP.NET** verranno visualizzate la scelta di autenticazione (**organizzativa** ) e la directory in cui verrà registrata la nuova applicazione (*aricka0yahoo.onmicrosoft.com* nell'immagine seguente). Al di sotto di queste informazioni, selezionare la casella **di controllo host nel cloud**. Se questa casella di controllo è selezionata, il progetto verrà sottoposta a provisioning come app Web di Azure e sarà abilitato per la pubblicazione semplificata in un secondo momento. Fare clic su **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image10.png)
6. Verrà visualizzata la finestra di dialogo **Configura sito Web di Azure** con un nome del sito e un'area generati automaticamente. Si noti anche l'account a cui si è attualmente connessi nella finestra di dialogo. Si vuole assicurarsi che questo account sia quello a cui è associata la sottoscrizione di Azure, in genere un account Microsoft.

    > [!NOTE]
    > Questo progetto richiede un database. È necessario selezionare uno dei database esistenti o crearne uno nuovo. È necessario un database perché il progetto utilizza già un file di database locale per archiviare una piccola quantità di dati di configurazione dell'autenticazione. Quando si distribuisce l'applicazione in un sito Web di Azure, questo database non è incluso nella distribuzione, quindi è necessario sceglierne uno accessibile nel cloud. Fare clic su **OK**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image11.png)
7. Il progetto verrà creato e le opzioni di autenticazione e le opzioni dell'app Web verranno configurate automaticamente con il progetto. Al termine del processo, eseguire il progetto localmente premendo **^ F5**. Verrà richiesto di accedere con l'account aziendale. Fornire il nome utente e la password per l'account creato in precedenza e fare clic su **Accedi**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image12.png)
8. Al termine dell'accesso, il sito di ASP.NET indicherà che è stata eseguita l'autenticazione visualizzando il nome utente nell'angolo in alto a destra della pagina.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image13.png)

   Se viene ricevuto l'errore, il valore non può essere null o vuoto. Nome parametro: linkText ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image14.png)

   vedere la sezione [debug](#dbg) alla fine dell'esercitazione.

## <a name="basics-of-the-graph-api"></a>Nozioni di base sul API Graph

[Il API Graph](https://msdn.microsoft.com/library/azure/hh974476.aspx) è l'interfaccia a livello di codice utilizzata per eseguire operazioni CRUD e altre operazioni sugli oggetti nella directory Azure ad. Se si seleziona un'opzione per l'account aziendale per l'autenticazione quando si crea un nuovo progetto in Visual Studio 2013, l'applicazione sarà già configurata per chiamare il API Graph. Questa sezione illustra brevemente il funzionamento del API Graph.

1. Nell'applicazione in esecuzione fare clic sul nome dell'utente connesso in alto a destra nella pagina. Verrà visualizzata la pagina del profilo utente, che è un'azione nel controller Home. Si noterà che la tabella contiene informazioni utente sull'account amministratore creato in precedenza. Queste informazioni vengono archiviate nella directory e il API Graph viene chiamato per recuperare queste informazioni quando viene caricata la pagina.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image15.png)
2. Tornare a Visual Studio ed espandere la cartella **Controllers** , quindi aprire il file **HomeController.cs** . Verrà visualizzata un'azione **USERPROFILE ()** che contiene il codice per recuperare un token e quindi chiamare il API Graph. Questo codice è duplicato di seguito:

    [!code-csharp[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample1.cs?highlight=22)]

    Per chiamare il API Graph, è innanzitutto necessario recuperare un token. Quando il token viene recuperato, il relativo valore stringa deve essere accodato nell'intestazione di autorizzazione per tutte le richieste successive al API Graph. La maggior parte del codice precedente gestisce i dettagli dell'autenticazione per Azure AD per ottenere un token, usando il token per effettuare una chiamata al API Graph e quindi trasformando la risposta in modo che possa essere visualizzata nella visualizzazione.

    La parte più rilevante per la discussione è la seguente riga evidenziata: `UserProfile profile = JsonConvert.DeserializeObject<UserProfile>(responseString);`. Questa riga rappresenta il nome dell'utente, che è stato deserializzato dalla risposta JSON e viene visualizzato nella vista.

    È possibile chiamare il API Graph usando HttpClient e gestire manualmente i dati non elaborati, ma un modo più semplice consiste nell'usare la [libreria client Graph, disponibile tramite NuGet](http://www.nuget.org/packages/Microsoft.Azure.ActiveDirectory.GraphClient/). La libreria client gestisce automaticamente le richieste HTTP non elaborate e la trasformazione dei dati restituiti e rende molto più semplice lavorare con il API Graph in un ambiente .NET. Vedere gli esempi di codice API Graph correlati su [github.](https://github.com/AzureADSamples)

## <a name="deploy-the-application-to-azure"></a>Distribuire l'applicazione in Azure

Nei passaggi seguenti viene illustrato come distribuire l'applicazione in Azure. Nei passaggi precedenti è stato connesso il nuovo progetto a un'app Web in Azure, quindi è pronto per essere pubblicato in pochi passaggi.

1. In Visual Studio fare clic con il pulsante destro del mouse sul progetto e scegliere **pubblica**. Verrà visualizzata la finestra di dialogo **pubblica sul Web** con ogni impostazione già configurata. Fare clic sul pulsante **Avanti** per passare alla pagina **Impostazioni** . Potrebbe essere richiesto di eseguire l'autenticazione; Assicurarsi di eseguire l'autenticazione usando l'account della sottoscrizione di Azure (in genere un account Microsoft) e non l'account aziendale creato in precedenza.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image16.png)
2. Selezionare l'opzione **Enable Organizational Authentication** . Nel campo **dominio** immettere il dominio per la directory. Dall'elenco a discesa **livello di accesso** selezionare **Single Sign-on, lettura dati directory**. Si noterà che il database precedente utilizzato è già popolato nella sezione **database** . Fare clic su **Pubblica**.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image17.png)
3. Visual Studio inizierà la distribuzione del sito Web e quindi verrà visualizzata una nuova finestra del browser. Potrebbe essere richiesto di eseguire di nuovo l'autenticazione alla directory. Una volta eseguita l'autenticazione, si verrà reindirizzati al sito Web appena pubblicato in Azure.

    ![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image18.png)

<a id="dbg"></a>
## <a name="debugging-the-app"></a>Debug dell'app

Se viene ricevuto il seguente errore: il valore non può essere null o vuoto. Nome parametro: linkText

![](developing-aspnet-apps-with-windows-azure-active-directory/_static/image19.png)

Sostituire il codice nel file *Views\Shared\\_LoginPartial. cshtml* con il codice seguente:

[!code-cshtml[Main](developing-aspnet-apps-with-windows-azure-active-directory/samples/sample2.cshtml?highlight=1-8,15-16)]

Dopo aver eseguito l'app, se l'utente connesso Visualizza "utente null", disconnettersi ed eseguire di nuovo l'accesso con l'account Active Directory creato in precedenza.

Un'eccellente esercitazione da seguire è il approfondimento di Rick Rainy [: siti Web di Azure e autenticazione organizzativa con Azure ad](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/).

## <a name="more-information"></a>Altre informazioni

- [Approfondimento: siti Web di Azure e autenticazione organizzativa con Azure AD](http://rickrainey.com/2014/08/19/deep-dive-azure-websites-and-organizational-authentication-using-azure-ad/)
- [Panoramica di API Graph Azure AD](https://msdn.microsoft.com/library/azure/hh974476.aspx)
- [Scenari di autenticazione in Azure AD](https://msdn.microsoft.com/library/azure/dn499820.aspx)
- [Esempi di codice Azure AD su GitHub](https://github.com/AzureADSamples)
