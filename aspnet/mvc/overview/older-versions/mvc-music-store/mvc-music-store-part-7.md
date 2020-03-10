---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: appartenenza e autorizzazione | Microsoft Docs'
author: jongalloway
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 7 riguarda l'appartenenza e l'autorizzazione.
ms.author: riande
ms.date: 10/13/2010
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a6a1a936e0ea29ea36721ba78f35845401f74c01
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78539200"
---
# <a name="part-7-membership-and-authorization"></a>Parte 7: appartenenza e autorizzazione

di [Jon Galloway](https://github.com/jongalloway)

> Music Store MVC è un'applicazione di esercitazione che introduce e spiega passo passo come usare ASP.NET MVC e Visual Studio per lo sviluppo Web.  
>   
> Music Store MVC è un semplice esempio di implementazione di un archivio per un negozio che vende album musicali online e implementa l'amministrazione di base del sito, l'accesso dell'utente e la funzionalità del carrello acquisti.  
>   
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio ASP.NET MVC Music Store. La parte 7 riguarda l'appartenenza e l'autorizzazione.

Il controller di Store Manager è attualmente accessibile a chiunque visiti il sito. Modificare questa opzione per limitare l'autorizzazione agli amministratori del sito.

## <a name="adding-the-accountcontroller-and-views"></a>Aggiunta di AccountController e views

Una differenza tra il modello di applicazione Web ASP.NET MVC 3 completo e il modello di applicazione Web vuota ASP.NET MVC 3 è che il modello vuoto non include un controller di account. Si aggiungerà un controller di account copiando alcuni file da una nuova applicazione MVC ASP.NET creata dal modello di applicazione Web ASP.NET MVC 3 completo.

Creare una nuova applicazione MVC ASP.NET usando il modello di applicazione Web ASP.NET MVC 3 completo e copiare i file seguenti nelle stesse directory del progetto:

1. Copiare AccountController.cs nella directory Controllers
2. Copiare AccountModels nella directory Models
3. Creare una directory dell'account all'interno della directory views e copiare tutte e quattro le visualizzazioni in

Modificare lo spazio dei nomi per le classi controller e modello in modo che inizino con MvcMusicStore. La classe AccountController deve usare lo spazio dei nomi MvcMusicStore. Controllers e la classe AccountModels deve usare lo spazio dei nomi MvcMusicStore. Models.

*Nota: questi file sono disponibili anche nel download MvcMusicStore-Assets. zip dal quale sono stati copiati i file di progettazione del sito all'inizio dell'esercitazione. I file di appartenenza si trovano nella directory del codice.*

La soluzione aggiornata dovrebbe essere simile alla seguente:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Aggiunta di un utente amministratore con il sito di configurazione di ASP.NET

Prima di richiedere l'autorizzazione nel sito Web, è necessario creare un utente con accesso. Il modo più semplice per creare un utente consiste nell'usare il sito Web di configurazione di ASP.NET incorporato.

Avviare il sito Web di configurazione di ASP.NET facendo clic sull'icona seguente nell'Esplora soluzioni.

![](mvc-music-store-part-7/_static/image2.png)

Verrà avviato un sito Web di configurazione. Fare clic sulla scheda sicurezza nella schermata iniziale, quindi fare clic sul collegamento "Abilita ruoli" al centro della schermata.

![](mvc-music-store-part-7/_static/image3.png)

Fare clic sul collegamento "crea o Gestisci ruoli".

![](mvc-music-store-part-7/_static/image4.png)

Immettere "Administrator" come nome del ruolo e premere il pulsante Aggiungi ruolo.

![](mvc-music-store-part-7/_static/image5.png)

Fai clic sul pulsante indietro, quindi fai clic sul collegamento Crea utente sul lato sinistro.

![](mvc-music-store-part-7/_static/image6.png)

Inserire i campi delle informazioni utente a sinistra usando le informazioni seguenti:

| **Campo** | **Valore** |
| --- | --- |
| **Nome utente** | Amministratore |
| **Password** | password123! |
| **Conferma password** | password123! |
| **Posta elettronica** | (qualsiasi indirizzo di posta elettronica funzionerà) |
| **Domanda segreta** | (qualsiasi cosa piaccia) |
| **Risposta segreta** | (qualsiasi cosa piaccia) |

*Nota: naturalmente, è possibile usare qualsiasi password. Le impostazioni di sicurezza predefinite per la password richiedono una password con una lunghezza di 7 caratteri e contengono un carattere non alfanumerico.*

Selezionare il ruolo di amministratore per l'utente e fare clic sul pulsante Crea utente.

![](mvc-music-store-part-7/_static/image7.png)

A questo punto, verrà visualizzato un messaggio che indica che l'utente è stato creato correttamente.

![](mvc-music-store-part-7/_static/image8.png)

È ora possibile chiudere la finestra del browser.

## <a name="role-based-authorization"></a>Autorizzazione basata sui ruoli

È ora possibile limitare l'accesso a StoreManagerController usando l'attributo [autorizzate], specificando che l'utente deve avere il ruolo di amministratore per accedere a qualsiasi azione del controller nella classe.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Nota: l'attributo [autorizzate] può essere posizionato su metodi di azione specifici, oltre che a livello di classe del controller.*

Ora passare a/StoreManager Visualizza una finestra di dialogo di accesso:

![](mvc-music-store-part-7/_static/image9.png)

Dopo aver eseguito l'accesso con il nuovo account amministratore, è possibile passare alla schermata di modifica dell'album come prima.

> [!div class="step-by-step"]
> [Precedente](mvc-music-store-part-6.md)
> [Successivo](mvc-music-store-part-8.md)
