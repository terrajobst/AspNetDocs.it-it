---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
title: Appartenenza e amministrazione | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 732a2316-e49f-4f72-becd-0cd72f14457e
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/membership-and-administration
msc.type: authoredcontent
ms.openlocfilehash: ab00bc90bfc767d06e747be6dfb973245b5aae88
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78546739"
---
# <a name="membership-and-administration"></a>Appartenenza e amministrazione

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

Questa esercitazione illustra come aggiornare l'applicazione di esempio Wingtip Toys per aggiungere un ruolo personalizzato e usare ASP.NET Identity. Viene inoltre illustrato come implementare una pagina di amministrazione dalla quale l'utente con un ruolo personalizzato può aggiungere e rimuovere prodotti dal sito Web.

[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md) è il sistema di appartenenze usato per compilare l'applicazione Web ASP.NET ed è disponibile in ASP.NET 4,5. ASP.NET Identity viene utilizzato nel modello di progetto Web Form Visual Studio 2013, nonché nei modelli per l' [applicazione a pagina singola](../../../../single-page-application/index.md) [ASP.NET MVC](../../../../mvc/index.md), [API Web ASP.NET](../../../../web-api/index.md)e ASP.NET. È anche possibile installare in modo specifico il sistema di ASP.NET Identity usando NuGet quando si inizia con un'applicazione Web vuota. Tuttavia, in questa serie di esercitazioni viene usato il ProjectTemplate **Web Form**, che include il sistema di ASP.NET Identity. ASP.NET Identity semplifica l'integrazione dei dati di profilo specifici dell'utente con i dati dell'applicazione. ASP.NET Identity consente inoltre di scegliere il modello di persistenza dei profili utente nell'applicazione. È possibile archiviare i dati in un database SQL Server o in un altro archivio dati, inclusi gli archivi dati *NoSQL* , ad esempio le tabelle di archiviazione di Microsoft Azure.

Questa esercitazione si basa sull'esercitazione precedente intitolata "Checkout e pagamento con PayPal" nella serie di esercitazioni su Wingtip Toys.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come usare il codice per aggiungere un ruolo personalizzato e un utente all'applicazione.
- Come limitare l'accesso alla cartella e alla pagina di amministrazione.
- Come fornire la navigazione per l'utente che appartiene al ruolo personalizzato.
- Come utilizzare l'associazione di modelli per popolare un controllo [DropDownList](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist(v=vs.110).aspx) con categorie di prodotti.
- Come caricare un file nell'applicazione Web usando il controllo [FileUpload](https://msdn.microsoft.com/library/system.web.ui.webcontrols.fileupload(v=vs.110).aspx) .
- Come usare i controlli di convalida per implementare la convalida dell'input.
- Come aggiungere e rimuovere prodotti dall'applicazione.

## <a name="these-features-are-included-in-the-tutorial"></a>Queste funzionalità sono incluse nell'esercitazione:

- Identità ASP.NET
- Configurazione e autorizzazione
- Associazione di modelli
- Convalida non intrusiva

ASP.NET Web Form fornisce funzionalità di appartenenza. Utilizzando il modello predefinito, sono disponibili funzionalità di appartenenza predefinite che è possibile utilizzare immediatamente durante l'esecuzione dell'applicazione. Questa esercitazione illustra come usare ASP.NET Identity per aggiungere un ruolo personalizzato e assegnare un utente a tale ruolo. Si apprenderà come limitare l'accesso alla cartella di amministrazione. Si aggiungerà una pagina alla cartella di amministrazione che consente a un utente con un ruolo personalizzato di aggiungere e rimuovere prodotti e di visualizzare in anteprima un prodotto dopo che è stato aggiunto.

## <a name="adding-a-custom-role"></a>Aggiunta di un ruolo personalizzato

Utilizzando ASP.NET Identity, è possibile aggiungere un ruolo personalizzato e assegnare un utente a tale ruolo utilizzando il codice.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella della *logica* e creare una nuova classe.
2. Assegnare alla nuova classe il nome *RoleActions.cs*.
3. Modificare il codice in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample1.cs?highlight=8)]
4. In **Esplora soluzioni**aprire il file *Global.asax.cs* .
5. Modificare il file *Global.asax.cs* aggiungendo il codice evidenziato in giallo, in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample2.cs?highlight=11,26-28)]
6. Si noti che `AddUserAndRole` è sottolineato in rosso. Fare doppio clic sul codice AddUserAndRole.  
   La lettera "A" all'inizio del metodo evidenziato sarà sottolineata.
7. Passare il puntatore sulla lettera "A" e fare clic sull'interfaccia utente che consente di generare uno stub del metodo per il metodo `AddUserAndRole`. 

    ![Appartenenza e amministrazione-genera stub metodo](membership-and-administration/_static/image1.png)
8. Fare clic sull'opzione denominata:  
    `Generate method stub for "AddUserAndRole" in "WingtipToys.Logic.RoleActions"`
9. Aprire il file *RoleActions.cs* dalla cartella *logica* .  
   Il metodo `AddUserAndRole` è stato aggiunto al file di classe.
10. Modificare il file *RoleActions.cs* rimuovendo il `NotImplementedException` e aggiungendo il codice evidenziato in giallo, in modo che venga visualizzato come segue:  

    [!code-csharp[Main](membership-and-administration/samples/sample3.cs?highlight=5-7,15-51)]

Il codice precedente stabilisce innanzitutto un contesto di database per il database delle appartenenze. Il database delle appartenenze viene inoltre archiviato come file con *estensione MDF* nella cartella *app\_data* . Sarà possibile visualizzare il database dopo che il primo utente ha eseguito l'accesso a questa applicazione Web. 

> [!NOTE] 
> 
> Se si desidera archiviare i dati di appartenenza insieme ai dati del prodotto, è possibile utilizzare lo stesso **DbContext** utilizzato per archiviare i dati del prodotto nel codice riportato in precedenza.

 La parola chiave *Internal* è un modificatore di accesso per i tipi, ad esempio le classi, e i membri del tipo (ad esempio metodi o proprietà). I tipi o i membri interni sono accessibili solo all'interno di file contenuti nello stesso assembly (file con *estensione dll* ). Quando si compila l'applicazione, viene creato un file di assembly *(con estensione dll*) che contiene il codice che viene eseguito quando si esegue l'applicazione. 

Un oggetto `RoleStore`, che fornisce la gestione dei ruoli, viene creato in base al contesto del database.

> [!NOTE] 
> 
> Si noti che quando viene creato l'oggetto `RoleStore`, viene usato un tipo di `IdentityRole` generico. Ciò significa che l'`RoleStore` è consentita solo per contenere `IdentityRole` oggetti. Inoltre, utilizzando i generics, le risorse in memoria sono gestite meglio.

Successivamente, l'oggetto `RoleManager` viene creato in base all'oggetto `RoleStore` appena creato. l'oggetto `RoleManager` espone l'API relativa ai ruoli che può essere usata per salvare automaticamente le modifiche apportate al `RoleStore`. Il `RoleManager` può contenere solo oggetti `IdentityRole` perché il codice usa il tipo generico `<IdentityRole>`.

Chiamare il metodo `RoleExists` per determinare se il ruolo "canEdit" è presente nel database delle appartenenze. In caso contrario, è necessario creare il ruolo.

La creazione dell'oggetto `UserManager` sembra essere più complessa rispetto al controllo `RoleManager`, ma è quasi uguale. Viene appena codificata in una riga anziché in più. In questo caso, il parametro passato sta creando un'istanza come nuovo oggetto contenuto nella parentesi.

Successivamente, creare l'utente "canEditUser" creando un nuovo oggetto `ApplicationUser`. Quindi, se l'utente è stato creato correttamente, l'utente viene aggiunto al nuovo ruolo.

> [!NOTE] 
> 
> La gestione degli errori verrà aggiornata durante l'esercitazione "gestione degli errori ASP.NET" più avanti in questa serie di esercitazioni.

Al successivo avvio dell'applicazione, l'utente denominato "canEditUser" verrà aggiunto come ruolo denominato "canEdit" dell'applicazione. Più avanti in questa esercitazione si effettuerà l'accesso come utente "canEditUser" per visualizzare le funzionalità aggiuntive che si aggiungeranno durante questa esercitazione. Per informazioni dettagliate sull'API ASP.NET Identity, vedere lo [spazio dei nomi Microsoft. AspNet. Identity](https://msdn.microsoft.com/library/microsoft.aspnet.identity(v=vs.111).aspx). Per ulteriori informazioni sull'inizializzazione del sistema di ASP.NET Identity, vedere [AspnetIdentitySample](https://github.com/rustd/AspnetIdentitySample/blob/master/AspnetIdentitySample/App_Start/IdentityConfig.cs).

### <a name="restricting-access-to-the-administration-page"></a>Limitazione dell'accesso alla pagina di amministrazione

L'applicazione di esempio Wingtip Toys consente a utenti anonimi e utenti connessi di visualizzare e acquistare i prodotti. Tuttavia, l'utente che ha eseguito l'accesso con il ruolo "canEdit" personalizzato può accedere a una pagina con restrizioni per aggiungere e rimuovere i prodotti.

#### <a name="add-an-administration-folder-and-page"></a>Aggiungere una cartella di amministrazione e una pagina

Si creerà quindi una cartella denominata *admin* per l'utente "canEditUser" appartenente al ruolo personalizzato dell'applicazione di esempio Wingtip Toys.

1. Fare clic con il pulsante destro del mouse sul nome del progetto (**Wingtip Toys**) in **Esplora soluzioni** e scegliere **Aggiungi** -&gt; **nuova cartella**.
2. Assegnare un nome alla nuova cartella *admin*.
3. Fare clic con il pulsante destro del mouse sulla cartella *Amministrazione* , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
4. Selezionare il <strong>gruppo C# Visual</strong>-&gt; modelli <strong>Web</strong> a sinistra. Dall'elenco centrale selezionare <strong>Web Form con la pagina master</strong>, denominarlo <em>AdminPage. aspx</em><strong>e quindi</strong> selezionare <strong>Aggiungi</strong>.
5. Selezionare il file *site. master* come pagina master, quindi scegliere **OK**.

#### <a name="add-a-webconfig-file"></a>Aggiungere un file Web. config

Aggiungendo un file *Web. config* alla cartella *amministratore* , è possibile limitare l'accesso alla pagina contenuta nella cartella.

1. Fare clic con il pulsante destro del mouse sulla cartella *Amministrazione* e scegliere **Aggiungi** -&gt; **nuovo elemento**.  
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Dall'elenco di modelli Web C# visivi, selezionare <strong>file di configurazione Web</strong>dall'elenco centrale, accettare il nome predefinito di <em>Web. config</em><strong>,</strong> quindi selezionare <strong>Aggiungi</strong>.
3. Sostituire il contenuto XML esistente nel file *Web.config* con il contenuto seguente:  

    [!code-xml[Main](membership-and-administration/samples/sample4.xml)]

Salvare il file *Web.config* . Il file *Web. config* specifica che solo l'utente che appartiene al ruolo "canEdit" dell'applicazione può accedere alla pagina contenuta nella cartella *admin* .

### <a name="including-custom-role-navigation"></a>Inclusione dell'esplorazione del ruolo personalizzato

Per consentire all'utente del ruolo "canEdit" personalizzato di passare alla sezione amministrazione dell'applicazione, è necessario aggiungere un collegamento alla pagina *site. master* . Solo gli utenti che appartengono al ruolo "canEdit" saranno in grado di visualizzare il collegamento **amministratore** e accedere alla sezione amministrazione.

1. In Esplora soluzioni individuare e aprire la pagina *site. master* .
2. Per creare un collegamento per l'utente del ruolo "canEdit", aggiungere il markup evidenziato in giallo all'elenco non ordinato seguente `<ul>` elemento in modo che l'elenco venga visualizzato come segue:  

    [!code-html[Main](membership-and-administration/samples/sample5.html?highlight=2-3)]
3. Aprire il file *site.master.cs* . Rendere il collegamento **amministratore** visibile solo all'utente "canEditUser" aggiungendo il codice evidenziato in giallo al gestore `Page_Load`. Il gestore `Page_Load` verrà visualizzato come segue:   

    [!code-csharp[Main](membership-and-administration/samples/sample6.cs?highlight=3-6)]

Quando la pagina viene caricata, il codice controlla se l'utente connesso ha il ruolo di "canEdit". Se l'utente appartiene al ruolo "canEdit", l'elemento Span contenente il collegamento alla pagina *AdminPage. aspx* (e di conseguenza il collegamento nell'intervallo) viene reso visibile.

### <a name="enabling-product-administration"></a>Abilitazione dell'amministrazione del prodotto

Fino a questo punto, è stato creato il ruolo "canEdit" e sono stati aggiunti un utente "canEditUser", una cartella di amministrazione e una pagina di amministrazione. Sono stati impostati i diritti di accesso per la cartella e la pagina di amministrazione e è stato aggiunto un collegamento di navigazione per l'utente del ruolo "canEdit" all'applicazione. Successivamente, si aggiungerà il markup alla pagina *AdminPage. aspx* e al codice al file code-behind *AdminPage.aspx.cs* che consentirà all'utente con il ruolo "canEdit" di aggiungere e rimuovere i prodotti.

1. In **Esplora soluzioni**aprire il file *AdminPage. aspx* dalla cartella *admin* .
2. Sostituire il markup esistente con il codice seguente:  

    [!code-aspx[Main](membership-and-administration/samples/sample7.aspx)]
3. Successivamente, aprire il file code-behind *AdminPage.aspx.cs* facendo clic con il pulsante destro del mouse su *AdminPage. aspx* e scegliendo **Visualizza codice**.
4. Sostituire il codice esistente nel file code-behind *AdminPage.aspx.cs* con il codice seguente:  

    [!code-csharp[Main](membership-and-administration/samples/sample8.cs)]

Nel codice immesso per il file code-behind *AdminPage.aspx.cs* , una classe denominata `AddProducts` esegue le operazioni effettive di aggiunta di prodotti al database. Questa classe non esiste ancora, quindi verrà creata ora.

1. In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *logica* , quindi scegliere **Aggiungi** -&gt; **nuovo elemento**.   
   Verrà visualizzata la finestra di dialogo **Aggiungi nuovo elemento**.
2. Selezionare il **gruppo C# Visual** -&gt; modelli di **codice** a sinistra. Quindi, selezionare **classe**nell'elenco centrale e denominarla *AddProducts.cs*.   
   Viene visualizzato il nuovo file di classe.
3. Sostituire il codice esistente con quello seguente:  

    [!code-csharp[Main](membership-and-administration/samples/sample9.cs)]

La pagina *AdminPage. aspx* consente all'utente che appartiene al ruolo "canEdit" di aggiungere e rimuovere i prodotti. Quando viene aggiunto un nuovo prodotto, i dettagli relativi al prodotto vengono convalidati e quindi immessi nel database. Il nuovo prodotto è immediatamente disponibile per tutti gli utenti dell'applicazione Web.

#### <a name="unobtrusive-validation"></a>Convalida non intrusiva

I dettagli del prodotto forniti dall'utente nella pagina *AdminPage. aspx* vengono convalidati utilizzando i controlli di convalida (`RequiredFieldValidator` e `RegularExpressionValidator`). Questi controlli utilizzano automaticamente la convalida non intrusiva. La convalida non intrusiva consente ai controlli di convalida di utilizzare JavaScript per la logica di convalida lato client, il che significa che la pagina non richiede un viaggio al server per la convalida. Per impostazione predefinita, la convalida non intrusiva è inclusa nel file *Web. config* in base all'impostazione di configurazione seguente:

[!code-xml[Main](membership-and-administration/samples/sample10.xml)]

#### <a name="regular-expressions"></a>Espressioni regolari

Il prezzo del prodotto nella pagina *AdminPage. aspx* viene convalidato tramite un controllo **RegularExpressionValidator** . Questo controllo consente di verificare se il valore del controllo di input associato (la casella di testo "AddProductPrice") corrisponde al modello specificato dall'espressione regolare. Un'espressione regolare è una notazione corrispondente che consente di trovare e confrontare rapidamente modelli di caratteri specifici. Il controllo **RegularExpressionValidator** include una proprietà denominata `ValidationExpression` che contiene l'espressione regolare usata per convalidare l'input del prezzo, come illustrato di seguito:

[!code-aspx[Main](membership-and-administration/samples/sample11.aspx)]

#### <a name="fileupload-control"></a>Controllo FileUpload

Oltre ai controlli di input e di convalida, il controllo **FileUpload** è stato aggiunto alla pagina *AdminPage. aspx* . Questo controllo fornisce la possibilità di caricare file. In questo caso, si consente il caricamento solo dei file di immagine. Nel file code-behind (*AdminPage.aspx.cs*), quando si fa clic sul `AddProductButton`, il codice controlla la proprietà `HasFile` del controllo **FileUpload** . Se il controllo contiene un file e il tipo di file (basato sull'estensione di file) è consentito, l'immagine viene salvata nella cartella *Immagini* e nella cartella *Immagini/thumbs* dell'applicazione.

#### <a name="model-binding"></a>Associazione di modelli

In precedenza in questa serie di esercitazioni è stata usata l'associazione di modelli per popolare un controllo **ListView** , un controllo **FormsView** , un controllo **GridView** e un controllo **controllo DetailView per** . In questa esercitazione si usa l'associazione di modelli per popolare un controllo **DropDownList** con un elenco di categorie di prodotti.

Il markup aggiunto al file *AdminPage. aspx* contiene un controllo **DropDownList** denominato `DropDownAddCategory`:

[!code-aspx[Main](membership-and-administration/samples/sample12.aspx)]

È possibile utilizzare l'associazione di modelli per popolare questo **DropDownList** impostando l'attributo `ItemType` e l'attributo `SelectMethod`. L'attributo `ItemType` specifica di usare il tipo di `WingtipToys.Models.Category` quando si compila il controllo. Questo tipo è stato definito all'inizio di questa serie di esercitazioni creando la classe `Category` (mostrata di seguito). La classe `Category` si trova nella cartella *Models* all'interno del file *Category.cs* .

[!code-csharp[Main](membership-and-administration/samples/sample13.cs)]

L'attributo `SelectMethod` del controllo **DropDownList** specifica che si usa il metodo `GetCategories` (illustrato di seguito) incluso nel file code-behind (*AdminPage.aspx.cs*).

[!code-csharp[Main](membership-and-administration/samples/sample14.cs)]

Questo metodo specifica che un'interfaccia `IQueryable` viene utilizzata per valutare una query in base a un tipo di `Category`. Il valore restituito viene utilizzato per popolare l'oggetto **DropDownList** nel markup della pagina (*AdminPage. aspx*).

Il testo visualizzato per ogni elemento nell'elenco viene specificato impostando l'attributo `DataTextField`. L'attributo `DataTextField` utilizza la `CategoryName` della classe `Category` (illustrata in precedenza) per visualizzare ogni categoria nel controllo **DropDownList** . Il valore effettivo che viene passato quando si seleziona un elemento nel controllo **DropDownList** è basato sull'attributo `DataValueField`. L'attributo `DataValueField` viene impostato sul `CategoryID` come definito nella classe `Category` (illustrata in precedenza).

### <a name="how-the-application-will-work"></a>Funzionamento dell'applicazione

Quando l'utente che appartiene al ruolo "canEdit" passa alla pagina per la prima volta, il `DropDownAddCategory`controllo **DropDownList** viene popolato come descritto in precedenza. Il `DropDownRemoveProduct`controllo **DropDownList** viene inoltre popolato con i prodotti che utilizzano lo stesso approccio. L'utente che appartiene al ruolo "canEdit" Seleziona il tipo di categoria e aggiunge i dettagli del prodotto, ovvero**nome**, **Descrizione**, **Prezzo**e **file di immagine**. Quando l'utente che appartiene al ruolo "canEdit" fa clic sul pulsante **Aggiungi prodotto** , viene attivato il gestore dell'evento `AddProductButton_Click`. Il gestore dell'evento `AddProductButton_Click` che si trova nel file code-behind (*AdminPage.aspx.cs*) controlla il file di immagine per assicurarsi che corrisponda ai tipi di file consentiti *(. gif*, *. png*, *. jpeg*o *. jpg*). Il file di immagine viene quindi salvato in una cartella dell'applicazione di esempio Wingtip Toys. Il nuovo prodotto verrà quindi aggiunto al database. Per completare l'aggiunta di un nuovo prodotto, viene creata una nuova istanza della classe `AddProducts` e vengono denominati i prodotti. La classe `AddProducts` dispone di un metodo denominato `AddProduct`e l'oggetto Products chiama questo metodo per aggiungere prodotti al database.

[!code-csharp[Main](membership-and-administration/samples/sample15.cs)]

Se il codice aggiunge correttamente il nuovo prodotto al database, la pagina viene ricaricata con il valore della stringa di query `ProductAction=add`.

[!code-csharp[Main](membership-and-administration/samples/sample16.cs)]

Quando la pagina viene ricaricata, la stringa di query viene inclusa nell'URL. Ricaricando la pagina, l'utente che appartiene al ruolo "canEdit" può visualizzare immediatamente gli aggiornamenti nei controlli **DropDownList** nella pagina *AdminPage. aspx* . Inoltre, includendo la stringa di query con l'URL, nella pagina è possibile visualizzare un messaggio di operazione riuscita per l'utente che appartiene al ruolo "canEdit".

Quando la pagina *AdminPage. aspx* viene ricaricata, viene chiamato l'evento `Page_Load`.

[!code-csharp[Main](membership-and-administration/samples/sample17.cs)]

Il gestore dell'evento `Page_Load` controlla il valore della stringa di query e determina se visualizzare un messaggio di operazione riuscita.

## <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire ora l'applicazione per vedere come è possibile aggiungere, eliminare e aggiornare gli elementi nel carrello acquisti. Il totale del carrello acquisti riflette il costo totale di tutti gli elementi nel carrello acquisti.

1. In Esplora soluzioni premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
   Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .
2. Fare clic sul collegamento **Accedi nella** parte superiore della pagina. 

    ![Appartenenza e amministrazione-collegamento accedi](membership-and-administration/_static/image2.png)

   Verrà visualizzata la pagina *login. aspx* .
3. Usare il nome utente e la password seguenti:  
   Nome utente: canEditUser@wingtiptoys.com  
   Password: Pa$$word1 

    ![Appartenenza e amministrazione-pagina di accesso](membership-and-administration/_static/image3.png)
4. Fare clic sul pulsante **Accedi nella** parte inferiore della pagina.
5. Nella parte superiore della pagina successiva selezionare il collegamento **amministratore** per passare alla pagina *AdminPage. aspx* . 

    ![Appartenenza e amministrazione-collegamento amministratore](membership-and-administration/_static/image4.png)
6. Per testare la convalida dell'input, fare clic sul pulsante **Aggiungi prodotto** senza aggiungere i dettagli del prodotto. 

    ![Appartenenza e amministrazione-pagina amministratore](membership-and-administration/_static/image5.png)

   Si noti che vengono visualizzati i messaggi di campo obbligatori.
7. Aggiungere i dettagli per un nuovo prodotto, quindi fare clic sul pulsante **Aggiungi prodotto** . 

    ![Appartenenza e amministrazione-Aggiungi prodotto](membership-and-administration/_static/image6.png)
8. Selezionare **prodotti** dal menu di spostamento superiore per visualizzare il nuovo prodotto aggiunto. 

    ![Appartenenza e amministrazione-Mostra nuovo prodotto](membership-and-administration/_static/image7.png)
9. Fare clic sul collegamento **amministratore** per tornare alla pagina di amministrazione.
10. Nella sezione **Rimuovi prodotto** della pagina selezionare il nuovo prodotto aggiunto in **DropDownListBox**.
11. Fare clic sul pulsante **Rimuovi prodotto** per rimuovere il nuovo prodotto dall'applicazione. 

    ![Appartenenza e amministrazione-Rimuovi prodotto](membership-and-administration/_static/image8.png)
12. Selezionare **prodotti** dal menu di spostamento superiore per confermare che il prodotto è stato rimosso.
13. Fare clic su **Disconnetti** per esistere in modalità di amministrazione.   
    Si noti che nel riquadro di spostamento superiore non viene più visualizzata la voce di menu **admin** .

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati aggiunti un ruolo personalizzato e un utente appartenente al ruolo personalizzato, l'accesso limitato alla cartella e alla pagina di amministrazione e la navigazione per l'utente appartenente al ruolo personalizzato. È stata utilizzata l'associazione di modelli per popolare un controllo **DropDownList** con dati. Sono stati implementati il controllo **FileUpload** e i controlli di convalida. Si è inoltre appreso come aggiungere e rimuovere prodotti da un database. Nell'esercitazione successiva si apprenderà come implementare il routing ASP.NET.

## <a name="additional-resources"></a>Risorse aggiuntive

[Web. config-elemento authorization](https://msdn.microsoft.com/library/8d82143t(v=vs.100).aspx)  
[ASP.NET Identity](../../../../identity/overview/getting-started/introduction-to-aspnet-identity.md)  
[Distribuire un'app Web Form ASP.NET sicura con appartenenza, OAuth e database SQL in un sito Web di Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Precedente](checkout-and-payment-with-paypal.md)
> [Successivo](url-routing.md)
