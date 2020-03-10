---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: aggiunta di una visualizzazione amministratore | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 664aeb33031e933322886a6d6bdd989277e9fda2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556014"
---
# <a name="part-4-adding-an-admin-view"></a>Parte 4: aggiunta di una visualizzazione amministratore

di [Mike Wasson](https://github.com/MikeWasson)

[Scarica progetto completato](https://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Aggiungere una visualizzazione amministratore

A questo punto, si passerà al lato client e si aggiungerà una pagina che può utilizzare i dati del controller di amministrazione. La pagina consente agli utenti di creare, modificare o eliminare prodotti inviando richieste AJAX al controller.

In Esplora soluzioni espandere la cartella controller e aprire il file denominato HomeController.cs. Questo file contiene un controller MVC. Aggiungere un metodo denominato `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

Il metodo **HttpRouteUrl** crea l'URI per l'API Web e lo archivia nell'elenco delle visualizzazioni per un momento successivo.

Posizionare quindi il cursore del testo all'interno del metodo di azione `Admin`, quindi fare clic con il pulsante destro del mouse e scegliere **Aggiungi visualizzazione**. Verrà visualizzata la finestra di dialogo **Aggiungi visualizzazione** .

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

Nella finestra di dialogo **Aggiungi visualizzazione** assegnare un nome alla vista "admin". Selezionare la casella di controllo **Crea una visualizzazione fortemente tipizzata**. In **classe modello**selezionare "Product (ProductStore. Models)". Lasciare invariati i valori predefiniti per tutte le altre opzioni.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Fare clic su **Aggiungi per aggiungere** un file denominato admin. cshtml in views/Home. Aprire il file e aggiungere il codice HTML seguente. Questo codice HTML definisce la struttura della pagina, ma nessuna funzionalità è ancora collegata.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Creare un collegamento alla pagina di amministrazione

In Esplora soluzioni espandere la cartella Views, quindi espandere la cartella Shared. Aprire il file denominato \_layout. cshtml. Individuare l'elemento **ul** con ID = "menu" e un collegamento all'azione per la visualizzazione amministratore:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> Nel progetto di esempio sono state apportate alcune altre modifiche estetiche, ad esempio la sostituzione della stringa "your logo here". Questi non influiscono sulla funzionalità dell'applicazione. È possibile scaricare il progetto e confrontare i file.

Eseguire l'applicazione e fare clic sul collegamento "admin" visualizzato nella parte superiore del home page. La pagina di amministrazione dovrebbe essere simile alla seguente:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

A questo punto, la pagina non esegue alcuna operazione. Nella sezione successiva verrà usato knockout. js per creare un'interfaccia utente dinamica.

## <a name="add-authorization"></a>Aggiungi autorizzazione

La pagina di amministrazione è attualmente accessibile a chiunque visiti il sito. Modificare questa opzione per limitare l'autorizzazione agli amministratori.

Per iniziare, aggiungere un ruolo "Administrator" e un utente amministratore. In Esplora soluzioni espandere la cartella filtri e aprire il file denominato InitializeSimpleMembershipAttribute.cs. Individuare il costruttore `SimpleMembershipInitializer`. Dopo la chiamata a **WebSecurity. InitializeDatabaseConnection**, aggiungere il codice seguente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Si tratta di un modo rapido e rapido per aggiungere il ruolo "Administrator" e creare un utente per il ruolo.

In Esplora soluzioni espandere la cartella controller e aprire il file HomeController.cs. Aggiungere l'attributo **autorizzi** al metodo `Admin`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Aprire il file AdminController.cs e aggiungere l'attributo **autorizza** all'intera classe `AdminController`.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC e l'API Web definiscono entrambi gli attributi di **autorizzazione** , in spazi dei nomi diversi. MVC usa **System. Web. Mvc. AuthorizeAttribute**, mentre l'API Web USA **System. Web. http. AuthorizeAttribute**.

Ora solo gli amministratori possono visualizzare la pagina di amministrazione. Se inoltre si invia una richiesta HTTP al controller di amministrazione, la richiesta deve contenere un cookie di autenticazione. In caso contrario, il server invia una risposta HTTP 401 (non autorizzato). È possibile visualizzarlo in Fiddler inviando una richiesta GET a `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Precedente](using-web-api-with-entity-framework-part-3.md)
> [Successivo](using-web-api-with-entity-framework-part-5.md)
