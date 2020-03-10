---
uid: web-api/overview/security/external-authentication-services
title: Servizi di autenticazione esterni con API Web ASP.NETC#() | Microsoft Docs
author: rmcmurray
description: Viene descritto l'utilizzo di servizi di autenticazione esterni in API Web ASP.NET.
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: b2571552a3f8040ff42bfa0a9fa48981f71a1e4b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555475"
---
# <a name="external-authentication-services-with-aspnet-web-api-c"></a>Servizi di autenticazione esterni con API Web ASP.NETC#()

Visual Studio 2017 e ASP.NET 4.7.2 espandere le opzioni di sicurezza per [le applicazioni a pagina singola](../../../single-page-application/index.md) (Spa) e i servizi [API Web](../../index.md) per l'integrazione con i servizi di autenticazione esterni, che includono diversi servizi di autenticazione OAuth/OpenID e social media: account Microsoft, Twitter, Facebook e Google.  

### <a name="in-this-walkthrough"></a>In questa procedura dettagliata

- [Uso di servizi di autenticazione esterni](#USING)
- [Creazione dell'applicazione Web di esempio](#SAMPLE)
- [Abilitazione dell'autenticazione di Facebook](#FACEBOOK)
- [Abilitazione dell'autenticazione di Google](#GOOGLE)
- [Abilitazione dell'autenticazione Microsoft](#MICROSOFT)
- [Abilitazione dell'autenticazione Twitter](#TWITTER)
- [Informazioni aggiuntive](#MOREINFO)

    - [Combinazione di servizi di autenticazione esterni](#COMBINE)
    - [Configurazione di IIS Express per l'utilizzo di un nome di dominio completo](#FQDN)
    - [Come ottenere le impostazioni dell'applicazione per l'autenticazione Microsoft](#OBTAIN)
    - [Facoltativo: disabilitare la registrazione locale](#DISABLE)

### <a name="prerequisites"></a>Prerequisiti

Per seguire gli esempi di questa procedura dettagliata, è necessario disporre di quanto segue:

- Visual Studio 2017
- Un account sviluppatore con l'identificatore dell'applicazione e la chiave privata per uno dei servizi di autenticazione dei social media seguenti:

  - Account Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
  - Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
  - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))
  - Google ([https://developers.google.com/](https://developers.google.com))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Uso di servizi di autenticazione esterni

L'abbondanza di servizi di autenticazione esterni attualmente disponibili per gli sviluppatori Web contribuisce a ridurre i tempi di sviluppo quando si creano nuove applicazioni Web. Gli utenti Web hanno in genere diversi account esistenti per i servizi Web più diffusi e i siti Web di social media, pertanto quando un'applicazione Web implementa i servizi di autenticazione da un servizio Web esterno o da un sito Web di social media, Salva il tempo di sviluppo che sarebbe stata impiegata la creazione di un'implementazione di autenticazione. L'utilizzo di un servizio di autenticazione esterno consente agli utenti finali di creare un altro account per l'applicazione Web e di dover ricordare un altro nome utente e una password.

In passato, gli sviluppatori hanno avuto due opzioni: creare una propria implementazione di autenticazione o apprendere come integrare un servizio di autenticazione esterno nelle proprie applicazioni. Al livello più elementare, il diagramma seguente illustra un semplice flusso di richieste per un agente utente (Web browser) che richiede informazioni da un'applicazione Web configurata per l'utilizzo di un servizio di autenticazione esterno:

[![](external-authentication-services/_static/image2.png "Click to Expand the Image")](external-authentication-services/_static/image1.png)

Nel diagramma precedente, l'agente utente (o il Web browser in questo esempio) Invia una richiesta a un'applicazione Web, che reindirizza il Web browser a un servizio di autenticazione esterno. L'agente utente invia le proprie credenziali al servizio di autenticazione esterno e, se l'agente utente è stato autenticato correttamente, il servizio di autenticazione esterno reindirizza l'agente utente all'applicazione Web originale con una forma di token che l'agente utente invierà all'applicazione Web. L'applicazione Web utilizzerà il token per verificare che l'agente utente sia stato autenticato correttamente dal servizio di autenticazione esterno e che l'applicazione Web possa usare il token per raccogliere altre informazioni sull'agente utente. Una volta che l'applicazione ha completato l'elaborazione delle informazioni dell'agente utente, l'applicazione Web restituirà la risposta appropriata all'agente utente in base alle impostazioni di autorizzazione.

In questo secondo esempio, l'agente utente negozia con l'applicazione Web e il server di autorizzazione esterno e l'applicazione Web esegue comunicazioni aggiuntive con il server di autorizzazione esterno per recuperare informazioni aggiuntive sull'utente agente

[![](external-authentication-services/_static/image4.png "Click to Expand the Image")](external-authentication-services/_static/image3.png)

Visual Studio 2017 e ASP.NET 4.7.2 semplificano l'integrazione con i servizi di autenticazione esterni per gli sviluppatori grazie all'integrazione predefinita per i servizi di autenticazione seguenti:

- Facebook
- Google
- Account Microsoft (account Windows Live ID)
- Twitter

Gli esempi in questa procedura dettagliata illustrano come configurare ognuno dei servizi di autenticazione esterni supportati usando il nuovo modello di applicazione Web ASP.NET fornito con Visual Studio 2017.

> [!NOTE]
> Se necessario, potrebbe essere necessario aggiungere il nome di dominio completo alle impostazioni per il servizio di autenticazione esterno. Questo requisito si basa sui vincoli di sicurezza per alcuni servizi di autenticazione esterni che richiedono che il nome di dominio completo nelle impostazioni dell'applicazione corrisponda al nome di dominio completo utilizzato dai client. I passaggi per questa operazione variano significativamente per ogni servizio di autenticazione esterno. per verificare se è necessario e come configurare queste impostazioni, è necessario consultare la documentazione di ogni servizio di autenticazione esterno. Se è necessario configurare IIS Express per l'uso di un FQDN per il test di questo ambiente, vedere la sezione [configurazione IIS Express per l'uso di un nome di dominio](#FQDN) completo più avanti in questa procedura dettagliata.

<a id="SAMPLE"></a>
## <a name="create-a-sample-web-application"></a>Creare un'applicazione Web di esempio

La procedura seguente consente di creare un'applicazione di esempio usando il modello di applicazione Web ASP.NET. questa applicazione di esempio verrà usata per ognuno dei servizi di autenticazione esterni, più avanti in questa procedura dettagliata.

Avviare Visual Studio 2017 e selezionare **nuovo progetto** nella pagina iniziale. In alternativa, scegliere **nuovo** dal menu **file** , quindi **progetto**.

<!-- [![](external-authentication-services/_static/image6.png "Click to Expand the Image")](external-authentication-services/_static/image5.png) -->

Quando viene visualizzata la finestra di dialogo **nuovo progetto** , selezionare **installato** ed espandere oggetto **visivo C#** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET (.NET Framework)** . Immettere un nome per il progetto e fare clic su **OK**.

[![](external-authentication-services/_static/image71.png "Click to Expand the Image")](external-authentication-services/_static/image71.png)

Quando viene visualizzato il **nuovo progetto ASP.NET** , selezionare il modello **applicazione a pagina singola** e fare clic su **Crea progetto**.

[![](external-authentication-services/_static/image72.png "Click to Expand the Image")](external-authentication-services/_static/image72.png)

Attendere che Visual Studio 2017 crei il progetto.

<!-- [![](external-authentication-services/_static/image12.png "Click to Expand the Image")](external-authentication-services/_static/image11.png) -->

Al termine della creazione del progetto in Visual Studio 2017, aprire il file *Startup.auth.cs* che si trova nella cartella **app\_Start** .

Quando si crea il progetto per la prima volta, nessuno dei servizi di autenticazione esterni viene abilitato nel file *Startup.auth.cs* ; di seguito viene illustrata la somiglianza del codice, con le sezioni evidenziate per la posizione in cui si Abilita un servizio di autenticazione esterno e qualsiasi impostazione pertinente per l'uso di account Microsoft, Twitter, Facebook o Google Authentication con l'applicazione ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Quando si preme F5 per compilare ed eseguire il debug dell'applicazione Web, viene visualizzata una schermata di accesso in cui si noterà che non sono stati definiti servizi di autenticazione esterni.

[![](external-authentication-services/_static/image73.png "Click to Expand the Image")](external-authentication-services/_static/image73.png)

Nelle sezioni seguenti si apprenderà come abilitare ogni servizio di autenticazione esterno fornito con ASP.NET in Visual Studio 2017.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Abilitazione dell'autenticazione di Facebook

Con l'autenticazione di Facebook è necessario creare un account Facebook Developer e il progetto richiederà un ID applicazione e una chiave privata da Facebook per funzionare. Per informazioni sulla creazione di un account Facebook Developer e sull'acquisizione dell'ID applicazione e della chiave privata, vedere [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Una volta ottenuti l'ID applicazione e la chiave privata, attenersi alla procedura seguente per abilitare l'autenticazione di Facebook per l'applicazione Web:

1. Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .

2. Individuare la sezione relativa all'autenticazione di Facebook del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere l'ID applicazione e la chiave privata. Una volta aggiunti tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Facebook è stato definito come servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image74.png "Click to Expand the Image")](external-authentication-services/_static/image74.png)
5. Quando si fa clic sul pulsante **Facebook** , il browser verrà reindirizzato alla pagina di accesso di Facebook:

    [![](external-authentication-services/_static/image22.png "Click to Expand the Image")](external-authentication-services/_static/image21.png)
6. Dopo aver immesso le credenziali di Facebook e fatto clic su **Accedi**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si vuole associare all'account Facebook:

    [![](external-authentication-services/_static/image24.png "Click to Expand the Image")](external-authentication-services/_static/image23.png)
7. Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per l'account Facebook:

    [![](external-authentication-services/_static/image26.png "Click to Expand the Image")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Abilitazione dell'autenticazione di Google

Con l'autenticazione di Google è necessario creare un account Google Developer e il progetto richiederà un ID applicazione e una chiave privata da Google per funzionare. Per informazioni sulla creazione di un account Google Developer e sull'ottenimento dell'ID applicazione e della chiave privata, vedere [https://developers.google.com](https://developers.google.com).

Per abilitare l'autenticazione di Google per l'applicazione Web, attenersi alla procedura seguente:

1. Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .

2. Individuare la sezione di autenticazione di Google di codice:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere l'ID applicazione e la chiave privata. Una volta aggiunti tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Google è stato definito come servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image75.png "Click to Expand the Image")](external-authentication-services/_static/image75.png)
5. Quando si fa clic sul pulsante **Google** , il browser verrà reindirizzato alla pagina di accesso di Google:

    [![](external-authentication-services/_static/image32.png "Click to Expand the Image")](external-authentication-services/_static/image31.png)
6. Dopo aver immesso le credenziali di Google e aver fatto clic su **Accedi**, Google chiederà di verificare che l'applicazione Web disponga delle autorizzazioni per l'accesso all'account Google:

    [![](external-authentication-services/_static/image34.png "Click to Expand the Image")](external-authentication-services/_static/image33.png)
7. Quando si fa clic su **Accetto**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si desidera associare al proprio account Google:

    [![](external-authentication-services/_static/image36.png "Click to Expand the Image")](external-authentication-services/_static/image35.png)
8. Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per l'account Google:

    [![](external-authentication-services/_static/image38.png "Click to Expand the Image")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Abilitazione dell'autenticazione Microsoft

L'autenticazione Microsoft richiede la creazione di un account sviluppatore e richiede un ID client e un segreto client per funzionare. Per informazioni sulla creazione di un account Microsoft Developer e sull'ottenimento dell'ID client e del segreto client, vedere [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070).

Una volta ottenuta la chiave utente e il segreto utente, attenersi alla procedura seguente per abilitare l'autenticazione Microsoft per l'applicazione Web:

1. Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .

2. Individuare la sezione Microsoft Authentication del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere l'ID client e il segreto client. Una volta aggiunti tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Microsoft è stato definito come servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image42.png "Click to Expand the Image")](external-authentication-services/_static/image41.png)
5. Quando si fa clic sul pulsante **Microsoft** , il browser verrà reindirizzato alla pagina di accesso di Microsoft:

    [![](external-authentication-services/_static/image44.png "Click to Expand the Image")](external-authentication-services/_static/image43.png)
6. Dopo aver immesso le credenziali Microsoft e fatto clic su **Accedi**, verrà richiesto di verificare che l'applicazione Web disponga delle autorizzazioni per accedere ai account Microsoft:

    [![](external-authentication-services/_static/image46.png "Click to Expand the Image")](external-authentication-services/_static/image45.png)
7. Quando si fa clic su **Sì**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si desidera associare al account Microsoft:

    [![](external-authentication-services/_static/image48.png "Click to Expand the Image")](external-authentication-services/_static/image47.png)
8. Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per la account Microsoft:

    [![](external-authentication-services/_static/image50.png "Click to Expand the Image")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Abilitazione dell'autenticazione Twitter

L'autenticazione di Twitter richiede la creazione di un account sviluppatore e richiede una chiave utente e un segreto utente per funzionare. Per informazioni sulla creazione di un account sviluppatore di Twitter e sul recupero della chiave utente e del segreto utente, vedere [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166).

Una volta ottenuta la chiave utente e il segreto utente, attenersi alla procedura seguente per abilitare l'autenticazione di Twitter per l'applicazione Web:

1. Quando il progetto è aperto in Visual Studio 2017, aprire il file *Startup.auth.cs* .

2. Individuare la sezione relativa all'autenticazione di Twitter del codice:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Rimuovere il &quot;//&quot; caratteri per rimuovere il commento dalle righe di codice evidenziate, quindi aggiungere la chiave utente e il segreto utente. Una volta aggiunti tali parametri, è possibile ricompilare il progetto:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Quando si preme F5 per aprire l'applicazione Web nel Web browser, si noterà che Twitter è stato definito come servizio di autenticazione esterno:

    [![](external-authentication-services/_static/image54.png "Click to Expand the Image")](external-authentication-services/_static/image53.png)
5. Quando si fa clic sul pulsante **Twitter** , il browser verrà reindirizzato alla pagina di accesso di Twitter:

    [![](external-authentication-services/_static/image56.png "Click to Expand the Image")](external-authentication-services/_static/image55.png)
6. Dopo aver immesso le credenziali di Twitter e aver fatto clic su **autorizza app**, il Web browser verrà reindirizzato all'applicazione Web, che richiederà il **nome utente** che si vuole associare all'account Twitter:

    [![](external-authentication-services/_static/image58.png "Click to Expand the Image")](external-authentication-services/_static/image57.png)
7. Dopo aver immesso il nome utente e aver fatto clic sul pulsante di **iscrizione** , l'applicazione Web visualizzerà il **Home page** predefinito per l'account Twitter:

    [![](external-authentication-services/_static/image60.png "Click to Expand the Image")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Informazioni aggiuntive

Per ulteriori informazioni sulla creazione di applicazioni che utilizzano OAuth e OpenID, vedere gli URL seguenti:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Combinazione di servizi di autenticazione esterni

Per una maggiore flessibilità, è possibile definire contemporaneamente più servizi di autenticazione esterni, in modo da consentire agli utenti dell'applicazione Web di usare un account di uno dei servizi di autenticazione esterni abilitati:

[![](external-authentication-services/_static/image62.png "Click to Expand the Image")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configure-iis-express-to-use-a-fully-qualified-domain-name"></a>Configurare IIS Express per l'uso di un nome di dominio completo

Alcuni provider di autenticazione esterni non supportano il test dell'applicazione utilizzando un indirizzo HTTP come `http://localhost:port/`. Per risolvere questo problema, è possibile aggiungere un mapping del nome di dominio completo (FQDN) statico al file degli host e configurare le opzioni del progetto in Visual Studio 2017 per usare l'FQDN per il testing/debug. A tale scopo, seguire questa procedura:

- Aggiungere un FQDN statico eseguendo il mapping del file HOSTs:

  1. Aprire un prompt dei comandi con privilegi elevati in Windows.
  2. Digitare il comando seguente:

      <kbd>blocco note%WinDir%\system32\drivers\etc\hosts</kbd>
  3. Aggiungere una voce simile alla seguente al file HOSTs:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Salvare e chiudere il file degli host.

- Configurare il progetto di Visual Studio in modo che usi il nome di dominio completo:

  1. Quando il progetto è aperto in Visual Studio 2017, fare clic sul menu **progetto** , quindi selezionare le proprietà del progetto. Ad esempio, è possibile selezionare le **proprietà di WebApplication1**.
  2. Selezionare la scheda **Web** .
  3. Immettere il nome di dominio completo per l' <strong>URL del progetto</strong>. Ad esempio, immettere <kbd><http://www.wingtiptoys.com></kbd> se si tratta del mapping FQDN aggiunto al file degli host.

- Configurare IIS Express per usare il nome di dominio completo per l'applicazione:

    1. Aprire un prompt dei comandi con privilegi elevati in Windows.
    2. Digitare il comando seguente per passare alla cartella IIS Express:

        <kbd>CD/d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Digitare il comando seguente per aggiungere il nome di dominio completo all'applicazione:

        <kbd>appcmd. exe set config-section: System. applicationHost/sites/+&quot;[Name =' WebApplication1']. Bindings. [Protocol =' http ', bindingInformation =' *: 80: www. wingtiptoys. com ']&quot;/commit: apphost</kbd>

  Dove **WebApplication1** è il nome del progetto e **BindingInformation** contiene il numero di porta e il nome di dominio completo che si desidera utilizzare per il test.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Come ottenere le impostazioni dell'applicazione per l'autenticazione Microsoft

Il collegamento di un'applicazione a Windows Live per l'autenticazione Microsoft è un processo semplice. Se un'applicazione non è ancora stata collegata a Windows Live, è possibile eseguire le operazioni seguenti:

1. Passare a [https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070) e immettere il nome e la password del account Microsoft quando richiesto, quindi fare clic su **Accedi**:

   <!--  [![](external-authentication-services/_static/image64.png "Click to Expand the Image")](external-authentication-services/_static/image63.png) -->
2. Selezionare **Aggiungi un'app** e immettere il nome dell'applicazione quando richiesto, quindi fare clic su **Crea**:

    [![](external-authentication-services/_static/image79.png "Click to Expand the Image")](external-authentication-services/_static/image79.png)
3. Selezionare l'app in **nome** e viene visualizzata la pagina delle proprietà dell'applicazione.

4. Immettere il dominio di reindirizzamento per l'applicazione. Copiare l' **ID applicazione** e, in **segreti applicazione**, selezionare **genera password**. Copiare la password visualizzata. L'ID e la password dell'applicazione sono l'ID client e il segreto client. Selezionare **OK** e quindi **Salva**.

    [![](external-authentication-services/_static/image77.png "Click to Expand the Image")](external-authentication-services/_static/image77.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Facoltativo: disabilitare la registrazione locale

La funzionalità di registrazione locale di ASP.NET corrente non impedisce la creazione di account membri da parte di programmi automatizzati. ad esempio, usando una tecnologia di prevenzione e convalida bot come [captcha](../../../web-pages/overview/security/16-adding-security-and-membership.md). Per questo motivo, è necessario rimuovere il modulo di accesso locale e il collegamento di registrazione nella pagina di accesso. A tale scopo, aprire la pagina *\_login. cshtml* nel progetto, quindi impostare come commento le righe per il pannello di accesso locale e il collegamento di registrazione. La pagina risultante dovrebbe essere simile all'esempio di codice seguente:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Una volta disabilitato il pannello di accesso locale e il collegamento di registrazione, nella pagina di accesso vengono visualizzati solo i provider di autenticazione esterni abilitati:

[![](external-authentication-services/_static/image70.png "Click to Expand the Image")](external-authentication-services/_static/image69.png)
