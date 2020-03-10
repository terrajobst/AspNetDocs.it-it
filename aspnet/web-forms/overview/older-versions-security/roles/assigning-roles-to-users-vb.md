---
uid: web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
title: Assegnazione di ruoli agli utenti (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione vengono create due pagine di ASP.NET per semplificare la gestione degli utenti che appartengono ai ruoli. La prima pagina includerà le funzionalità per vedere cosa...
ms.author: riande
ms.date: 03/24/2008
ms.assetid: fd208ee9-69cc-4467-9783-b4e039bdd1d3
msc.legacyurl: /web-forms/overview/older-versions-security/roles/assigning-roles-to-users-vb
msc.type: authoredcontent
ms.openlocfilehash: b53df4494eb0faef7c5e4547c2bf95e5fb071298
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78625328"
---
# <a name="assigning-roles-to-users-vb"></a>Assegnazione di ruoli agli utenti (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/VB.10.zip) o [Scarica PDF](https://download.microsoft.com/download/6/0/3/6032582f-360d-4739-b935-38721fdb86ea/aspnet_tutorial10_AssigningRoles_vb.pdf)

> In questa esercitazione vengono create due pagine di ASP.NET per semplificare la gestione degli utenti che appartengono ai ruoli. La prima pagina includerà le funzionalità per visualizzare gli utenti che appartengono a un determinato ruolo, i ruoli a cui appartiene un determinato utente e la possibilità di assegnare o rimuovere un determinato utente da un particolare ruolo. Nella seconda pagina verrà aumentato il controllo CreateUserWizard in modo da includere un passaggio per specificare i ruoli a cui appartiene l'utente appena creato. Questa operazione è utile negli scenari in cui un amministratore è in grado di creare nuovi account utente.

## <a name="introduction"></a>Introduzione

L' <a id="_msoanchor_1"> </a> [esercitazione precedente](creating-and-managing-roles-vb.md) ha esaminato il Framework dei ruoli e il `SqlRoleProvider`; Abbiamo visto come usare la classe `Roles` per creare, recuperare ed eliminare ruoli. Oltre alla creazione e all'eliminazione di ruoli, è necessario essere in grado di assegnare o rimuovere utenti da un ruolo. Sfortunatamente, ASP.NET non viene fornito con alcun controllo Web per la gestione degli utenti che appartengono ai ruoli. Al contrario, è necessario creare le proprie pagine ASP.NET per gestire queste associazioni. L'aspetto positivo è che l'aggiunta e la rimozione di utenti ai ruoli è piuttosto semplice. La classe `Roles` contiene una serie di metodi per l'aggiunta di uno o più utenti a uno o più ruoli.

In questa esercitazione vengono create due pagine di ASP.NET per semplificare la gestione degli utenti che appartengono ai ruoli. La prima pagina includerà le funzionalità per visualizzare gli utenti che appartengono a un determinato ruolo, i ruoli a cui appartiene un determinato utente e la possibilità di assegnare o rimuovere un determinato utente da un particolare ruolo. Nella seconda pagina verrà aumentato il controllo CreateUserWizard in modo da includere un passaggio per specificare i ruoli a cui appartiene l'utente appena creato. Questa operazione è utile negli scenari in cui un amministratore è in grado di creare nuovi account utente.

Ecco come procedere.

## <a name="listing-what-users-belong-to-what-roles"></a>Elenco degli utenti che appartengono ai ruoli

Il primo ordine aziendale per questa esercitazione consiste nel creare una pagina Web da cui gli utenti possono essere assegnati ai ruoli. Prima di dedicarsi a come assegnare gli utenti ai ruoli, si concentrerà innanzitutto su come determinare quali utenti appartengono ai ruoli. Esistono due modi per visualizzare queste informazioni: "per ruolo" o "per utente". È possibile consentire al visitatore di selezionare un ruolo e quindi visualizzare tutti gli utenti che appartengono al ruolo (visualizzazione "per ruolo"), oppure è possibile chiedere al visitatore di selezionare un utente e quindi di visualizzare i ruoli assegnati a tale utente (la visualizzazione "da parte dell'utente").

La vista "per ruolo" è utile nei casi in cui il visitatore desidera conoscere il set di utenti che appartengono a un ruolo specifico. la visualizzazione "per utente" è la scelta ideale quando il visitatore deve essere a conoscenza dei ruoli specifici di un utente. Nella pagina sono incluse le interfacce "per ruolo" e "per utente".

Si inizierà con la creazione dell'interfaccia "by User". Questa interfaccia è costituita da un elenco a discesa e da un elenco di caselle di controllo. L'elenco a discesa verrà popolato con il set di utenti nel sistema; le caselle di controllo enumerano i ruoli. Selezionando un utente dall'elenco a discesa, verranno verificati i ruoli a cui appartiene l'utente. La persona che visita la pagina può quindi selezionare o deselezionare le caselle di controllo per aggiungere o rimuovere l'utente selezionato dai ruoli corrispondenti.

> [!NOTE]
> L'uso di un elenco a discesa per elencare gli account utente non è la scelta ideale per i siti Web in cui potrebbero essere presenti centinaia di account utente. Un elenco a discesa è progettato per consentire a un utente di selezionare un elemento da un elenco di opzioni relativamente breve. Il numero di elementi dell'elenco cresce rapidamente. Se si compila un sito Web che avrà un numero potenzialmente elevato di account utente, potrebbe essere necessario considerare l'utilizzo di un'interfaccia utente alternativa, ad esempio un controllo GridView paginabile o un'interfaccia filtrabile che elenca la richiesta al visitatore di scegliere una lettera e quindi solo Mostra gli utenti il cui nome utente inizia con la lettera selezionata.

## <a name="step-1-building-the-by-user-user-interface"></a>Passaggio 1: compilazione dell'interfaccia utente "per utente"

Aprire la pagina `UsersAndRoles.aspx`. Nella parte superiore della pagina aggiungere un controllo Web Label denominato `ActionStatus` e deselezionare la relativa proprietà `Text`. Questa etichetta verrà usata per fornire commenti e suggerimenti sulle azioni eseguite, visualizzando messaggi quali "l'utente Tito è stato aggiunto al ruolo di amministratore" o "l'utente Jisun è stato rimosso dal ruolo Supervisore". Per far emergere questi messaggi, impostare la proprietà `CssClass` dell'etichetta su "important".

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample1.aspx)]

Successivamente, aggiungere la seguente definizione di classe CSS al foglio di stile `Styles.css`:

[!code-css[Main](assigning-roles-to-users-vb/samples/sample2.css)]

Questa definizione CSS indica al browser di visualizzare l'etichetta usando un tipo di carattere rosso grande. La figura 1 Mostra questo effetto tramite la finestra di progettazione di Visual Studio.

[![la proprietà CssClass dell'etichetta restituisce un tipo di carattere rosso grande](assigning-roles-to-users-vb/_static/image2.png)](assigning-roles-to-users-vb/_static/image1.png)

**Figura 1**: la proprietà `CssClass` dell'etichetta restituisce un carattere di colore rosso grande ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image3.png))

Successivamente, aggiungere un oggetto DropDownList alla pagina, impostare la relativa proprietà `ID` su `UserList`e impostare la relativa proprietà `AutoPostBack` su true. Questo DropDownList verrà usato per elencare tutti gli utenti del sistema. Questo DropDownList verrà associato a una raccolta di oggetti MembershipUser. Poiché si desidera che la DropDownList visualizzi la proprietà UserName dell'oggetto MembershipUser (e la utilizzi come valore degli elementi dell'elenco), impostare le proprietà `DataTextField` e `DataValueField` di DropDownList su "UserName".

Sotto il DropDownList aggiungere un Repeater denominato `UsersRoleList`. Questo Repeater elenca tutti i ruoli del sistema sotto forma di una serie di caselle di controllo. Definire la `ItemTemplate` del Repeater usando il markup dichiarativo seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample3.aspx)]

Il markup `ItemTemplate` include un singolo controllo Web CheckBox denominato `RoleCheckBox`. La proprietà `AutoPostBack` della casella di controllo è impostata su true e la proprietà `Text` è associata a `Container.DataItem`. Il motivo della sintassi di DataBinding è semplicemente `Container.DataItem` perché il Framework dei ruoli restituisce l'elenco di nomi di ruoli come matrice di stringhe ed è questa matrice di stringhe che verrà associato al ripetitore. Una descrizione dettagliata del motivo per cui questa sintassi viene utilizzata per visualizzare il contenuto di una matrice associata a un controllo Web dati esula dall'ambito di questa esercitazione. Per ulteriori informazioni su questo argomento, vedere [associazione di una matrice scalare a un controllo Web di dati](http://aspnet.4guysfromrolla.com/articles/082504-1.aspx).

A questo punto il markup dichiarativo dell'interfaccia "by User" dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample4.aspx)]

A questo punto è possibile scrivere il codice per associare il set di account utente al DropDownList e il set di ruoli al Repeater. Nella classe code-behind della pagina aggiungere un metodo denominato `BindUsersToUserList` e un altro `BindRolesList`denominato, usando il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample5.vb)]

Il metodo `BindUsersToUserList` recupera tutti gli account utente nel sistema tramite il [metodo`Membership.GetAllUsers`](https://msdn.microsoft.com/library/dy8swhya.aspx). Viene restituito un [oggetto`MembershipUserCollection`](https://msdn.microsoft.com/library/system.web.security.membershipusercollection.aspx), ovvero una raccolta di [istanze di`MembershipUser`](https://msdn.microsoft.com/library/system.web.security.membershipuser.aspx). Questa raccolta viene quindi associata al `UserList` DropDownList. Le istanze `MembershipUser` che comportano il makeup della raccolta contengono diverse proprietà, ad esempio `UserName`, `Email`, `CreationDate`e `IsOnline`. Per indicare al DropDownList di visualizzare il valore della proprietà `UserName`, assicurarsi che le proprietà `DataTextField` e `DataValueField` del `UserList` DropDownList siano state impostate su "UserName".

> [!NOTE]
> Il metodo `Membership.GetAllUsers` dispone di due overload: uno che non accetta parametri di input e restituisce tutti gli utenti e uno che accetta valori integer per l'indice e le dimensioni della pagina e restituisce solo il subset specificato di utenti. Quando sono presenti grandi quantità di account utente visualizzati in un elemento dell'interfaccia utente di cui è possibile eseguire il paging, il secondo overload può essere usato per passare in modo più efficiente alle pagine degli utenti, perché restituisce solo il subset preciso di account utente anziché tutti.

Il metodo `BindRolesToList` inizia chiamando il [metodo`GetAllRoles`](https://msdn.microsoft.com/library/system.web.security.roles.getallroles.aspx)della classe `Roles`, che restituisce una matrice di stringhe contenente i ruoli nel sistema. Questa matrice di stringhe viene quindi associata al Repeater.

Infine, è necessario chiamare questi due metodi quando la pagina viene caricata per la prima volta. Aggiungere il codice seguente al gestore eventi `Page_Load` :

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample6.vb)]

Con questo codice, dedicare un po' di tempo alla visita della pagina tramite un browser. la schermata dovrebbe essere simile alla figura 2. Tutti gli account utente vengono popolati nell'elenco a discesa e, sotto questo, ogni ruolo viene visualizzato come casella di controllo. Poiché si impostano le proprietà `AutoPostBack` di DropDownList e le caselle di controllo su true, la modifica dell'utente selezionato o la verifica o la deselezione di un ruolo causa un postback. Tuttavia, non viene eseguita alcuna azione, perché è ancora necessario scrivere codice per gestire queste azioni. Queste attività verranno affrontate nelle due sezioni successive.

[![la pagina Visualizza gli utenti e i ruoli](assigning-roles-to-users-vb/_static/image5.png)](assigning-roles-to-users-vb/_static/image4.png)

**Figura 2**: la pagina Visualizza gli utenti e i ruoli ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image6.png))

### <a name="checking-the-roles-the-selected-user-belongs-to"></a>Verifica dei ruoli a cui appartiene l'utente selezionato

Quando la pagina viene caricata per la prima volta o ogni volta che il visitatore seleziona un nuovo utente dall'elenco a discesa, è necessario aggiornare le caselle di controllo della `UsersRoleList`in modo che la casella di controllo del ruolo specificato venga controllata solo se l'utente selezionato appartiene a tale ruolo. A tale scopo, creare un metodo denominato `CheckRolesForSelectedUser` con il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample7.vb)]

Il codice precedente inizia determinando l'utente selezionato. USA quindi il [metodo`GetRolesForUser(userName)`](https://msdn.microsoft.com/library/system.web.security.roles.getrolesforuser.aspx) della classe Roles per restituire il set di ruoli dell'utente specificato come matrice di stringhe. Vengono quindi enumerati gli elementi del Repeater e viene fatto riferimento a a livello di codice a ogni elemento della casella di controllo `RoleCheckBox`. La casella di controllo viene controllata solo se il ruolo a cui corrisponde è contenuto all'interno della matrice di stringhe `selectedUsersRoles`.

> [!NOTE]
> Se si usa ASP.NET versione 2,0, la sintassi del `Linq.Enumerable.Contains(Of String)(...)` non verrà compilata. Il `Contains(Of String)` metodo fa parte della [libreria LINQ](http://en.wikipedia.org/wiki/Language_Integrated_Query), che è una novità di ASP.NET 3,5. Se si sta ancora usando ASP.NET versione 2,0, usare invece il [metodo`Array.IndexOf(Of String)`](https://msdn.microsoft.com/library/eha9t187.aspx) .

È necessario chiamare il metodo `CheckRolesForSelectedUser` in due casi: quando la pagina viene caricata per la prima volta e ogni volta che viene modificato l'indice selezionato del `UserList` DropDownList. Chiamare pertanto questo metodo dal gestore dell'evento `Page_Load` (dopo le chiamate a `BindUsersToUserList` e `BindRolesToList`). Inoltre, creare un gestore eventi per l'evento `SelectedIndexChanged` di DropDownList e chiamare questo metodo da questa posizione.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample8.vb)]

Con questo codice, è possibile eseguire il test della pagina tramite il browser. Tuttavia, poiché la pagina `UsersAndRoles.aspx` attualmente non dispone della possibilità di assegnare utenti ai ruoli, nessun utente dispone di ruoli. Verrà creata l'interfaccia per l'assegnazione di utenti ai ruoli in un determinato momento, pertanto è possibile utilizzare la parola che il codice funziona e verificare che venga eseguito in un secondo momento oppure è possibile aggiungere manualmente utenti ai ruoli inserendo i record nella tabella `aspnet_UsersInRoles` per testare subito questa funzionalità.

### <a name="assigning-and-removing-users-from-roles"></a>Assegnazione e rimozione di utenti dai ruoli

Quando il visitatore controlla o deseleziona una casella di controllo in `UsersRoleList` Repeater è necessario aggiungere o rimuovere l'utente selezionato dal ruolo corrispondente. La proprietà `AutoPostBack` della casella di controllo è attualmente impostata su true, causando un postback ogni volta che una casella di controllo nel ripetitore viene selezionata o deselezionata. In breve, è necessario creare un gestore eventi per l'evento `CheckChanged` della casella di controllo. Poiché la casella di controllo si trova in un controllo Repeater, è necessario aggiungere manualmente il plumbing del gestore eventi. Per iniziare, aggiungere il gestore eventi alla classe code-behind come metodo di `Protected`, come indicato di seguito:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample9.vb)]

Si tornerà a scrivere il codice per questo gestore eventi in un determinato momento. Tuttavia, prima di tutto è necessario completare il plumbing di gestione degli eventi. Dalla casella di controllo all'interno del `ItemTemplate`del Repeater aggiungere `OnCheckedChanged="RoleCheckBox_CheckChanged"`. Questa sintassi consente di collegare il gestore dell'evento `RoleCheckBox_CheckChanged` all'evento `CheckedChanged` di `RoleCheckBox`.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample10.aspx)]

L'attività finale consiste nel completare il gestore dell'evento `RoleCheckBox_CheckChanged`. È necessario iniziare facendo riferimento al controllo CheckBox che ha generato l'evento perché questa istanza di CheckBox indica il ruolo selezionato o deselezionato tramite le proprietà `Text` e `Checked`. Usando queste informazioni insieme al nome utente dell'utente selezionato, l'utente viene aggiunto o rimosso dal ruolo tramite il metodo [`AddUserToRole`](https://msdn.microsoft.com/library/system.web.security.roles.addusertorole.aspx) o [`RemoveUserFromRole`](https://msdn.microsoft.com/library/system.web.security.roles.removeuserfromrole.aspx)della classe `Roles`.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample11.vb)]

Il codice precedente inizia con un riferimento a livello di codice alla casella di controllo che ha generato l'evento, disponibile tramite il parametro di input `sender`. Se la casella di controllo è selezionata, l'utente selezionato viene aggiunto al ruolo specificato; in caso contrario, viene rimosso dal ruolo. In entrambi i casi, nell'etichetta `ActionStatus` viene visualizzato un messaggio in cui viene riepilogata l'azione appena eseguita.

È possibile eseguire il test di questa pagina tramite un browser. Selezionare utente Tito, quindi aggiungere Tito ai ruoli amministratori e supervisori.

[![Tito è stato aggiunto ai ruoli amministratori e supervisori](assigning-roles-to-users-vb/_static/image8.png)](assigning-roles-to-users-vb/_static/image7.png)

**Figura 3**: Tito è stato aggiunto ai ruoli Administrators e Supervisors ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image9.png))

Selezionare quindi utente Bruce dall'elenco a discesa. Viene eseguito un postback e le caselle di controllo del Repeater vengono aggiornate tramite il `CheckRolesForSelectedUser`. Poiché Bruce non appartiene ancora ad alcun ruolo, le due caselle di controllo sono deselezionate. Aggiungere quindi Bruce al ruolo supervisori.

[![Bruce è stato aggiunto al ruolo supervisori](assigning-roles-to-users-vb/_static/image11.png)](assigning-roles-to-users-vb/_static/image10.png)

**Figura 4**: Bruce è stato aggiunto al ruolo supervisori ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image12.png))

Per verificare ulteriormente la funzionalità del metodo di `CheckRolesForSelectedUser`, selezionare un utente diverso da Tito o Bruce. Si noti che le caselle di controllo vengono deselezionate automaticamente, indicando che non appartengono ad alcun ruolo. Tornare a Tito. È necessario selezionare le caselle di controllo amministratori e supervisori.

## <a name="step-2-building-the-by-roles-user-interface"></a>Passaggio 2: compilazione dell'interfaccia utente "per ruoli"

A questo punto è stata completata l'interfaccia "by users" e si è pronti per iniziare a affrontare l'interfaccia "by Roles". L'interfaccia "per ruoli" richiede all'utente di selezionare un ruolo da un elenco a discesa e quindi Visualizza il set di utenti che appartengono a tale ruolo in GridView.

Aggiungere un altro controllo DropDownList al `UsersAndRoles.aspx page`. Inserire questo oggetto sotto il controllo Repeater, denominarlo `RoleList`e impostare la relativa proprietà `AutoPostBack` su true. Al di sotto, aggiungere un controllo GridView e denominarlo `RolesUserList`. In questo GridView vengono elencati gli utenti che appartengono al ruolo selezionato. Impostare la proprietà `AutoGenerateColumns` di GridView su false, aggiungere un TemplateField alla raccolta `Columns` della griglia e impostare la relativa proprietà `HeaderText` su "Users". Definire il `ItemTemplate` di TemplateField in modo che visualizzi il valore dell'espressione DataBinding `Container.DataItem` nella proprietà `Text` di un'etichetta denominata `UserNameLabel`.

Dopo l'aggiunta e la configurazione di GridView, il markup dichiarativo dell'interfaccia "by Role" dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample12.aspx)]

È necessario popolare il `RoleList` DropDownList con il set di ruoli nel sistema. A tale scopo, aggiornare il metodo `BindRolesToList` in modo che associ la matrice di stringhe restituita dal metodo `Roles.GetAllRoles` al `RolesList` DropDownList (e `UsersRoleList` Repeater).

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample13.vb)]

Le ultime due righe nel metodo `BindRolesToList` sono state aggiunte per associare il set di ruoli al controllo `RoleList` DropDownList. La figura 5 Mostra il risultato finale quando viene visualizzato tramite un browser, ovvero un elenco a discesa popolato con i ruoli del sistema.

[![i ruoli vengono visualizzati nell'oggetto DropDownList del ruolo](assigning-roles-to-users-vb/_static/image14.png)](assigning-roles-to-users-vb/_static/image13.png)

**Figura 5**: i ruoli vengono visualizzati nel `RoleList` DropDownList ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image15.png))

### <a name="displaying-the-users-that-belong-to-the-selected-role"></a>Visualizzazione degli utenti che appartengono al ruolo selezionato

Quando la pagina viene caricata per la prima volta o quando viene selezionato un nuovo ruolo dal `RoleList` DropDownList, è necessario visualizzare l'elenco di utenti che appartengono a tale ruolo in GridView. Creare un metodo denominato `DisplayUsersBelongingToRole` usando il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample14.vb)]

Questo metodo inizia ottenendo il ruolo selezionato dal `RoleList` DropDownList. USA quindi il [metodo`Roles.GetUsersInRole(roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.getusersinrole.aspx) per recuperare una matrice di stringhe dei nomi utente degli utenti che appartengono a tale ruolo. Questa matrice viene quindi associata al `RolesUserList` GridView.

Questo metodo deve essere chiamato in due circostanze: quando la pagina viene caricata inizialmente e quando viene modificato il ruolo selezionato nell'`RoleList` DropDownList. Aggiornare quindi il gestore dell'evento `Page_Load` in modo che questo metodo venga richiamato dopo la chiamata a `CheckRolesForSelectedUser`. Successivamente, creare un gestore eventi per l'evento `SelectedIndexChanged` del `RoleList`e chiamare anche questo metodo.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample15.vb)]

Con questo codice, il `RolesUserList` GridView dovrebbe visualizzare gli utenti che appartengono al ruolo selezionato. Come illustrato nella figura 6, il ruolo supervisori è costituito da due membri: Bruce e Tito.

[![GridView elenca gli utenti che appartengono al ruolo selezionato](assigning-roles-to-users-vb/_static/image17.png)](assigning-roles-to-users-vb/_static/image16.png)

**Figura 6**: GridView elenca gli utenti che appartengono al ruolo selezionato ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image18.png))

### <a name="removing-users-from-the-selected-role"></a>Rimozione degli utenti dal ruolo selezionato

Verrà ora aumentata la `RolesUserList` GridView in modo che includa una colonna di pulsanti "Rimuovi". Se si fa clic sul pulsante "Rimuovi" per un determinato utente, questi vengono rimossi da tale ruolo.

Per iniziare, aggiungere un campo pulsante Elimina al controllo GridView. Fare in modo che questo campo appaia come il più a sinistra e modificare la proprietà `DeleteText` da "Delete" (impostazione predefinita) a "Remove".

[![aggiungere il](assigning-roles-to-users-vb/_static/image20.png)](assigning-roles-to-users-vb/_static/image19.png)

**Figura 7**: aggiungere il pulsante "Rimuovi" al GridView ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image21.png))

Quando si fa clic sul pulsante "Rimuovi", viene restituito un postback e viene generato l'evento `RowDeleting` di GridView. È necessario creare un gestore eventi per questo evento e scrivere codice per rimuovere l'utente dal ruolo selezionato. Creare il gestore eventi e aggiungere il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample16.vb)]

Il codice inizia determinando il nome del ruolo selezionato. Quindi fa riferimento a livello di codice al controllo `UserNameLabel` dalla riga di cui è stato fatto clic sul pulsante "Rimuovi" per determinare il nome utente dell'utente da rimuovere. L'utente viene quindi rimosso dal ruolo tramite una chiamata al metodo `Roles.RemoveUserFromRole`. Il `RolesUserList` GridView viene quindi aggiornato e viene visualizzato un messaggio tramite il controllo `ActionStatus` Label.

> [!NOTE]
> Il pulsante "Rimuovi" non richiede alcun tipo di conferma da parte dell'utente prima di rimuovere l'utente dal ruolo. Invito ad aggiungere un certo livello di conferma dell'utente. Uno dei modi più semplici per confermare un'azione è tramite una finestra di dialogo di conferma lato client. Per ulteriori informazioni su questa tecnica, vedere [aggiunta di una conferma lato client durante l'eliminazione](https://asp.net/learn/data-access/tutorial-42-vb.aspx).

Nella figura 8 viene illustrata la pagina dopo la rimozione dell'utente Tito dal gruppo Supervisors.

[![ahimè, Tito non è più un supervisore](assigning-roles-to-users-vb/_static/image23.png)](assigning-roles-to-users-vb/_static/image22.png)

**Figura 8**: Ahimè, Tito non è più un supervisore ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image24.png))

### <a name="adding-new-users-to-the-selected-role"></a>Aggiunta di nuovi utenti al ruolo selezionato

Insieme alla rimozione di utenti dal ruolo selezionato, anche il visitatore di questa pagina deve essere in grado di aggiungere un utente al ruolo selezionato. La migliore interfaccia per l'aggiunta di un utente al ruolo selezionato dipende dal numero di account utente previsti. Se il sito Web ospita solo alcune dozzine di account utente o meno, è possibile usare un oggetto DropDownList qui. Se potrebbero essere presenti migliaia di account utente, è opportuno includere un'interfaccia utente che consenta al visitatore di spostarsi tra gli account, cercare un account specifico o filtrare gli account utente in un altro modo.

Per questa pagina, si userà un'interfaccia molto semplice che funziona indipendentemente dal numero di account utente nel sistema. In particolare, verrà usata una casella di testo che richiede al visitatore di digitare il nome utente dell'utente che desidera aggiungere al ruolo selezionato. Se non esiste alcun utente con lo stesso nome o se l'utente è già un membro del ruolo, verrà visualizzato un messaggio nell'etichetta `ActionStatus`. Tuttavia, se l'utente esiste e non è un membro del ruolo, verranno aggiunti al ruolo e verrà aggiornata la griglia.

Aggiungere una casella di testo e un pulsante sotto GridView. Impostare la `ID` della casella di testo su `UserNameToAddToRole` e impostare le proprietà `ID` e `Text` del pulsante su `AddUserToRoleButton` e "Aggiungi utente al ruolo", rispettivamente.

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample17.aspx)]

Successivamente, creare un gestore dell'evento `Click` per il `AddUserToRoleButton` e aggiungere il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample18.vb)]

La maggior parte del codice nel gestore dell'evento `Click` esegue diversi controlli di convalida. Garantisce che il visitatore abbia fornito un nome utente nella casella di testo `UserNameToAddToRole`, che l'utente esista nel sistema e che non appartenga già al ruolo selezionato. Se uno di questi controlli ha esito negativo, viene visualizzato un messaggio appropriato in `ActionStatus` e il gestore eventi viene terminato. Se tutti i controlli vengono superati, l'utente viene aggiunto al ruolo tramite il metodo `Roles.AddUserToRole`. In seguito, la proprietà `Text` della casella di testo viene cancellata, il GridView viene aggiornato e l'etichetta `ActionStatus` Visualizza un messaggio che indica che l'utente specificato è stato aggiunto correttamente al ruolo selezionato.

> [!NOTE]
> Per assicurarsi che l'utente specificato non appartenga già al ruolo selezionato, viene usato il [metodo`Roles.IsUserInRole(userName, roleName)`](https://msdn.microsoft.com/library/system.web.security.roles.isuserinrole.aspx), che restituisce un valore booleano che indica se *username* è un membro di *roleName*. Questo metodo verrà usato nuovamente nell' <a id="_msoanchor_2"> </a> [esercitazione successiva](role-based-authorization-vb.md) quando si esamina l'autorizzazione basata sui ruoli.

Visitare la pagina tramite un browser e selezionare il ruolo supervisori dal `RoleList` DropDownList. Provare a immettere un nome utente non valido. verrà visualizzato un messaggio che informa che l'utente non esiste nel sistema.

[![non è possibile aggiungere un utente non esistente a un ruolo](assigning-roles-to-users-vb/_static/image26.png)](assigning-roles-to-users-vb/_static/image25.png)

**Figura 9**: non è possibile aggiungere un utente non esistente a un ruolo ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image27.png))

A questo punto, provare ad aggiungere un utente valido. Procedere e aggiungere nuovamente Tito al ruolo supervisori.

[![Tito è ancora una volta un supervisore.](assigning-roles-to-users-vb/_static/image29.png)](assigning-roles-to-users-vb/_static/image28.png)

**Figura 10**: Tito è ancora una volta un supervisore.  ([Fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image30.png))

## <a name="step-3-cross-updating-the-by-user-and-by-role-interfaces"></a>Passaggio 3: aggiornamento incrociato delle interfacce "per utente" e "per ruolo"

La pagina `UsersAndRoles.aspx` offre due interfacce distinte per la gestione di utenti e ruoli. Attualmente, queste due interfacce agiscono in modo indipendente l'una dall'altra, quindi è possibile che una modifica apportata in un'interfaccia non venga riflessa immediatamente nell'altra. Si supponga, ad esempio, che il visitatore della pagina selezioni il ruolo Supervisore dalla `RoleList` DropDownList, che elenca Bruce e Tito come membri. Successivamente, il visitatore seleziona Tito dal `UserList` DropDownList, che controlla le caselle di controllo amministratori e supervisori nel `UsersRoleList` Repeater. Se il visitatore deseleziona il ruolo di supervisore dal Repeater, Tito viene rimosso dal ruolo supervisori, ma questa modifica non viene riflessa nell'interfaccia "per ruolo". Il controllo GridView mostrerà ancora Tito come membro del ruolo supervisori.

Per risolvere questo problema, è necessario aggiornare GridView ogni volta che un ruolo viene selezionato o deselezionato dal ripetitore `UsersRoleList`. Analogamente, è necessario aggiornare il ripetitore ogni volta che un utente viene rimosso o aggiunto a un ruolo dall'interfaccia "by Role".

Il ripetitore nell'interfaccia "da utente" viene aggiornato chiamando il metodo `CheckRolesForSelectedUser`. L'interfaccia "per ruolo" può essere modificata nel gestore dell'evento `RolesUserList` GridView `RowDeleting` e nel gestore dell'evento `Click` del pulsante `AddUserToRoleButton`. Pertanto, è necessario chiamare il metodo `CheckRolesForSelectedUser` da ognuno di questi metodi.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample19.vb)]

Analogamente, GridView nell'interfaccia "by Role" viene aggiornato chiamando il metodo `DisplayUsersBelongingToRole` e l'interfaccia "by User" viene modificata tramite il gestore dell'evento `RoleCheckBox_CheckChanged`. Pertanto, è necessario chiamare il metodo `DisplayUsersBelongingToRole` da questo gestore eventi.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample20.vb)]

Con queste modifiche minime al codice, le interfacce "per utente" e "per ruolo" ora vengono correttamente incrociate. Per verificarlo, visitare la pagina tramite un browser e selezionare Tito e Supervisors rispettivamente dal `UserList` e `RoleList` DropDownList. Si noti che quando si deseleziona il ruolo Supervisors per Tito dal Repeater nell'interfaccia "by User", Tito viene rimosso automaticamente da GridView nell'interfaccia "by Role". L'aggiunta di Tito al ruolo Supervisore dall'interfaccia "per ruolo" Ricontrolla automaticamente la casella di controllo supervisori nell'interfaccia "da utente".

## <a name="step-4-customizing-the-createuserwizard-to-include-a-specify-roles-step"></a>Passaggio 4: personalizzazione di CreateUserWizard per includere un passaggio "specifica ruoli"

<a id="_msoanchor_3"> </a>Nell'esercitazione [*creazione di account utente*](../membership/creating-user-accounts-vb.md) è stato illustrato come utilizzare il controllo Web CreateUserWizard per fornire un'interfaccia per la creazione di un nuovo account utente. Il controllo CreateUserWizard può essere utilizzato in uno dei due modi seguenti:

- Come mezzo per i visitatori per creare il proprio account utente nel sito e
- Come mezzo per gli amministratori per la creazione di nuovi account

Nel primo caso di utilizzo, un visitatore si avvicina al sito e compila l'oggetto CreateUserWizard, immettendo le informazioni per la registrazione sul sito. Nel secondo caso, un amministratore crea un nuovo account per un altro utente.

Quando un account viene creato da un amministratore per un altro utente, può essere utile consentire all'amministratore di specificare a quali ruoli appartiene il nuovo account utente. Nell'esercitazione sull' [ *archiviazione* di *informazioni utente aggiuntive* ](../membership/storing-additional-user-information-vb.md) è stato illustrato come personalizzare l'oggetto CreateUserWizard aggiungendo ulteriori `WizardSteps`. <a id="_msoanchor_4"> </a> Viene ora illustrato come aggiungere un passaggio aggiuntivo a CreateUserWizard per specificare i ruoli del nuovo utente.

Aprire la pagina `CreateUserWizardWithRoles.aspx` e aggiungere un controllo CreateUserWizard denominato `RegisterUserWithRoles`. Impostare la proprietà `ContinueDestinationPageUrl` del controllo su "~/default.aspx". Poiché l'idea è che un amministratore utilizzerà questo controllo CreateUserWizard per creare nuovi account utente, impostare la proprietà `LoginCreatedUser` del controllo su false. Questa proprietà `LoginCreatedUser` specifica se il visitatore viene automaticamente connesso come utente appena creato e il valore predefinito è true. Il valore è impostato su false, perché quando un amministratore crea un nuovo account si vuole mantenerlo connesso come se stesso.

Selezionare quindi "Aggiungi/Rimuovi `WizardSteps`..." dallo smart tag di CreateUserWizard e aggiungere una nuova `WizardStep`, impostando la relativa `ID` su `SpecifyRolesStep`. Spostare il `SpecifyRolesStep WizardStep` in modo che venga eseguito dopo il passaggio "iscrizione al nuovo account", ma prima del passaggio "completo". Impostare la proprietà `Title` di `WizardStep`su "specifica ruoli", la relativa proprietà `StepType` su `Step`e la relativa proprietà `AllowReturn` su false.

[![aggiungere il](assigning-roles-to-users-vb/_static/image32.png)](assigning-roles-to-users-vb/_static/image31.png)

**Figura 11**: aggiungere il `WizardStep` "specificare i ruoli" a CreateUserWizard ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image33.png))

Dopo questa modifica, il markup dichiarativo di CreateUserWizard dovrebbe essere simile al seguente:

[!code-aspx[Main](assigning-roles-to-users-vb/samples/sample21.aspx)]

Nella `WizardStep`"specificare i ruoli" aggiungere un oggetto CheckBoxList denominato `RoleList.` questo oggetto CheckBoxList elenca i ruoli disponibili, abilitando la persona che visita la pagina per verificare i ruoli a cui appartiene l'utente appena creato.

Si rimane con due attività di codifica: prima di tutto è necessario popolare il `RoleList` CheckBoxList con i ruoli del sistema; in secondo luogo, è necessario aggiungere l'utente creato ai ruoli selezionati quando l'utente passa dal passaggio "specifica ruoli" al passaggio "completato". È possibile eseguire la prima attività nel gestore dell'evento `Page_Load`. Il codice seguente fa riferimento a livello di codice alla casella di controllo `RoleList` alla prima visita alla pagina e associa i ruoli nel sistema.

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample22.vb)]

Il codice precedente dovrebbe avere un aspetto familiare. Nell'esercitazione <a id="_msoanchor_5"> </a> [ *archiviazione* di *informazioni aggiuntive sull'utente* ](../membership/storing-additional-user-information-vb.md) sono state usate due istruzioni `FindControl` per fare riferimento a un controllo Web dall'interno di un `WizardStep`personalizzato. Il codice che associa i ruoli al CheckBoxList è stato ricavato precedentemente in questa esercitazione.

Per eseguire la seconda attività di programmazione, è necessario stabilire quando è stato completato il passaggio "specifica ruoli". Tenere presente che CreateUserWizard ha un evento `ActiveStepChanged`, che viene attivato ogni volta che il visitatore passa da un passaggio a un altro. Qui è possibile determinare se l'utente ha raggiunto il passaggio "completo"; in tal caso, è necessario aggiungere l'utente ai ruoli selezionati.

Creare un gestore eventi per l'evento `ActiveStepChanged` e aggiungere il codice seguente:

[!code-vb[Main](assigning-roles-to-users-vb/samples/sample23.vb)]

Se l'utente ha appena raggiunto il passaggio "completato", il gestore eventi enumera gli elementi del `RoleList` CheckBoxList e l'utente appena creato viene assegnato ai ruoli selezionati.

Visitare questa pagina tramite un browser. Il primo passaggio di CreateUserWizard è il passaggio standard "iscrizione al nuovo account", che richiede il nome utente, la password, la posta elettronica e altre informazioni chiave del nuovo utente. Immettere le informazioni per creare un nuovo utente denominato Wanda.

[![creare un nuovo utente denominato Wanda](assigning-roles-to-users-vb/_static/image35.png)](assigning-roles-to-users-vb/_static/image34.png)

**Figura 12**: creare un nuovo utente denominato Wanda ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image36.png))

Fare clic sul pulsante "Crea utente". L'oggetto CreateUserWizard chiama internamente il metodo `Membership.CreateUser`, creando il nuovo account utente e quindi procede al passaggio successivo, "specifica ruoli". Qui vengono elencati i ruoli di sistema. Controllare la casella di controllo supervisori e fare clic su Avanti.

[![rendere Wanda un membro del ruolo supervisori](assigning-roles-to-users-vb/_static/image38.png)](assigning-roles-to-users-vb/_static/image37.png)

**Figura 13**: rendere Wanda un membro del ruolo supervisori ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image39.png))

Se si fa clic su Next, viene attivato un postback e viene aggiornato il `ActiveStep` al passaggio "complete". Nel gestore dell'evento `ActiveStepChanged`, l'account utente creato di recente viene assegnato al ruolo Supervisore. Per verificarlo, tornare alla pagina `UsersAndRoles.aspx` e selezionare supervisori dall'`RoleList` DropDownList. Come illustrato nella figura 14, i supervisori sono ora costituiti da tre utenti: Bruce, Tito e Wanda.

[![Bruce, Tito e Wanda sono tutti i supervisori](assigning-roles-to-users-vb/_static/image41.png)](assigning-roles-to-users-vb/_static/image40.png)

**Figura 14**: Bruce, Tito e Wanda sono tutti i supervisori ([fare clic per visualizzare l'immagine con dimensioni complete](assigning-roles-to-users-vb/_static/image42.png))

## <a name="summary"></a>Riepilogo

Il Framework dei ruoli offre metodi per il recupero di informazioni sui ruoli e i metodi di un utente specifico per determinare gli utenti che appartengono a un ruolo specificato. Sono inoltre disponibili diversi metodi per l'aggiunta e la rimozione di uno o più utenti in uno o più ruoli. In questa esercitazione sono stati concentrati solo due di questi metodi: `AddUserToRole` e `RemoveUserFromRole`. Sono disponibili varianti aggiuntive progettate per aggiungere più utenti a un singolo ruolo e per assegnare più ruoli a un singolo utente.

Questa esercitazione include anche un'estensione del controllo CreateUserWizard per includere un `WizardStep` per specificare i ruoli utente appena creati. Questo passaggio potrebbe consentire a un amministratore di semplificare il processo di creazione di account utente per i nuovi utenti.

A questo punto è stato illustrato come creare ed eliminare ruoli e come aggiungere e rimuovere utenti dai ruoli. Tuttavia è ancora necessario esaminare l'applicazione dell'autorizzazione basata sui ruoli. <a id="_msoanchor_6"> </a>Nell' [esercitazione seguente](role-based-authorization-vb.md) si esaminerà la definizione delle regole di autorizzazione degli URL in base al ruolo, nonché come limitare la funzionalità a livello di pagina in base ai ruoli dell'utente attualmente connesso.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Panoramica dello strumento Amministrazione sito Web di ASP.NET](https://msdn.microsoft.com/library/ms228053.aspx)
- [Esame di ASP. Appartenenza, ruoli e profilo di NET](http://aspnet.4guysfromrolla.com/articles/120705-1.aspx)
- [Implementazione dello strumento di amministrazione del sito Web](http://aspnet.4guysfromrolla.com/articles/052307-1.aspx)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Ringraziamento speciale a...

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione era Teresa Murphy. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](creating-and-managing-roles-vb.md)
> [Successivo](role-based-authorization-vb.md)
