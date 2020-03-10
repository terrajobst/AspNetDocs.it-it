---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
title: Checkout e pagamento con PayPal | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 664ec95e-b0c9-4f43-a39f-798d0f2a7e08
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/checkout-and-payment-with-paypal
msc.type: authoredcontent
ms.openlocfilehash: 62d00a86c6c5845fb894896df65002c7086d039f
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78565408"
---
# <a name="checkout-and-payment-with-paypal"></a>Completamento della transazione e pagamento con PayPal

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

Questa esercitazione illustra come modificare l'applicazione di esempio Wingtip Toys in modo da includere l'autorizzazione utente, la registrazione e il pagamento tramite PayPal. Solo gli utenti connessi avranno l'autorizzazione per l'acquisto di prodotti. La funzionalità di registrazione utente predefinita del modello di progetto Web Form ASP.NET 4,5 include già la maggior parte degli elementi necessari. Si aggiungerà la funzionalità PayPal Express Checkout. In questa esercitazione si usa l'ambiente di test per sviluppatori PayPal, quindi non verrà trasferito alcun denaro effettivo. Al termine dell'esercitazione, si eseguirà il test dell'applicazione selezionando i prodotti da aggiungere al carrello della spesa, facendo clic sul pulsante Estrai e trasferendo i dati al sito Web di test di PayPal. Nel sito Web di test di PayPal è necessario confermare le informazioni di spedizione e pagamento, quindi tornare all'applicazione di esempio Wingtip Toys locale per confermare e completare l'acquisto.

Sono disponibili diversi processori di pagamento di terze parti che si specializzano nello shopping online che risolve la scalabilità e la sicurezza. Gli sviluppatori ASP.NET devono considerare i vantaggi derivanti dall'utilizzo di una soluzione di pagamento di terze parti prima di implementare una soluzione acquisti e acquisti.

> [!NOTE] 
> 
> L'applicazione di esempio Wingtip Toys è stata progettata per illustrare le funzionalità e i concetti specifici di ASP.NET disponibili per gli sviluppatori Web di ASP.NET. Questa applicazione di esempio non è stata ottimizzata per tutte le circostanze possibili per quanto riguarda la scalabilità e la sicurezza.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come limitare l'accesso a pagine specifiche in una cartella.
- Come creare un carrello acquisti noto da un carrello acquisti anonimo.
- Come abilitare SSL per il progetto.
- Come aggiungere un provider OAuth al progetto.
- Come utilizzare PayPal per acquistare prodotti tramite l'ambiente di test di PayPal.
- Come visualizzare i dettagli da PayPal in un controllo **DetailsView** .
- Come aggiornare il database dell'applicazione Wingtip Toys con i dettagli ottenuti da PayPal.

## <a name="adding-order-tracking"></a>Aggiunta del rilevamento degli ordini

In questa esercitazione verranno create due nuove classi per tenere traccia dei dati dall'ordine di creazione di un utente. Le classi consentono di tenere traccia dei dati relativi a informazioni sulla spedizione, totale acquisti e conferma di pagamento.

### <a name="add-the-order-and-orderdetail-model-classes"></a>Aggiungere le classi del modello Order e OrderDetail

In precedenza in questa serie di esercitazioni è stato definito lo schema per le categorie, i prodotti e gli elementi del carrello acquisti creando le classi `Category`, `Product`e `CartItem` nella cartella *Models* . Verranno ora aggiunte due nuove classi per definire lo schema per l'ordine del prodotto e i dettagli dell'ordine.

1. Nella cartella **Models** aggiungere una nuova classe denominata *Order.cs*.   
   Il nuovo file di classe verrà visualizzato nell'editor.
2. Sostituire il codice predefinito con quello riportato di seguito:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample1.cs)]
3. Aggiungere una classe *OrderDetail.cs* alla cartella *Models* .
4. Sostituire il codice predefinito con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample2.cs)]

Le classi `Order` e `OrderDetail` contengono lo schema per definire le informazioni sugli ordini utilizzate per l'acquisto e la spedizione.

Inoltre, sarà necessario aggiornare la classe del contesto di database che gestisce le classi di entità e che fornisce l'accesso ai dati al database. A tale scopo, si aggiungeranno le classi di ordine appena creato e `OrderDetail` modello a `ProductContext` classe.

1. In **Esplora soluzioni**individuare e aprire il file *ProductContext.cs* .
2. Aggiungere il codice evidenziato al file *ProductContext.cs* come illustrato di seguito:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample3.cs?highlight=14-15)]

Come indicato in precedenza in questa serie di esercitazioni, il codice nel file *ProductContext.cs* aggiunge lo spazio dei nomi `System.Data.Entity` in modo da avere accesso a tutte le funzionalità di base del Entity Framework. Questa funzionalità include la funzionalità per eseguire query e inserire, aggiornare ed eliminare dati mediante l'utilizzo di oggetti fortemente tipizzati. Il codice precedente nella classe `ProductContext` aggiunge Entity Framework accesso alle classi `Order` e `OrderDetail` appena aggiunte.

## <a name="adding-checkout-access"></a>Aggiunta dell'accesso di estrazione

L'applicazione di esempio Wingtip Toys consente agli utenti anonimi di esaminare e aggiungere i prodotti a un carrello acquisti. Tuttavia, quando gli utenti anonimi scelgono di acquistare i prodotti aggiunti al carrello acquisti, devono accedere al sito. Una volta effettuato l'accesso, gli utenti possono accedere alle pagine con restrizioni dell'applicazione Web che gestiscono il processo di estrazione e acquisto. Queste pagine con restrizioni sono contenute nella cartella *checkout* dell'applicazione.

### <a name="add-a-checkout-folder-and-pages"></a>Aggiungere una cartella e una pagina di estrazione

Verranno ora create la cartella *checkout* e le pagine che verranno visualizzate dal cliente durante il processo di estrazione. Queste pagine vengono aggiornate più avanti in questa esercitazione.

1. Fare clic con il pulsante destro del mouse sul nome del progetto (**Wingtip Toys**) in **Esplora soluzioni** e selezionare **Aggiungi una nuova cartella**. 

    ![Checkout e pagamento con PayPal-nuova cartella](checkout-and-payment-with-paypal/_static/image1.png)
2. Denominare la nuova cartella *checkout*.
3. Fare clic con il pulsante destro del mouse sulla cartella *checkout* , quindi scegliere **Aggiungi**-&gt;**nuovo elemento**. 

    ![Checkout e pagamento con PayPal-nuovo elemento](checkout-and-payment-with-paypal/_static/image2.png)
4. Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
5. Selezionare il **gruppo C# Visual** -&gt; modelli **Web** a sinistra. Quindi, dal riquadro centrale, selezionare **Web Form con la pagina master**e denominarlo *CheckoutStart. aspx*. 

    ![Estrazione e pagamento con PayPal-finestra di dialogo Aggiungi nuovo elemento](checkout-and-payment-with-paypal/_static/image3.png)
6. Come in precedenza, selezionare il file *site. master* come pagina master.
7. Aggiungere le seguenti pagine aggiuntive alla cartella *checkout* utilizzando la stessa procedura precedente:   

    - CheckoutReview.aspx
    - CheckoutComplete.aspx
    - CheckoutCancel.aspx
    - CheckoutError.aspx

### <a name="add-a-webconfig-file"></a>Aggiungere un file Web. config

Se si aggiunge un nuovo file *Web. config* alla cartella *checkout* , sarà possibile limitare l'accesso a tutte le pagine contenute nella cartella.

1. Fare clic con il pulsante destro del mouse sulla cartella *checkout* e scegliere **Aggiungi** -&gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **gruppo C# Visual** -&gt; modelli **Web** a sinistra. Quindi, nel riquadro centrale selezionare file di **configurazione Web**, accettare il nome predefinito di *Web. config*, quindi selezionare **Aggiungi**.
3. Sostituire il contenuto XML esistente nel file *Web.config* con il contenuto seguente:  

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample4.xml)]
4. Salvare il file *Web.config* .

Il file *Web. config* specifica che è necessario negare l'accesso alle pagine contenute nella cartella *checkout* a tutti gli utenti sconosciuti dell'applicazione Web. Tuttavia, se l'utente ha registrato un account ed è connesso, sarà un utente noto e avrà accesso alle pagine nella cartella *checkout* .

È importante notare che la configurazione di ASP.NET segue una gerarchia, in cui ogni file *Web. config* applica le impostazioni di configurazione alla cartella in cui si trova e a tutte le directory figlio sottostanti.

<a id="SSLWebForms"></a>
## <a name="enable-ssl-for-the-project"></a>Abilitare SSL per il progetto

 Secure Sockets Layer (SSL) è un protocollo definito per consentire ai server Web e ai client Web di comunicare in modo più sicuro tramite l'uso della crittografia. Quando non si usa SSL, i dati inviati tra il client e il server possono essere soggetti allo sniffing dei pacchetti da qualsiasi soggetto con accesso fisico alla rete. Inoltre, numerosi schemi di autenticazione comuni non sono sicuri sul protocollo HTTP normale. In particolare, l'autenticazione di base e l'autenticazione basata su form inviano credenziali non crittografate. Per essere sicuri, questi schemi di autenticazione devono usare il protocollo SSL. 

1. In **Esplora soluzioni**fare clic sul progetto **WingtipToys** , quindi premere **F4** per visualizzare la finestra **Proprietà** .
2. Modificare **SSL abilitato** in `true`.
3. Copiare l' **URL SSL** per poterlo usare in un secondo momento.   
 L'URL SSL verrà `https://localhost:44300/` a meno che non siano stati creati in precedenza siti Web SSL (come illustrato di seguito).   
    ![Proprietà di progetti](checkout-and-payment-with-paypal/_static/image4.png)
4. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sul progetto **WingtipToys** e scegliere **proprietà**.
5. Nella scheda a sinistra fare clic su **Web**.
6. Modificare l' **URL del progetto** in modo da usare l' **URL SSL** salvato in precedenza.   
    ![Proprietà del progetto Web](checkout-and-payment-with-paypal/_static/image5.png)
7. Per salvare la pagina, premere **CTRL+S**.
8. Premere **CTRL+F5** per eseguire l'applicazione. In Visual Studio verrà visualizzata un'opzione che consente di evitare eventuali avvisi SSL.
9. Fare clic su **Sì** per considerare attendibile il certificato SSL di IIS Express e continuare.   
    ![Dettagli del certificato SSL di IIS Express](checkout-and-payment-with-paypal/_static/image6.png)  
 Verrà visualizzato un avviso di sicurezza.
10. Fare clic su **Sì** per installare il certificato per l'host locale.   
    ![Finestra di dialogo Avviso di sicurezza](checkout-and-payment-with-paypal/_static/image7.png)  
 Verrà visualizzata la finestra del browser.

È ora possibile testare facilmente l'applicazione Web in locale usando SSL.

<a id="OAuthWebForms"></a>
## <a name="add-an-oauth-20-provider"></a>Aggiungere un provider OAuth 2.0

ASP.NET Web Form fornisce opzioni avanzate per l'appartenenza e l'autenticazione. Questi miglioramenti includono OAuth. OAuth è un protocollo aperto che consente l'autorizzazione sicura in un metodo semplice e standard da applicazioni Web, per dispositivi mobili e desktop. Il modello Web Form ASP.NET usa OAuth per esporre Facebook, Twitter, Google e Microsoft come provider di autenticazione. Sebbene in questa esercitazione venga usato solo Google come provider di autenticazione, è possibile modificare facilmente il codice per usare uno dei provider. I passaggi per l'implementazione di altri provider sono molto simili a quelli che verranno visualizzati in questa esercitazione.

Oltre all'autenticazione, nell'esercitazione verranno utilizzati anche i ruoli per implementare l'autorizzazione. Solo gli utenti aggiunti al ruolo `canEdit` saranno in grado di modificare i dati, ovvero creare, modificare o eliminare i contatti.

> [!NOTE] 
> 
> Le applicazioni Windows Live accettano solo un URL Live per un sito Web funzionante, pertanto non è possibile usare un URL del sito Web locale per il test degli account di accesso.

La procedura seguente consente di aggiungere un provider di autenticazione Google.

1. Aprire l' *App\_file Start\Startup.auth.cs* .
2. Rimuovere i caratteri di commento dal metodo `app.UseGoogleAuthentication()` in modo da ottenere il codice seguente: 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample5.cs)]
3. Passare a [Google Developers Console](https://console.developers.google.com/). Sarà necessario eseguire l'accesso con l'account di posta elettronica per sviluppatori di Google (gmail.com). Se non si dispone di un account Google, selezionare il collegamento **Crea un account** .   
   Verrà quindi visualizzata la pagina **Google Developers Console**.   
    ![Google Developers Console](checkout-and-payment-with-paypal/_static/image8.png)
4. Fare clic sul pulsante **Crea progetto** e immettere un nome e un ID per il progetto (è possibile usare i valori predefiniti). Quindi, fare clic sulla casella di controllo **contratto** e sul pulsante **Crea** .  

    ![Google-nuovo progetto](checkout-and-payment-with-paypal/_static/image9.png)

   In pochi secondi verrà creato il nuovo progetto e nel browser verrà visualizzata la pagina nuovi progetti.
5. Nella scheda a sinistra fare clic su **api &amp; autenticazione**, quindi fare clic su **credenziali**.
6. Fare clic su **Crea nuovo ID client** in **OAuth**.   
   Verrà visualizzata la finestra di dialogo **Create Client ID** .   
    ![Google - creare ID Client](checkout-and-payment-with-paypal/_static/image10.png)
7. Nella finestra di dialogo **Crea ID client** , salvare l' **applicazione Web** predefinita per il tipo di applicazione.
8. Impostare le **origini JavaScript autorizzate** sull'URL SSL usato in precedenza in questa esercitazione (`https://localhost:44300/` a meno che non siano stati creati altri progetti SSL).   
   Questo URL rappresenta l'origine dell'applicazione. Per questo esempio, sarà necessario immettere solo l'URL di test localhost. Tuttavia, è possibile immettere più URL per tenere conto di localhost e di produzione.
9. Per **Authorized Redirect URI** immettere le impostazioni seguenti: 

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample6.html)]

   Questo valore è l'URI che verrà usato da ASP.NET OAuth per comunicare con il server OAuth di Google. Ricordare l'URL SSL usato in precedenza (`https://localhost:44300/`, a meno che non siano stati creati altri progetti SSL).
10. Fare clic sul pulsante **Crea ID client** .
11. Nel menu a sinistra di Google Developers console fare clic sulla voce di menu della **schermata di consenso** , quindi impostare l'indirizzo di posta elettronica e il nome del prodotto. Una volta completato il modulo, fare clic su **Salva**.
12. Fare clic sulla voce di menu **API** , scorrere verso il basso e fare clic sul pulsante **off** accanto a **Google + API**.   
    Se si accetta questa opzione, l'API Google + verrà abilitata.
13. È anche necessario aggiornare il pacchetto NuGet **Microsoft. Owin** alla versione 3.0.0.   
    Dal menu **strumenti** selezionare **Gestione pacchetti NuGet** e quindi selezionare **Gestisci pacchetti NuGet per la soluzione**.  
    Dalla finestra **Gestisci pacchetti NuGet** trovare e aggiornare il pacchetto **Microsoft. Owin** alla versione 3.0.0.
14. In Visual Studio aggiornare il metodo `UseGoogleAuthentication` della pagina *Startup.auth.cs* copiando e incollando l' **ID client** e il **segreto client** nel metodo. I valori relativi all' **ID client** e al **segreto client** riportati di seguito sono esempi e non funzioneranno. 

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample7.cs?highlight=64-65)]
15. Premere **CTRL+F5** per compilare ed eseguire l'applicazione. Fare clic sul collegamento **Accedi** .
16. In **Usa un altro servizio per accedere**fare clic su **Google**.  
    ![Accesso](checkout-and-payment-with-paypal/_static/image11.png)
17. Se è necessario immettere le credenziali, si verrà reindirizzati al sito Google in cui verranno immesse le credenziali.  
    ![Google - accesso](checkout-and-payment-with-paypal/_static/image12.png)
18. Dopo aver immesso le credenziali, verrà richiesto di concedere le autorizzazioni all'applicazione Web appena creata.  
    ![Account del servizio di progetto predefinito](checkout-and-payment-with-paypal/_static/image13.png)
19. Fare clic **Accept**. A questo punto si verrà reindirizzati alla pagina **Register** dell'applicazione **WingtipToys** in cui è possibile registrare il proprio account Google.  
    ![Registrare con l'Account Google](checkout-and-payment-with-paypal/_static/image14.png)
20. Sarà possibile modificare il nome di registrazione dell'indirizzo di posta elettronica locale usato per l'account Gmail, anche se in generale è preferibile mantenere l'alias di posta elettronica predefinito, ovvero quello usato per l'autenticazione. Fare clic su Accedi come illustrato **in** precedenza.

### <a name="modifying-login-functionality"></a>Modifica della funzionalità di accesso

Come indicato in precedenza in questa serie di esercitazioni, la maggior parte delle funzionalità di registrazione utente è stata inclusa nel modello Web Forms di ASP.NET per impostazione predefinita. Verranno ora modificate le pagine default *login. aspx* e *Register. aspx* per chiamare il metodo `MigrateCart`. Il metodo `MigrateCart` associa un nuovo utente connesso a un carrello acquisti anonimo. Associando l'utente e il carrello acquisti, l'applicazione di esempio Wingtip Toys sarà in grado di gestire il carrello degli acquisti dell'utente tra le visite.

1. In **Esplora soluzioni**individuare e aprire la cartella *account* .
2. Modificare la pagina code-behind denominata *login.aspx.cs* per includere il codice evidenziato in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample8.cs?highlight=41-43)]
3. Salvare il file *login.aspx.cs* .

Per il momento, è possibile ignorare l'avviso indicante che non esiste alcuna definizione per il metodo `MigrateCart`. Questa operazione verrà aggiunta leggermente più avanti in questa esercitazione.

Il file code-behind *login.aspx.cs* supporta un metodo login. Esaminando la pagina login. aspx si noterà che in questa pagina è incluso un pulsante "Accedi" che, quando si fa clic, attiva il gestore `LogIn` sul code-behind.

Quando viene chiamato il metodo `Login` su *login.aspx.cs* , viene creata una nuova istanza del carrello acquisti denominato `usersShoppingCart`. L'ID del carrello acquisti (un GUID) viene recuperato e impostato sulla variabile `cartId`. Viene quindi chiamato il metodo `MigrateCart`, passando il `cartId` e il nome dell'utente connesso a questo metodo. Quando viene eseguita la migrazione del carrello acquisti, il GUID usato per identificare il carrello acquisti anonimo viene sostituito con il nome utente.

Oltre a modificare il file code-behind *login.aspx.cs* per eseguire la migrazione del carrello acquisti quando l'utente effettua l'accesso, è necessario modificare anche il *file code-behind Register.aspx.cs* per eseguire la migrazione del carrello acquisti quando l'utente crea un nuovo account e accede.

1. Nella cartella *account* aprire il file code-behind denominato *Register.aspx.cs*.
2. Modificare il file code-behind includendo il codice in giallo, in modo che venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample9.cs?highlight=28-32)]
3. Salvare il file *Register.aspx.cs* . Ancora una volta, ignorare l'avviso relativo al metodo `MigrateCart`.

Si noti che il codice usato nel gestore eventi `CreateUser_Click` è molto simile al codice usato nel metodo `LogIn`. Quando l'utente registra o accede al sito, viene effettuata una chiamata al metodo `MigrateCart`.

## <a name="migrating-the-shopping-cart"></a>Migrazione del carrello acquisti

Ora che il processo di accesso e registrazione è stato aggiornato, è possibile aggiungere il codice per eseguire la migrazione del carrello acquisti usando il metodo `MigrateCart`.

1. In **Esplora soluzioni**individuare la cartella *logica* e aprire il file di classe *ShoppingCartActions.cs* .
2. Aggiungere il codice evidenziato in giallo al codice esistente nel file *ShoppingCartActions.cs* , in modo che il codice nel file *ShoppingCartActions.cs* venga visualizzato come segue:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample10.cs?highlight=215-224)]

Il metodo `MigrateCart` usa il cartId esistente per trovare il carrello degli acquisti dell'utente. Il codice scorre quindi tutti gli elementi del carrello acquisti e sostituisce la proprietà `CartId` (come specificato dallo schema `CartItem`) con il nome utente connesso.

### <a name="updating-the-database-connection"></a>Aggiornamento della connessione al database

Se si segue questa esercitazione usando l'applicazione **di esempio Wingtip Toys** predefinita, è necessario ricreare il database delle appartenenze predefinito. Modificando la stringa di connessione predefinita, il database di appartenenza verrà creato alla successiva esecuzione dell'applicazione.

1. Aprire il file *Web. config* nella radice del progetto.
2. Aggiornare la stringa di connessione predefinita in modo che appaia come segue:   

    [!code-xml[Main](checkout-and-payment-with-paypal/samples/sample11.xml)]

<a id="PayPalWebForms"></a>
## <a name="integrating-paypal"></a>Integrazione di PayPal

PayPal è una piattaforma di fatturazione basata sul Web che accetta i pagamenti dei commercianti online. Questa esercitazione illustra come integrare la funzionalità di estrazione rapida di PayPal nell'applicazione. Express Checkout consente ai clienti di usare PayPal per pagare gli elementi aggiunti al carrello acquisti.

### <a name="create-paypal-test-accounts"></a>Crea account di test PayPal

Per usare l'ambiente di test di PayPal, è necessario creare e verificare un account di test per sviluppatori. Si userà l'account di test per sviluppatori per creare un account di test dell'acquirente e un account di prova del venditore. Le credenziali dell'account di test per sviluppatori consentiranno anche all'applicazione di esempio Wingtip Toys di accedere all'ambiente di test di PayPal.

1. In un browser passare al sito di test per sviluppatori PayPal:   
    [https://developer.paypal.com](https://developer.paypal.com/)
2. Se non si dispone di un account per sviluppatore PayPal, creare un nuovo account facendo clic su **Iscriviti**e seguendo la procedura di iscrizione. Se si dispone di un account sviluppatore PayPal esistente, effettuare l'accesso facendo clic su **Accedi**. È necessario un account per sviluppatore PayPal per testare l'applicazione di esempio Wingtip Toys più avanti in questa esercitazione.
3. Se è stata appena effettuata l'iscrizione per l'account sviluppatore PayPal, potrebbe essere necessario verificare l'account per sviluppatore PayPal con PayPal. È possibile verificare l'account attenendosi alla procedura inviata da PayPal al proprio account di posta elettronica. Dopo aver verificato l'account per sviluppatore PayPal, accedere nuovamente al sito di test per sviluppatori PayPal.
4. Dopo aver effettuato l'accesso al sito per sviluppatori PayPal con l'account per sviluppatore PayPal, è necessario creare un account di prova del compratore PayPal se non ne è già presente uno. Per creare un account di test del compratore, nel sito PayPal fare clic sulla scheda **applicazioni** e quindi fare clic su **account sandbox**.   
 Viene visualizzata la pagina **account di test Sandbox** .   

    > [!NOTE] 
    > 
    > Il sito per sviluppatori PayPal dispone già di un account di test di Merchant.

    ![Checkout e pagamento con account di test PayPal-Sandbox](checkout-and-payment-with-paypal/_static/image15.png)
5. Nella pagina account di test Sandbox fare clic su **Crea account**.
6. Nella pagina **Crea account di test** scegliere l'indirizzo di posta elettronica e la password di un account di prova dell'acquirente.   

    > [!NOTE] 
    > 
    > Sono necessari gli indirizzi di posta elettronica e la password dell'acquirente per testare l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione.

    ![Checkout e pagamento con account di test PayPal-Sandbox](checkout-and-payment-with-paypal/_static/image16.png)
7. Creare l'account di test dell'acquirente facendo clic sul pulsante **Crea account** .  
 Viene visualizzata la pagina **account di test Sandbox** . 

    ![Checkout e pagamento con PayPal-account PayPal](checkout-and-payment-with-paypal/_static/image17.png)
8. Nella pagina **account di test Sandbox** fare clic sull'account di posta elettronica **facilitatore** .  
    Verranno visualizzate le opzioni **profilo** e **notifica** .
9. Selezionare l'opzione **profilo** , quindi fare clic su **credenziali API** per visualizzare le credenziali API per l'account di test Merchant.
10. Copiare le credenziali dell'API di TEST nel blocco note.

Sono necessarie le credenziali dell'API di TEST classica visualizzate (nome utente, password e firma) per effettuare chiamate API dall'applicazione di esempio Wingtip Toys all'ambiente di test di PayPal. Le credenziali vengono aggiunte nel passaggio successivo.

### <a name="add-paypal-class-and-api-credentials"></a>Aggiungere le credenziali di classe e API PayPal

La maggior parte del codice PayPal viene posizionata in un'unica classe. Questa classe contiene i metodi utilizzati per comunicare con PayPal. Inoltre, sarà necessario aggiungere le credenziali di PayPal a questa classe.

1. Nell'applicazione di esempio Wingtip Toys in Visual Studio fare clic con il pulsante destro del mouse sulla cartella **logica** , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. In **visuale C#**  dal riquadro **installato** a sinistra, selezionare **codice**.
3. Dal riquadro centrale selezionare **classe**. Assegnare alla nuova classe il nome **PayPalFunctions.cs**.
4. Fare clic su **Aggiungi**.  
   Il nuovo file di classe verrà visualizzato nell'editor.
5. Sostituire il codice predefinito con il codice seguente:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample12.cs)]
6. Aggiungere le credenziali dell'API Merchant (nome utente, password e firma) visualizzate in precedenza in questa esercitazione, in modo da poter effettuare chiamate di funzione all'ambiente di test di PayPal.  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample13.cs)]

> [!NOTE] 
> 
> In questa applicazione di esempio è sufficiente aggiungere le credenziali a C# un file (. cs). Tuttavia, in una soluzione implementata è opportuno prendere in considerazione la possibilità di crittografare le credenziali in un file di configurazione.

La classe NVPAPICaller contiene la maggior parte della funzionalità PayPal. Il codice nella classe fornisce i metodi necessari per effettuare un acquisto di test dall'ambiente di test di PayPal. Per effettuare acquisti vengono usate le tre funzioni PayPal seguenti:

- Funzione `SetExpressCheckout`
- Funzione `GetExpressCheckoutDetails`
- Funzione `DoExpressCheckoutPayment`

Il metodo `ShortcutExpressCheckout` raccoglie le informazioni di acquisto del test e i dettagli del prodotto dal carrello acquisti e chiama la funzione `SetExpressCheckout` PayPal. Il metodo `GetCheckoutDetails` conferma i dettagli di acquisto e chiama la funzione `GetExpressCheckoutDetails` PayPal prima di effettuare l'acquisto del test. Il metodo `DoCheckoutPayment` completa l'acquisto di test dall'ambiente di test chiamando la funzione `DoExpressCheckoutPayment` PayPal. Il codice rimanente supporta i metodi e il processo PayPal, ad esempio stringhe di codifica, decodifica di stringhe, elaborazione di matrici e determinazione delle credenziali.

> [!NOTE] 
> 
> PayPal consente di includere i dettagli di acquisto facoltativi in base alle [specifiche API di PayPal](https://cms.paypal.com/us/cgi-bin/?cmd=_render-content&amp;content_ID=developer/e_howto_api_nvp_r_SetExpressCheckout). Estendendo il codice nell'applicazione di esempio Wingtip Toys, è possibile includere i dettagli di localizzazione, le descrizioni dei prodotti, le imposte, il numero di servizio del cliente, oltre a molti altri campi facoltativi.

Si noti che gli URL return e Cancel specificati nel metodo **ShortcutExpressCheckout** utilizzano un numero di porta.

[!code-html[Main](checkout-and-payment-with-paypal/samples/sample14.html)]

Quando in Visual Web Developer viene eseguito un progetto Web utilizzando SSL, in genere viene utilizzata la porta 44300 per il server Web. Come illustrato sopra, il numero di porta è 44300. Quando si esegue l'applicazione, è possibile che venga visualizzato un numero di porta diverso. Il numero di porta deve essere impostato correttamente nel codice in modo da poter eseguire l'applicazione di esempio Wingtip Toys alla fine di questa esercitazione. Nella sezione successiva di questa esercitazione viene illustrato come recuperare il numero di porta host locale e aggiornare la classe PayPal.

### <a name="update-the-localhost-port-number-in-the-paypal-class"></a>Aggiornare il numero di porta LocalHost nella classe PayPal

L'applicazione di esempio Wingtip Toys Acquista i prodotti passando al sito di test di PayPal e tornando all'istanza locale dell'applicazione di esempio Wingtip Toys. Per fare in modo che PayPal torni all'URL corretto, è necessario specificare il numero di porta dell'applicazione di esempio in esecuzione localmente nel codice PayPal menzionato in precedenza.

1. Fare clic con il pulsante destro del mouse sul nome del progetto (**WingtipToys**) in **Esplora soluzioni** e selezionare **proprietà**.
2. Nella colonna sinistra selezionare la scheda **Web** .
3. Recuperare il numero di porta dalla casella **URL progetto** .
4. Se necessario, aggiornare il `returnURL` e `cancelURL` nella classe PayPal (`NVPAPICaller`) nel file *PayPalFunctions.cs* per usare il numero di porta dell'applicazione Web:   

    [!code-html[Main](checkout-and-payment-with-paypal/samples/sample15.html?highlight=1-2)]

A questo punto il codice aggiunto corrisponderà alla porta prevista per l'applicazione Web locale. PayPal sarà in grado di tornare all'URL corretto nel computer locale.

### <a name="add-the-paypal-checkout-button"></a>Aggiungere il pulsante PayPal Checkout

Ora che le funzioni di PayPal primarie sono state aggiunte all'applicazione di esempio, è possibile iniziare ad aggiungere il markup e il codice necessari per chiamare tali funzioni. Per prima cosa, è necessario aggiungere il pulsante di estrazione visualizzato dall'utente nella pagina del carrello acquisti.

1. Aprire il file *ShoppingCart. aspx* .
2. Scorrere fino alla fine del file e trovare il commento `<!--Checkout Placeholder -->`.
3. Sostituire il commento con un controllo `ImageButton` in modo che il contrassegno venga sostituito come segue:  

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample16.aspx)]
4. Nel file *ShoppingCart.aspx.cs* , dopo il gestore dell'evento `UpdateBtn_Click` in prossimità della fine del file, aggiungere il gestore dell'evento `CheckOutBtn_Click`:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample17.cs)]
5. Inoltre, nel file *ShoppingCart.aspx.cs* aggiungere un riferimento al `CheckoutBtn`, in modo che venga fatto riferimento al pulsante nuova immagine, come indicato di seguito:  

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample18.cs?highlight=18)]
6. Salvare le modifiche apportate al file *ShoppingCart. aspx* e al file *ShoppingCart.aspx.cs* .
7. Dal menu selezionare **Debug**-&gt;**Build WingtipToys**.  
   Il progetto verrà ricompilato con il controllo **ImageButton** appena aggiunto.

### <a name="send-purchase-details-to-paypal"></a>Inviare i dettagli di acquisto a PayPal

Quando l'utente fa clic sul pulsante **Estrai** nella pagina del carrello acquisti (*ShoppingCart. aspx*), inizierà il processo di acquisto. Il codice seguente chiama la prima funzione PayPal necessaria per acquistare i prodotti.

1. Dalla cartella *checkout* aprire il file code-behind denominato *CheckoutStart.aspx.cs*.   
   Assicurarsi di aprire il file code-behind.
2. Sostituire il codice esistente con quello seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample19.cs)]

Quando l'utente dell'applicazione fa clic sul pulsante **Estrai** nella pagina carrello acquisti, il browser passerà alla pagina *CheckoutStart. aspx* . Quando viene caricata la pagina *CheckoutStart. aspx* , viene chiamato il metodo `ShortcutExpressCheckout`. A questo punto, l'utente viene trasferito al sito Web di test di PayPal. Nel sito PayPal l'utente immette le proprie credenziali PayPal, esamina i dettagli di acquisto, accetta il contratto PayPal e torna all'applicazione di esempio Wingtip Toys in cui il metodo `ShortcutExpressCheckout` viene completato. Al termine del `ShortcutExpressCheckout` metodo, l'utente verrà reindirizzato alla pagina *CheckoutReview. aspx* specificata nel metodo `ShortcutExpressCheckout`. Ciò consente all'utente di esaminare i dettagli dell'ordine dall'applicazione di esempio Wingtip Toys.

### <a name="review-order-details"></a>Dettagli ordine di Revisione

Dopo la restituzione da PayPal, la pagina *CheckoutReview. aspx* dell'applicazione di esempio Wingtip Toys Visualizza i dettagli dell'ordine. Questa pagina consente all'utente di esaminare i dettagli dell'ordine prima di acquistare i prodotti. È necessario creare la pagina *CheckoutReview. aspx* come indicato di seguito:

1. Nella cartella *checkout* aprire la pagina denominata *CheckoutReview. aspx*.
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample20.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutReview.aspx.cs* e sostituire il codice esistente con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample21.cs)]

Il controllo **DetailsView** viene utilizzato per visualizzare i dettagli dell'ordine restituiti da PayPal. Il codice precedente, inoltre, Salva i dettagli dell'ordine nel database di Wingtip Toys come oggetto `OrderDetail`. Quando l'utente fa clic sul pulsante **completa ordine** , viene reindirizzato alla pagina *CheckoutComplete. aspx* .

> [!NOTE] 
> 
> **Suggerimento**
> 
> Nel markup della pagina *CheckoutReview. aspx* si noti che il tag `<ItemStyle>` viene usato per modificare lo stile degli elementi all'interno del controllo **DetailsView** nella parte inferiore della pagina. Visualizzando la pagina nella **visualizzazione progettazione** (selezionando **progetta** nell'angolo inferiore sinistro di Visual Studio), quindi selezionando il controllo **DetailsView** e selezionando lo **Smart tag** (l'icona a forma di freccia nella parte superiore destra del controllo), sarà possibile visualizzare le attività di **DetailsView**.
> 
> ![Checkout e pagamento con PayPal: modificare i campi](checkout-and-payment-with-paypal/_static/image18.png)
> 
> Selezionando **modifica campi**verrà visualizzata la finestra di dialogo **campi** . In questa finestra di dialogo è possibile controllare facilmente le proprietà visive, ad esempio **ItemStyle**, del controllo **DetailsView** .
> 
> ![Finestra di dialogo Estrai e pagamenti con PayPal-campi](checkout-and-payment-with-paypal/_static/image19.png)

### <a name="complete-purchase"></a>Acquisto completo

La pagina *CheckoutComplete. aspx* effettua l'acquisto da PayPal. Come indicato in precedenza, l'utente deve fare clic sul pulsante **completa ordine** prima che l'applicazione passi alla pagina *CheckoutComplete. aspx* .

1. Nella cartella *checkout* aprire la pagina denominata *CheckoutComplete. aspx*.
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample22.aspx)]
3. Aprire la pagina code-behind denominata *CheckoutComplete.aspx.cs* e sostituire il codice esistente con il codice seguente:   

    [!code-csharp[Main](checkout-and-payment-with-paypal/samples/sample23.cs)]

Quando viene caricata la pagina *CheckoutComplete. aspx* , viene chiamato il metodo `DoCheckoutPayment`. Come indicato in precedenza, il metodo `DoCheckoutPayment` completa l'acquisto dall'ambiente di test di PayPal. Una volta che PayPal ha completato l'acquisto dell'ordine, nella pagina *CheckoutComplete. aspx* viene visualizzata una transazione di pagamento `ID` all'acquirente.

### <a name="handle-cancel-purchase"></a>Gestisci acquisto annullamento

Se l'utente decide di annullare l'acquisto, viene indirizzato alla pagina *CheckoutCancel. aspx* , in cui si noterà che l'ordine è stato annullato.

1. Aprire la pagina denominata *CheckoutCancel. aspx* nella cartella *checkout* .
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample24.aspx)]

### <a name="handle-purchase-errors"></a>Gestione degli errori di acquisto

Gli errori durante il processo di acquisto verranno gestiti dalla pagina *CheckoutError. aspx* . Se si verifica un errore, il code-behind della pagina *CheckoutStart. aspx* , della pagina *CheckoutReview. aspx* e della pagina *CheckoutComplete. aspx* verrà reindirizzato alla pagina *CheckoutError. aspx* .

1. Aprire la pagina denominata *CheckoutError. aspx* nella cartella *checkout* .
2. Sostituire il markup esistente con il codice seguente:   

    [!code-aspx[Main](checkout-and-payment-with-paypal/samples/sample25.aspx)]

La pagina *CheckoutError. aspx* viene visualizzata con i dettagli dell'errore quando si verifica un errore durante il processo di estrazione.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

Eseguire l'applicazione per vedere come acquistare i prodotti. Si noti che verrà eseguito nell'ambiente di test di PayPal. Nessun denaro effettivo scambiato.

1. Assicurarsi che tutti i file siano salvati in Visual Studio.
2. Aprire una Web browser e passare a [https://developer.paypal.com](https://developer.paypal.com/).
3. Accedere con l'account sviluppatore PayPal creato in precedenza in questa esercitazione.  
   Per la sandbox per sviluppatori di PayPal è necessario accedere all' [https://developer.paypal.com](https://developer.paypal.com/) per testare Express Checkout. Questo vale solo per i test sandbox di PayPal, non per l'ambiente live di PayPal.
4. In Visual Studio premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
   Dopo la ricompilazione del database, il browser si aprirà e visualizzerà la pagina *default. aspx* .
5. Aggiungere tre prodotti diversi al carrello della spesa selezionando la categoria del prodotto, ad esempio "Cars" e quindi facendo clic su **Aggiungi al carrello** accanto a ciascun prodotto.  
   Il carrello acquisti visualizzerà il prodotto selezionato.
6. Fare clic sul pulsante **PayPal** per eseguire il checkout. 

    ![Checkout e pagamento con PayPal-cart](checkout-and-payment-with-paypal/_static/image20.png)

   Per l'estrazione sarà necessario disporre di un account utente per l'applicazione di esempio Wingtip Toys.
7. Fare clic sul collegamento **Google** a destra della pagina per accedere con un account di posta elettronica Gmail.com esistente.  
   Se non si dispone di un account gmail.com, è possibile crearne uno a scopo di test in [www.gmail.com](https://www.gmail.com/). È anche possibile usare un account locale standard facendo clic su "Registra". 

    ![Checkout e pagamento con PayPal-accesso](checkout-and-payment-with-paypal/_static/image21.png)
8. Accedere con l'account Gmail e la password. 

    ![Checkout e pagamento con PayPal-accesso a Gmail](checkout-and-payment-with-paypal/_static/image22.png)
9. Fare clic sul pulsante **Accedi** per registrare l'account Gmail con il nome utente dell'applicazione di esempio Wingtip Toys. 

    ![Checkout e pagamento con PayPal-Registra account](checkout-and-payment-with-paypal/_static/image23.png)
10. Nel sito di test di PayPal aggiungere l'indirizzo di posta elettronica dell' **acquirente** e la password creati in precedenza in questa esercitazione, quindi fare clic sul pulsante **Accedi** . 

    ![Checkout e pagamento con PayPal-accesso PayPal](checkout-and-payment-with-paypal/_static/image24.png)
11. Accettare i criteri PayPal, quindi fare clic sul pulsante **Accetto e continua** .  
    Si noti che questa pagina viene visualizzata solo la prima volta che si usa questo account PayPal. Si noti anche che si tratta di un account di prova, non viene scambiato alcun denaro reale. 

    ![Checkout e pagamento con PayPal-criteri PayPal](checkout-and-payment-with-paypal/_static/image25.png)
12. Esaminare le informazioni sull'ordine nella pagina di verifica dell'ambiente di testing PayPal e fare clic su **continua**. 

    ![Checkout e pagamento con PayPal-informazioni sulla revisione](checkout-and-payment-with-paypal/_static/image26.png)
13. Nella pagina *CheckoutReview. aspx* verificare l'importo dell'ordine e visualizzare l'indirizzo di spedizione generato. Quindi, fare clic sul pulsante **completa ordine** . 

    ![Checkout e pagamento con la revisione dell'ordine di PayPal](checkout-and-payment-with-paypal/_static/image27.png)
14. La pagina **CheckoutComplete. aspx** viene visualizzata con un ID transazione di pagamento. 

    ![Checkout e pagamento con PayPal-Checkout completato](checkout-and-payment-with-paypal/_static/image28.png)

<a id="ReviewDBWebForms"></a>
## <a name="reviewing-the-database"></a>Revisione del database

Esaminando i dati aggiornati nel database dell'applicazione di esempio Wingtip Toys dopo l'esecuzione dell'applicazione, è possibile osservare che l'applicazione ha registrato correttamente l'acquisto dei prodotti.

È possibile esaminare i dati contenuti nel file di database *wingtiptoys. MDF* usando la finestra di **Esplora database** (**Esplora server** finestra in Visual Studio), come è stato fatto in precedenza in questa serie di esercitazioni.

1. Chiudere la finestra del browser se è ancora aperta.
2. In Visual Studio selezionare l'icona **Mostra tutti i file** nella parte superiore di **Esplora soluzioni** per consentire di espandere la cartella **app\_data** .
3. Espandere la cartella **App\_data** .  
 Potrebbe essere necessario selezionare l'icona **Mostra tutti i file** per la cartella.
4. Fare clic con il pulsante destro del mouse sul file di database *wingtiptoys. MDF* e scegliere **Apri**.  
    Viene visualizzato **Esplora server** .
5. Espandere la cartella **Tabelle** .
6. Fare clic con il pulsante destro del mouse sulla tabella **Orders**e scegliere **Mostra dati tabella**.  
 Viene visualizzata la tabella **Orders** .
7. Esaminare la colonna **PaymentTransactionID** per confermare le transazioni riuscite. 

    ![Checkout e pagamento con PayPal-esaminare il database](checkout-and-payment-with-paypal/_static/image29.png)
8. Chiudere la finestra della tabella **Orders** .
9. Nella Esplora server fare clic con il pulsante destro del mouse sulla tabella **OrderDetails** e scegliere **Mostra dati tabella**.
10. Esaminare i valori `OrderId` e `Username` nella tabella **OrderDetails** . Si noti che questi valori corrispondono ai valori `OrderId` e `Username` inclusi nella tabella **Orders** .
11. Chiudere la finestra della tabella **OrderDetails** .
12. Fare clic con il pulsante destro del mouse sul file di database Wingtip Toys (*wingtiptoys. MDF*) e scegliere **Chiudi connessione**.
13. Se la finestra **Esplora soluzioni** non viene visualizzata, fare clic su **Esplora soluzioni** nella parte inferiore della finestra **Esplora server** per visualizzare nuovamente il **Esplora soluzioni** .

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati aggiunti gli schemi Order e Order Details per tenere traccia dell'acquisto di prodotti. È stata anche integrata la funzionalità PayPal nell'applicazione di esempio Wingtip Toys.

## <a name="additional-resources"></a>Risorse aggiuntive

[Panoramica della configurazione di ASP.NET](https://msdn.microsoft.com/library/ms178683(v=vs.100).aspx)  
[Distribuire un'app Web Form ASP.NET sicura con appartenenza, OAuth e database SQL al servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

## <a name="disclaimer"></a>Dichiarazione di non responsabilità

Questa esercitazione contiene codice di esempio. Questo codice di esempio viene fornito "così com'è" senza garanzia di alcun tipo. Di conseguenza, Microsoft non garantisce l'accuratezza, l'integrità o la qualità del codice di esempio. Si accetta di usare il codice di esempio a proprio rischio. In nessuna circostanza Microsoft sarà responsabile in alcun modo per qualsiasi codice di esempio, contenuto, inclusi, a titolo esemplificativo, eventuali errori o omissioni nel codice di esempio, contenuto o eventuali perdite o danni di qualsiasi tipo in seguito all'utilizzo di un codice di esempio. Il licenziatario riceve una notifica e accetta di indennizzare, salvare e mantenere Microsoft innocuo da e verso eventuali perdite, reclami di perdita, infortuni o danni di qualsiasi tipo, inclusi, a titolo esemplificativo, quelli originati o derivanti dal materiale pubblicato, trasmettere, utilizzare o basarsi sull'inclusione, ma non limitata, delle visualizzazioni espresse in esso.

> [!div class="step-by-step"]
> [Precedente](shopping-cart.md)
> [Successivo](membership-and-administration.md)
