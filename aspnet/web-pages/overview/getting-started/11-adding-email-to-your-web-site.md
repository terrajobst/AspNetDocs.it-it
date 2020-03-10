---
uid: web-pages/overview/getting-started/11-adding-email-to-your-web-site
title: Invio di posta elettronica da un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: In questo capitolo viene illustrato come inviare un messaggio di posta elettronica automatizzato da un sito Web.
ms.author: riande
ms.date: 02/20/2014
ms.assetid: fc49bcb9-f1a9-4048-8c3f-b60951853200
msc.legacyurl: /web-pages/overview/getting-started/11-adding-email-to-your-web-site
msc.type: authoredcontent
ms.openlocfilehash: 23e9717329525fb5a0ed505c9dc94505d4f9dbbe
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78634715"
---
# <a name="sending-email-from-an-aspnet-web-pages-razor-site"></a>Invio di posta elettronica da un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo illustra come inviare un messaggio di posta elettronica da un sito Web quando si usa Pagine Web ASP.NET (Razor).
> 
> Contenuto dell'esercitazione:
> 
> - Inviare un messaggio di posta elettronica dal sito Web.
> - Come aggiungere un file a un messaggio di posta elettronica.
> 
> Questa è la funzionalità ASP.NET introdotta nell'articolo:
> 
> - Helper `WebMail`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

<a id="Sending_Email_Messages"></a>
## <a name="sending-email-messages-from-your-website"></a>Invio di messaggi di posta elettronica dal sito Web

Ci sono diversi motivi per cui potrebbe essere necessario inviare messaggi di posta elettronica dal sito Web. È possibile inviare messaggi di conferma agli utenti o inviare notifiche a se stessi, ad esempio se un nuovo utente ha effettuato la registrazione. Il `WebMail` Helper semplifica l'invio di messaggi di posta elettronica.

Per usare l'helper `WebMail`, è necessario avere accesso a un server SMTP. (SMTP è l'acronimo di *Simple Mail Transfer Protocol*). Un server SMTP è un server di posta elettronica che trasmette solo i messaggi al server &#8212; del destinatario perché è il lato in uscita del messaggio di posta elettronica. Se si usa un provider di hosting per il sito Web, è probabile che si sia configurato con un messaggio di posta elettronica e che sia in grado di indicare il nome del server SMTP. Se si lavora all'interno di una rete aziendale, un amministratore o il reparto IT può in genere fornire le informazioni su un server SMTP che è possibile usare. Se si lavora a casa, potrebbe anche essere possibile eseguire il test usando il provider di posta elettronica comune, che può indicare il nome del server SMTP. È in genere necessario:

- Nome del server SMTP.
- Numero di porta. Si tratta quasi sempre di 25. Tuttavia, il provider di servizi Internet potrebbe richiedere l'uso della porta 587. Se si utilizza Secure Sockets Layer (SSL) per la posta elettronica, potrebbe essere necessaria una porta diversa. Rivolgersi al provider di posta elettronica.
- Credenziali (nome utente, password).

In questa procedura vengono create due pagine. La prima pagina dispone di un modulo che consente agli utenti di immettere una descrizione, come se compilassero un modulo di supporto tecnico. La prima pagina Invia le informazioni a una seconda pagina. Nella seconda pagina, il codice estrae le informazioni dell'utente e invia un messaggio di posta elettronica. Viene inoltre visualizzato un messaggio che conferma la ricezione del report sul problema.

![[immagine]](11-adding-email-to-your-web-site/_static/image1.jpg)

> [!NOTE]
> Per semplificare questo esempio, il codice inizializza il `WebMail` Helper direttamente nella pagina in cui viene usato. Tuttavia, per i siti Web reali, è preferibile inserire codice di inizializzazione simile al seguente in un file globale, in modo da inizializzare l'helper `WebMail` per tutti i file nel sito Web. Per ulteriori informazioni, vedere [personalizzazione del comportamento a livello di sito per pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906#Setting_Values_For_Helpers).

1. Creare un nuovo sito Web.
2. Aggiungere una nuova pagina denominata *EmailRequest. cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample1.html)]

    Si noti che l'attributo `action` dell'elemento form è stato impostato su *ProcessRequest. cshtml*. Questo significa che il modulo verrà inviato a tale pagina anziché tornare alla pagina corrente.
3. Aggiungere una nuova pagina denominata *ProcessRequest. cshtml* al sito Web e aggiungere il codice e il markup seguenti:   

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample2.cshtml)]

    Nel codice si ottengono i valori dei campi del modulo inviati alla pagina. Chiamare quindi il metodo di `Send` dell'helper `WebMail` per creare e inviare il messaggio di posta elettronica. In questo caso, i valori da utilizzare sono costituiti da testo concatenato con i valori inviati dal form.

    Il codice per questa pagina si trova all'interno di un blocco `try/catch`. Se per qualsiasi motivo il tentativo di inviare un messaggio di posta elettronica non funziona (ad esempio, le impostazioni non sono corrette), il codice nel blocco `catch` viene eseguito e imposta la variabile `errorMessage` sull'errore che si è verificato. Per ulteriori informazioni sui blocchi di `try/catch` o sul tag di `<text>`, vedere [Introduzione alla programmazione di pagine Web ASP.NET utilizzando la sintassi Razor](https://go.microsoft.com/fwlink/?LinkID=251587#ID_HandlingErrors).

    Nel corpo della pagina, se la variabile `errorMessage` è vuota (impostazione predefinita), l'utente visualizza un messaggio che indica che il messaggio di posta elettronica è stato inviato. Se la variabile `errorMessage` è impostata su true, l'utente visualizza un messaggio che indica che si è verificato un problema durante l'invio del messaggio.

    Si noti che nella parte della pagina che visualizza un messaggio di errore è presente un test aggiuntivo: `if(debuggingFlag)`. Si tratta di una variabile che può essere impostata su true se si verificano problemi durante l'invio della posta elettronica. Quando `debuggingFlag` è true e se si verifica un problema durante l'invio di messaggi di posta elettronica, viene visualizzato un messaggio di errore aggiuntivo che Mostra qualsiasi ASP.NET segnalato durante il tentativo di inviare il messaggio di posta elettronica. Avviso equo, tuttavia: i messaggi di errore che ASP.NET segnala quando non è possibile inviare un messaggio di posta elettronica possono essere generici. Se, ad esempio, ASP.NET non è in grado di contattare il server SMTP, ad esempio perché è stato creato un errore nel nome del server, l'errore viene `Failure sending mail`.

    > [!NOTE] 
    > 
    > **Importante** Quando si riceve un messaggio di errore da un oggetto eccezione (`ex` nel codice), *non* passare periodicamente il messaggio agli utenti. Gli oggetti eccezione includono spesso informazioni che gli utenti non dovrebbero visualizzare e che possono anche essere una vulnerabilità di sicurezza. Questo è il motivo per cui questo codice include la variabile `debuggingFlag` usata come opzione per visualizzare il messaggio di errore e perché la variabile per impostazione predefinita è impostata su false. È necessario impostare la variabile su true (e pertanto visualizzare il messaggio di errore) *solo* se si verifica un problema con l'invio di posta elettronica ed è necessario eseguire il debug. Una volta risolti i problemi, impostare di nuovo `debuggingFlag` su false.

    Modificare le seguenti impostazioni relative al messaggio di posta elettronica nel codice:

   - Impostare `your-SMTP-host` sul nome del server SMTP a cui si ha accesso.
   - Impostare `your-user-name-here` sul nome utente per l'account del server SMTP.
   - Impostare `your-account-password` sulla password per l'account del server SMTP.
   - Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio. Alcuni provider di posta elettronica non consentono di specificare un indirizzo di `From` diverso e utilizzeranno il nome utente come indirizzo di `From`.

     > [!TIP] 
     > 
     > <a id="configuring_email_settings"></a>
     > ### <a name="configuring-email-settings"></a>Configurazione delle impostazioni di posta elettronica
     > 
     > In alcuni casi può essere difficile verificare di disporre delle impostazioni corrette per il server SMTP, il numero di porta e così via. Di seguito sono riportati alcuni suggerimenti:
     > 
     > - Il nome del server SMTP è spesso simile `smtp.provider.com` o `smtp.provider.net`. Tuttavia, se si pubblica il sito in un provider di hosting, il nome del server SMTP in quel momento potrebbe essere `localhost`. Questo avviene perché dopo la pubblicazione e il sito è in esecuzione nel server del provider, il server di posta elettronica potrebbe essere locale dal punto di vista dell'applicazione. Questa modifica nei nomi dei server potrebbe significare che è necessario modificare il nome del server SMTP come parte del processo di pubblicazione.
     > - Il numero di porta è in genere 25. Tuttavia, per alcuni provider è necessario usare la porta 587 o un'altra porta.
     > - Assicurarsi di usare le credenziali corrette. Se il sito è stato pubblicato in un provider di hosting, utilizzare le credenziali indicate in modo specifico dal provider per la posta elettronica. Potrebbero essere diverse dalle credenziali usate per la pubblicazione.
     > - In alcuni casi non è necessaria alcuna credenziale. Se si sta inviando un messaggio di posta elettronica con l'ISP personale, è possibile che il provider di posta elettronica conosca già le credenziali. Dopo la pubblicazione, potrebbe essere necessario utilizzare credenziali diverse rispetto a quando si esegue il test nel computer locale.
     > - Se il provider di posta elettronica utilizza la crittografia, è necessario impostare `WebMail.EnableSsl` su `true`.
4. Eseguire la pagina *EmailRequest. cshtml* in un browser. Assicurarsi che la pagina sia selezionata nell'area di lavoro **file** prima di eseguirla.
5. Immettere il nome e la descrizione del problema, quindi fare clic sul pulsante **Submit (Invia** ). Si viene reindirizzati alla pagina *ProcessRequest. cshtml* , che conferma il messaggio e che invia un messaggio di posta elettronica. 

    ![[immagine]](11-adding-email-to-your-web-site/_static/image2.jpg)

<a id="Sending_a_File"></a>
## <a name="sending-a-file-using-email"></a>Invio di un file tramite posta elettronica

È anche possibile inviare i file collegati ai messaggi di posta elettronica. In questa procedura vengono creati un file di testo e due pagine HTML. Il file di testo verrà usato come allegato di posta elettronica.

1. Nel sito Web aggiungere un nuovo file di testo e denominarlo *MyFile. txt*.
2. Copiare il testo seguente e incollarlo nel file: 

    `Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.`
3. Creare una pagina denominata *sendfile. cshtml* e aggiungere il markup seguente: 

    [!code-html[Main](11-adding-email-to-your-web-site/samples/sample3.html)]
4. Creare una pagina denominata *ProcessFile. cshtml* e aggiungere il markup seguente: 

    [!code-cshtml[Main](11-adding-email-to-your-web-site/samples/sample4.cshtml)]
5. Modificare le seguenti impostazioni relative al messaggio di posta elettronica nel codice dall'esempio:

    - Impostare `your-SMTP-host` sul nome di un server SMTP a cui si ha accesso.
    - Impostare `your-user-name-here` sul nome utente per l'account del server SMTP.
    - Impostare `your-email-address-here` per il proprio indirizzo di posta elettronica. Si tratta dell'indirizzo di posta elettronica da cui viene inviato il messaggio.
    - Impostare `your-account-password` sulla password per l'account del server SMTP.
    - Impostare `target-email-address-here` per il proprio indirizzo di posta elettronica. Come prima, in genere si invia un messaggio di posta elettronica a un altro utente, ma per i test è possibile inviarlo a se stessi.
6. Eseguire la pagina *sendfile. cshtml* in un browser.
7. Immettere il nome, la riga dell'oggetto e il nome del file di testo da aggiungere (*MyFile. txt*).
8. Fare clic sul pulsante `Submit`. Come in precedenza, si viene reindirizzati alla pagina *ProcessFile. cshtml* , che conferma il messaggio e che invia un messaggio di posta elettronica con il file allegato.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

- [Guida alla risoluzione dei problemi delle pagine Web ASP.NET (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)
- [Simple Mail Transfer Protocol](https://msdn.microsoft.com/library/aa480435.aspx)
- [Personalizzazione del comportamento a livello di sito per Pagine Web ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202906)
