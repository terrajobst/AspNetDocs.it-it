---
uid: web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
title: Archiviazione di informazioni utente aggiuntive (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si risponderà a questa domanda compilando un'applicazione guestbook molto rudimentale. In questo modo, si esamineranno diverse opzioni per modeli...
ms.author: riande
ms.date: 01/18/2008
ms.assetid: ee4b924e-8002-4dc3-819f-695fca1ff867
msc.legacyurl: /web-forms/overview/older-versions-security/membership/storing-additional-user-information-vb
msc.type: authoredcontent
ms.openlocfilehash: cb352de6f7c2d117b41532112a87956c8dde62f8
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74638749"
---
# <a name="storing-additional-user-information-vb"></a>Archiviazione di informazioni utente aggiuntive (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/ASPNET_Security_Tutorial_08_VB.zip) o [Scarica PDF](https://download.microsoft.com/download/3/f/5/3f5a8605-c526-4b34-b3fd-a34167117633/aspnet_tutorial08_ExtraUserInfo_vb.pdf)

> In questa esercitazione si risponderà a questa domanda compilando un'applicazione guestbook molto rudimentale. In questo modo, si esamineranno diverse opzioni per la modellazione delle informazioni utente in un database e quindi si vedrà come associare questi dati agli account utente creati dal framework delle appartenenze.

## <a name="introduction"></a>Introduzione

ASP. Il Framework di appartenenza di NET offre un'interfaccia flessibile per la gestione degli utenti. L'API di appartenenza include metodi per la convalida delle credenziali, il recupero delle informazioni sull'utente attualmente connesso, la creazione di un nuovo account utente e l'eliminazione di un account utente, tra gli altri. Ogni account utente nel Framework di appartenenza contiene solo le proprietà necessarie per la convalida delle credenziali e l'esecuzione di attività essenziali relative agli account utente. Questa operazione viene evidenziata dai metodi e dalle proprietà della [classe`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx), che modella un account utente nel framework delle appartenenze. Questa classe dispone di proprietà quali [`UserName`](https://msdn.microsoft.com/library/system.web.security.membershipuser.username.aspx), [`Email`](https://msdn.microsoft.com/library/system.web.security.membershipuser.email.aspx)e [`IsLockedOut`](https://msdn.microsoft.com/library/system.web.security.membershipuser.islockedout.aspx)e metodi come [`GetPassword`](https://msdn.microsoft.com/library/system.web.security.membershipuser.getpassword.aspx) e [`UnlockUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.unlockuser.aspx).

Spesso, le applicazioni devono archiviare informazioni aggiuntive sull'utente non incluse nel framework delle appartenenze. Ad esempio, un rivenditore online potrebbe dover consentire a ogni utente di archiviare gli indirizzi di spedizione e fatturazione, le informazioni di pagamento, le preferenze di recapito e il numero di telefono di contatto. Ogni ordine del sistema è inoltre associato a un determinato account utente.

La classe `MembershipUser` non include proprietà come `PhoneNumber` o `DeliveryPreferences` o `PastOrders`. Quindi, in che modo è possibile tenere traccia delle informazioni utente richieste dall'applicazione e integrarle con il framework delle appartenenze? In questa esercitazione si risponderà a questa domanda compilando un'applicazione guestbook molto rudimentale. In questo modo, si esamineranno diverse opzioni per la modellazione delle informazioni utente in un database e quindi si vedrà come associare questi dati agli account utente creati dal framework delle appartenenze. Iniziamo!

## <a name="step-1-creating-the-guestbook-applications-data-model"></a>Passaggio 1: creazione del modello di dati dell'applicazione Guestbook

Sono disponibili diverse tecniche che è possibile utilizzare per acquisire le informazioni sugli utenti in un database e associarle agli account utente creati dal framework di appartenenza. Per illustrare queste tecniche, è necessario potenziare l'applicazione Web tutorial in modo che acquisisca alcuni dati correlati all'utente. Attualmente, il modello di dati dell'applicazione contiene solo le tabelle dei servizi dell'applicazione necessarie per la `SqlMembershipProvider`.

Viene ora creata un'applicazione guestbook molto semplice in cui un utente autenticato può lasciare un commento. Oltre a archiviare i commenti del Guestbook, consentire a ogni utente di archiviare la propria città, la Home page e la firma. Se specificato, la città, la Home page e la firma dell'utente verranno visualizzate in ogni messaggio che ha lasciato nel guestbook.

### <a name="adding-theguestbookcommentstable"></a>Aggiunta della tabella`GuestbookComments`

Per acquisire i commenti del Guestbook, è necessario creare una tabella di database denominata `GuestbookComments` con colonne quali `CommentId`, `Subject`, `Body`e `CommentDate`. È anche necessario che ogni record nella tabella `GuestbookComments` faccia riferimento all'utente che ha lasciato il commento.

Per aggiungere la tabella al database, passare al Esplora database in Visual Studio ed eseguire il drill-down nel database `SecurityTutorials`. Fare clic con il pulsante destro del mouse sulla cartella tabelle e scegliere Aggiungi nuova tabella. Viene visualizzata un'interfaccia che consente di definire le colonne per la nuova tabella.

[![aggiungere una nuova tabella al database SecurityTutorials](storing-additional-user-information-vb/_static/image2.png)](storing-additional-user-information-vb/_static/image1.png)

**Figura 1**: aggiungere una nuova tabella al Database di `SecurityTutorials` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image3.png))

Definire quindi le colonne del `GuestbookComments`. Per iniziare, aggiungere una colonna denominata `CommentId` di tipo `uniqueidentifier`. Questa colonna identificherà in modo univoco ogni commento nel guestbook, quindi non consentire `NULL` s e contrassegnarlo come chiave primaria della tabella. Anziché fornire un valore per il campo `CommentId` in ogni `INSERT`, è possibile indicare che per questo campo è necessario generare automaticamente un nuovo valore `uniqueidentifier`, impostando il valore predefinito della colonna `INSERT` su `NEWID()`. Dopo aver aggiunto il primo campo, contrassegnato come chiave primaria e impostato il valore predefinito, la schermata dovrebbe essere simile allo screenshot illustrato nella figura 2.

[![aggiungere una colonna primaria denominata CommentId](storing-additional-user-information-vb/_static/image5.png)](storing-additional-user-information-vb/_static/image4.png)

**Figura 2**: aggiungere una colonna primaria denominata `CommentId` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image6.png))

Successivamente, aggiungere una colonna denominata `Subject` di tipo `nvarchar(50)` e una colonna denominata `Body` di tipo `nvarchar(MAX)`, disabilitando `NULL` s in entrambe le colonne. Successivamente, aggiungere una colonna denominata `CommentDate` di tipo `datetime`. Non consentire `NULL` s e impostare il valore predefinito della colonna `CommentDate` su `getdate()`.

Rimane solo l'aggiunta di una colonna che associa un account utente a ogni commento del Guestbook. Un'opzione consiste nell'aggiungere una colonna denominata `UserName` di tipo `nvarchar(256)`. Si tratta di una scelta appropriata quando si usa un provider di appartenenze diverso dal `SqlMembershipProvider`. Tuttavia, quando si usa la `SqlMembershipProvider`, come in questa serie di esercitazioni, la colonna `UserName` della tabella `aspnet_Users` non è necessariamente univoca. La chiave primaria della tabella `aspnet_Users` è `UserId` ed è di tipo `uniqueidentifier`. Per questo motivo, la tabella `GuestbookComments` necessita di una colonna denominata `UserId` di tipo `uniqueidentifier` (che non consente valori `NULL`). Procedere e aggiungere questa colonna.

> [!NOTE]
> Come illustrato nell'esercitazione [*creazione dello schema di appartenenza in SQL Server*](creating-the-membership-schema-in-sql-server-vb.md) , il Framework di appartenenza è progettato per consentire a più applicazioni Web con account utente diversi di condividere lo stesso archivio utente. Questa operazione viene eseguita tramite il partizionamento degli account utente in applicazioni diverse. Mentre ogni nome utente è sicuramente univoco all'interno di un'applicazione, lo stesso nome utente può essere usato in applicazioni diverse con lo stesso archivio utente. È presente un vincolo di `UNIQUE` composito nella tabella `aspnet_Users` dei campi `UserName` e `ApplicationId`, ma non uno solo nel campo `UserName`. Di conseguenza, è possibile che la tabella ASPNET\_Users disponga di due o più record con lo stesso valore `UserName`. Esiste, tuttavia, un vincolo `UNIQUE` nel campo `UserId` della tabella `aspnet_Users` (poiché si tratta della chiave primaria). Un vincolo di `UNIQUE` è importante perché senza di esso non è possibile stabilire un vincolo di chiave esterna tra le tabelle `GuestbookComments` e `aspnet_Users`.

Dopo aver aggiunto la colonna `UserId`, salvare la tabella facendo clic sull'icona Salva sulla barra degli strumenti. Assegnare un nome alla nuova tabella `GuestbookComments`.

È stato riscontrato un ultimo problema da partecipare alla tabella `GuestbookComments`: è necessario creare un [vincolo FOREIGN KEY](https://msdn.microsoft.com/library/ms175464.aspx) tra la colonna `GuestbookComments.UserId` e la colonna `aspnet_Users.UserId`. A tale scopo, fare clic sull'icona relazione sulla barra degli strumenti per aprire la finestra di dialogo relazioni di chiave esterna. In alternativa, è possibile avviare questa finestra di dialogo passando al menu Progettazione tabelle e scegliendo relazioni.

Fare clic sul pulsante Aggiungi nell'angolo inferiore sinistro della finestra di dialogo relazioni di chiave esterna. Verrà aggiunto un nuovo vincolo FOREIGN KEY, sebbene sia ancora necessario definire le tabelle che fanno parte della relazione.

[![utilizzare la finestra di dialogo Relazioni chiavi esterne per gestire i vincoli di chiave esterna di una tabella](storing-additional-user-information-vb/_static/image8.png)](storing-additional-user-information-vb/_static/image7.png)

**Figura 3**: utilizzare la finestra di dialogo relazioni di chiave esterna per gestire i vincoli di chiave esterna di una tabella ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image9.png))

Fare quindi clic sull'icona con i puntini di sospensione nella riga "specifiche tabelle e colonne" a destra. Verrà avviata la finestra di dialogo tabelle e colonne dalla quale è possibile specificare la tabella e la colonna della chiave primaria e la colonna chiave esterna della tabella `GuestbookComments`. In particolare, selezionare `aspnet_Users` e `UserId` come colonna e tabella chiave primaria e `UserId` dalla tabella `GuestbookComments` come colonna chiave esterna (vedere la figura 4). Dopo aver definito le colonne e le tabelle di chiave primaria ed esterna, fare clic su OK per tornare alla finestra di dialogo relazioni di chiave esterna.

[![stabilire un vincolo di chiave esterna tra le tabelle aspnet_Users e GuesbookComments](storing-additional-user-information-vb/_static/image11.png)](storing-additional-user-information-vb/_static/image10.png)

**Figura 4**: stabilire un vincolo di chiave esterna tra le tabelle `aspnet_Users` e `GuesbookComments` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image12.png))

A questo punto è stato stabilito il vincolo FOREIGN KEY. La presenza di questo vincolo garantisce l' [integrità relazionale](http://en.wikipedia.org/wiki/Referential_integrity) tra le due tabelle garantendo che non vi sia mai una voce del guestbook che fa riferimento a un account utente non esistente. Per impostazione predefinita, un vincolo di chiave esterna non consente l'eliminazione di un record padre se sono presenti record figlio corrispondenti. Ovvero, se un utente crea uno o più commenti del Guestbook e si tenta di eliminare l'account utente, l'eliminazione avrà esito negativo a meno che i commenti del Guestbook non vengano eliminati per primi.

È possibile configurare vincoli di chiave esterna per eliminare automaticamente i record figlio associati quando viene eliminato un record padre. In altre parole, è possibile configurare questo vincolo FOREIGN KEY in modo che le voci del Guestbook di un utente vengano eliminate automaticamente quando viene eliminato l'account utente. A tale scopo, espandere la sezione "specifica di inserimento e aggiornamento" e impostare la proprietà "Elimina regola" su Cascade.

[![configurare il vincolo FOREIGN KEY per le eliminazioni a catena](storing-additional-user-information-vb/_static/image14.png)](storing-additional-user-information-vb/_static/image13.png)

**Figura 5**: configurare il vincolo FOREIGN KEY per le eliminazioni a catena ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image15.png))

Per salvare il vincolo FOREIGN KEY, fare clic sul pulsante Chiudi per uscire dalle relazioni di chiave esterna. Fare quindi clic sull'icona Salva sulla barra degli strumenti per salvare la tabella e la relazione.

### <a name="storing-the-users-home-town-homepage-and-signature"></a>Archiviazione della città, della Home page e della firma dell'utente

Nella tabella `GuestbookComments` viene illustrato come archiviare informazioni che condividono una relazione uno-a-molti con gli account utente. Poiché ogni account utente può avere un numero arbitrario di commenti associati, questa relazione viene modellata creando una tabella per contenere il set di commenti che include una colonna che collega ogni commento a un determinato utente. Quando si usa la `SqlMembershipProvider`, questo collegamento viene stabilito in modo ottimale creando una colonna denominata `UserId` di tipo `uniqueidentifier` e un vincolo di chiave esterna tra la colonna e `aspnet_Users.UserId`.

È ora necessario associare tre colonne a ogni account utente per archiviare la città, la Home page e la firma dell'utente, che verranno visualizzate nei commenti del Guestbook. Per eseguire questa operazione è possibile procedere in diversi modi:

- <strong>Aggiungere nuove colonne alle</strong> tabelle<strong>`aspnet_Users`</strong> <strong>o</strong> <strong>`aspnet_Membership`</strong> <strong>.</strong> Questo approccio non è consigliato perché modifica lo schema utilizzato dal `SqlMembershipProvider`. Questa decisione potrebbe tornare a ricorrere a una strada. Ad esempio, cosa accade se una versione futura di ASP.NET usa uno schema di `SqlMembershipProvider` diverso. Microsoft può includere uno strumento per eseguire la migrazione dei dati di `SqlMembershipProvider` ASP.NET 2,0 al nuovo schema, ma se è stato modificato lo schema `SqlMembershipProvider` ASP.NET 2,0, tale conversione potrebbe non essere possibile.

- **Utilizzare ASP. Framework del profilo di NET, che definisce una proprietà del profilo per la città, la Home page e la firma.** ASP.NET include un Framework del profilo progettato per archiviare dati aggiuntivi specifici dell'utente. Come il Framework di appartenenza, il Framework del profilo è compilato in cima al modello del provider. Il .NET Framework viene fornito con un `SqlProfileProvider` che archivia i dati di profilo in un database SQL Server. Infatti, il database dispone già della tabella usata dal `SqlProfileProvider` (`aspnet_Profile`), perché è stata aggiunta quando i servizi dell'applicazione sono stati aggiunti di nuovo nell'esercitazione [*creazione dello schema di appartenenza in SQL Server*](creating-the-membership-schema-in-sql-server-vb.md) .   
  Il vantaggio principale del Framework del profilo è che consente agli sviluppatori di definire le proprietà del profilo in `Web.config`: non è necessario scrivere codice per serializzare i dati del profilo da e verso l'archivio dati sottostante. In breve, è estremamente facile definire un set di proprietà del profilo e utilizzarle nel codice. Tuttavia, il sistema del profilo lascia molto da desiderare per quanto riguarda il controllo delle versioni, pertanto se si dispone di un'applicazione in cui si prevede di aggiungere nuove proprietà specifiche dell'utente in un secondo momento oppure di quelle esistenti da rimuovere o modificare, il Framework del profilo potrebbe non essere il  opzione migliore. Inoltre, il `SqlProfileProvider` archivia le proprietà del profilo in modo estremamente denormalizzato, rendendo impossibile l'esecuzione delle query direttamente sui dati del profilo, ad esempio il numero di utenti che hanno una città di New York.   
  Per ulteriori informazioni sul Framework del profilo, consultare la sezione "ulteriori letture" alla fine di questa esercitazione.

- <strong>Aggiungere queste tre colonne a una nuova tabella nel database e stabilire una relazione uno-a-uno tra la tabella e</strong> <strong>`aspnet_Users`</strong> <strong>.</strong> Questo approccio richiede un po' più di lavoro rispetto al Framework del profilo, ma offre la massima flessibilità nel modo in cui le proprietà utente aggiuntive vengono modellate nel database. Questa è l'opzione che verrà usata in questa esercitazione.

Viene creata una nuova tabella denominata `UserProfiles` per salvare la città, la Home page e la firma per ogni utente. Fare clic con il pulsante destro del mouse sulla cartella tabelle nella finestra Esplora database e scegliere di creare una nuova tabella. Denominare la prima colonna `UserId` e impostarne il tipo su `uniqueidentifier`. Non consentire i valori `NULL` e contrassegnare la colonna come chiave primaria. Successivamente, aggiungere le colonne denominate: `HomeTown` di tipo `nvarchar(50)`; `HomepageUrl` di tipo `nvarchar(100)`; e firma di tipo `nvarchar(500)`. Ognuna di queste tre colonne può accettare un valore `NULL`.

[![creare la tabella UserProfiles](storing-additional-user-information-vb/_static/image17.png)](storing-additional-user-information-vb/_static/image16.png)

**Figura 6**: creare la tabella `UserProfiles` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image18.png))

Salvare la tabella e denominarla `UserProfiles`. Infine, stabilire un vincolo FOREIGN KEY tra il campo `UserId` della tabella `UserProfiles` e il campo `aspnet_Users.UserId`. Come è stato fatto con il vincolo FOREIGN KEY tra le tabelle `GuestbookComments` e `aspnet_Users`, è necessario eliminare questo vincolo a cascata. Poiché il campo `UserId` in `UserProfiles` è la chiave primaria, in questo modo si garantisce che non siano presenti più record nella tabella `UserProfiles` per ogni account utente. Questo tipo di relazione viene definito uno-a-uno.

Ora che è stato creato il modello di dati, è possibile usarlo. Nei passaggi 2 e 3 si osserverà il modo in cui l'utente attualmente connesso può visualizzare e modificare le informazioni relative alla città, alla Home page e alla firma. Nel passaggio 4 viene creata l'interfaccia per consentire agli utenti autenticati di inviare nuovi commenti al guestbook e visualizzare quelli esistenti.

## <a name="step-2-displaying-the-users-home-town-homepage-and-signature"></a>Passaggio 2: visualizzazione della città, della Home page e della firma dell'utente

Esistono diversi modi per consentire all'utente che ha eseguito l'accesso di visualizzare e modificare le informazioni relative alla città, alla Home page e alla firma. È possibile creare manualmente l'interfaccia utente con i controlli TextBox e Label oppure è possibile usare uno dei controlli Web dei dati, ad esempio il controllo DetailsView. Per eseguire il `SELECT` di database e le istruzioni `UPDATE` è possibile scrivere codice ADO.NET nella classe code-behind della pagina o, in alternativa, utilizzare un approccio dichiarativo con il SqlDataSource. Idealmente, l'applicazione conterrà un'architettura a più livelli, che è possibile richiamare a livello di codice dalla classe code-behind della pagina o in modo dichiarativo tramite il controllo ObjectDataSource.

Poiché questa serie di esercitazioni è incentrata sull'autenticazione basata su form, sull'autorizzazione, sugli account utente e sui ruoli, non sarà disponibile una descrizione approfondita di queste diverse opzioni di accesso ai dati o perché un'architettura a più livelli è preferibile rispetto all'esecuzione diretta di istruzioni SQL dalla pagina ASP.NET. Illustrerò l'utilizzo di DetailsView e SqlDataSource, l'opzione più rapida e più semplice, ma i concetti discussi possono essere certamente applicati a controlli Web alternativi e alla logica di accesso ai dati. Per altre informazioni sull'uso dei dati in ASP.NET, vedere la serie di esercitazioni sull' *[uso dei dati in ASP.NET 2,0](../../data-access/index.md)* .

Aprire la pagina `AdditionalUserInfo.aspx` nella cartella `Membership` e aggiungere un controllo DetailsView alla pagina, impostando la relativa proprietà ID su `UserProfile` e deselezionando le proprietà `Width` e `Height`. Espandere lo smart tag di DetailsView e scegliere di associarlo a un nuovo controllo origine dati. Verrà avviata la configurazione guidata origine dati (vedere la figura 7). Il primo passaggio richiede di specificare il tipo di origine dati. Poiché ci si connette direttamente al database di `SecurityTutorials`, scegliere l'icona del database, specificando il `ID` come `UserProfileDataSource`.

[![aggiungere un nuovo controllo SqlDataSource denominato UserProfileDataSource](storing-additional-user-information-vb/_static/image20.png)](storing-additional-user-information-vb/_static/image19.png)

**Figura 7**: aggiungere un nuovo controllo SqlDataSource denominato `UserProfileDataSource` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image21.png))

La schermata successiva richiede il database da usare. È già stata definita una stringa di connessione in `Web.config` per il database `SecurityTutorials`. Il nome della stringa di connessione, `SecurityTutorialsConnectionString`, deve essere presente nell'elenco a discesa. Selezionare questa opzione e fare clic su Avanti.

[![scegliere SecurityTutorialsConnectionString dall'elenco a discesa](storing-additional-user-information-vb/_static/image23.png)](storing-additional-user-information-vb/_static/image22.png)

**Figura 8**: scegliere `SecurityTutorialsConnectionString` dall'elenco a discesa ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image24.png))

Nella schermata successiva viene richiesto di specificare la tabella e le colonne per cui eseguire la query. Scegliere la tabella `UserProfiles` dall'elenco a discesa e selezionare tutte le colonne.

[![ripristinare tutte le colonne dalla tabella UserProfiles](storing-additional-user-information-vb/_static/image26.png)](storing-additional-user-information-vb/_static/image25.png)

**Figura 9**: ripristinare tutte le colonne dalla tabella `UserProfiles` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image27.png))

La query corrente nella figura 9 restituisce *tutti* i record in `UserProfiles`, ma è interessato solo al record dell'utente attualmente connesso. Per aggiungere una clausola `WHERE`, fare clic sul pulsante `WHERE` per visualizzare la finestra di dialogo Aggiungi clausola `WHERE` (vedere la figura 10). Qui è possibile selezionare la colonna in base a cui filtrare, l'operatore e l'origine del parametro Filter. Selezionare `UserId` come colonna e "=" come operatore.

Sfortunatamente non è disponibile un'origine di parametro predefinita per restituire il valore `UserId` dell'utente attualmente connesso. È necessario acquisire questo valore a livello di codice. Quindi, impostare l'elenco a discesa origine su "nessuno", fare clic sul pulsante Aggiungi per aggiungere il parametro e quindi fare clic su OK.

[![aggiungere un parametro di filtro nella colonna UserId](storing-additional-user-information-vb/_static/image29.png)](storing-additional-user-information-vb/_static/image28.png)

**Figura 10**: aggiungere un parametro di filtro nella colonna `UserId` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image30.png))

Dopo aver fatto clic su OK, verrà visualizzata la schermata illustrata nella figura 9. Questa volta, tuttavia, la query SQL nella parte inferiore della schermata deve includere una clausola `WHERE`. Fare clic su Avanti per passare alla schermata "query di test". Qui è possibile eseguire la query e visualizzare i risultati. Fare clic su Fine per completare la procedura guidata.

Al termine della configurazione guidata origine dati, Visual Studio crea il controllo SqlDataSource in base alle impostazioni specificate nella procedura guidata. Inoltre, aggiunge manualmente i BoundField al DetailsView per ogni colonna restituita dal `SelectCommand`di SqlDataSource. Non è necessario visualizzare il campo `UserId` in DetailsView, perché non è necessario che l'utente conosca questo valore. È possibile rimuovere questo campo direttamente dal markup dichiarativo del controllo DetailsView o facendo clic sul collegamento "modifica campi" dal relativo smart tag.

A questo punto il markup dichiarativo della pagina avrà un aspetto simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample1.aspx)]

È necessario impostare a livello di codice il parametro `UserId` del controllo SqlDataSource sul `UserId` dell'utente attualmente connesso prima che i dati siano selezionati. Questa operazione può essere eseguita creando un gestore eventi per l'evento `Selecting` di SqlDataSource e aggiungendo il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample2.vb)]

Il codice precedente inizia ottenendo un riferimento all'utente attualmente connesso chiamando il metodo `GetUser` della classe `Membership`. Viene restituito un oggetto `MembershipUser`, la cui proprietà `ProviderUserKey` contiene il `UserId`. Il valore `UserId` viene quindi assegnato al parametro `@UserId` di SqlDataSource.

> [!NOTE]
> Il metodo `Membership.GetUser()` restituisce informazioni sull'utente attualmente connesso. Se un utente anonimo sta visitando la pagina, restituirà il valore `Nothing`. In tal caso, si verificherà un `NullReferenceException` sulla riga di codice seguente quando si tenta di leggere la proprietà `ProviderUserKey`. Naturalmente, non è necessario preoccuparsi `Membership.GetUser()` non venga restituito alcun elemento nella pagina `AdditionalUserInfo.aspx` perché è stata configurata l'autorizzazione URL in un'esercitazione precedente, in modo che solo gli utenti autenticati possano accedere alle risorse ASP.NET in questa cartella. Se è necessario accedere alle informazioni sull'utente attualmente connesso in una pagina in cui è consentito l'accesso anonimo, assicurarsi di controllare che l'oggetto `MembershipUser` restituito dal metodo `GetUser()` non sia Nothing prima di fare riferimento alle relative proprietà.

Se si visita la pagina `AdditionalUserInfo.aspx` tramite un browser, viene visualizzata una pagina vuota perché è ancora necessario aggiungere righe alla tabella `UserProfiles`. Nel passaggio 6 verrà illustrato come personalizzare il controllo CreateUserWizard per aggiungere automaticamente una nuova riga alla tabella `UserProfiles` quando viene creato un nuovo account utente. Per il momento, tuttavia, è necessario creare manualmente un record nella tabella.

Passare alla Esplora database in Visual Studio ed espandere la cartella tabelle. Fare clic con il pulsante destro del mouse sulla tabella `aspnet_Users` e scegliere "Mostra dati tabella" per visualizzare i record nella tabella. eseguire la stessa operazione per la tabella `UserProfiles`. La figura 11 Mostra questi risultati quando viene affiancato verticalmente. Nel database sono attualmente presenti `aspnet_Users` record per Bruce, Fred e Tito, ma nessun record nella tabella `UserProfiles`.

[![viene visualizzato il contenuto delle tabelle aspnet_Users e UserProfiles](storing-additional-user-information-vb/_static/image32.png)](storing-additional-user-information-vb/_static/image31.png)

**Figura 11**: viene visualizzato il contenuto delle tabelle `aspnet_Users` e `UserProfiles` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image33.png))

Aggiungere un nuovo record alla tabella `UserProfiles` digitando manualmente i valori per i campi `HomeTown`, `HomepageUrl`e `Signature`. Il modo più semplice per ottenere un valore `UserId` valido nel nuovo record `UserProfiles` consiste nel selezionare il campo `UserId` da un determinato account utente nella tabella `aspnet_Users` e copiarlo e incollarlo nel campo `UserId` in `UserProfiles`. La figura 12 Mostra la tabella `UserProfiles` dopo l'aggiunta di un nuovo record per Bruce.

[![è stato aggiunto un record a UserProfiles per Bruce](storing-additional-user-information-vb/_static/image35.png)](storing-additional-user-information-vb/_static/image34.png)

**Figura 12**: è stato aggiunto un Record a `UserProfiles` per Bruce ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image36.png))

Tornare alla `AdditionalUserInfo.aspx page`, connesso come Bruce. Come illustrato nella figura 13, vengono visualizzate le impostazioni di Bruce.

[![l'utente attualmente visitato viene visualizzato le impostazioni](storing-additional-user-information-vb/_static/image38.png)](storing-additional-user-information-vb/_static/image37.png)

**Figura 13**: l'utente attualmente visitato viene visualizzato le impostazioni ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image39.png))

> [!NOTE]
> Procedere e aggiungere manualmente i record nella tabella `UserProfiles` per ogni utente di appartenenza. Nel passaggio 6 verrà illustrato come personalizzare il controllo CreateUserWizard per aggiungere automaticamente una nuova riga alla tabella `UserProfiles` quando viene creato un nuovo account utente.

## <a name="step-3-allowing-the-user-to-edit-his-home-town-homepage-and-signature"></a>Passaggio 3: consentire all'utente di modificare la propria città, la Home page e la firma

A questo punto, l'utente attualmente connesso può visualizzare la propria città, la Home page e l'impostazione della firma, ma non possono ancora modificarle. Verrà ora aggiornato il controllo DetailsView in modo che i dati possano essere modificati.

Per prima cosa è necessario aggiungere un `UpdateCommand` per il SqlDataSource, specificando l'istruzione `UPDATE` da eseguire e i parametri corrispondenti. Selezionare il SqlDataSource e, dal Finestra Proprietà, fare clic sui puntini di sospensione accanto alla proprietà UpdateQuery per visualizzare la finestra di dialogo Editor comandi e parametri. Immettere la seguente istruzione `UPDATE` nella casella di testo:

[!code-sql[Main](storing-additional-user-information-vb/samples/sample3.sql)]

Fare quindi clic sul pulsante "Aggiorna parametri" per creare un parametro nella raccolta `UpdateParameters` del controllo SqlDataSource per ognuno dei parametri nell'istruzione `UPDATE`. Lasciare l'origine di tutti i parametri impostati su None e fare clic sul pulsante OK per completare la finestra di dialogo.

[![specificare l'oggetto UpdateCommand e UpdateParameters di SqlDataSource](storing-additional-user-information-vb/_static/image41.png)](storing-additional-user-information-vb/_static/image40.png)

**Figura 14**: specificare il `UpdateCommand` e il `UpdateParameters` del SqlDataSource ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image42.png))

A causa delle aggiunte apportate al controllo SqlDataSource, il controllo DetailsView può ora supportare la modifica. Dallo smart tag di DetailsView, selezionare la casella di controllo "Abilita modifica". Viene aggiunto un oggetto CommandField alla raccolta `Fields` del controllo con la relativa proprietà `ShowEditButton` impostata su true. Viene eseguito il rendering di un pulsante modifica quando DetailsView viene visualizzato in modalità di sola lettura e pulsanti Aggiorna e Annulla quando viene visualizzato in modalità di modifica. Anziché richiedere all'utente di fare clic su modifica, tuttavia, è possibile eseguire il rendering di DetailsView in uno stato "sempre modificabile" impostando la [proprietà`DefaultMode`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.detailsview.defaultmode.aspx) del controllo detailsview su `Edit`.

Con queste modifiche, il markup dichiarativo del controllo DetailsView dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample4.aspx)]

Si noti l'aggiunta di CommandField e della proprietà `DefaultMode`.

Procedere e testare questa pagina tramite un browser. Quando si visita un utente con un record corrispondente in `UserProfiles`, le impostazioni dell'utente vengono visualizzate in un'interfaccia modificabile.

[![DetailsView esegue il rendering di un'interfaccia modificabile](storing-additional-user-information-vb/_static/image44.png)](storing-additional-user-information-vb/_static/image43.png)

**Figura 15**: il DetailsView esegue il rendering di un'interfaccia modificabile ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image45.png))

Provare a modificare i valori e fare clic sul pulsante Aggiorna. Sembra che non accada nulla. È presente un postback e i valori vengono salvati nel database, ma non è presente alcun feedback visivo che si è verificato durante il salvataggio.

Per risolvere questo problema, tornare a Visual Studio e aggiungere un controllo etichetta sopra il DetailsView. Impostare la relativa `ID` su `SettingsUpdatedMessage`, la relativa proprietà `Text` su "le impostazioni sono state aggiornate" e le proprietà `Visible` e `EnableViewState` per `False`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample5.aspx)]

È necessario visualizzare l'etichetta di `SettingsUpdatedMessage` ogni volta che DetailsView viene aggiornato. A tale scopo, creare un gestore eventi per l'evento `ItemUpdated` di DetailsView e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample6.vb)]

Tornare alla pagina `AdditionalUserInfo.aspx` tramite un browser e aggiornare i dati. Questa volta, viene visualizzato un messaggio di stato utile.

[![viene visualizzato un breve messaggio quando le impostazioni vengono aggiornate](storing-additional-user-information-vb/_static/image47.png)](storing-additional-user-information-vb/_static/image46.png)

**Figura 16**: viene visualizzato un breve messaggio quando le impostazioni vengono aggiornate ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image48.png))

> [!NOTE]
> L'interfaccia di modifica del controllo DetailsView lascia molto da desiderare. USA caselle di testo di dimensioni standard, ma il campo della firma deve essere probabilmente una casella di testo a più righe. È consigliabile usare un RegularExpressionValidator per assicurarsi che l'URL della Home page, se immesso, inizi con "http://" o "https://". Inoltre, poiché il controllo DetailsView ha la proprietà `DefaultMode` impostata su `Edit`, il pulsante Annulla non esegue alcuna operazione. Deve essere rimosso o, se selezionato, reindirizzare l'utente a un'altra pagina, ad esempio `~/Default.aspx`. Lascio questi miglioramenti come esercizio per il lettore.

### <a name="adding-a-link-to-theadditionaluserinfoaspxpage-in-the-master-page"></a>Aggiunta di un collegamento alla pagina`AdditionalUserInfo.aspx`nella pagina master

Attualmente, il sito Web non fornisce alcun collegamento alla pagina `AdditionalUserInfo.aspx`. L'unico modo per raggiungerlo consiste nell'immettere l'URL della pagina direttamente nella barra degli indirizzi del browser. Verrà ora aggiunto un collegamento a questa pagina nella pagina master `Site.master`.

Si ricordi che la pagina master contiene un controllo Web LoginView nella `LoginContent` ContentPlaceHolder che visualizza un markup diverso per i visitatori autenticati e anonimi. Aggiornare la `LoggedInTemplate` del controllo LoginView per includere un collegamento alla pagina di `AdditionalUserInfo.aspx`. Dopo avere apportato queste modifiche, il markup dichiarativo del controllo LoginView dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample7.aspx)]

Si noti l'aggiunta del controllo collegamento ipertestuale `lnkUpdateSettings` al `LoggedInTemplate`. Con questo collegamento, gli utenti autenticati possono passare rapidamente alla pagina per visualizzare e modificare le impostazioni della città, della Home page e della firma.

## <a name="step-4-adding-new-guestbook-comments"></a>Passaggio 4: aggiunta di nuovi commenti del Guestbook

La pagina `Guestbook.aspx` consente agli utenti autenticati di visualizzare il Guestbook e lasciare un commento. Iniziamo con la creazione dell'interfaccia per aggiungere nuovi commenti del Guestbook.

Aprire la pagina `Guestbook.aspx` in Visual Studio e creare un'interfaccia utente costituita da due controlli TextBox, uno per l'oggetto del nuovo commento e uno per il corpo. Impostare la proprietà `ID` del primo controllo TextBox su `Subject` e la relativa proprietà `Columns` su 40; impostare la `ID` del secondo su `Body`, la relativa `TextMode` su `MultiLine`e le proprietà `Width` e `Rows` rispettivamente su "95%" e 8. Per completare l'interfaccia utente, aggiungere un controllo Web Button denominato `PostCommentButton` e impostare la relativa proprietà `Text` su "pubblicare il commento".

Poiché ogni commento del Guestbook richiede un oggetto e un corpo, aggiungere un RequiredFieldValidator per ciascuna casella di testo. Impostare la proprietà `ValidationGroup` di questi controlli su "EnterComment"; allo stesso modo, impostare la proprietà `ValidationGroup` del controllo `PostCommentButton` su "EnterComment". Per ulteriori informazioni su ASP. I controlli di convalida di NET, la verifica della [convalida dei moduli in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml), [la dissezione dei controlli di convalida in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)e l' [esercitazione sui controlli server di convalida](http://www.w3schools.com/aspnet/aspnet_refvalidationcontrols.asp) in [W3Schools](http://www.w3schools.com/).

Dopo aver creato l'interfaccia utente, il markup dichiarativo della pagina avrà un aspetto simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample8.aspx)]

Con l'interfaccia utente completata, l'attività successiva consiste nell'inserire un nuovo record nella tabella `GuestbookComments` quando si fa clic sull'`PostCommentButton`. Questa operazione può essere eseguita in diversi modi: è possibile scrivere codice ADO.NET nel gestore dell'evento `Click` del pulsante; è possibile aggiungere un controllo SqlDataSource alla pagina, configurare la relativa `InsertCommand`e quindi chiamare il relativo metodo `Insert` dal gestore dell'evento `Click`; in alternativa, è possibile creare un livello intermedio responsabile dell'inserimento dei nuovi commenti del Guestbook e richiamare questa funzionalità dal gestore dell'evento `Click`. Dal momento che è stato esaminato l'uso di un SqlDataSource nel passaggio 3, è ora usato il codice ADO.NET.

> [!NOTE]
> Le classi ADO.NET utilizzate per accedere a livello di codice ai dati da un database di Microsoft SQL Server si trovano nello spazio dei nomi `System.Data.SqlClient`. Potrebbe essere necessario importare questo spazio dei nomi nella classe code-behind della pagina, ad esempio `Imports System.Data.SqlClient`.

Creare un gestore eventi per l'evento `Click` del `PostCommentButton`e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample9.vb)]

Il gestore dell'evento `Click` inizia verificando che i dati specificati dall'utente siano validi. In caso contrario, il gestore eventi viene chiuso prima di inserire un record. Supponendo che i dati specificati siano validi, il valore `UserId` dell'utente attualmente connesso viene recuperato e archiviato nella variabile locale `currentUserId`. Questo valore è necessario perché è necessario fornire un valore `UserId` quando si inserisce un record in `GuestbookComments`.

In seguito, la stringa di connessione per il database `SecurityTutorials` viene recuperata da `Web.config` e viene specificata l'istruzione SQL `INSERT`. Viene quindi creato e aperto un oggetto `SqlConnection`. Viene quindi creato un oggetto `SqlCommand` e vengono assegnati i valori per i parametri utilizzati nella query `INSERT`. L'istruzione `INSERT` viene quindi eseguita e la connessione viene chiusa. Alla fine del gestore dell'evento, le `Subject` e `Body` le caselle di testo ' `Text` le proprietà vengono cancellate in modo che i valori dell'utente non siano salvati in modo permanente nel postback.

Procedere e testare questa pagina in un browser. Poiché questa pagina si trova nella cartella `Membership` non è accessibile ai visitatori anonimi. Pertanto, sarà necessario prima accedere (se non è già stato fatto). Immettere un valore nelle caselle di testo `Subject` e `Body` e fare clic sul pulsante `PostCommentButton`. In questo modo verrà aggiunto un nuovo record a `GuestbookComments`. Durante il postback, l'oggetto e il corpo specificati vengono cancellati dalle caselle di testo.

Dopo aver fatto clic sul pulsante `PostCommentButton` non sono presenti commenti visivi che il commento è stato aggiunto al guestbook. È ancora necessario aggiornare questa pagina per visualizzare i commenti del Guestbook esistente, che verrà eseguita nel passaggio 5. Al termine di questa operazione, il commento appena aggiunto verrà visualizzato nell'elenco di commenti, fornendo un feedback visivo adeguato. Per il momento, verificare che il commento del Guestbook sia stato salvato esaminando il contenuto della tabella `GuestbookComments`.

Nella figura 17 viene illustrato il contenuto della tabella `GuestbookComments` dopo che sono stati lasciati due commenti.

[![è possibile visualizzare i commenti del Guestbook nella tabella GuestbookComments](storing-additional-user-information-vb/_static/image50.png)](storing-additional-user-information-vb/_static/image49.png)

**Figura 17**: è possibile visualizzare i commenti del Guestbook nella tabella `GuestbookComments` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image51.png))

> [!NOTE]
> Se un utente tenta di inserire un commento del guestbook che contiene un markup potenzialmente pericoloso, ad esempio HTML – ASP.NET genererà un `HttpRequestValidationException`. Per altre informazioni su questa eccezione, perché viene generata e come consentire agli utenti di inviare valori potenzialmente pericolosi, vedere il [white paper sulla convalida della richiesta](../../../../whitepapers/request-validation.md).

## <a name="step-5-listing-the-existing-guestbook-comments"></a>Passaggio 5: elencare i commenti del Guestbook esistente

Oltre a lasciare commenti, un utente che visita la pagina `Guestbook.aspx` dovrebbe anche poter visualizzare i commenti esistenti del Guestbook. A tale scopo, aggiungere un controllo ListView denominato `CommentList` nella parte inferiore della pagina.

> [!NOTE]
> Il controllo ListView è una novità della versione 3,5 di ASP.NET. È progettato per visualizzare un elenco di elementi in un layout molto personalizzabile e flessibile, pur continuando a offrire funzionalità predefinite di modifica, inserimento, eliminazione, paging e ordinamento come GridView. Se si usa ASP.NET 2,0, sarà necessario usare invece il controllo DataList o Repeater. Per altre informazioni sull'uso di ListView, vedere il post del Blog di [Scott Guthrie](https://weblogs.asp.net/scottgu/), [il controllo ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)e il mio articolo, che [Visualizza i dati con il controllo ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx).

Aprire lo smart tag del ListView e, dall'elenco a discesa Scegli origine dati, associare il controllo a una nuova origine dati. Come illustrato nel passaggio 2, verrà avviata la configurazione guidata origine dati. Selezionare l'icona del database, denominare il `CommentsDataSource`SqlDataSource risultante e fare clic su OK. Selezionare quindi la stringa di connessione `SecurityTutorialsConnectionString` dall'elenco a discesa e fare clic su Avanti.

A questo punto nel passaggio 2 sono stati specificati i dati da interrogare selezionando la tabella `UserProfiles` dall'elenco a discesa e selezionando le colonne da restituire (fare riferimento alla figura 9). Questa volta, tuttavia, si vuole creare un'istruzione SQL che estrae non solo i record da `GuestbookComments`, ma anche la città principale, la firma e il nome utente del commentatore. Selezionare quindi il pulsante di opzione "specificare un'istruzione SQL personalizzata o stored procedure" e fare clic su Avanti.

Verrà visualizzata la schermata "Definisci istruzioni personalizzate o stored procedure". Per compilare graficamente la query, fare clic sul pulsante Generatore di query. Il Generatore di query inizia richiedendo di specificare le tabelle da cui si desidera eseguire una query. Selezionare le tabelle `GuestbookComments`, `UserProfiles`e `aspnet_Users`, quindi fare clic su OK. In questo modo, tutte e tre le tabelle vengono aggiunte all'area di progettazione. Poiché esistono vincoli di chiave esterna tra le tabelle `GuestbookComments`, `UserProfiles`e `aspnet_Users`, il Generatore di query automaticamente `JOIN` le tabelle.

Non resta che specificare le colonne da restituire. Dalla tabella `GuestbookComments` selezionare le colonne `Subject`, `Body`e `CommentDate`; restituire le colonne `HomeTown`, `HomepageUrl`e `Signature` della tabella `UserProfiles`; e restituiscono `UserName` da `aspnet_Users`. Aggiungere anche "`ORDER BY CommentDate DESC`" alla fine della query `SELECT` in modo che vengano restituiti per primi i post più recenti. Dopo aver effettuato queste selezioni, l'interfaccia Generatore di query dovrebbe essere simile a quella illustrata nella figura 18.

[![la query costruita unisce le tabelle GuestbookComments, UserProfile e aspnet_Users](storing-additional-user-information-vb/_static/image53.png)](storing-additional-user-information-vb/_static/image52.png)

**Figura 18**: la Query costruita `JOIN` s le tabelle `GuestbookComments`, `UserProfiles`e `aspnet_Users` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image54.png))

Fare clic su OK per chiudere la finestra di Generatore di query e tornare alla schermata "Definisci istruzioni personalizzate o stored procedure". Fare clic su Avanti per passare alla schermata "Test query", in cui è possibile visualizzare i risultati della query facendo clic sul pulsante Test query. Quando si è pronti, fare clic su fine per completare la procedura guidata Configura origine dati.

Al termine della configurazione guidata origine dati nel passaggio 2, l'insieme di `Fields` del controllo DetailsView associato è stato aggiornato in modo da includere un BoundField per ogni colonna restituita dall'`SelectCommand`. Il controllo ListView, tuttavia, rimane invariato; è comunque necessario definirne il layout. Il layout di ListView può essere costruito manualmente tramite il markup dichiarativo o dall'opzione "Configura ListView" nello smart tag. Generalmente preferisco definire il markup manualmente, ma usare qualsiasi metodo più naturale.

Ho terminato di usare i seguenti `LayoutTemplate`, `ItemTemplate`e `ItemSeparatorTemplate` per il controllo ListView:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample10.aspx)]

Il `LayoutTemplate` definisce il markup emesso dal controllo, mentre il `ItemTemplate` esegue il rendering di ogni elemento restituito dal SqlDataSource. Il markup risultante del `ItemTemplate`viene inserito nel controllo `itemPlaceholder` di `LayoutTemplate`. Oltre al `itemPlaceholder`, il `LayoutTemplate` include un controllo DataPager, che limita il ListView a visualizzare solo 10 commenti del Guestbook per pagina (impostazione predefinita) ed esegue il rendering di un'interfaccia di paging.

My `ItemTemplate` Visualizza l'oggetto del commento di ogni Guestbook in un elemento `<h4>` con il corpo situato al di sotto dell'oggetto. Si noti che la sintassi utilizzata per la visualizzazione del corpo prende i dati restituiti dall'istruzione `Eval("Body")` DataBinding, li converte in una stringa e sostituisce le interruzioni di riga con l'elemento `<br />`. Questa conversione è necessaria per mostrare le interruzioni di riga immesse durante l'invio del commento poiché lo spazio vuoto viene ignorato da HTML. La firma dell'utente viene visualizzata sotto il corpo in corsivo, seguita dalla Home page dell'utente, da un collegamento alla Home page, dalla data e dall'ora in cui è stato creato il commento e dal nome utente della persona che ha lasciato il commento.

È possibile visualizzare la pagina tramite un browser. Verranno visualizzati i commenti aggiunti al guestbook nel passaggio 5 visualizzato qui.

[![Guestbook. aspx ora Visualizza i commenti del Guestbook](storing-additional-user-information-vb/_static/image56.png)](storing-additional-user-information-vb/_static/image55.png)

**Figura 19**: `Guestbook.aspx` ora Visualizza i commenti del Guestbook ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image57.png))

Provare ad aggiungere un nuovo commento al guestbook. Quando si fa clic sul pulsante `PostCommentButton` viene eseguito il postback della pagina e il commento viene aggiunto al database, ma il controllo ListView non viene aggiornato per visualizzare il nuovo commento. Questo problema può essere risolto da uno dei seguenti valori:

- Aggiornamento del gestore dell'evento `Click` del pulsante `PostCommentButton` in modo che richiami il metodo di `DataBind()` del controllo ListView dopo l'inserimento del nuovo commento nel database.
- Impostazione della proprietà `EnableViewState` del controllo ListView su `False`. Questo approccio funziona perché disabilitando lo stato di visualizzazione del controllo, deve essere riassociato ai dati sottostanti in ogni postback.

Il sito Web dell'esercitazione scaricabile da questa esercitazione illustra entrambe le tecniche. Il `EnableViewState` proprietà del controllo ListView da `False` e il codice necessario per riassociare i dati al controllo ListView sono presenti nel gestore dell'evento `Click`, ma è impostato come commento.

> [!NOTE]
> Attualmente la pagina `AdditionalUserInfo.aspx` consente all'utente di visualizzare e modificare le impostazioni della città, della Home page e della firma. Potrebbe essere utile aggiornare `AdditionalUserInfo.aspx` per visualizzare i commenti del Guestbook dell'utente connesso. Ovvero, oltre a esaminare e modificare le informazioni, un utente può visitare la pagina `AdditionalUserInfo.aspx` per vedere quali commenti del Guestbook è stato fatto in passato. Lo lascio come esercizio per il lettore interessato.

## <a name="step-6-customizing-the-createuserwizard-control-to-include-an-interface-for-the-home-town-homepage-and-signature"></a>Passaggio 6: personalizzazione del controllo CreateUserWizard per includere un'interfaccia per la città, la Home page e la firma

La query `SELECT` utilizzata dalla pagina `Guestbook.aspx` utilizza un `INNER JOIN` per combinare i record correlati tra le tabelle `GuestbookComments`, `UserProfiles`e `aspnet_Users`. Se un utente privo di record in `UserProfiles` crea un commento del Guestbook, il commento non verrà visualizzato in ListView perché il `INNER JOIN` restituisce solo `GuestbookComments` record quando sono presenti record corrispondenti in `UserProfiles` e `aspnet_Users`. Come illustrato nel passaggio 3, se un utente non dispone di un record in `UserProfiles` non è in grado di visualizzare o modificare le impostazioni nella pagina `AdditionalUserInfo.aspx`.

Inutile dire che, a causa delle decisioni di progettazione, è importante che ogni account utente nel sistema di appartenenza disponga di un record corrispondente nella tabella `UserProfiles`. Si vuole che un record corrispondente venga aggiunto a `UserProfiles` ogni volta che viene creato un nuovo account utente di appartenenza tramite CreateUserWizard.

Come illustrato nell'esercitazione [*creazione di account utente*](creating-user-accounts-vb.md) , dopo la creazione del nuovo account utente di appartenenza, il controllo CreateUserWizard genera il relativo [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx). È possibile creare un gestore eventi per questo evento, ottenere l'ID utente per l'utente appena creato e quindi inserire un record nella tabella `UserProfiles` con i valori predefiniti per le colonne `HomeTown`, `HomepageUrl`e `Signature`. Inoltre, è possibile richiedere all'utente questi valori personalizzando l'interfaccia del controllo CreateUserWizard per includere altre caselle di testo.

Si osservi innanzitutto come aggiungere una nuova riga alla tabella `UserProfiles` nel gestore eventi `CreatedUser` con i valori predefiniti. In seguito, si vedrà come personalizzare l'interfaccia utente del controllo CreateUserWizard per includere campi modulo aggiuntivi per raccogliere la Home page, la Home page e la firma del nuovo utente.

### <a name="adding-a-default-row-touserprofiles"></a>Aggiunta di una riga predefinita a`UserProfiles`

Nell'esercitazione [*creazione di account utente*](creating-user-accounts-vb.md) è stato aggiunto un controllo CreateUserWizard alla pagina `CreatingUserAccounts.aspx` nella cartella `Membership`. Per fare in modo che il controllo CreateUserWizard aggiunga un record a `UserProfiles` tabella durante la creazione dell'account utente, è necessario aggiornare la funzionalità del controllo CreateUserWizard. Invece di apportare queste modifiche alla pagina `CreatingUserAccounts.aspx`, è necessario aggiungere un nuovo controllo CreateUserWizard a `EnhancedCreateUserWizard.aspx` pagina e apportare le modifiche per questa esercitazione.

Aprire la pagina `EnhancedCreateUserWizard.aspx` in Visual Studio e trascinare un controllo CreateUserWizard dalla casella degli strumenti nella pagina. Impostare la proprietà `ID` del controllo CreateUserWizard su `NewUserWizard`. Come illustrato nell'esercitazione [*creazione di account utente*](creating-user-accounts-vb.md) , l'interfaccia utente predefinita di CreateUserWizard richiede al visitatore le informazioni necessarie. Una volta fornite queste informazioni, il controllo crea internamente un nuovo account utente nel Framework di appartenenza, il tutto senza dover scrivere una sola riga di codice.

Il controllo CreateUserWizard genera un certo numero di eventi durante il relativo flusso di lavoro. Quando un visitatore fornisce le informazioni sulla richiesta e invia il modulo, il controllo CreateUserWizard genera inizialmente il relativo [evento`CreatingUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.creatinguser.aspx). Se si verifica un problema durante il processo di creazione, viene generato l' [evento`CreateUserError`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createusererror.aspx) ; Tuttavia, se l'utente viene creato correttamente, viene generato l' [evento`CreatedUser`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.createduser.aspx) . Nell'esercitazione [*creazione di account utente*](creating-user-accounts-vb.md) è stato creato un gestore eventi per l'evento `CreatingUser` per garantire che il nome utente fornito non contenesse spazi iniziali o finali e che il nome utente non fosse presente in nessun punto della password.

Per aggiungere una riga nella tabella `UserProfiles` per l'utente appena creato, è necessario creare un gestore eventi per l'evento `CreatedUser`. Nel momento in cui è stato generato l'evento `CreatedUser`, l'account utente è già stato creato nel Framework di appartenenza, consentendo di recuperare il valore UserId dell'account.

Creare un gestore eventi per l'evento `CreatedUser` del `NewUserWizard`e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample11.vb)]

Gli esseri di codice precedenti recuperano l'ID utente dell'account utente appena aggiunto. Questa operazione viene eseguita utilizzando il metodo `Membership.GetUser(username)` per restituire informazioni su un utente specifico e quindi utilizzando la proprietà `ProviderUserKey` per recuperare l'ID utente. Il nome utente immesso dall'utente nel controllo CreateUserWizard è disponibile tramite la relativa [proprietà`UserName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.username.aspx).

Successivamente, la stringa di connessione viene recuperata da `Web.config` e viene specificata l'istruzione `INSERT`. Viene creata un'istanza degli oggetti ADO.NET necessari e viene eseguito il comando. Il codice assegna un'istanza di [`DBNull`](https://msdn.microsoft.com/library/system.dbnull.aspx) ai parametri `@HomeTown`, `@HomepageUrl`e `@Signature`, che ha l'effetto di inserire i valori di `NULL` database per i campi `HomeTown`, `HomepageUrl`e `Signature`.

Visitare la pagina `EnhancedCreateUserWizard.aspx` tramite un browser e creare un nuovo account utente. Al termine di questa operazione, tornare a Visual Studio ed esaminare il contenuto delle tabelle `aspnet_Users` e `UserProfiles`, come è stato riportato nella figura 12. Verrà visualizzato il nuovo account utente in `aspnet_Users` e una riga `UserProfiles` corrispondente (con `NULL` valori per `HomeTown`, `HomepageUrl`e `Signature`).

[![è stato aggiunto un nuovo account utente e un record UserProfile](storing-additional-user-information-vb/_static/image59.png)](storing-additional-user-information-vb/_static/image58.png)

**Figura 20**: è stato aggiunto un nuovo account utente e `UserProfiles` record ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image60.png))

Dopo che il visitatore ha fornito le informazioni sul nuovo account e ha fatto clic sul pulsante "Crea utente", l'account utente viene creato e viene aggiunta una riga alla tabella `UserProfiles`. L'oggetto CreateUserWizard Visualizza quindi la `CompleteWizardStep`, che visualizza un messaggio di operazione riuscita e un pulsante continua. Se si fa clic sul pulsante continua, viene generato un postback, ma non viene eseguita alcuna azione, lasciando l'utente bloccato nella pagina `EnhancedCreateUserWizard.aspx`.

È possibile specificare un URL a cui inviare l'utente quando si fa clic sul pulsante continua tramite la [proprietà`ContinueDestinationPageUrl`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.continuedestinationpageurl.aspx)del controllo CreateUserWizard. Impostare la proprietà `ContinueDestinationPageUrl` su "~/Membership/AdditionalUserInfo.aspx". In questo modo il nuovo utente sarà `AdditionalUserInfo.aspx`, dove potranno visualizzare e aggiornare le impostazioni.

### <a name="customizing-the-createuserwizards-interface-to-prompt-for-the-new-users-home-town-homepage-and-signature"></a>Personalizzazione dell'interfaccia di CreateUserWizard per la richiesta della città, della Home page e della firma del nuovo utente

L'interfaccia predefinita del controllo CreateUserWizard è sufficiente per gli scenari di creazione di account semplici in cui è necessario raccogliere solo le informazioni relative all'account utente di base, ad esempio nome utente, password e posta elettronica. Ma cosa accade se si vuole chiedere al visitatore di immettere la propria città, la Home page e la firma durante la creazione dell'account? È possibile personalizzare l'interfaccia del controllo CreateUserWizard per raccogliere informazioni aggiuntive all'iscrizione e tali informazioni possono essere utilizzate nel gestore eventi `CreatedUser` per inserire record aggiuntivi nel database sottostante.

Il controllo CreateUserWizard estende il controllo della procedura guidata ASP.NET, un controllo che consente a uno sviluppatore di pagine di definire una serie di `WizardSteps`ordinati. Il controllo procedura guidata esegue il rendering del passaggio attivo e fornisce un'interfaccia di navigazione che consente al visitatore di spostarsi attraverso questi passaggi. Il controllo della procedura guidata è ideale per suddividere una grande attività in diversi brevi passaggi. Per ulteriori informazioni sul controllo della procedura guidata, vedere la pagina relativa alla [creazione di un'interfaccia utente dettagliata con il controllo ASP.NET 2,0 Wizard](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx).

Il markup predefinito del controllo CreateUserWizard definisce due `WizardSteps`: `CreateUserWizardStep` e `CompleteWizardStep`.

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample12.aspx)]

Il primo `WizardStep`, `CreateUserWizardStep`, esegue il rendering dell'interfaccia che richiede il nome utente, la password, l'indirizzo di posta elettronica e così via. Quando il visitatore fornisce queste informazioni e fa clic su "Crea utente", viene visualizzata la `CompleteWizardStep`, che mostra il messaggio di operazione riuscita e un pulsante continua.

Per personalizzare l'interfaccia del controllo CreateUserWizard per includere altri campi del modulo, è possibile:

- <strong>Creare uno o più nuovi</strong> <strong>`WizardStep`</strong> <strong>per contenere gli elementi aggiuntivi dell'interfaccia utente</strong>. Per aggiungere un nuovo `WizardStep` a CreateUserWizard, fare clic sul collegamento "Aggiungi/Rimuovi `WizardStep` s" dal relativo smart tag per avviare l'editor della raccolta `WizardStep`. Da qui è possibile aggiungere, rimuovere o riordinare i passaggi nella procedura guidata. Questo è l'approccio che verrà usato per questa esercitazione.

- <strong>Converte il</strong> <strong>`CreateUserWizardStep`</strong> <strong>in un`WizardStep`modificabile</strong> <strong>.</strong> Questa operazione sostituisce la `CreateUserWizardStep` con un `WizardStep` equivalente il cui markup definisce un'interfaccia utente che corrisponde al `CreateUserWizardStep`. Convertendo il `CreateUserWizardStep` in una `WizardStep` è possibile riposizionare i controlli o aggiungere altri elementi dell'interfaccia utente a questo passaggio. Per convertire il `CreateUserWizardStep` o `CompleteWizardStep` in un `WizardStep`modificabile, fare clic sul collegamento "Personalizza passaggio Crea utente" o "Personalizza passaggio completo" dallo smart tag del controllo.

- **Usare una combinazione delle due opzioni precedenti.**

Un aspetto importante da tenere presente è che il controllo CreateUserWizard esegue il processo di creazione dell'account utente quando si fa clic sul pulsante "Crea utente" dall'interno del `CreateUserWizardStep`. Non è importante se sono presenti ulteriori `WizardStep` dopo il `CreateUserWizardStep` o meno.

Quando si aggiunge un `WizardStep` personalizzato al controllo CreateUserWizard per raccogliere un input utente aggiuntivo, il `WizardStep` personalizzato può essere inserito prima o dopo il `CreateUserWizardStep`. Se precede il `CreateUserWizardStep`, l'input utente aggiuntivo raccolto dal `WizardStep` personalizzato sarà disponibile per il gestore dell'evento `CreatedUser`. Tuttavia, se il `WizardStep` personalizzato viene eseguito dopo `CreateUserWizardStep` quindi al momento della visualizzazione del `WizardStep` personalizzato, il nuovo account utente è già stato creato e l'evento `CreatedUser` è già stato attivato.

Nella figura 21 viene illustrato il flusso di lavoro quando il `WizardStep` aggiunto precede l'`CreateUserWizardStep`. Poiché le informazioni aggiuntive sull'utente sono state raccolte nel momento in cui viene generato l'evento `CreatedUser`, è sufficiente aggiornare il gestore dell'evento `CreatedUser` per recuperare questi input e utilizzarli per i valori dei parametri dell'istruzione `INSERT` (anziché `DBNull.Value`).

[![il flusso di lavoro CreateUserWizard quando un WizardStep aggiuntivo precede CreateUserWizardStep](storing-additional-user-information-vb/_static/image62.png)](storing-additional-user-information-vb/_static/image61.png)

**Figura 21**: flusso di lavoro di CreateUserWizard quando un `WizardStep` aggiuntivo precede il `CreateUserWizardStep` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image63.png))

Se la `WizardStep` personalizzata viene posizionata *dopo* il `CreateUserWizardStep`, tuttavia, il processo di creazione dell'account utente si verifica prima che l'utente abbia la possibilità di immettere la propria città, la Home page o la firma. In tal caso, è necessario inserire queste informazioni aggiuntive nel database dopo che è stato creato l'account utente, come illustrato nella figura 22.

[![il flusso di lavoro CreateUserWizard quando un WizardStep aggiuntivo viene eseguito dopo CreateUserWizardStep](storing-additional-user-information-vb/_static/image65.png)](storing-additional-user-information-vb/_static/image64.png)

**Figura 22**: flusso di lavoro di CreateUserWizard quando un `WizardStep` aggiuntivo viene eseguito dopo il `CreateUserWizardStep` ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image66.png))

Il flusso di lavoro illustrato nella figura 22 attende di inserire un record nella tabella `UserProfiles` finché non viene completato il passaggio 2. Se il visitatore chiude il browser dopo il passaggio 1, tuttavia, verrà raggiunto uno stato in cui è stato creato un account utente, ma non è stato aggiunto alcun record a `UserProfiles`. Una soluzione alternativa consiste nel disporre di un record con `NULL` o valori predefiniti inseriti in `UserProfiles` nel gestore dell'evento `CreatedUser` (che viene attivato dopo il passaggio 1) e quindi aggiornare il record dopo il completamento del passaggio 2. In questo modo si garantisce che venga aggiunto un record `UserProfiles` per l'account utente, anche se l'utente chiude il processo di registrazione a metà.

Per questa esercitazione verrà creato un nuovo `WizardStep` che si verifica dopo il `CreateUserWizardStep` ma prima del `CompleteWizardStep`. Per prima cosa è necessario ottenere il WizardStep e configurarlo e quindi esaminare il codice.

Dallo smart tag del controllo CreateUserWizard selezionare "Aggiungi/Rimuovi `WizardStep` s", che visualizza la finestra di dialogo Editor dell'insieme di `WizardStep`. Aggiungere una nuova `WizardStep`, impostando la relativa `ID` su `UserSettings`, la `Title` su "Impostazioni" e il `StepType` `Step`. Quindi posizionarlo in modo che venga eseguito dopo il `CreateUserWizardStep` ("iscriversi al nuovo account") e prima della `CompleteWizardStep` ("completa"), come illustrato nella figura 23.

[![aggiungere un nuovo WizardStep al controllo CreateUserWizard](storing-additional-user-information-vb/_static/image68.png)](storing-additional-user-information-vb/_static/image67.png)

**Figura 23**: aggiungere un nuovo `WizardStep` al controllo CreateUserWizard ([fare clic per visualizzare l'immagine con dimensioni complete](storing-additional-user-information-vb/_static/image69.png))

Fare clic su OK per chiudere la finestra di dialogo Editor della raccolta `WizardStep`. Il nuovo `WizardStep` viene evidenziato dal markup dichiarativo aggiornato del controllo CreateUserWizard:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample13.aspx)]

Si noti il nuovo elemento `<asp:WizardStep>`. È necessario aggiungere l'interfaccia utente per raccogliere qui la città, la Home page e la firma del nuovo utente. È possibile immettere questo contenuto nella sintassi dichiarativa o nella finestra di progettazione. Per usare la finestra di progettazione, selezionare il passaggio "Impostazioni" dall'elenco a discesa nello smart tag per visualizzare il passaggio nella finestra di progettazione.

> [!NOTE]
> Se si seleziona un passaggio nell'elenco a discesa smart tag, viene aggiornata la [proprietà`ActiveStepIndex`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.createuserwizard.activestepindex.aspx)del controllo CreateUserWizard, che specifica l'indice del passaggio iniziale. Se pertanto si utilizza questo elenco a discesa per modificare il passaggio "Impostazioni" nella finestra di progettazione, assicurarsi di impostarlo di nuovo su "iscrizione al nuovo account" in modo che questo passaggio venga visualizzato quando gli utenti visitano prima la pagina `EnhancedCreateUserWizard.aspx`.

Creare un'interfaccia utente nel passaggio "Your Settings" (impostazioni) contenente tre controlli TextBox denominati `HomeTown`, `HomepageUrl`e `Signature`. Dopo la costruzione di questa interfaccia, il markup dichiarativo di CreateUserWizard dovrebbe essere simile al seguente:

[!code-aspx[Main](storing-additional-user-information-vb/samples/sample14.aspx)]

Andare avanti e visitare questa pagina tramite un browser e creare un nuovo account utente, specificando i valori per la città Home, la Home page e la firma. Al termine dell'`CreateUserWizardStep` viene creato l'account utente nel Framework di appartenenza e viene eseguito il gestore eventi `CreatedUser`, che aggiunge una nuova riga a `UserProfiles`, ma con un valore `NULL` database per `HomeTown`, `HomepageUrl`e `Signature`. I valori immessi per la città principale, la Home page e la firma non vengono mai usati. Il risultato finale è un nuovo account utente con un record `UserProfiles` i cui campi `HomeTown`, `HomepageUrl`e `Signature` devono ancora essere specificati.

È necessario eseguire il codice dopo il passaggio "Your Settings" (impostazioni) che accetta i valori della città iniziale, della honepage e della firma immessi dall'utente e aggiorna il record `UserProfiles` appropriato. Ogni volta che l'utente passa da una procedura all'altra in un controllo della procedura guidata, viene generato l' [evento`ActiveStepChanged`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.wizard.activestepchanged.aspx) della procedura guidata. È possibile creare un gestore eventi per questo evento e aggiornare la tabella `UserProfiles` quando il passaggio "Impostazioni" è stato completato.

Aggiungere un gestore eventi per l'evento `ActiveStepChanged` di CreateUserWizard e aggiungere il codice seguente:

[!code-vb[Main](storing-additional-user-information-vb/samples/sample15.vb)]

Il codice precedente inizia determinando se è stato appena raggiunto il passaggio "completo". Poiché il passaggio "complete" si verifica immediatamente dopo il passaggio "Your Settings" (impostazioni), quando il visitatore raggiunge il passaggio "complete" significa che ha appena terminato il passaggio "Your Settings" (impostazioni).

In tal caso, è necessario fare riferimento a livello di codice ai controlli TextBox all'interno del `UserSettings WizardStep`. Questa operazione viene eseguita usando prima il metodo `FindControl` per fare riferimento a a livello di codice al `UserSettings WizardStep`e quindi di nuovo per fare riferimento alle caselle di testo all'interno della `WizardStep`. Una volta fatto riferimento alle caselle di testo, è possibile eseguire l'istruzione `UPDATE`. L'istruzione `UPDATE` ha lo stesso numero di parametri dell'istruzione `INSERT` nel gestore dell'evento `CreatedUser`, ma in questo caso vengono usati i valori della città, della Home page e della firma forniti dall'utente.

Con questo gestore eventi, visitare la pagina `EnhancedCreateUserWizard.aspx` tramite un browser e creare un nuovo account utente specificando i valori per la città, la Home page e la firma. Dopo aver creato il nuovo account, è necessario reindirizzare alla pagina `AdditionalUserInfo.aspx`, in cui vengono visualizzate le informazioni relative alla Home page, alla Home page e alla firma appena immesse.

> [!NOTE]
> Il sito Web include attualmente due pagine da cui un visitatore può creare un nuovo account: `CreatingUserAccounts.aspx` e `EnhancedCreateUserWizard.aspx`. La pagina Sitemap e l'account di accesso del sito Web puntano alla pagina `CreatingUserAccounts.aspx`, ma la pagina `CreatingUserAccounts.aspx` non richiede all'utente le informazioni relative alla città iniziale, alla Home page e alla firma e non aggiunge una riga corrispondente a `UserProfiles`. Aggiornare quindi la pagina `CreatingUserAccounts.aspx` in modo che offra questa funzionalità oppure aggiornare la pagina Sitemap e l'account di accesso per fare riferimento `EnhancedCreateUserWizard.aspx` invece di `CreatingUserAccounts.aspx`. Se si sceglie la seconda opzione, assicurarsi di aggiornare il file di `Web.config` della cartella `Membership` in modo da consentire agli utenti anonimi di accedere alla pagina `EnhancedCreateUserWizard.aspx`.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state esaminate le tecniche per la modellazione dei dati correlati agli account utente all'interno del framework delle appartenenze. In particolare, è stata esaminata la modellazione delle entità che condividono una relazione uno-a-molti con gli account utente e i dati che condividono una relazione uno-a-uno. È stato inoltre illustrato come visualizzare, inserire e aggiornare le informazioni correlate, con alcuni esempi che usano il controllo SqlDataSource e altre che usano il codice ADO.NET.

Questa esercitazione completa l'aspetto degli account utente. A partire dall'esercitazione successiva, verrà rivolto l'attenzione ai ruoli. Nelle varie esercitazioni successive verrà esaminato il Framework dei ruoli, viene illustrato come creare nuovi ruoli, come assegnare i ruoli agli utenti, come determinare i ruoli a cui un utente appartiene e come applicare l'autorizzazione basata sui ruoli.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Accesso e aggiornamento dei dati in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/011106-1.aspx)
- [Controllo procedura guidata ASP.NET 2,0](https://weblogs.asp.net/scottgu/archive/2006/02/21/438732.aspx)
- [Creazione di un'interfaccia utente dettagliata con il controllo procedura guidata ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/061406-1.aspx)
- [Creazione di parametri di controllo DataSource personalizzati](http://aspnet.4guysfromrolla.com/articles/110106-1.aspx)
- [Personalizzazione del controllo CreateUserWizard](http://aspnet.4guysfromrolla.com/articles/070506-1.aspx)
- [Guide introduttive del controllo DetailsView](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/detailsview.aspx)
- [Visualizzazione di dati con il controllo ListView](http://aspnet.4guysfromrolla.com/articles/122607-1.aspx)
- [Dissezione dei controlli di convalida in ASP.NET 2,0](http://aspnet.4guysfromrolla.com/articles/112305-1.aspx)
- [Modifica di inserimento ed eliminazione di dati](../../data-access/editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)
- [Convalida del modulo in ASP.NET](http://www.4guysfromrolla.com/webtech/090200-1.shtml)
- [Raccolta di informazioni di registrazione utente personalizzate](https://weblogs.asp.net/scottgu/archive/2006/07/05/Tip_2F00_Trick_3A00_-Gathering-Custom-User-Registration-Information.aspx)
- [Profili in ASP.NET 2,0](http://www.odetocode.com/Articles/440.aspx)
- [Il controllo ASP: ListView](https://weblogs.asp.net/scottgu/archive/2007/08/10/the-asp-listview-control-part-1-building-a-product-listing-page-with-clean-css-ui.aspx)
- [Guida introduttiva ai profili utente](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/profile/default.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Precedente](user-based-authorization-vb.md)
