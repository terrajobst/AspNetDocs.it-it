---
uid: single-page-application/overview/templates/emberjs-template
title: Modello EmberJS | Microsoft Docs
author: xqiu
description: Modello EmberJS
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1aefa46dd0841b1b06675409cc8a09f9a218d7ac
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578505"
---
# <a name="emberjs-template"></a>Modello EmberJS

di [Xinyang Qiu](https://github.com/xqiu)

> Il modello MVC EmberJS è scritto da Nathan Lucchesi, Thiago Santos e Xinyang Qiu.
> 
> [Scaricare il modello MVC EmberJS](https://go.microsoft.com/fwlink/?LinkId=282647)

Il modello EmberJS SPA è progettato per iniziare rapidamente a creare app Web interattive sul lato client usando EmberJS.

"Applicazione a pagina singola" (SPA) è il termine generale per un'applicazione Web che carica una singola pagina HTML e quindi aggiorna la pagina in modo dinamico, anziché caricare nuove pagine. Al termine del caricamento iniziale della pagina, la SPA comunica con il server tramite richieste AJAX.

![](emberjs-template/_static/image1.png)

AJAX non è una novità, ma oggi sono disponibili Framework JavaScript che semplificano la creazione e la gestione di un'applicazione SPA sofisticata di grandi dimensioni. Inoltre, HTML 5 e CSS3 semplificano la creazione di interfacce utente avanzate.

Il modello di EmberJS SPA usa la libreria [per la gestione di pagine JavaScript per](http://emberjs.com/) gestire gli aggiornamenti delle pagine dalle richieste AJAX. Brac. js USA data binding per sincronizzare la pagina con i dati più recenti. In questo modo, non è necessario scrivere codice che analizza i dati JSON e aggiorna il DOM. Al contrario, si inseriscono gli attributi dichiarativi nel codice HTML che indicano a Brac. js come presentare i dati.

Sul lato server, il modello EmberJS è quasi identico al [modello di Knockout Spa](../introduction/knockoutjs-template.md). USA ASP.NET MVC per fornire documenti HTML e API Web ASP.NET per gestire le richieste AJAX dal client. Per ulteriori informazioni su questi aspetti del modello, vedere la documentazione del [modello knockout](../introduction/knockoutjs-template.md) . Questo argomento è incentrato sulle differenze tra il modello knockout e il modello EmberJS.

## <a name="create-an-emberjs-spa-template-project"></a>Creare un progetto di modello EmberJS SPA

Scaricare e installare il modello facendo clic sul pulsante di download riportato sopra. Potrebbe essere necessario riavviare Visual Studio.

Nel riquadro **modelli** selezionare **modelli installati** ed espandere il nodo  **C# visivo** . In **Visual C#** Selezionare **Web**. Nell'elenco dei modelli di progetto selezionare **applicazione Web ASP.NET MVC 4**. Assegnare un nome al progetto e fare clic su **OK**.

![](emberjs-template/_static/image2.png)

Nella creazione guidata **nuovo progetto** , selezionare **progetto Spa Brac. js**.

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>Panoramica del modello di EmberJS SPA

Il modello EmberJS usa una combinazione di jQuery, Brac. js, manopole. js per creare un'interfaccia utente interattiva e uniforme.

Brac. js è una libreria JavaScript che usa uno schema MVC sul lato client.

- Un *modello*, scritto nel linguaggio del modello manubrio, descrive l'interfaccia utente dell'applicazione. In modalità di rilascio, il [compilatore manubri](https://github.com/Myslik/csharp-ember-handlebars) viene usato per aggregare e compilare il modello manubrio.
- Un *modello* archivia i dati dell'applicazione ottenuti dal server (elenchi todo e elementi todo).
- Un *controller* archivia lo stato dell'applicazione. I controller spesso presentano dati del modello ai modelli corrispondenti.
- Una *visualizzazione* converte gli eventi primitivi dall'applicazione e li passa al controller.
- Un *router* gestisce lo stato dell'applicazione, mantenendo sincronizzati gli URL e i modelli.

Inoltre, la libreria di dati di Brac può essere usata per sincronizzare gli oggetti JSON (ottenuti dal server tramite un'API RESTful) e i modelli client.

Il modello EmberJS SPA organizza gli script in otto livelli:

- WebAPI\_adapter. js, WebAPI\_serializer. js: estende la libreria di dati Brac per lavorare con API Web ASP.NET.
- Scripts/helper. js: definisce nuovi Helper del manubrio Brac.
- Scripts/app. js: crea l'app e configura l'adapter e il serializzatore.
- Scripts/app/models/\*. js: definisce i modelli.
- Scripts/app/views/\*. js: definisce le visualizzazioni.
- Scripts/app/controllers/\*. js: definisce i controller.
- Scripts/app/routes, scripts/app/router. js: definisce le route.
- Templates/\*. HBS: definisce i modelli dei manubri.

Esaminiamo alcuni di questi script in modo più dettagliato.

## <a name="models"></a>Modelli

I modelli sono definiti nella cartella Scripts/app/models. Sono disponibili due file di modello: todoItem. js e todo. js.

**todo. Model. js** definisce i modelli lato client (browser) per gli elenchi di attività. Sono disponibili due classi di modello: todoItem e todo. In Brac, i modelli sono sottoclassi di servizi di dominio Active Directory. Modello. Un modello può avere proprietà con attributi:

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

I modelli possono definire relazioni con altri modelli:

[!code-css[Main](emberjs-template/samples/sample2.css)]

I modelli possono avere proprietà calcolate associate ad altre proprietà:

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

I modelli possono avere funzioni Observer, che vengono richiamate quando una proprietà osservata cambia:

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Visualizzazioni

Le visualizzazioni sono definite nella cartella Scripts/app/views. Una vista converte gli eventi dall'interfaccia utente dell'applicazione. Un gestore eventi può richiamare le funzioni del controller o semplicemente chiamare direttamente il contesto dati.

Il codice seguente, ad esempio, è da views/TodoItemEditView. js. Definisce la gestione degli eventi per un campo di testo di input.

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>Controller

I controller sono definiti nella cartella Scripts/app/controllers. Per rappresentare un singolo modello, estendere `Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

Un controller può anche rappresentare una raccolta di modelli estendendo `Ember.ArrayController`. Ad esempio, TodoListController rappresenta una matrice di oggetti `todoList`. Il controller Ordina in base all'ID dell'oggetto todo, in ordine decrescente:

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

Il controller definisce una funzione denominata `addTodoList`, che crea un nuovo elenco attività e lo aggiunge alla matrice. Per vedere come viene chiamata questa funzione, aprire il file modello denominato todoListTemplate. html nella cartella Templates. Il codice di modello seguente associa un pulsante alla funzione `addTodoList`:

[!code-html[Main](emberjs-template/samples/sample8.html)]

Il controller contiene anche una proprietà `error`, che contiene un messaggio di errore. Ecco il codice del modello per visualizzare il messaggio di errore (anche in todoListTemplate. html):

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>Route

Router. js definisce le route e il modello predefinito da visualizzare, imposta lo stato dell'applicazione e corrisponde agli URL delle route:

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute. js carica i dati per il TodoListRoute eseguendo l'override della funzione setupController:

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Brac usa le convenzioni di denominazione per associare URL, nomi di route, controller e modelli. Per ulteriori informazioni, vedere [http://emberjs.com/guides/routing/defining-your-routes/](http://emberjs.com/guides/routing/defining-your-routes/) nella documentazione di EmberJS.

## <a name="templates"></a>Modelli

La cartella Templates contiene quattro modelli:

- Application. HBS: modello predefinito di cui viene eseguito il rendering all'avvio dell'applicazione.
- about. HBS: modello per la route "/about".
- index. HBS: modello per la route radice "/".
- todo. HBS: modello per la route "/TODO".
- \_barra di spostamento. HBS: il modello definisce il menu di navigazione.

Il modello di applicazione funge da pagina master. Contiene un'intestazione, un piè di pagina e un "{{Outlet}}" per inserire altri modelli in a seconda della route. Per ulteriori informazioni sui modelli di applicazione in Brac, vedere [http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/).

Il modello "/todoList" contiene due espressioni di ciclo. Il ciclo esterno è `{{#each controller}}`e il ciclo interno è `{{#each todos}}`. Il codice seguente illustra una visualizzazione `Ember.Checkbox` incorporata, una `App.TodoItemEditView`personalizzata e un collegamento con un'azione di `deleteTodo`.

[!code-html[Main](emberjs-template/samples/sample12.html)]

La classe `HtmlHelperExtensions`, definita in Controllers/HtmlHelperExtensions. cs, definisce una funzione helper per memorizzare nella cache e inserire i file di modello quando il **debug** è impostato su **true** nel file Web. config. Questa funzione viene chiamata dal file di visualizzazione MVC ASP.NET definito in views/Home/app. cshtml:

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

Chiamato senza argomenti, la funzione esegue il rendering di tutti i file di modello nella cartella Templates. È anche possibile specificare una sottocartella o un file di modello specifico.

Quando il **debug** è **false** in Web. config, l'applicazione include l'elemento del bundle "~/Bundles/templates". Questo elemento del bundle viene aggiunto in BundleConfig.cs, usando la libreria del compilatore dei manubri:

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
