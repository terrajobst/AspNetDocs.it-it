---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
title: Informazioni sull'autenticazione e sul profilo di ASP.NET AJAX Servizi per le applicazioni | Microsoft Docs
author: scottcate
description: Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire l'utente personalizzato...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: 6ab4efb6-aab6-45ac-ad2c-bdec5848ef9e
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-authentication-and-profile-application-services
msc.type: authoredcontent
ms.openlocfilehash: cab9acb1ffd75cca87f6c575a6abdd000235828e
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640532"
---
# <a name="understanding-aspnet-ajax-authentication-and-profile-application-services"></a>Informazioni sui servizi di autenticazione e applicazione del profilo di ASP.NET AJAX

di [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial03_MSAjax_ASP.NET_Services_cs.pdf)

> Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire i profili utente personalizzati forniti da ASP.NET. L'uso del servizio di autenticazione ASP.NET AJAX è compatibile con l'autenticazione basata su form ASP.NET standard, pertanto le applicazioni che attualmente usano l'autenticazione basata su form (ad esempio con il controllo Login) non verranno interrotte eseguendo l'aggiornamento al servizio di autenticazione AJAX.

## <a name="introduction"></a>Introduzione

Come parte del .NET Framework 3,5, Microsoft sta distribuendo un aggiornamento di ambiente considerevole; non solo è disponibile un nuovo ambiente di sviluppo, ma le nuove funzionalità LINQ (Language Integrated Query) e altri miglioramenti del linguaggio sono imminenti. Inoltre, alcune funzionalità note di altri set di strumenti, in particolare le estensioni ASP.NET AJAX, vengono incluse come membri di prima classe della libreria di classi di base .NET Framework. Queste estensioni abilitano molte nuove funzionalità rich client, incluso il rendering parziale delle pagine senza richiedere un aggiornamento completo della pagina, la possibilità di accedere ai servizi Web tramite script client (inclusa l'API di profilatura ASP.NET) e un'API lato client estesa progettato per eseguire il mirroring di molti degli schemi di controllo visualizzati nel set di controlli lato server di ASP.NET.

Questo white paper esamina l'implementazione e l'uso dei servizi di autenticazione basata su form e di profilatura di ASP.NET così come sono esposti dal Microsoft ASP.NET AJAX ExtensionsThe AJAX Extensions rende l'autenticazione basata su form incredibilmente facile da supportare, così come il servizio di profilatura viene esposto tramite uno script del proxy del servizio Web. Le estensioni AJAX supportano anche l'autenticazione personalizzata tramite la classe AuthenticationServiceManager.

Questo white paper è basato sulla versione beta 2 di Visual Studio 2008 e .NET Framework 3,5. Questo white paper presuppone inoltre che si lavorerà con Visual Studio 2008 Beta 2, non con Visual Web Developer Express, e verranno fornite procedure dettagliate in base all'interfaccia utente di Visual Studio. Alcuni esempi di codice possono utilizzare modelli di progetto non disponibili in Visual Web Developer Express.

## <a name="profiles-and-authentication"></a>*Profili e autenticazione*

I profili di Microsoft ASP.NET e i servizi di autenticazione sono forniti dal sistema di autenticazione basata su form ASP.NET e sono componenti standard di ASP.NET. Le estensioni ASP.NET AJAX consentono di accedere allo script a questi servizi tramite proxy di script, tramite un modello piuttosto semplice nello spazio dei nomi sys. Services della libreria AJAX client.

Il servizio di autenticazione consente agli utenti di fornire le credenziali per ricevere un cookie di autenticazione ed è il servizio gateway per consentire i profili utente personalizzati forniti da ASP.NET. L'uso del servizio di autenticazione ASP.NET AJAX è compatibile con l'autenticazione basata su form ASP.NET standard, pertanto le applicazioni che attualmente usano l'autenticazione basata su form (ad esempio con il controllo Login) non verranno interrotte eseguendo l'aggiornamento al servizio di autenticazione AJAX.

Il servizio profilo consente l'integrazione e l'archiviazione automatica dei dati utente in base all'appartenenza fornita dal servizio di autenticazione. I dati archiviati vengono specificati dal file Web. config e i vari provider del servizio di profilatura gestiscono la gestione dei dati. Come per il servizio di autenticazione, il servizio profili AJAX è compatibile con il servizio profilo ASP.NET standard, in modo che le pagine che includono attualmente le funzionalità del servizio profili ASP.NET non vengano interrotte includendo il supporto AJAX.

Incorporare i servizi di profilatura e autenticazione ASP.NET in un'applicazione esula dall'ambito di questo white paper. Per ulteriori informazioni sull'argomento, vedere l'articolo di riferimento di MSDN Library gestione degli utenti tramite l'appartenenza all' [https://msdn.microsoft.com/library/tw292whz.aspx](https://msdn.microsoft.com/library/tw292whz.aspx). ASP.NET include anche un'utilità per configurare automaticamente l'appartenenza a un SQL Server, ovvero il provider di servizi di autenticazione predefinito per l'appartenenza a ASP.NET. Per ulteriori informazioni, vedere l'articolo ASP.NET SQL Server Registration Tool (ASPNET\_RegSql. exe) in [https://msdn.microsoft.com/library/ms229862(vs.80).aspx](https://msdn.microsoft.com/library/ms229862(vs.80).aspx).

## <a name="using-the-aspnet-ajax-authentication-service"></a>*Uso del servizio di autenticazione ASP.NET AJAX*

Il servizio di autenticazione ASP.NET AJAX deve essere abilitato nel file Web. config:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample1.xml)]

Il servizio di autenticazione richiede l'abilitazione dell'autenticazione basata su form ASP.NET e richiede l'abilitazione dei cookie nel browser client (uno script non può abilitare una sessione senza cookie perché le sessioni senza cookie richiedono parametri URL).

Una volta abilitato e configurato il servizio di autenticazione AJAX, lo script client può trarre immediatamente vantaggio dall'oggetto sys. Services. AuthenticationService. Principalmente, lo script client desidera sfruttare il metodo `login` e la proprietà `isLoggedIn`. Esistono diverse proprietà per fornire le impostazioni predefinite per il metodo login, che può accettare un numero elevato di parametri.

*Membri sys. Services. AuthenticationService*

*Metodo di accesso:*

Il metodo login () avvia una richiesta di autenticazione delle credenziali dell'utente. Questo metodo è asincrono e non blocca l'esecuzione.

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| userName | Obbligatorio. Nome utente da autenticare. |
| password | Facoltativo (il valore predefinito è null). Password dell'utente. |
| isPersistent | Facoltativo (il valore predefinito è false). Indica se il cookie di autenticazione dell'utente deve essere mantenuto tra le sessioni. Se false, l'utente si disconnetterà alla chiusura del browser o alla scadenza della sessione. |
| redirectUrl | Facoltativo (il valore predefinito è null). URL al quale reindirizzare il browser al completamento dell'autenticazione. Se questo parametro è null o una stringa vuota, non si verifica alcun reindirizzamento. |
| customInfo | Facoltativo (il valore predefinito è null). Questo parametro è attualmente inutilizzato ed è riservato per un utilizzo futuro. |
| loginCompletedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare quando l'account di accesso è stato completato correttamente. Se specificato, questo parametro esegue l'override della proprietà defaultLoginCompleted. |
| failedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare quando l'account di accesso ha avuto esito negativo. Se specificato, questo parametro esegue l'override della proprietà defaultFailedCallback. |
| userContext | Facoltativo (il valore predefinito è null). Dati del contesto utente personalizzati che devono essere passati alle funzioni di callback. |

*Return Value* (Valore restituito):

Questa funzione non include un valore restituito. Al completamento di una chiamata a questa funzione, tuttavia, vengono inclusi diversi comportamenti:

- La pagina corrente verrà aggiornata o verrà modificata se il parametro `redirectUrl` non è né null né una stringa vuota.
- Tuttavia, se il parametro è null o una stringa vuota, viene chiamato il `loginCompletedCallback` parametro o la proprietà `defaultLoginCompletedCallback`.
- Se la chiamata al servizio Web ha esito negativo, viene chiamato il parametro `failedCallback` della proprietà `defaultFailedCallback`.

*Metodo di disconnessione:*

Il metodo Logout () rimuove il cookie delle credenziali e disconnette l'utente corrente dall'applicazione Web.

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| redirectUrl | Facoltativo (il valore predefinito è null). URL al quale reindirizzare il browser al completamento dell'autenticazione. Se questo parametro è null o una stringa vuota, non si verifica alcun reindirizzamento. |
| logoutCompletedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare quando la disconnessione è stata completata correttamente. Se specificato, questo parametro esegue l'override della proprietà defaultLogoutCompleted. |
| failedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare quando l'account di accesso ha avuto esito negativo. Se specificato, questo parametro esegue l'override della proprietà defaultFailedCallback. |
| userContext | Facoltativo (il valore predefinito è null). Dati del contesto utente personalizzati che devono essere passati alle funzioni di callback. |

*Return Value* (Valore restituito):

Questa funzione non include un valore restituito. Al completamento di una chiamata a questa funzione, tuttavia, vengono inclusi diversi comportamenti:

- La pagina corrente verrà aggiornata o verrà modificata se il parametro `redirectUrl` non è né null né una stringa vuota.
- Tuttavia, se il parametro è null o una stringa vuota, viene chiamato il `logoutCompletedCallback` parametro o la proprietà `defaultLogoutCompletedCallback`.
- Se la chiamata al servizio Web ha esito negativo, viene chiamato il parametro `failedCallback` della proprietà `defaultFailedCallback`.

*Proprietà defaultFailedCallback (Get, set):*

Questa proprietà specifica una funzione che deve essere chiamata se si verifica un errore di comunicazione con il servizio Web. Deve ricevere un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificato da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample2.js)]

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| error | Specifica le informazioni sull'errore. |
| userContext | Specifica le informazioni sul contesto utente fornite quando è stata chiamata la funzione di accesso o di disconnessione. |
| methodName | Nome del metodo chiamante. |

*Proprietà defaultLoginCompletedCallback (Get, set):*

Questa proprietà specifica una funzione che deve essere chiamata quando la chiamata al servizio Web login è stata completata. Deve ricevere un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificato da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample3.js)]

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| validCredentials | Specifica se l'utente ha fornito credenziali valide. `true` se l'utente ha eseguito correttamente l'accesso; in caso contrario `false`. |
| userContext | Specifica le informazioni sul contesto utente fornite quando è stata chiamata la funzione login. |
| methodName | Nome del metodo chiamante. |

*Proprietà defaultLogoutCompletedCallback (Get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al completamento della chiamata al servizio Web di disconnessione. Deve ricevere un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificato da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample4.js)]

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| risultato | Questo parametro sarà sempre `null`; è riservata per un utilizzo futuro. |
| userContext | Specifica le informazioni sul contesto utente fornite quando è stata chiamata la funzione login. |
| methodName | Nome del metodo chiamante. |

*Proprietà di login (Get):*

Questa proprietà ottiene lo stato di autenticazione corrente dell'utente. viene impostato dall'oggetto ScriptManager durante una richiesta di pagina.

Questa proprietà restituisce `true` se l'utente è attualmente connesso; in caso contrario, restituisce `false`.

*Proprietà Path (Get, set):*

Questa proprietà determina la posizione del servizio Web di autenticazione a livello di codice. Può essere utilizzato per eseguire l'override del provider di autenticazione predefinito e un set in modo dichiarativo nella proprietà Path del nodo figlio AuthenticationService del controllo ScriptManager (per ulteriori informazioni, vedere la pagina relativa all'utilizzo di un provider di servizi di autenticazione personalizzato. argomento riportato di seguito.

Si noti che il percorso del servizio di autenticazione predefinito non cambia. Tuttavia, ASP.NET AJAX consente di specificare la posizione di un servizio Web che fornisce la stessa interfaccia di classe del proxy del servizio di autenticazione ASP.NET AJAX.

Si noti inoltre che questa proprietà non deve essere impostata su un valore che indirizza la richiesta di script al di fuori del sito corrente. Poiché l'applicazione corrente non riceverà le credenziali di autenticazione, sarebbe inutile; Inoltre, la tecnologia AJAX sottostante non deve inviare richieste tra siti e potrebbe generare un'eccezione di sicurezza nel browser client.

Questa proprietà è un oggetto `String` che rappresenta il percorso del servizio Web di autenticazione.

*Proprietà timeout (Get, set):*

Questa proprietà determina l'intervallo di tempo di attesa del servizio di autenticazione prima di presumere che la richiesta di accesso non sia riuscita. Se il timeout scade durante l'attesa del completamento di una chiamata, viene chiamato il callback request-failed e la chiamata non viene completata.

Questa proprietà è un oggetto `Number` che rappresenta il numero di millisecondi di attesa per i risultati del servizio di autenticazione.

*Esempio di codice: accesso al servizio di autenticazione*

Il markup seguente è un esempio di pagina ASP.NET con una semplice chiamata di script ai metodi login e logout della classe AuthenticationService.

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample5.aspx)]

## <a name="accessing-aspnet-profiling-data-via-ajax"></a>Accesso ai dati di profilatura di ASP.NET tramite AJAX

Il servizio di profilatura ASP.NET viene inoltre esposto tramite le estensioni ASP.NET AJAX. Poiché il servizio di profilatura ASP.NET fornisce un'API dettagliata e granulare in cui archiviare e recuperare i dati utente, questo può essere uno strumento di produttività eccellente.

Il servizio profili deve essere abilitato nel file Web. config; non è per impostazione predefinita. A tale scopo, verificare che il `profileService` elemento figlio abbia abilitato = true specificato in Web. config e che siano state specificate le proprietà che possono essere lette o scritte come indicato di seguito:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample6.xml)]

È necessario configurare anche il servizio profili. Sebbene la configurazione del servizio di profilatura esula dall'ambito di questo white paper, è opportuno notare che i gruppi definiti nelle impostazioni di configurazione del profilo saranno accessibili come proprietà secondarie del nome del gruppo. Ad esempio, con la seguente sezione del profilo specificata:

[!code-xml[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample7.xml)]

Lo script client può accedere a Name, Address. riga 1, Address. Riga2, Address. City, Address. state, Address. zip e BackgroundColor come proprietà del campo Properties della classe ProfileService.

Una volta configurato, il servizio di profilatura AJAX sarà immediatamente disponibile nelle pagine. Tuttavia, sarà necessario caricarla una volta prima dell'utilizzo.

*Membri sys. Services. ProfileService*

*campo proprietà:*

Il campo proprietà espone tutti i dati di profilo configurati come proprietà figlio a cui è possibile fare riferimento tramite la convenzione punto-operatore-nome. Le proprietà che sono elementi figlio dei gruppi di proprietà sono definite GroupName. PropertyName. Nella configurazione del profilo di esempio illustrata in precedenza, per ottenere lo stato dell'utente, è possibile usare l'identificatore seguente:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample8.cs)]

*Metodo Load:*

Carica un elenco selezionato o tutte le proprietà dal server.

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| propertyNames | Facoltativo (il valore predefinito è null). Proprietà da caricare dal server. |
| loadCompletedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare al termine del caricamento. |
| failedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare se si verifica un errore. |
| userContext | Facoltativo (il valore predefinito è null). Informazioni di contesto da passare alla funzione di callback. |

La funzione Load non ha un valore restituito. Se la chiamata è stata completata correttamente, chiamerà il parametro `loadCompletedCallback` o la proprietà `defaultLoadCompletedCallback`. Se la chiamata ha esito negativo o il timeout è scaduto, verrà chiamato il `failedCallback` parametro o la proprietà `defaultFailedCallback`.

Se il parametro `propertyNames` non viene specificato, tutte le proprietà configurate in lettura vengono recuperate dal server.

*Metodo Save:*

Il metodo Save () Salva l'elenco di proprietà specificato (o tutte le proprietà) nel profilo ASP.NET dell'utente.

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| propertyNames | Facoltativo (il valore predefinito è null). Proprietà da salvare nel server. |
| saveCompletedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare al termine del salvataggio. |
| failedCallback | Facoltativo (il valore predefinito è null). Funzione da chiamare se si verifica un errore. |
| userContext | Facoltativo (il valore predefinito è null). Informazioni di contesto da passare alla funzione di callback. |

La funzione Save non ha un valore restituito. Se la chiamata viene completata correttamente, chiamerà il parametro `saveCompletedCallback` o la proprietà `defaultSaveCompletedCallback`. Se la chiamata ha esito negativo o il timeout è scaduto, verrà chiamata la proprietà `failedCallback` o `defaultFailedCallback`.

Se il parametro `propertyNames` è null, tutte le proprietà del profilo verranno inviate al server e il server deciderà quali proprietà possono essere salvate e quali non è possibile.

*Proprietà defaultFailedCallback (Get, set):*

Questa proprietà specifica una funzione che deve essere chiamata se si verifica un errore di comunicazione con il servizio Web. Deve ricevere un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificato da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample9.js)]

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| Errore | Specifica le informazioni sull'errore. |
| userContext | Specifica le informazioni sul contesto utente fornite quando è stata chiamata la funzione Load o Save. |
| methodName | Nome del metodo chiamante. |

*Proprietà defaultSaveCompleted (Get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al completamento del salvataggio dei dati del profilo dell'utente. Deve ricevere un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificato da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample10.js)]

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| numPropsSaved | Specifica il numero di proprietà salvate. |
| userContext | Specifica le informazioni sul contesto utente fornite quando è stata chiamata la funzione Load o Save. |
| methodName | Nome del metodo chiamante. |

*Proprietà defaultLoadCompleted (Get, set):*

Questa proprietà specifica una funzione che deve essere chiamata al completamento del caricamento dei dati del profilo dell'utente. Deve ricevere un delegato (o un riferimento alla funzione).

Il riferimento alla funzione specificato da questa proprietà deve avere la firma seguente:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample11.js)]

*Parametri:*

| **Nome parametro** | **Significato** |
| --- | --- |
| numPropsLoaded | Specifica il numero di proprietà caricate. |
| userContext | Specifica le informazioni sul contesto utente fornite quando è stata chiamata la funzione Load o Save. |
| methodName | Nome del metodo chiamante. |

*Proprietà Path (Get, set):*

Questa proprietà determina il percorso del servizio Web del profilo a livello di codice. Può essere utilizzato per eseguire l'override del provider di servizi profili predefinito, nonché di un set in modo dichiarativo nella proprietà Path del nodo figlio ProfileService del controllo ScriptManager.

Si noti che il percorso del servizio profili predefinito non viene modificato. Tuttavia, ASP.NET AJAX consente di specificare la posizione di un servizio Web che fornisce la stessa interfaccia di classe del proxy del servizio di autenticazione ASP.NET AJAX.

Si noti inoltre che questa proprietà non deve essere impostata su un valore che indirizza la richiesta di script al di fuori del sito corrente. La tecnologia AJAX sottostante non deve inviare richieste tra siti e potrebbe generare un'eccezione di sicurezza nel browser client.

Questa proprietà è un oggetto `String` che rappresenta il percorso del servizio Web del profilo.

*Proprietà timeout (Get, set):*

Questa proprietà determina l'intervallo di tempo di attesa per il servizio profili prima di presumere che la richiesta di caricamento o salvataggio non sia riuscita. Se il timeout scade durante l'attesa del completamento di una chiamata, viene chiamato il callback request-failed e la chiamata non viene completata.

Questa proprietà è un oggetto `Number` che rappresenta il numero di millisecondi di attesa dei risultati dal servizio profili.

*Esempio di codice: caricamento dei dati del profilo al caricamento della pagina*

Il codice seguente verificherà se un utente è autenticato e, in tal caso, caricherà il colore di sfondo preferito dell'utente come pagina.

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample12.js)]

## <a name="using-a-custom-authentication-service-provider"></a>*Uso di un provider di servizi di autenticazione personalizzato*

Le estensioni ASP.NET AJAX consentono di creare un provider di servizi di autenticazione script personalizzato esponendo la funzionalità tramite un servizio Web personalizzato. Per poter essere usato, il servizio Web deve esporre due metodi, `Login` e `Logout`; questi metodi devono essere specificati con le stesse firme del metodo del servizio Web di autenticazione ASP.NET AJAX predefinito.

Dopo aver creato il servizio Web personalizzato, sarà necessario specificarne il percorso, in modo dichiarativo nella pagina, a livello di codice o tramite lo script client.

*Per impostare il percorso in modo dichiarativo:*

Per impostare il percorso in modo dichiarativo, includere il figlio AuthenticationService dell'oggetto ScriptManager nella pagina ASP.NET:

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample13.aspx)]

*Per impostare il percorso nel codice:*

Per impostare il percorso a livello di programmazione, specificare il percorso tramite l'istanza di gestione script:

[!code-csharp[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample14.cs)]

*Per impostare il percorso nello script:*

Per impostare il percorso a livello di codice nello script, utilizzare la proprietà `path` della classe AuthenticationService:

[!code-javascript[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample15.js)]

*Servizio Web di esempio per l'autenticazione personalizzata*

[!code-aspx[Main](understanding-asp-net-ajax-authentication-and-profile-application-services/samples/sample16.aspx)]

## <a name="summary"></a>Riepilogo

I servizi ASP.NET, in particolare i servizi di profilatura, appartenenza e autenticazione, sono facilmente esposti a JavaScript nel browser client. In questo modo gli sviluppatori possono integrare il proprio codice sul lato client con il meccanismo di autenticazione senza alcuna dipendenza da controlli come gli UpdatePanel. I dati del profilo possono essere protetti anche dal client usando le impostazioni di configurazione Web. non sono disponibili dati per impostazione predefinita e gli sviluppatori devono acconsentire esplicitamente alle proprietà del profilo.

Inoltre, grazie alla creazione di implementazioni di servizi web semplificate con firme di metodi equivalenti, gli sviluppatori possono creare provider di script personalizzati per questi servizi ASP.NET intrinseci. Il supporto per queste tecniche semplifica lo sviluppo di applicazioni rich client, garantendo allo stesso tempo agli sviluppatori un'ampia gamma di flessibilità per soddisfare esigenze specifiche.

## <a name="bio"></a>*Biografia*

Scott Cate collabora con le tecnologie Web Microsoft a partire dal 1997 ed è il Presidente di myKB.com ([www.myKB.com](http://www.myKB.com)), dove si specializza nella scrittura di applicazioni basate su ASP.NET incentrate sulle soluzioni software della Knowledge base. Scott può essere contattato tramite posta elettronica all' [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o al suo blog all' [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-updatepanel-triggers.md)
> [Successivo](understanding-asp-net-ajax-localization.md)
