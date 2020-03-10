---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Routing degli URL | Microsoft Docs
author: Erikre
description: Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: 66b727b69ca4f9a3d35b67f492f9a554146e09ef
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78587437"
---
# <a name="url-routing"></a>Routing degli URL

di [Erik Reitan](https://github.com/Erikre)

[Scaricare il progetto di esempio WingtipC#Toys ()](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [scaricare l'E-Book (PDF)](https://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Questa serie di esercitazioni illustra le nozioni di base per la creazione di un'applicazione Web Form ASP.NET con ASP.NET 4,5 e Microsoft Visual Studio Express 2013 per il Web. Per accompagnare questa serie di esercitazioni è disponibile un [progetto Visual Studio 2013 C# con codice sorgente](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) .

In questa esercitazione verrà modificata l'applicazione di esempio Wingtip Toys per supportare il routing degli URL. Il routing consente all'applicazione Web di usare URL semplici, più facili da ricordare e meglio supportati dai motori di ricerca. Questa esercitazione si basa sull'esercitazione precedente "appartenenza e amministrazione" ed è parte della serie di esercitazioni su Wingtip Toys.

## <a name="what-youll-learn"></a>Contenuto dell'esercitazione:

- Come registrare le route per un'applicazione Web Form ASP.NET.
- Come aggiungere route a una pagina Web.
- Come selezionare i dati da un database per supportare le route.

## <a name="aspnet-routing-overview"></a>Panoramica del routing di ASP.NET

Il routing degli URL consente di configurare un'applicazione in modo che accetti URL di richiesta che non eseguono il mapping a file fisici. Un URL di richiesta è semplicemente l'URL che un utente immette nel browser per trovare una pagina nel sito Web. Si usa il routing per definire URL semanticamente significativi per gli utenti e che possono essere utili per l'ottimizzazione del motore di ricerca (SEO).

Per impostazione predefinita, il modello Web Form include [URL brevi di ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Gran parte del lavoro di routing di base verrà implementato usando *URL brevi*. In questa esercitazione verrà tuttavia aggiunta la funzionalità di routing personalizzata.

Prima di personalizzare il routing degli URL, l'applicazione di esempio Wingtip Toys può collegarsi a un prodotto usando l'URL seguente:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Personalizzando il routing degli URL, l'applicazione di esempio Wingtip Toys si collegherà allo stesso prodotto usando un URL più semplice da leggere:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Route

Una route è un modello di URL mappato a un gestore. Il gestore può essere un file fisico, ad esempio un file aspx in un'applicazione Web Form. Un gestore può anche essere una classe che elabora la richiesta. Per definire una route, è necessario creare un'istanza della classe Route specificando il modello di URL, il gestore e, facoltativamente, un nome per la route.

Per aggiungere la route all'applicazione, aggiungere l'oggetto `Route` alla proprietà `Routes` statico della classe `RouteTable`. La proprietà Routes è un oggetto `RouteCollection` che archivia tutte le route per l'applicazione.

### <a name="url-patterns"></a>Modelli di URL

Un modello di URL può contenere valori letterali e segnaposto di variabile (detti parametri URL). I valori letterali e i segnaposto si trovano in segmenti dell'URL delimitati dal carattere barra (`/`).

Quando viene effettuata una richiesta all'applicazione Web, l'URL viene analizzato in segmenti e segnaposto e i valori delle variabili vengono forniti al gestore della richiesta. Questo processo è simile al modo in cui i dati di una stringa di query vengono analizzati e passati al gestore della richiesta. In entrambi i casi, le informazioni sulle variabili sono incluse nell'URL e passate al gestore sotto forma di coppie chiave-valore. Per le stringhe di query, le chiavi e i valori sono inclusi nell'URL. Per le route, le chiavi sono i nomi di segnaposto definiti nel modello di URL e solo i valori sono presenti nell'URL.

In un modello di URL, i segnaposto vengono definiti racchiusi tra parentesi graffe (`{` e `}`). È possibile definire più di un segnaposto in un segmento, ma i segnaposto devono essere separati da un valore letterale. Ad esempio, `{language}-{country}/{action}` è un modello di route valido. Tuttavia, `{language}{country}/{action}` non è un modello valido perché non esiste alcun valore letterale o delimitatore tra i segnaposto. Pertanto, il routing non è in grado di determinare dove separare il valore per il segnaposto della lingua dal valore per il segnaposto Country.

### <a name="mapping-and-registering-routes"></a>Mapping e registrazione delle route

Prima di poter includere le route alle pagine dell'applicazione di esempio Wingtip Toys, è necessario registrare le route all'avvio dell'applicazione. Per registrare le route, è necessario modificare il gestore dell'evento `Application_Start`.

1. In **Esplora soluzioni**di Visual Studio individuare e aprire il file *Global.asax.cs* .
2. Aggiungere il codice evidenziato in giallo al file *Global.asax.cs* nel modo seguente:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Quando viene avviata l'applicazione di esempio Wingtip Toys, viene chiamato il gestore dell'evento `Application_Start`. Alla fine di questo gestore eventi, viene chiamato il metodo `RegisterCustomRoutes`. Il metodo `RegisterCustomRoutes` aggiunge ogni route chiamando il metodo `MapPageRoute` dell'oggetto `RouteCollection`. Le route vengono definite usando un nome di route, un URL di route e un URL fisico.

Il primo parametro ("`ProductsByCategoryRoute`") è il nome della route. Viene usato per chiamare la route quando è necessario. Il secondo parametro ("`Category/{categoryName}`") definisce l'URL di sostituzione descrittivo che può essere dinamico in base al codice. Questa route viene utilizzata quando si popola un controllo dati con i collegamenti generati in base ai dati. Una route viene visualizzata come segue:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

Il secondo parametro della route include un valore dinamico specificato da parentesi graffe (`{ }`). In questo caso, il `categoryName` è una variabile che verrà utilizzata per determinare il percorso di routing appropriato.

> [!NOTE] 
> 
> **Optional**
> 
> Potrebbe risultare più semplice gestire il codice spostando il `RegisterCustomRoutes` metodo in una classe separata. Nella cartella *logica* creare una classe `RouteActions` separata. Spostare il metodo `RegisterCustomRoutes` precedente dal file *Global.asax.cs* alla nuova classe `RoutesActions`. Usare la classe `RoleActions` e il metodo `createAdmin` come esempio di come chiamare il metodo `RegisterCustomRoutes` dal file *Global.asax.cs* .

È anche possibile notare la chiamata al metodo `RegisterRoutes` usando l'oggetto `RouteConfig` all'inizio del gestore dell'evento `Application_Start`. Questa chiamata viene eseguita per implementare il routing predefinito. È stato incluso come codice predefinito quando l'applicazione è stata creata usando il modello Web Form di Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recupero e utilizzo dei dati di route

Come indicato in precedenza, è possibile definire le route. Il codice aggiunto al gestore dell'evento `Application_Start` nel file *Global.asax.cs* carica le route definibili.

### <a name="setting-routes"></a>Impostazione delle route

Per le route è necessario aggiungere altro codice. In questa esercitazione si userà l'associazione di modelli per recuperare un oggetto `RouteValueDictionary` usato quando si generano le route usando i dati di un controllo dati. L'oggetto `RouteValueDictionary` conterrà un elenco di nomi di prodotto appartenenti a una categoria specifica di prodotti. Viene creato un collegamento per ogni prodotto in base ai dati e alla route.

#### <a name="enable-routes-for-categories-and-products"></a>Abilitare le route per categorie e prodotti

Successivamente, si aggiornerà l'applicazione in modo da usare la `ProductsByCategoryRoute` per determinare la route corretta da includere per ogni collegamento alla categoria di prodotto. Verrà inoltre aggiornata la pagina *Production. aspx* per includere un collegamento indirizzato per ogni prodotto. I collegamenti verranno visualizzati come prima della modifica, ma ora i collegamenti utilizzeranno il routing degli URL.

1. In **Esplora soluzioni**aprire la pagina *site. master* se non è già aperta.
2. Aggiornare il controllo **ListView** denominato "`categoryList`" con le modifiche evidenziate in giallo, quindi il markup viene visualizzato come segue:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. In **Esplora soluzioni**aprire la pagina *Production. aspx* .
4. Aggiornare l'elemento `ItemTemplate` della pagina *Production. aspx* con gli aggiornamenti evidenziati in giallo, quindi il markup viene visualizzato come segue:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Aprire il code-behind di *ProductList.aspx.cs* e aggiungere lo spazio dei nomi seguente come evidenziato in giallo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Sostituire il metodo `GetProducts` del code-behind (*ProductList.aspx.cs*) con il codice seguente:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Aggiungi codice per i dettagli del prodotto

A questo punto, aggiornare il code-behind (*productDetails.aspx.cs*) per la pagina *productDetails. aspx* per usare i dati della route. Si noti che il nuovo metodo `GetProduct` accetta anche un valore della stringa di query nel caso in cui l'utente disponga di un collegamento con un segnalibro che usa l'URL non descrittivo meno recente.

1. Sostituire il metodo `GetProduct` del code-behind (*productDetails.aspx.cs*) con il codice seguente:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Esecuzione dell'applicazione

È possibile eseguire ora l'applicazione per visualizzare le route aggiornate.

1. Premere **F5** per eseguire l'applicazione di esempio Wingtip Toys.  
 Verrà aperto il browser e verrà visualizzata la pagina *default. aspx* .
2. Fare clic sul collegamento **prodotti** nella parte superiore della pagina.  
 Tutti i prodotti vengono visualizzati nella pagina *Production. aspx* . Viene visualizzato l'URL seguente (usando il numero di porta) per il browser:  
    `https://localhost:44300/ProductList`
3. Fare quindi clic sul collegamento categoria **automobili** nella parte superiore della pagina.  
 Nella pagina *Production. aspx* vengono visualizzate solo le automobili. Viene visualizzato l'URL seguente (usando il numero di porta) per il browser:  
    `https://localhost:44300/Category/Cars`
4. Fare clic sul collegamento contenente il nome della prima automobile elencata nella pagina ("**auto convertibile**") per visualizzare i dettagli del prodotto.  
 Viene visualizzato l'URL seguente (usando il numero di porta) per il browser:  
    `https://localhost:44300/Product/Convertible%20Car`
5. Quindi, immettere il seguente URL non indirizzato (usando il numero di porta) nel browser:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 Il codice riconosce ancora un URL che include una stringa di query, nel caso in cui un utente disponga di un collegamento con segnalibro.

## <a name="summary"></a>Riepilogo

In questa esercitazione sono state aggiunte le route per le categorie e i prodotti. Si è appreso come è possibile integrare le route con i controlli dati che usano l'associazione di modelli. Nell'esercitazione successiva verrà implementata la gestione globale degli errori.

## <a name="additional-resources"></a>Risorse aggiuntive

[URL brevi di ASP.NET](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Distribuire un'app Web Form ASP.NET sicura con appartenenza, OAuth e database SQL al servizio app Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Versione di valutazione gratuita di Microsoft Azure](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Precedente](membership-and-administration.md)
> [Successivo](aspnet-error-handling.md)
