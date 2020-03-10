---
uid: web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
title: Personalizzazione del comportamento a livello di sito per i siti Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo capitolo illustra come apportare le impostazioni all'intero sito Web o a un'intera cartella, anziché solo a una pagina.
ms.author: riande
ms.date: 02/17/2014
ms.assetid: e158bed7-226f-4275-b02e-7553bd58c669
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/18-customizing-site-wide-behavior
msc.type: authoredcontent
ms.openlocfilehash: f05e05f725d9209bce283ce18659ae5efe4de2ee
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634624"
---
# <a name="customizing-site-wide-behavior-for-aspnet-web-pages-razor-sites"></a>Personalizzazione del comportamento a livello di sito per i siti Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come creare impostazioni lato sito per le pagine in un sito Web di Pagine Web ASP.NET (Razor).
> 
> Contenuto dell'esercitazione:
> 
> - Come eseguire codice che consente di impostare valori (valori globali o impostazioni Helper) per tutte le pagine di un sito.
> - Come eseguire codice che consente di impostare i valori per tutte le pagine in una cartella.
> - Come eseguire il codice prima e dopo il caricamento di una pagina.
> - Come inviare gli errori a una pagina di errore centrale.
> - Come aggiungere l'autenticazione a tutte le pagine di una cartella.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 2
> - WebMatrix 3
> - ASP.NET Web Helper Library (pacchetto NuGet)
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 3 e Visual Studio 2013 (o Visual Studio Express 2013 per il Web), ad eccezione del fatto che non è possibile usare la libreria ASP.NET Web Helper.

<a id="Adding_Website_Startup_Code"></a>
## <a name="adding-website-startup-code-for-aspnet-web-pages"></a>Aggiunta del codice di avvio del sito Web per Pagine Web ASP.NET

Per gran parte del codice scritto in Pagine Web ASP.NET, una singola pagina può contenere tutto il codice necessario per la pagina. Se, ad esempio, una pagina Invia un messaggio di posta elettronica, è possibile inserire tutto il codice per l'operazione in un'unica pagina. Questo può includere il codice per inizializzare le impostazioni per l'invio di messaggi di posta elettronica, ovvero per il server SMTP, e per l'invio del messaggio di posta elettronica.

Tuttavia, in alcune situazioni, potrebbe essere necessario eseguire del codice prima che venga eseguita una pagina nel sito. Questa operazione è utile per l'impostazione di valori che possono essere utilizzati in qualsiasi punto del sito, detti *valori globali*. Alcuni helper, ad esempio, richiedono la specifica di valori quali le impostazioni di posta elettronica o le chiavi dell'account. Può essere utile tenere queste impostazioni in valori globali.

A tale scopo, è possibile creare una pagina denominata *\_AppStart. cshtml* nella radice del sito. Se questa pagina esiste, viene eseguita la prima volta che viene richiesta una pagina nel sito. Pertanto, è opportuno eseguire codice per impostare i valori globali. Poiché *\_AppStart. cshtml* ha un prefisso di sottolineatura, ASP.NET non invierà la pagina a un browser anche se gli utenti lo richiedono direttamente.

Il diagramma seguente illustra il funzionamento della pagina *\_AppStart. cshtml* . Quando viene ricevuta una richiesta per una pagina e, se questa è la prima richiesta per una pagina nel sito, ASP.NET controlla prima di tutto se esiste una pagina *\_AppStart. cshtml* . In tal caso, viene eseguito il codice della pagina *\_AppStart. cshtml* e quindi viene eseguita la pagina richiesta.

![[immagine]](18-customizing-site-wide-behavior/_static/image1.jpg)

## <a name="setting-global-values-for-your-website"></a>Impostazione dei valori globali per il sito Web

1. Nella cartella radice di un sito Web di WebMatrix creare un file denominato *\_AppStart. cshtml*. Il file deve trovarsi nella radice del sito.
2. Sostituire il contenuto esistente con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample1.cshtml)]

    Questo codice archivia un valore nel dizionario `AppState`, che è automaticamente disponibile per tutte le pagine del sito. Si noti che il file *\_AppStart. cshtml* non contiene markup. La pagina eseguirà il codice e quindi reindirizza alla pagina che è stata originariamente richiesta.

    > [!NOTE]
    > Prestare attenzione quando si inserisce il codice nel file *\_AppStart. cshtml* . Se si verificano errori nel codice del file *\_AppStart. cshtml* , il sito Web non verrà avviato.
3. Nella cartella radice creare una nuova pagina denominata *appname. cshtml*.
4. Sostituire il markup e il codice predefiniti con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample2.html)]

    Questo codice estrae il valore dall'oggetto `AppState` impostato nella pagina *\_AppStart. cshtml* .
5. Eseguire la pagina *appname. cshtml* in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla. Nella pagina viene visualizzato il valore globale. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image2.jpg)

<a id="Setting_Values_For_Helpers"></a>
## <a name="setting-values-for-helpers"></a>Impostazione di valori per Helper

Per il *\_file AppStart. cshtml* è consigliabile impostare i valori per gli helper usati nel sito e che devono essere inizializzati. Esempi tipici sono le impostazioni di posta elettronica per l'helper `WebMail` e le chiavi pubbliche e private per l'helper `ReCaptcha`. In casi come questi, è possibile impostare i valori una volta nel *\_AppStart. cshtml* e quindi sono già impostati per tutte le pagine del sito.

Questa procedura illustra come impostare le impostazioni `WebMail` a livello globale. Per ulteriori informazioni sull'utilizzo dell'helper `WebMail`, vedere l'argomento relativo all' [aggiunta di messaggi di posta elettronica a un sito pagine Web ASP.NET](../getting-started/11-adding-email-to-your-web-site.md).

1. Aggiungere la libreria ASP.NET Web Helper al sito Web come descritto in [installazione di helper in un sito di pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=252372), se non è già stato aggiunto.
2. Se non si dispone già di un file *\_AppStart. cshtml* , nella cartella radice di un sito Web creare un file denominato *\_AppStart. cshtml*.
3. Aggiungere le impostazioni di `WebMail` seguenti al file *\_AppStart. cshtml* : 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample3.cshtml?highlight=2-7)]

    Modificare le seguenti impostazioni relative al messaggio di posta elettronica nel codice:

   - Impostare `your-SMTP-host` sul nome del server SMTP a cui si ha accesso.
   - Impostare `your-user-name-here` sul nome utente per l'account del server SMTP.
   - Impostare `your-account-password` sulla password per l'account del server SMTP.
   - Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio. Alcuni provider di posta elettronica non consentono di specificare un indirizzo di `From` diverso e utilizzeranno il nome utente come indirizzo di `From`.

     Per ulteriori informazioni sulle impostazioni SMTP, vedere [Configuring email settings](https://go.microsoft.com/fwlink/?LinkID=202899#configuring_email_settings) nell'articolo [invio di messaggi di posta elettronica da un sito di pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkID=202899) e [problemi di invio di messaggi di posta elettronica](https://go.microsoft.com/fwlink/?LinkId=253001#email) nella [Guida alla risoluzione dei problemi di pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).
4. Salvare il file *\_AppStart. cshtml* e chiuderlo.
5. Nella cartella radice di un sito Web creare una nuova pagina denominata *TestEmail. cshtml*.
6. Sostituire il contenuto esistente con il codice seguente: 

     [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample4.cshtml)]
7. Eseguire la pagina *TestEmail. cshtml* in un browser.
8. Compilare i campi per inviare a se stessi un messaggio di posta elettronica e quindi fare clic su **Send**.
9. Controllare la posta elettronica per assicurarsi di aver ricevuto il messaggio.

La parte importante di questo esempio è che le impostazioni che in genere non cambiano, come il nome del server SMTP e le credenziali di posta elettronica, sono impostate nel file *\_AppStart. cshtml* . In questo modo non è necessario impostarli nuovamente in ogni pagina in cui si invia un messaggio di posta elettronica. Se per qualche motivo è necessario modificare queste impostazioni, è possibile impostarle singolarmente in una pagina. Nella pagina è possibile impostare solo i valori che in genere cambiano ogni volta, ad esempio il destinatario e il corpo del messaggio di posta elettronica.

<a id="Running_Code_Before_and_After"></a>
## <a name="running-code-before-and-after-files-in-a-folder"></a>Esecuzione di codice prima e dopo i file in una cartella

Analogamente a quanto si può usare *\_AppStart. cshtml* per scrivere codice prima dell'esecuzione di pagine nel sito, è possibile scrivere codice che viene eseguito prima e dopo l'esecuzione di qualsiasi pagina in una cartella specifica. Questa operazione è utile per elementi come l'impostazione della stessa pagina di layout per tutte le pagine in una cartella o per la verifica dell'accesso di un utente prima di eseguire una pagina nella cartella.

Per le pagine in particolari cartelle, è possibile creare codice in un file denominato *\_PageStart. cshtml*. Il diagramma seguente illustra il funzionamento della pagina *\_PageStart. cshtml* . Quando viene ricevuta una richiesta per una pagina, ASP.NET verifica innanzitutto la presenza di una pagina *\_AppStart. cshtml* e la esegue. Quindi, ASP.NET controlla se è presente una pagina *\_PageStart. cshtml* e, in tal caso, lo esegue. Esegue quindi la pagina richiesta.

All'interno della pagina *\_PageStart. cshtml* è possibile specificare la posizione in cui si desidera eseguire la pagina richiesta includendo un metodo di `RunPage`. In questo modo è possibile eseguire il codice prima dell'esecuzione della pagina richiesta e quindi di nuovo dopo di esso. Se non si include `RunPage`, viene eseguito tutto il codice *\_PageStart. cshtml* e quindi la pagina richiesta viene eseguita automaticamente.

![[immagine]](18-customizing-site-wide-behavior/_static/image3.jpg)

ASP.NET consente di creare una gerarchia di *\_file PageStart. cshtml* . È possibile inserire un *\_file PageStart. cshtml* nella radice del sito e in qualsiasi sottocartella. Quando viene richiesta una pagina, il *\_file PageStart. cshtml* al livello superiore (più vicino alla radice del sito) viene eseguito, seguito dal file *\_PageStart. cshtml* nella sottocartella successiva e così via, nella struttura della sottocartella fino a quando la richiesta non raggiunge la cartella che contiene la pagina richiesta. Dopo l'esecuzione di tutti i file *\_PageStart. cshtml* applicabili, viene eseguita la pagina richiesta.

Ad esempio, si potrebbe avere la combinazione seguente di *\_file PageStart. cshtml* e il file *default. cshtml* :

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample5.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample6.cshtml)]

[!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample7.cshtml)]

Quando si esegue */MyFolder/default.cshtml*, viene visualizzato quanto segue:

[!code-console[Main](18-customizing-site-wide-behavior/samples/sample8.cmd)]

## <a name="running-initialization-code-for-all-pages-in-a-folder"></a>Esecuzione del codice di inizializzazione per tutte le pagine di una cartella

Un valido utilizzo per *\_file PageStart. cshtml* è l'inizializzazione della stessa pagina di layout per tutti i file in una singola cartella.

1. Nella cartella radice creare una nuova cartella denominata *InitPages*.
2. Nella cartella *InitPages* del sito Web creare un file denominato *\_PageStart. cshtml* e sostituire il markup e il codice predefiniti con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample9.cshtml)]
3. Nella radice del sito Web creare una cartella denominata *Shared*.
4. Nella cartella *condivisa* creare un file denominato *\_Layout1. cshtml* e sostituire il markup e il codice predefiniti con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample10.cshtml)]
5. Nella cartella *InitPages* creare un file denominato *Content1. cshtml* e sostituire il contenuto esistente con il codice seguente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample11.html)]
6. Nella cartella *InitPages* creare un altro file denominato *Content2. cshtml* e sostituire il markup predefinito con quanto segue: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample12.html)]
7. Eseguire *Content1. cshtml* in un browser. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image4.jpg)

    Quando viene eseguita la pagina *Content1. cshtml* , il file *\_PageStart. cshtml* imposta `Layout` e imposta anche `PageData["MyBackground"]` su un colore. In *Content1. cshtml*viene applicato il layout e il colore.
8. Visualizza *Content2. cshtml* in un browser. 

    Il layout è lo stesso, perché entrambe le pagine usano la stessa pagina di layout e il colore inizializzato in *\_PageStart. cshtml*.

## <a name="using-_pagestartcshtml-to-handle-errors"></a>Utilizzo di \_PageStart. cshtml per gestire gli errori

Un altro valido utilizzo del file *\_PageStart. cshtml* consiste nel creare un modo per gestire gli errori di programmazione (eccezioni) che possono verificarsi in qualsiasi pagina *. cshtml* in una cartella. Questo esempio illustra un modo per eseguire questa operazione.

1. Nella cartella radice creare una cartella denominata *InitCatch*.
2. Nella cartella *InitCatch* del sito Web creare un file denominato *\_PageStart. cshtml* e sostituire il markup e il codice esistenti con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample13.cshtml)]

    In questo codice provare a eseguire la pagina richiesta in modo esplicito chiamando il metodo `RunPage` all'interno di un blocco di `try`. Se si verificano errori di programmazione nella pagina richiesta, il codice all'interno del blocco `catch` viene eseguito. In questo caso, il codice reindirizza a una pagina (*Error. cshtml*) e passa il nome del file in cui si è verificato l'errore come parte dell'URL. La pagina verrà creata a breve.
3. Nella cartella *InitCatch* del sito Web creare un file denominato *Exception. cshtml* e sostituire il markup e il codice esistenti con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample14.cshtml)]

    Ai fini di questo esempio, ciò che si sta facendo in questa pagina sta deliberatamente creando un errore tentando di aprire un file di database che non esiste.
4. Nella cartella radice creare un file denominato *Error. cshtml* e sostituire il markup e il codice esistenti con il codice seguente: 

    [!code-html[Main](18-customizing-site-wide-behavior/samples/sample15.html)]

    In questa pagina, l'espressione `@Request["source"]` ottiene il valore dall'URL e lo Visualizza.
5. Sulla barra degli strumenti fare clic su **Salva**.
6. Eseguire *Exception. cshtml* in un browser. 

    ![[immagine]](18-customizing-site-wide-behavior/_static/image5.jpg)

    Poiché si verifica un errore in *Exception. cshtml*, la pagina *\_PageStart. cshtml* reindirizza al file *Error. cshtml* , che Visualizza il messaggio.

    Per ulteriori informazioni sulle eccezioni, vedere [Introduction to pagine Web ASP.NET Programming using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkID=251587).

<a id="Using__PageStart.cshtml_to_Restrict_Folder_Access"></a>
## <a name="using-_pagestartcshtml-to-restrict-folder-access"></a>Uso di \_PageStart. cshtml per limitare l'accesso alle cartelle

È anche possibile usare il file *\_PageStart. cshtml* per limitare l'accesso a tutti i file in una cartella.

1. In WebMatrix creare un nuovo sito Web usando l'opzione **sito da modello** .
2. Scegliere **sito iniziale**dai modelli disponibili.
3. Nella cartella radice creare una cartella denominata *AuthenticatedContent*.
4. Nella cartella *AuthenticatedContent* creare un file denominato *\_PageStart. cshtml* e sostituire il markup e il codice esistenti con il codice seguente: 

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample16.cshtml)]

    Il codice inizia impedendo la memorizzazione nella cache di tutti i file nella cartella. Questa operazione è necessaria per gli scenari, ad esempio i computer pubblici, in cui non si vuole che le pagine memorizzate nella cache di un utente siano disponibili per l'utente successivo. Successivamente, il codice determina se l'utente ha eseguito l'accesso al sito prima di poter visualizzare le pagine della cartella. Se l'utente non è connesso, il codice viene reindirizzato alla pagina di accesso. La pagina di accesso può restituire l'utente alla pagina richiesta originariamente se si include un valore di stringa di query denominato `ReturnUrl`.
5. Creare una nuova pagina nella cartella *AuthenticatedContent* denominata *Page. cshtml*.
6. Sostituire il markup predefinito con quanto segue:  

    [!code-cshtml[Main](18-customizing-site-wide-behavior/samples/sample17.cshtml)]
7. Eseguire *Page. cshtml* in un browser. Il codice reindirizza l'utente a una pagina di accesso. Prima di eseguire l'accesso, è necessario registrarsi. Dopo aver eseguito la registrazione e l'accesso, è possibile passare alla pagina e visualizzarne il contenuto.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Introduzione alla programmazione Pagine Web ASP.NET con la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587)
