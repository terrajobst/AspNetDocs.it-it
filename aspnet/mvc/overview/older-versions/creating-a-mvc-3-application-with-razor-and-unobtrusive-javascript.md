---
uid: mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
title: Creazione di un'applicazione MVC 3 con Razor e JavaScript non intrusivo | Microsoft Docs
author: microsoft
description: L'applicazione Web di esempio elenco utenti dimostra quanto sia semplice creare applicazioni ASP.NET MVC 3 usando il motore di visualizzazione Razor. L'applicazione di esempio s...
ms.author: riande
ms.date: 11/01/2010
ms.assetid: 658b149b-d770-46bf-8b4b-4e47cca242f3
msc.legacyurl: /mvc/overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript
msc.type: authoredcontent
ms.openlocfilehash: fb63493ff22c9261fc5746a998a32f2511141f87
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78540985"
---
# <a name="creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript"></a>Creazione di un'applicazione MVC 3 con Razor e JavaScript discreto

[Microsoft](https://github.com/microsoft)

> L'applicazione Web di esempio elenco utenti dimostra quanto sia semplice creare applicazioni ASP.NET MVC 3 usando il motore di visualizzazione Razor. L'applicazione di esempio Mostra come usare il nuovo motore di visualizzazione Razor con ASP.NET MVC versione 3 e Visual Studio 2010 per creare un sito Web di elenco utenti fittizio che include funzionalità quali la creazione, la visualizzazione, la modifica e l'eliminazione di utenti.
> 
> Questa esercitazione descrive i passaggi eseguiti per compilare l'applicazione di esempio ASP.NET MVC 3 dell'elenco utenti. Un progetto di Visual Studio C# con e codice sorgente VB è disponibile a questo argomento: [download](https://code.msdn.microsoft.com/aspnetmvcsamples/Release/ProjectReleases.aspx?ReleaseId=5114). In caso di domande su questa esercitazione, pubblicarle nel forum di [MVC](https://forums.asp.net/1146.aspx).

## <a name="overview"></a>Panoramica

L'applicazione che verrà compilata è un semplice sito Web di elenco utenti. Gli utenti possono immettere, visualizzare e aggiornare le informazioni utente.

![Sito di esempio](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image1.png)

È possibile scaricare il progetto VB C# e il progetto completato [qui](https://code.msdn.microsoft.com/Creating-a-MVC-3-28883c0f).

## <a name="creating-the-web-application"></a>Creazione dell'applicazione Web

Per avviare l'esercitazione, aprire Visual Studio 2010 e creare un nuovo progetto usando il modello di *applicazione Web MVC 3 ASP.NET* . Assegnare un nome all'applicazione &quot;&quot;Mvc3Razor.

[![nuovo progetto MVC 3](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image3.png)](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image2.png)

Nella finestra di dialogo **nuovo progetto ASP.NET MVC 3** selezionare **applicazione Internet**, selezionare il motore di visualizzazione Razor, quindi fare clic su **OK**.

![Finestra di dialogo nuovo progetto MVC 3 ASP.NET](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image4.png)

In questa esercitazione non verrà usato il provider di appartenenze di ASP.NET, pertanto è possibile eliminare tutti i file associati all'accesso e all'appartenenza. In **Esplora soluzioni**rimuovere i file e le directory seguenti:

- *Controllers\AccountController*
- *Models\AccountModels*
- *Views\Shared\\_LogOnPartial*
- *Views\Account* (e tutti i file in questa directory)

![Soln exp](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image5.png)

Modificare il file <em>\_layout. cshtml</em> e sostituire il markup all'interno dell'elemento `<div>` denominato `logindisplay` con il messaggio <em>&quot;</em>login disabilitato&quot;. Nell'esempio seguente viene illustrato il nuovo markup:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample1.cshtml)]

## <a name="adding-the-model"></a>Aggiunta del modello

In **Esplora soluzioni**fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi fare clic su **classe**.

![Nuova classe MDL utente](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image6.png)

Assegnare alla classe il nome `UserModel`. Sostituire il contenuto del file *UserModel* con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample2.cs)]

La classe `UserModel` rappresenta gli utenti. Ogni membro della classe viene annotato con l'attributo [obbligatorio](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) dello spazio dei nomi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) . Gli attributi nello spazio dei nomi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) forniscono la convalida automatica lato client e lato server per le applicazioni Web.

Aprire la classe `HomeController` e aggiungere una direttiva `using` in modo che sia possibile accedere alle classi `UserModel` e `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample3.cs)]

Subito dopo la dichiarazione di `HomeController` aggiungere il commento seguente e il riferimento a una classe `Users`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample4.cs)]

La classe `Users` è un archivio dati semplificato in memoria che verrà usato in questa esercitazione. In un'applicazione reale è possibile utilizzare un database per archiviare le informazioni sull'utente. Nell'esempio seguente vengono illustrate le prime righe del file `HomeController`:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample5.cs)]

Compilare l'applicazione in modo che il modello utente sarà disponibile per l'impalcatura guidata nel passaggio successivo.

## <a name="creating-the-default-view"></a>Creazione della visualizzazione predefinita

Il passaggio successivo consiste nell'aggiungere un metodo di azione e una visualizzazione per visualizzare gli utenti.

Eliminare il file *Views\Home\Index* esistente. Si creerà un nuovo file di *Indice* per visualizzare gli utenti.

Nella classe `HomeController` sostituire il contenuto del metodo `Index` con il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample6.cs)]

Fare clic con il pulsante destro del mouse all'interno del metodo `Index`, quindi scegliere **Aggiungi visualizzazione**.

![Aggiungi visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image7.png)

Selezionare l'opzione **Crea una visualizzazione fortemente tipizzata** . Per la **classe di dati View**, selezionare **Mvc3Razor. Models. UserModel**. Se non viene visualizzato **Mvc3Razor. Models. UserModel** nella finestra di dialogo **Visualizza classe dati** , è necessario compilare il progetto. Verificare che il motore di visualizzazione sia impostato su **Razor**. Impostare **Visualizza contenuto** su **elenco** e quindi fare clic su **Aggiungi**.

![Aggiungi visualizzazione indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image8.png)

La nuova visualizzazione consente di eseguire automaticamente il patibolo dei dati utente passati alla visualizzazione `Index`. Esaminare il file *Views\Home\Index* appena generato. I collegamenti **Crea nuovo**, **modifica**, **Dettagli**ed **Elimina** non funzionano, ma il resto della pagina è funzionante. Eseguire la pagina. Viene visualizzato un elenco di utenti.

![Pagina di indice](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image9.png)

Aprire il file *index. cshtml* e sostituire il markup `ActionLink` per **Edit**, **Details**ed **Delete** con il codice seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample7.cshtml)]

Il nome utente viene usato come ID per trovare il record selezionato nei collegamenti **modifica**, **Dettagli**ed **Elimina** .

## <a name="creating-the-details-view"></a>Creazione della visualizzazione dettagli

Il passaggio successivo consiste nell'aggiungere un metodo di azione `Details` e una visualizzazione per visualizzare i dettagli dell'utente.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image10.png)

Aggiungere il seguente metodo di `Details` al controller Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample8.cs)]

Fare clic con il pulsante destro del mouse all'interno del metodo `Details`, quindi scegliere <strong>Aggiungi visualizzazione</strong>. Verificare che la casella della <strong>classe Visualizza dati</strong> includa <strong>Mvc3Razor. Models. UserModel</strong><em>.</em> Impostare <strong>Visualizza contenuto</strong> su <strong>Dettagli</strong> e quindi fare clic su <strong>Aggiungi</strong>.

![Aggiungi visualizzazione dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image11.png)

Eseguire l'applicazione e selezionare un collegamento Dettagli. L'impalcatura automatica Mostra ogni proprietà nel modello.

![Dettagli](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image12.png)

## <a name="creating-the-edit-view"></a>Creazione della visualizzazione di modifica

Aggiungere il seguente metodo di `Edit` al controller Home.

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample9.cs)]

Aggiungere una visualizzazione come nei passaggi precedenti, ma impostare **Visualizza contenuto** da **modificare**.

![Aggiungi visualizzazione di modifica](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image13.png)

Eseguire l'applicazione e modificare il nome e il cognome di uno degli utenti. Se si violano i vincoli `DataAnnotation` applicati alla classe `UserModel`, quando si invia il modulo verranno visualizzati errori di convalida generati dal codice del server. Se ad esempio si modifica il nome &quot;Ann&quot; per &quot;un&quot;, quando si invia il modulo, viene visualizzato il seguente messaggio di errore nel form:

`The field First Name must be a string with a minimum length of 3 and a maximum length of 8.`

In questa esercitazione si sta trattando il nome utente come chiave primaria. Non è pertanto possibile modificare la proprietà del nome utente. Nel file *Edit. cshtml* , subito dopo l'istruzione `Html.BeginForm`, impostare il nome utente come campo nascosto. In questo modo la proprietà viene passata nel modello. Nel frammento di codice seguente viene illustrata la posizione dell'istruzione `Hidden`:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample10.cshtml)]

Sostituire il `TextBoxFor` e `ValidationMessageFor` markup per il nome utente con una chiamata di `DisplayFor`. Il metodo `DisplayFor` Visualizza la proprietà come un elemento di sola lettura. Nell'esempio seguente viene illustrato il markup completato. Le chiamate originali `TextBoxFor` e `ValidationMessageFor` sono impostate come commento con i caratteri di inizio e commento di Razor (`@* *@`)

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample11.cshtml)]

## <a name="enabling-client-side-validation"></a>Abilitazione della convalida lato client

Per abilitare la convalida lato client in ASP.NET MVC 3, è necessario impostare due flag ed è necessario includere tre file JavaScript.

Aprire il file *Web. config* dell'applicazione. Verificare `that ClientValidationEnabled` e `UnobtrusiveJavaScriptEnabled` siano impostati su true nelle impostazioni dell'applicazione. Il frammento seguente nel file *Web. config* radice Mostra le impostazioni corrette:

[!code-xml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample12.xml)]

Impostando `UnobtrusiveJavaScriptEnabled` su true, viene abilitata la convalida Ajax e non intrusiva del client. Quando si usa la convalida non intrusiva, le regole di convalida vengono convertite in attributi HTML5. I nomi degli attributi HTML5 possono essere costituiti solo da lettere minuscole, numeri e trattini.

Impostando `ClientValidationEnabled` su true, viene abilitata la convalida lato client. Impostando queste chiavi nel file *Web. config* dell'applicazione, si Abilita la convalida del client e JavaScript non intrusivo per l'intera applicazione. È anche possibile abilitare o disabilitare queste impostazioni in singole visualizzazioni o nei metodi controller usando il codice seguente:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample13.cs)]

È anche necessario includere diversi file JavaScript nella visualizzazione di cui è stato eseguito il rendering. Un modo semplice per includere il codice JavaScript in tutte le visualizzazioni consiste nell'aggiungerli al file *Views\Shared\\_Layout. cshtml* . Sostituire l'elemento `<head>` del file *\_layout. cshtml* con il codice seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample14.cshtml)]

I primi due script jQuery sono ospitati dalla rete per la distribuzione di contenuti (CDN) di Microsoft AJAX. Sfruttando la rete CDN Microsoft AJAX, è possibile migliorare significativamente le prestazioni del primo hit delle applicazioni.

Eseguire l'applicazione e fare clic su un collegamento di modifica. Visualizzare l'origine della pagina nel browser. L'origine del browser mostra molti attributi nel formato `data-val` (per la convalida dei dati). Quando è abilitata la convalida del client e di JavaScript non intrusivo, i campi di input con una regola di convalida client contengono l'attributo `data-val="true"` per attivare la convalida client non intrusiva. Il campo `City` nel modello, ad esempio, è stato decorato con l'attributo [required](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx) , che restituisce il codice HTML illustrato nell'esempio seguente:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample15.cshtml)]

Per ogni regola di convalida client viene aggiunto un attributo con il formato `data-val-rulename="message"`. Utilizzando l'esempio di campo `City` illustrato in precedenza, la regola di convalida client richiesta genera l'attributo `data-val-required` e il messaggio &quot;il campo City è obbligatorio&quot;. Eseguire l'applicazione, modificare uno degli utenti e deselezionare il campo `City`. Quando si esce dalla tabulazione del campo, viene visualizzato un messaggio di errore di convalida lato client.

![Città obbligatoria](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image14.png)

Analogamente, per ogni parametro nella regola di convalida client viene aggiunto un attributo con il formato `data-val-rulename-paramname=paramvalue`. Ad esempio, la proprietà `FirstName` viene annotata con l'attributo [StringLength](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) e specifica una lunghezza minima di 3 e una lunghezza massima di 8. La regola di convalida dei dati denominata `length` ha il nome del parametro `max` e il valore del parametro 8. Di seguito viene illustrato il codice HTML generato per il campo `FirstName` quando si modifica uno degli utenti:

[!code-cshtml[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample16.cshtml)]

Per ulteriori informazioni sulla convalida client non intrusiva, vedere la voce relativa alla [convalida client non intrusiva in ASP.NET MVC 3](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) nel Blog di Brad Wilson.

> [!NOTE]
> In ASP.NET MVC 3 beta è talvolta necessario inviare il modulo per avviare la convalida lato client. Questa operazione potrebbe essere cambiata per la versione finale.

## <a name="creating-the-create-view"></a>Creazione della visualizzazione di creazione

Il passaggio successivo consiste nell'aggiungere un metodo di azione `Create` e una visualizzazione per consentire all'utente di creare un nuovo utente. Aggiungere il seguente metodo di `Create` al controller Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample17.cs)]

Aggiungere una visualizzazione come nei passaggi precedenti, ma impostare **Visualizza contenuto** da **creare**.

![Create View](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image15.png)

Eseguire l'applicazione, selezionare il collegamento **Crea** e aggiungere un nuovo utente. Il metodo `Create` sfrutta automaticamente i vantaggi della convalida lato client e lato server. Provare ad immettere un nome utente che contiene uno spazio vuoto, ad esempio &quot;ben X&quot;. Quando si esce dalla tabulazione del campo nome utente, viene visualizzato un errore di convalida lato client (`White space is not allowed`).

## <a name="add-the-delete-method"></a>Aggiungere il metodo Delete

Per completare l'esercitazione, aggiungere il seguente metodo di `Delete` al controller Home:

[!code-csharp[Main](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/samples/sample18.cs)]

Aggiungere una visualizzazione `Delete` come nei passaggi precedenti, impostando **Visualizza contenuto** da **eliminare**.

![Eliminare la visualizzazione](creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript/_static/image16.png)

È ora disponibile un'applicazione ASP.NET MVC 3 semplice ma completamente funzionale con convalida.
