---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Uso di provider OAuth con MVC 4 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra come creare un'applicazione Web MVC 4 ASP.NET che consente agli utenti di accedere con le credenziali di un provider esterno, ad esempio Facebo...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 5dfd1305376a62f4987caea242ca0f6aac1018e9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539081"
---
# <a name="using-oauth-providers-with-mvc-4"></a>Uso di provider OAuth con MVC 4

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questa esercitazione illustra come creare un'applicazione Web MVC 4 ASP.NET che consente agli utenti di accedere con le credenziali di un provider esterno, ad esempio Facebook, Twitter, Microsoft o Google, e quindi di integrare alcune delle funzionalità di tali provider nel applicazione Web. Per semplicità, questa esercitazione è incentrata sull'uso delle credenziali da Facebook.
> 
> Per usare le credenziali esterne in un'applicazione Web MVC 5 ASP.NET, vedere [creare un'App ASP.NET MVC 5 con Facebook e Google OAuth2 e OpenID sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> L'abilitazione di queste credenziali nei siti Web offre un vantaggio significativo perché milioni di utenti dispongono già di account con questi provider esterni. Questi utenti potrebbero essere più inclini ad iscriversi al sito se non è necessario creare e ricordare un nuovo set di credenziali. Inoltre, dopo che un utente ha effettuato l'accesso tramite uno di questi provider, è possibile incorporare le operazioni di social networking dal provider.

## <a name="what-youll-build"></a>Elementi da compilare

Questa esercitazione prevede due obiettivi principali:

1. Consentire a un utente di accedere con le credenziali di un provider OAuth.
2. Recuperare le informazioni sull'account dal provider e integrare tali informazioni con la registrazione dell'account per il sito.

Sebbene gli esempi in questa esercitazione siano incentrati sull'uso di Facebook come provider di autenticazione, è possibile modificare il codice per usare uno dei provider. I passaggi per implementare qualsiasi provider sono molto simili ai passaggi che verranno visualizzati in questa esercitazione. Si noteranno differenze significative solo quando si effettuano chiamate dirette al set di API del provider.

## <a name="prerequisites"></a>Prerequisiti

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) o [Microsoft Visual Studio Express 2012 per il Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

Oppure

- Microsoft Visual Studio 2010 SP1 o [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

In questo argomento si presuppone inoltre la conoscenza di base di ASP.NET MVC e Visual Studio. Se è necessaria un'introduzione a ASP.NET MVC 4, vedere [Introduzione a ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Creare il progetto

In Visual Studio creare una nuova applicazione Web MVC 4 ASP.NET e denominarla &quot;OAuthMVC&quot;. È possibile specificare come destinazione .NET Framework 4,5 o 4.

![crea progetto](using-oauth-providers-with-mvc/_static/image1.png)

Nella finestra nuovo progetto MVC 4 ASP.NET selezionare **applicazione Internet** e lasciare **Razor** come motore di visualizzazione.

![Selezionare l'applicazione Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Abilita un provider

Quando si crea un'applicazione Web MVC 4 con il modello di applicazione Internet, il progetto viene creato con un file denominato AuthConfig.cs nella cartella di avvio dell'app\_.

![File AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

Il file AuthConfig contiene il codice per registrare i client per i provider di autenticazione esterni. Per impostazione predefinita, questo codice è impostato come commento, quindi nessuno dei provider esterni è abilitato.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

È necessario rimuovere il commento dal codice per usare il client di autenticazione esterno. Rimuovere il commento solo dei provider che si desidera includere nel sito. Per questa esercitazione si consentiranno solo le credenziali di Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Si noti nell'esempio precedente che il metodo include stringhe vuote per i parametri di registrazione. Se si tenta di eseguire l'applicazione ora, l'applicazione genera un'eccezione di argomento perché non sono consentite stringhe vuote per i parametri. Per fornire valori validi, è necessario registrare il sito Web con i provider esterni, come illustrato nella sezione successiva.

## <a name="registering-with-an-external-provider"></a>Registrazione con un provider esterno

Per autenticare gli utenti con credenziali da un provider esterno, è necessario registrare il sito Web con il provider. Quando si registra il sito, si riceveranno i parametri, ad esempio chiave, ID e segreto, da includere durante la registrazione del client. È necessario disporre di un account con i provider che si desidera utilizzare.

Questa esercitazione non Mostra tutti i passaggi da eseguire per la registrazione con questi provider. I passaggi non sono in genere difficili. Per registrare correttamente il sito, seguire le istruzioni fornite in questi siti. Per iniziare a registrare il sito, vedere il sito per sviluppatori per:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Quando si registra il sito con Facebook, è possibile fornire &quot;localhost&quot; per il dominio del sito e `&quot; http://localhost/&quot;` per l'URL, come illustrato nell'immagine seguente. L'uso di localhost funziona con la maggior parte dei provider, ma attualmente non funziona con il provider Microsoft. Per il provider Microsoft, è necessario includere un URL del sito Web valido.

![registra sito](using-oauth-providers-with-mvc/_static/image4.png)

Nell'immagine precedente, i valori per l'ID app, il segreto app e il messaggio di posta elettronica di contatto sono stati rimossi. Quando si registra effettivamente il sito, saranno presenti tali valori. È possibile prendere nota dei valori per ID app e segreto app perché vengono aggiunti all'applicazione.

## <a name="creating-test-users"></a>Creazione di utenti di test

Se non si vuole usare un account Facebook esistente per testare il sito, è possibile ignorare questa sezione.

È possibile creare facilmente utenti di test per l'applicazione nella pagina di gestione delle app di Facebook. È possibile usare questi account di test per accedere al sito. Per creare utenti di test, fare clic sul collegamento **ruoli** nel riquadro di spostamento a sinistra e fare clic sul collegamento **Crea** .

![creare utenti di test](using-oauth-providers-with-mvc/_static/image5.png)

Il sito Facebook crea automaticamente il numero di account di test richiesti.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Aggiunta dell'ID applicazione e del segreto dal provider

Ora che sono stati ricevuti l'ID e il segreto da Facebook, tornare al file AuthConfig e aggiungerli come valori dei parametri. I valori indicati di seguito non sono valori reali.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Accedi con credenziali esterne

Questa operazione è sufficiente per abilitare le credenziali esterne nel sito. Eseguire l'applicazione e fare clic sul collegamento di accesso nell'angolo superiore destro. Il modello riconosce automaticamente di aver registrato Facebook come provider e include un pulsante per il provider. Se si registrano più provider, viene automaticamente incluso un pulsante per ciascuna di esse.

![accesso esterno](using-oauth-providers-with-mvc/_static/image6.png)

Questa esercitazione non illustra come personalizzare i pulsanti di accesso per i provider esterni. Per informazioni, vedere [personalizzazione dell'interfaccia utente di accesso quando si usa OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Fai clic sul pulsante Facebook per accedere con le credenziali di Facebook. Quando si seleziona uno dei provider esterni, si viene reindirizzati a tale sito e viene richiesto da tale servizio di effettuare l'accesso.

La figura seguente mostra la schermata di accesso per Facebook. Si nota che si sta usando l'account Facebook per accedere a un sito denominato oauthmvcexample.

![autenticazione di Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Dopo aver eseguito l'accesso con le credenziali di Facebook, una pagina informa l'utente che il sito avrà accesso alle informazioni di base.

![Richiedi autorizzazione](using-oauth-providers-with-mvc/_static/image8.png)

Dopo aver selezionato **Vai all'app**, l'utente deve registrarsi per il sito. La figura seguente mostra la pagina di registrazione dopo che un utente ha eseguito l'accesso con le credenziali di Facebook. Il nome utente è in genere precompilato da un nome del provider.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Fare clic su **registra** per completare la registrazione. Chiudere il browser.

È possibile notare che il nuovo account è stato aggiunto al database. In Esplora server aprire il database **DefaultConnection** e aprire la cartella **tabelle** .

![tabelle di database](using-oauth-providers-with-mvc/_static/image10.png)

Fare clic con il pulsante destro del mouse sulla tabella **USERPROFILE** e scegliere **Mostra dati tabella**.

![Mostra dati](using-oauth-providers-with-mvc/_static/image11.png)

Viene visualizzato il nuovo account aggiunto. Esaminare i dati nella **pagina web\_tabella OAuthMembership** . Vengono visualizzati più dati correlati al provider esterno per l'account appena aggiunto.

Se si vuole solo abilitare l'autenticazione esterna, l'operazione è stata eseguita. Tuttavia, è possibile integrare ulteriormente le informazioni del provider nel processo di registrazione del nuovo utente, come illustrato nelle sezioni riportate di seguito.

## <a name="create-models-for-additional-user-information"></a>Creazione di modelli per informazioni aggiuntive sull'utente

Come si è notato nelle sezioni precedenti, non è necessario recuperare informazioni aggiuntive per il funzionamento della registrazione dell'account predefinita. Tuttavia, la maggior parte dei provider esterni restituiscono informazioni aggiuntive sull'utente. Le sezioni seguenti illustrano come conservare tali informazioni e salvarle in un database. In particolare, si manterranno i valori per il nome completo dell'utente, l'URI della pagina Web personale dell'utente e un valore che indica se Facebook ha verificato l'account.

Si utilizzerà [migrazioni Code First](https://msdn.microsoft.com/data/jj591621) per aggiungere una tabella per l'archiviazione di informazioni utente aggiuntive. Per aggiungere la tabella a un database esistente, è necessario innanzitutto creare uno snapshot del database corrente. Creando uno snapshot del database corrente, è possibile creare in un secondo momento una migrazione che contenga solo la nuova tabella. Per creare uno snapshot del database corrente:

1. Aprire la **console di gestione pacchetti**
2. Eseguire il comando **Enable-Migrations**
3. Eseguire il comando **Add-Migration Initial-IgnoreChanges**
4. Eseguire il comando **Update-database**

A questo punto, verranno aggiunte le nuove proprietà. Nella cartella Models (modelli) aprire il file AccountModels.cs e trovare la classe RegisterExternalLoginModel. La classe RegisterExternalLoginModel include i valori restituiti dal provider di autenticazione. Aggiungere le proprietà denominate FullName e link, come illustrato di seguito.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

Inoltre, in AccountModels.cs aggiungere una nuova classe denominata ExtraUserInformation. Questa classe rappresenta la nuova tabella che verrà creata nel database.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

Nella classe UsersContext aggiungere il codice evidenziato di seguito per creare una proprietà DbSet per la nuova classe.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

A questo punto è possibile creare la nuova tabella. Aprire di nuovo la console di gestione pacchetti e questa volta:

1. Eseguire il comando **Add-Migration AddExtraUserInformation**
2. Eseguire il comando **Update-database**

La nuova tabella è ora presente nel database.

## <a name="retrieve-the-additional-data"></a>Recuperare i dati aggiuntivi

Esistono due modi per recuperare dati utente aggiuntivi. Il primo consiste nel mantenere i dati utente restituiti, per impostazione predefinita, durante la richiesta di autenticazione. Il secondo consiste nel chiamare in modo specifico l'API del provider e richiedere ulteriori informazioni. I valori per FullName e link vengono passati automaticamente di nuovo da Facebook. Valore che indica se Facebook ha verificato che l'account venga recuperato tramite una chiamata all'API di Facebook. In primo luogo, si popolano i valori per FullName e link e successivamente si otterrà il valore verificato.

Per recuperare i dati utente aggiuntivi, aprire il file **AccountController.cs** nella cartella **Controllers** .

Questo file contiene la logica per la registrazione, la registrazione e la gestione degli account. In particolare, si notino i metodi denominati **ExternalLoginCallback** e **ExternalLoginConfirmation**. All'interno di questi metodi, è possibile aggiungere codice per personalizzare le operazioni di accesso esterno per l'applicazione. La prima riga del metodo **ExternalLoginCallback** contiene:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

I dati utente aggiuntivi vengono passati nuovamente nella proprietà dei **dati di dati** di **AuthenticationResult** dell'oggetto restituito dal metodo **VerifyAuthentication** . Il client Facebook contiene i valori seguenti nella proprietà dei **dati di dati** :

- ID
- name
- collegamento
- gender
- AccessToken

Altri provider avranno dati simili ma leggermente diversi nella proprietà dei dati.

Se l'utente non ha familiarità con il sito, sarà possibile recuperare alcuni dati aggiuntivi e passare i dati alla visualizzazione di conferma. L'ultimo blocco di codice nel metodo viene eseguito solo se l'utente è nuovo per il sito. Sostituire la riga seguente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Con la riga seguente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Questa modifica include solo i valori per le proprietà FullName e link.

Nel metodo **ExternalLoginConfirmation** modificare il codice come evidenziato di seguito per salvare le informazioni aggiuntive sull'utente.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Regolazione della visualizzazione

I dati aggiuntivi dell'utente recuperati dal provider verranno visualizzati nella pagina di registrazione.

Nella cartella **Views**/**account** aprire **ExternalLoginConfirmation. cshtml**. Sotto il campo esistente per nome utente aggiungere i campi per FullName, link e PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

A questo punto è quasi possibile eseguire l'applicazione e registrare un nuovo utente con le informazioni aggiuntive salvate. È necessario disporre di un account che non sia già registrato con il sito. È possibile usare un account di test diverso oppure eliminare le righe nel **USERPROFILE** e nelle **pagine Web\_** le tabelle OAuthMembership per l'account da riutilizzare. Eliminando tali righe, sarà necessario assicurarsi che l'account sia registrato nuovamente.

Eseguire l'applicazione e registrare il nuovo utente. Si noti che questa volta la pagina di conferma contiene più valori.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Al termine della registrazione, chiudere il browser. Esaminare il database per rilevare i nuovi valori nella tabella **ExtraUserInformation** .

## <a name="install-nuget-package-for-facebook-api"></a>Installare il pacchetto NuGet per l'API Facebook

Facebook fornisce un' [API](https://developers.facebook.com/docs/reference/apis/) che è possibile chiamare per eseguire operazioni. È possibile chiamare l'API di Facebook indirizzando l'invio di richieste HTTP o usando l'installazione di un pacchetto NuGet che facilita l'invio di tali richieste. In questa esercitazione viene illustrato l'uso di un pacchetto NuGet, ma l'installazione del pacchetto NuGet non è essenziale. Questa esercitazione illustra come usare il pacchetto di C# Facebook SDK. Sono disponibili altri pacchetti NuGet che facilitano la chiamata dell'API di Facebook.

Nella finestra **Gestisci pacchetti NuGet** selezionare il pacchetto di C# Facebook SDK.

![Installa pacchetto](using-oauth-providers-with-mvc/_static/image13.png)

Si userà l'SDK di C# Facebook per chiamare un'operazione che richiede il token di accesso per l'utente. La sezione successiva Mostra come ottenere il token di accesso.

## <a name="retrieve-access-token"></a>Recuperare il token di accesso

La maggior parte dei provider esterni passa di nuovo un token di accesso dopo la verifica delle credenziali dell'utente. Questo token di accesso è molto importante perché consente di chiamare le operazioni disponibili solo per gli utenti autenticati. Pertanto, il recupero e l'archiviazione del token di accesso è essenziale quando si desidera fornire altre funzionalità.

A seconda del provider esterno, il token di accesso può essere valido solo per un periodo di tempo limitato. Per assicurarsi di disporre di un token di accesso valido, sarà possibile recuperarlo ogni volta che l'utente esegue l'accesso e archiviarlo come valore di sessione anziché salvarlo in un database.

Nel metodo **ExternalLoginCallback** , anche il token di accesso viene restituito nella proprietà dei **dati di dati** di questo oggetto **AuthenticationResult** . Aggiungere il codice evidenziato a **ExternalLoginCallback** per salvare il token di accesso nell'oggetto **sessione** . Questo codice viene eseguito ogni volta che l'utente accede con un account Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Sebbene in questo esempio venga recuperato un token di accesso da Facebook, è possibile recuperare il token di accesso da qualsiasi provider esterno tramite la stessa chiave denominata &quot;AccessToken&quot;.

## <a name="logging-off"></a>Disconnessione

Il metodo di **disconnessione** predefinito disconnette l'utente dall'applicazione, ma non disconnette l'utente dal provider esterno. Per disconnettere l'utente da Facebook e impedire che il token venga salvato in modo permanente dopo che l'utente si è disconnesso, è possibile aggiungere il codice evidenziato seguente al metodo di **disconnessione** in AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

Il valore fornito nel parametro `next` è l'URL da usare dopo che l'utente si è disconnesso da Facebook. Quando si esegue il test nel computer locale, è necessario fornire il numero di porta corretto per il sito locale. In produzione, è necessario specificare una pagina predefinita, ad esempio contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperare le informazioni utente che richiedono il token di accesso

Ora che è stato archiviato il token di accesso e installato il C# pacchetto di Facebook SDK, è possibile usarli insieme per richiedere altre informazioni utente da Facebook. Nel metodo **ExternalLoginConfirmation** creare un'istanza della classe **FacebookClient** passando il valore del token di accesso. Richiedere il valore della proprietà **verificata** per l'utente autenticato corrente. La proprietà **verificata** indica se Facebook ha convalidato l'account con altri mezzi, ad esempio l'invio di un messaggio a un telefono cellulare. Salvare questo valore nel database.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Sarà necessario eliminare i record nel database per l'utente oppure usare un account Facebook diverso.

Eseguire l'applicazione e registrare il nuovo utente. Esaminare la tabella **ExtraUserInformation** per visualizzare il valore della proprietà verificata.

## <a name="conclusion"></a>Conclusione

In questa esercitazione è stato creato un sito integrato con Facebook per l'autenticazione utente e i dati di registrazione. Si è appreso il comportamento predefinito configurato per l'applicazione Web MVC 4 e come personalizzare il comportamento predefinito.

## <a name="related-topics"></a>Argomenti correlati

- [Creare un'app ASP.NET MVC con autenticazione e database SQL e distribuirla nel servizio app di Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
