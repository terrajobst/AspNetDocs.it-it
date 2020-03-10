---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Configurazione dell'autenticazione basata su form eC#argomenti avanzati () | Microsoft Docs
author: rick-anderson
description: In questa esercitazione si esamineranno le varie impostazioni di autenticazione basata su form e si vedrà come modificarle tramite l'elemento Forms. Questa operazione comporterà una descrizione dettagliata...
ms.author: riande
ms.date: 01/14/2008
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: b296f31da1c73df97175d94402b4d618df425d8d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78641771"
---
# <a name="forms-authentication-configuration-and-advanced-topics-c"></a>Configurazione dell'autenticazione basata su form e argomenti avanzati (C#)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scarica codice](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) o [Scarica PDF](https://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> In questa esercitazione si esamineranno le varie impostazioni di autenticazione basata su form e si vedrà come modificarle tramite l'elemento Forms. Questo comporterà una descrizione dettagliata della personalizzazione del valore di timeout del ticket di autenticazione basata su form, usando una pagina di accesso con un URL personalizzato, ad esempio signin. aspx anziché login. aspx, e ticket di autenticazione basata su form senza cookie.

## <a name="introduction"></a>Introduzione

Nell' [esercitazione precedente](an-overview-of-forms-authentication-cs.md) sono stati esaminati i passaggi necessari per implementare l'autenticazione basata su form in un'applicazione ASP.NET, da specificare le impostazioni di configurazione in Web. config per creare una pagina di accesso per visualizzare contenuti diversi per utenti autenticati e anonimi. Si ricordi che il sito Web è stato configurato per l'uso dell'autenticazione basata su form impostando l'attributo mode dell'elemento &lt;Authentication&gt; su Forms. L'elemento &lt;Authentication&gt; può includere facoltativamente un elemento &lt;Forms&gt; elemento figlio, tramite il quale è possibile specificare un'ampia gamma di impostazioni di autenticazione basata su form.

In questa esercitazione vengono esaminate le diverse impostazioni di autenticazione basata su form e viene illustrato come modificarle tramite l'elemento &lt;Forms&gt;. Questo comporterà una descrizione dettagliata della personalizzazione del valore di timeout del ticket di autenticazione basata su form, usando una pagina di accesso con un URL personalizzato, ad esempio signin. aspx anziché login. aspx, e ticket di autenticazione basata su form senza cookie. Si esaminerà anche il trucco del ticket di autenticazione basata su form e si vedranno le precauzioni necessarie per assicurarsi che i dati del ticket siano protetti da ispezione e manomissioni. Infine, si esaminerà come archiviare dati utente aggiuntivi nel ticket di autenticazione basata su form e come modellare i dati tramite un oggetto Principal personalizzato.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Passaggio 1: esame dei moduli &lt;&gt; impostazioni di configurazione

Il sistema di autenticazione basata su form in ASP.NET offre una serie di impostazioni di configurazione che possono essere personalizzate in base alle singole applicazioni. Sono incluse impostazioni come: la durata del ticket di autenticazione basata su form; quale tipo di protezione viene applicato al ticket; in quali condizioni vengono usati i ticket di autenticazione senza cookie; percorso della pagina di accesso. e altre informazioni. Per modificare i valori predefiniti, aggiungere un [&lt;forms&gt; elemento](https://msdn.microsoft.com/library/1d3t3c61.aspx) come figlio dell' [elemento&lt;Authentication&gt;](https://msdn.microsoft.com/library/532aee0e.aspx), specificando i valori di proprietà che si desidera personalizzare come attributi XML come segue:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Nella tabella 1 sono riepilogate le proprietà che è possibile personalizzare tramite l'elemento &lt;Forms&gt;. Poiché Web. config è un file XML, i nomi degli attributi nella colonna sinistra fanno distinzione tra maiuscole e minuscole.

| <strong>Attributo</strong> |                                                                                                                                                                                                                                     <strong>Descrizione</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         Cookieless         |                                                                                                                Questo attributo specifica le condizioni in base alle quali il ticket di autenticazione viene archiviato in un cookie anziché incorporato nell'URL. I valori consentiti sono: UseCookies; UseUri Rilevamento automatico e UseDeviceProfile (impostazione predefinita). Il passaggio 2 esamina questa impostazione in modo più dettagliato.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Indica l'URL a cui gli utenti vengono reindirizzati dopo l'accesso dalla pagina di accesso se non è stato specificato alcun valore RedirectUrl in QueryString. Il valore predefinito è default. aspx.                                                                                                                                                         |
|           dominio           | Quando si usano i ticket di autenticazione basati su cookie, questa impostazione specifica il valore del dominio del cookie. Il valore predefinito è una stringa vuota, che fa sì che il browser usi il dominio da cui è stato emesso, ad esempio www.yourdomain.com. In questo caso, il cookie <strong>non</strong> verrà inviato quando si effettuano richieste a sottodomini, ad esempio admin.yourdomain.com. Se si desidera che il cookie venga passato a tutti i sottodomini, è necessario personalizzare l'attributo di dominio impostandolo su yourdomain.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Valore booleano che indica se gli utenti autenticati vengono ricordati quando vengono reindirizzati agli URL in altre applicazioni Web nello stesso server. Il valore predefinito è false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      URL della pagina di accesso. Il valore predefinito è login. aspx.                                                                                                                                                                                                                      |
|            name            |                                                                                                                                                                                                   Quando si usano i ticket di autenticazione basati su cookie, il nome del cookie. Il valore predefinito è. ASPXAUTH.                                                                                                                                                                                                   |
|            path            |                                                                             Quando si usano i ticket di autenticazione basati su cookie, questa impostazione specifica l'attributo path del cookie. L'attributo Path consente a uno sviluppatore di limitare l'ambito di un cookie a una particolare gerarchia di directory. Il valore predefinito è/, che informa il browser di inviare il cookie del ticket di autenticazione a qualsiasi richiesta effettuata al dominio.                                                                              |
|         protezione         |                                                                                                                                            Indica le tecniche utilizzate per proteggere il ticket di autenticazione basata su form. I valori consentiti sono: All (impostazione predefinita); Crittografia Nessuno e la convalida. Queste impostazioni sono descritte in dettaglio nel passaggio 3.                                                                                                                                            |
|         requireSSL         |                                                                                                                                                                                Valore booleano che indica se è necessaria una connessione SSL per trasmettere il cookie di autenticazione. Il valore predefinito è false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Valore booleano che indica se il timeout del cookie di autenticazione viene reimpostato ogni volta che l'utente visita il sito durante una singola sessione. Il valore predefinito è true. I criteri di timeout dei ticket di autenticazione vengono descritti in modo più dettagliato nella sezione specifica del valore di timeout del ticket.                                                                                                 |
|          timeout           |                                                                                                                               Specifica il tempo, in minuti, trascorso il quale il cookie del ticket di autenticazione scade. Il valore predefinito è 30. I criteri di timeout dei ticket di autenticazione vengono descritti in modo più dettagliato nella sezione specifica del valore di timeout del ticket.                                                                                                                               |

**Tabella 1**: riepilogo degli attributi dell'elemento &lt;forms&gt;

In ASP.NET 2,0 e versioni successive i valori predefiniti di autenticazione basata su form sono hardcoded nella classe FormsAuthenticationConfiguration della .NET Framework. Tutte le modifiche devono essere applicate a un'applicazione in base all'applicazione nel file Web. config. Questo comportamento è diverso da ASP.NET 1. x, in cui i valori predefiniti di autenticazione basata su form sono stati archiviati nel file Machine. config (e possono quindi essere modificati tramite la modifica di Machine. config). Nell'argomento di ASP.NET 1. x, vale la pena ricordare che alcune impostazioni del sistema di autenticazione basata su form hanno valori predefiniti diversi in ASP.NET 2,0 e oltre a ASP.NET 1. x. Se si esegue la migrazione dell'applicazione da un ambiente ASP.NET 1. x, è importante tenere presente queste differenze. Per un elenco delle differenze, consultare [la documentazione tecnica sull'elemento &lt;forms&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx) .

> [!NOTE]
> Diverse impostazioni di autenticazione basata su form, ad esempio il timeout, il dominio e il percorso, specificano i dettagli per il cookie del ticket di autenticazione basata su form risultante. Per altre informazioni sui cookie, sul loro funzionamento e sulle relative proprietà, vedere [l'esercitazione](http://www.quirksmode.org/js/cookies.html)relativa ai cookie.

### <a name="specifying-the-tickets-timeout-value"></a>Specifica del valore di timeout del ticket

Il ticket di autenticazione basata su form è un token che rappresenta un'identità. Con i ticket di autenticazione basati su cookie, questo token viene mantenuto sotto forma di cookie e inviato al server Web a ogni richiesta. Il possesso del token, in sostanza, dichiara, il *nome utente*, è già stato effettuato l'accesso e viene usato in modo che l'identità di un utente possa essere memorizzata nelle visite di pagina.

Il ticket di autenticazione basata su form non solo include l'identità dell'utente, ma contiene anche informazioni che consentono di garantire l'integrità e la sicurezza del token. Dopotutto, non si vuole che un utente maldato sia in grado di creare un token contraffatto o di modificare un token legit in un certo modo.

Una di queste informazioni incluse nel ticket è una *scadenza*, ovvero la data e l'ora in cui il ticket non è più valido. Ogni volta che il FormsAuthenticationModule controlla un ticket di autenticazione, garantisce che la scadenza del ticket non sia stata ancora superata. In caso contrario, ignora il ticket e identifica l'utente come anonimo. Questa protezione consente di proteggersi da attacchi di riproduzione. Senza una scadenza, se un pirata informatico è stato in grado di ottenere il ticket di autenticazione valido di un utente, probabilmente ottenendo l'accesso fisico al computer e la radice dei cookie, può inviare una richiesta al server con il ticket di autenticazione rubato e ottenere una voce. Mentre la scadenza non impedisce questo scenario, limita la finestra durante la quale un attacco può avere esito positivo.

> [!NOTE]
> Il passaggio 3 illustra le tecniche aggiuntive usate dal sistema di autenticazione basata su form per proteggere il ticket di autenticazione.

Quando si crea il ticket di autenticazione, il sistema di autenticazione basata su form ne determina la scadenza consultando l'impostazione del timeout. Come indicato nella tabella 1, per impostazione predefinita il timeout è di 30 minuti, quindi, quando viene creato il ticket di autenticazione basata su form, la scadenza viene impostata su una data e un'ora di 30 minuti in futuro.

La scadenza definisce un tempo assoluto in futuro al termine del ticket di autenticazione basata su form. Tuttavia, in genere gli sviluppatori vogliono implementare una scadenza variabile, una che viene reimpostata ogni volta che l'utente visita il sito. Questo comportamento è determinato dalle impostazioni slidingExpiration. Se è impostato su true (impostazione predefinita), ogni volta che il FormsAuthenticationModule autentica un utente, viene aggiornata la scadenza del ticket. Se è impostato su false, la scadenza non viene aggiornata per ogni richiesta, causando in tal modo la scadenza esatta del ticket per il numero di minuti passati dalla prima creazione del ticket.

> [!NOTE]
> La scadenza archiviata nel ticket di autenticazione è un valore di data e ora assoluto, ad esempio il 2 agosto 2008 11:34 AM. Inoltre, la data e l'ora sono relative all'ora locale del server Web. Questa decisione di progettazione può avere effetti collaterali interessanti sull'ora legale (DST), ovvero quando gli orologi nel Stati Uniti vengono spostati avanti di un'ora (presupponendo che il server Web sia ospitato in impostazioni locali in cui si osserva l'ora legale). Si consideri cosa accadrebbe per un sito Web ASP.NET con una scadenza di 30 minuti vicino all'ora di inizio dell'ora legale (2:00 AM). Si supponga che un visitatore acceda al sito l'11 marzo 2008 alle 1:55. Verrà generato un ticket di autenticazione basata su form che scade l'11 marzo 2008 alle 2:25 AM (30 minuti in futuro). Tuttavia, una volta 2:00, il clock passa a 3:00 AM a causa dell'ora legale. Quando l'utente carica una nuova pagina sei minuti dopo l'accesso (alle 3:01 AM), FormsAuthenticationModule rileva che il ticket è scaduto e reindirizza l'utente alla pagina di accesso. Per una discussione più approfondita su questa e altre stranezze per il timeout dei ticket di autenticazione, nonché per le soluzioni alternative, è possibile scegliere una copia della *sicurezza, dell'appartenenza e della gestione dei ruoli di Stefan Schackow Professional ASP.NET 2,0* (ISBN: 978-0-7645-9698-8).

Nella figura 1 viene illustrato il flusso di lavoro quando slidingExpiration è impostato su false e timeout è impostato su 30. Si noti che il ticket di autenticazione generato al momento dell'accesso contiene la data di scadenza e questo valore non viene aggiornato nelle richieste successive. Se il FormsAuthenticationModule rileva che il ticket è scaduto, lo ignora e considera la richiesta come anonima.

[![una rappresentazione grafica della scadenza del ticket di autenticazione basata su form quando slidingExpiration è false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Figura 01**: rappresentazione grafica della scadenza del ticket di autenticazione basata su form quando slidingExpiration è false ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))

Nella figura 2 viene illustrato il flusso di lavoro quando slidingExpiration è impostato su true e timeout è impostato su 30. Quando viene ricevuta una richiesta autenticata (con un ticket non scaduto), la relativa scadenza viene aggiornata in base al numero di minuti di timeout in futuro.

[![una rappresentazione grafica del ticket di autenticazione basata su form quando slidingExpiration è true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Figura 02**: rappresentazione grafica del ticket di autenticazione basata su form quando slidingExpiration è true ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))

Quando si usano i ticket di autenticazione basati su cookie (impostazione predefinita), questa discussione diventa leggermente più confusa perché i cookie possono anche avere una scadenza specifica. La scadenza o la mancanza di un cookie indica al browser quando il cookie deve essere eliminato definitivamente. Se il cookie non dispone di una scadenza, viene eliminato definitivamente quando il browser viene arrestato. Se è presente una scadenza, tuttavia, il cookie rimane archiviato nel computer dell'utente finché non vengono superate la data e l'ora specificate nella scadenza. Quando un cookie viene eliminato dal browser, non viene più inviato al server Web. Di conseguenza, l'eliminazione di un cookie è analoga alla disconnessione dell'utente dal sito.

> [!NOTE]
> Naturalmente, un utente può rimuovere in modo proattivo tutti i cookie archiviati nel computer. In Internet Explorer 7 passare a strumenti, opzioni e fare clic sul pulsante Elimina nella sezione cronologia esplorazioni. Da qui, fare clic sul pulsante Elimina cookie.

Il sistema di autenticazione basata su form crea cookie basati sulla sessione o sulla scadenza a seconda del valore passato al parametro *persistCookie* . Ricordare che i metodi GetAuthCookie, SetAuthCookie e RedirectFromLoginPage della classe FormsAuthentication accettano due parametri di input: *username* e *persistCookie*. La pagina di accesso creata nell'esercitazione precedente include la casella di controllo memorizza me, che determina se è stato creato un cookie permanente. I cookie permanenti sono basati sulla scadenza; i cookie non permanenti sono basati sulla sessione.

I concetti di timeout e di slidingExpiration già illustrati si applicano allo stesso modo ai cookie basati sulla sessione e sulla scadenza. Esiste una sola differenza secondaria nell'esecuzione: quando si usano i cookie basati sulla scadenza con slidingTimeout impostato su true, la scadenza del cookie viene aggiornata solo quando è trascorso più della metà del tempo specificato.

Aggiornare i criteri di timeout dei ticket di autenticazione del sito Web in modo che il timeout dei ticket venga effettuato dopo un'ora (60 minuti), usando una scadenza variabile. Per applicare questa modifica, aggiornare il file Web. config, aggiungendo un elemento &lt;Forms&gt; all'elemento &lt;Authentication&gt; con il markup seguente:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Utilizzo di un URL della pagina di accesso diverso da login. aspx

Poiché FormsAuthenticationModule reindirizza automaticamente gli utenti non autorizzati alla pagina di accesso, deve essere a conoscenza dell'URL della pagina di accesso. Questo URL viene specificato dall'attributo loginUrl nell'elemento &lt;Forms&gt; e per impostazione predefinita è login. aspx. Se si esegue il porting su un sito Web esistente, è possibile che sia già presente una pagina di accesso con un URL diverso, che è già stato aggiunto a un segnalibro e indicizzato dai motori di ricerca. Anziché ridenominare la pagina di accesso esistente a login. aspx e ai segnalibri dei collegamenti e degli utenti, è possibile modificare l'attributo loginUrl in modo che punti alla pagina di accesso.

Ad esempio, se la pagina di accesso è denominata signin. aspx e si trova nella directory Users, è possibile puntare l'impostazione di configurazione loginUrl a ~/Users/SignIn.aspx come segue:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Poiché l'applicazione corrente dispone già di una pagina di accesso denominata Login. aspx, non è necessario specificare un valore personalizzato nell'elemento &lt;Forms&gt;.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Passaggio 2: uso di ticket di autenticazione basata su form senza cookie

Per impostazione predefinita, il sistema di autenticazione basata su form determina se archiviare i ticket di autenticazione nella raccolta dei cookie o incorporarli nell'URL in base all'agente utente che visita il sito. Tutti i principali browser desktop come Internet Explorer, Firefox, opera e Safari supportano i cookie, ma non tutti i dispositivi mobili.

I criteri dei cookie usati dal sistema di autenticazione basata su form dipendono dall'impostazione senza cookie nell'elemento &lt;Forms&gt;, a cui è possibile assegnare uno dei quattro valori seguenti:

- UseCookies: specifica che verranno sempre usati i ticket di autenticazione basati su cookie.
- UseUri: indica che i ticket di autenticazione basati su cookie non verranno mai usati.
- Rilevamento automatico: se il profilo del dispositivo non supporta i cookie, i ticket di autenticazione basati su cookie non vengono usati. Se il profilo del dispositivo supporta i cookie, viene usato un meccanismo di probe per determinare se i cookie sono abilitati.
- UseDeviceProfile: valore predefinito. USA i ticket di autenticazione basati su cookie solo se il profilo del dispositivo supporta i cookie. Non viene usato alcun meccanismo di probe.

Le impostazioni di rilevamento automatico e UseDeviceProfile si basano su un *profilo di dispositivo* per verificare se usare i ticket di autenticazione cookie o basati su cookie. ASP.NET gestisce un database di diversi dispositivi e delle relative funzionalità, ad esempio se supportano i cookie, la versione di JavaScript supportata e così via. Ogni volta che un dispositivo richiede una pagina Web da un server Web invia lungo un'intestazione HTTP *dell'agente utente* che identifica il tipo di dispositivo. ASP.NET corrisponde automaticamente alla stringa dell'agente utente fornita con il profilo corrispondente specificato nel relativo database.

> [!NOTE]
> Questo database di funzionalità del dispositivo viene archiviato in una serie di file XML che rispettano lo [schema del file di definizione del browser](https://msdn.microsoft.com/library/ms228122.aspx). I file del profilo di dispositivo predefiniti si trovano in%WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. È anche possibile aggiungere file personalizzati alla cartella app\_browser dell'applicazione. Per altre informazioni, vedere [procedura: rilevare i tipi di browser in pagine Web ASP.NET](https://msdn.microsoft.com/library/3yekbd5b.aspx).

Poiché l'impostazione predefinita è UseDeviceProfile, i ticket di autenticazione basata su form senza cookie verranno usati quando il sito viene visitato da un dispositivo il cui profilo segnala che non supporta i cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Codifica del ticket di autenticazione nell'URL

I cookie sono un supporto naturale per includere le informazioni del browser in ogni richiesta a un particolare sito Web, motivo per cui le impostazioni di autenticazione basata su form predefinite utilizzano i cookie se il dispositivo di visita li supporta. Se i cookie non sono supportati, è necessario utilizzare un metodo alternativo per passare il ticket di autenticazione dal client al server. Una soluzione alternativa comune utilizzata negli ambienti senza cookie consiste nel codificare i dati dei cookie nell'URL.

Il modo migliore per vedere come tali informazioni possono essere incorporate all'interno dell'URL consiste nel forzare il sito a usare i ticket di autenticazione senza cookie. Questa operazione può essere eseguita impostando l'impostazione di configurazione senza cookie su UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Dopo aver apportato questa modifica, visitare il sito tramite un browser. Quando si visita come utente anonimo, gli URL avranno un aspetto analogo al precedente. Ad esempio, quando si visita la pagina default. aspx la barra degli indirizzi del browser Mostra l'URL seguente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Tuttavia, al momento dell'accesso, il ticket di autenticazione basata su form viene incorporato nell'URL. Ad esempio, dopo aver visitato la pagina di accesso e aver eseguito l'accesso come Sam, viene restituita la pagina default. aspx, ma l'URL è il seguente:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Il ticket di autenticazione basata su form è stato incorporato nell'URL. La stringa (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2) rappresenta le informazioni del ticket di autenticazione con codifica esadecimale ed è costituita dagli stessi dati archiviati in genere all'interno di un cookie.

Per il corretto funzionamento dei ticket di autenticazione senza cookie, il sistema deve codificare tutti gli URL della pagina in modo da includere i dati del ticket di autenticazione. in caso contrario, il ticket di autenticazione andrà perso quando l'utente fa clic su un collegamento. Fortunatamente, questa logica di incorporamento viene eseguita automaticamente. Per illustrare questa funzionalità, aprire la pagina default. aspx e aggiungere un controllo collegamento ipertestuale, impostando le proprietà Text e NavigateUrl su test link e SomePage. aspx, rispettivamente. Non è importante che nel progetto sia presente una pagina denominata SomePage. aspx.

Salvare le modifiche apportate a default. aspx e quindi visitarle tramite un browser. Accedere al sito in modo che il ticket di autenticazione basata su form sia incorporato nell'URL. Quindi, da default. aspx, fare clic sul collegamento test link. Cosa è successo? Se non esiste alcuna pagina denominata SomePage. aspx, si è verificato un errore 404, ma ciò non è importante. Concentrarsi invece sulla barra degli indirizzi nel browser. Si noti che include il ticket di autenticazione basata su form nell'URL.

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

L'URL SomePage. aspx nel collegamento è stato convertito automaticamente in un URL che includeva il ticket di autenticazione. non è stato necessario scrivere un codice. Il ticket di autenticazione del modulo verrà incorporato automaticamente nell'URL per qualsiasi collegamento ipertestuale che non inizia con `http://` o `/`. Non è importante se il collegamento ipertestuale viene visualizzato in una chiamata a Response. redirect, in un controllo collegamento ipertestuale o in un elemento HTML di ancoraggio, ad esempio `<a href="...">...</a>`. Fino a quando l'URL non è simile `http://www.someserver.com/SomePage.aspx` o `/SomePage.aspx`, il ticket di autenticazione basata su form verrà incorporato per noi.

> [!NOTE]
> I ticket di autenticazione basata su form senza cookie rispettano gli stessi criteri di timeout dei ticket di autenticazione basati su cookie. Tuttavia, i ticket di autenticazione senza cookie sono più soggetti a attacchi di riproduzione poiché il ticket di autenticazione è incorporato direttamente nell'URL. Immaginate un utente che visita un sito Web, esegue l'accesso e incolla l'URL in un messaggio di posta elettronica a un collega. Se il collega fa clic su tale collegamento prima che venga raggiunta la scadenza, verrà eseguito l'accesso come utente che ha inviato il messaggio di posta elettronica.

## <a name="step-3-securing-the-authentication-ticket"></a>Passaggio 3: protezione del ticket di autenticazione

Il ticket di autenticazione basata su form viene trasmesso in transito in un cookie o incorporato direttamente all'interno dell'URL. Oltre alle informazioni di identità, il ticket di autenticazione può includere anche i dati utente (come si vedrà nel passaggio 4). Di conseguenza, è importante che i dati del ticket vengano crittografati da occhi indiscreti e (ancora più importante) che il sistema di autenticazione basata su form sia in grado di garantire che il ticket non sia stato alterato.

Per garantire la privacy dei dati del ticket, il sistema di autenticazione basata su form può crittografare i dati del ticket. La mancata crittografia dei dati del ticket invia informazioni potenzialmente riservate in transito in testo normale.

Per garantire l'autenticità di un ticket, il sistema di autenticazione basata su form deve *convalidare* il ticket. La convalida è l'atto di garantire che una particolare porzione di dati non sia stata modificata e venga eseguita tramite un *[codice Mac (Message Authentication Code)](http://en.wikipedia.org/wiki/Message_authentication_code)* . In breve, il MAC è costituito da una piccola quantità di informazioni che identifica i dati che devono essere convalidati (in questo caso, il ticket). Se i dati rappresentati dal MAC vengono modificati, il MAC e i dati non corrisponderanno. Inoltre, è difficile dal punto di vista del calcolo per un pirata informatico modificare i dati e generare il proprio MAC in modo che corrisponda ai dati modificati.

Quando si crea o si modifica un ticket, il sistema di autenticazione basata su form crea un MAC e lo collega ai dati del ticket. Quando arriva una richiesta successiva, il sistema di autenticazione basata su form Confronta i dati MAC e ticket per convalidare l'autenticità dei dati del ticket. Nella figura 3 viene illustrato il flusso di lavoro graficamente.

[![l'autenticità del ticket viene garantita tramite un MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Figura 03**: l'autenticità del ticket è garantita tramite un Mac ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))

Le misure di sicurezza applicate al ticket di autenticazione variano a seconda dell'impostazione di protezione nell'elemento &lt;Forms&gt;. L'impostazione di protezione può essere assegnata a uno dei tre valori seguenti:

- Tutto-il ticket è crittografato e con firma digitale (impostazione predefinita).
- Viene applicata la crittografia di sola crittografia. non viene generato alcun MAC.
- None: il ticket non è né crittografato né firmato digitalmente.
- Convalida: viene generato un MAC, ma i dati del ticket vengono inviati in rete in formato testo normale.

Microsoft consiglia vivamente di utilizzare l'impostazione tutte.

### <a name="setting-the-validation-and-decryption-keys"></a>Impostazione delle chiavi di convalida e decrittografia

Gli algoritmi di crittografia e hashing utilizzati dal sistema di autenticazione basata su form per crittografare e convalidare il ticket di autenticazione sono personalizzabili tramite l' [elemento&gt; machineKey di&lt;](https://msdn.microsoft.com/library/w8h3skw9.aspx) in Web. config. Nella tabella 2 sono descritti gli attributi dell'elemento&gt; di &lt;machineKey e i relativi valori possibili.

| **Attributo** | **Descrizione** |
| --- | --- |
| decrittografia | Indica l'algoritmo utilizzato per la crittografia. Questo attributo può avere uno dei quattro valori seguenti:-auto-l'impostazione predefinita; determina l'algoritmo in base alla lunghezza dell'attributo decryptionKey. -AES-usa l'algoritmo di [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) . -DES-usa il [Data Encryption Standard (des)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) questo algoritmo è considerato vulnerabile dal punto di vista del calcolo e non deve essere usato. -3DES: usa l'algoritmo [triple des](http://en.wikipedia.org/wiki/Triple_DES) , che funziona applicando l'algoritmo des tre volte. |
| decryptionKey | Chiave privata usata dall'algoritmo di crittografia. Questo valore deve essere una stringa esadecimale della lunghezza appropriata (in base al valore nella decrittografia), generare automaticamente o uno dei valori aggiunti con, IsolateApps. L'aggiunta di IsolateApps indica a ASP.NET di usare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |
| convalida | Indica l'algoritmo utilizzato per la convalida. Questo attributo può avere uno dei quattro valori seguenti:-AES-usa l'algoritmo di Advanced Encryption Standard (AES). -MD5: usa l'algoritmo [Message-Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) . -SHA1: usa l'algoritmo [SHA1](http://en.wikipedia.org/wiki/Sha1) (impostazione predefinita). -3DES: usa l'algoritmo Triple DES. |
| validationKey | Chiave privata usata dall'algoritmo di convalida. Questo valore deve essere una stringa esadecimale della lunghezza appropriata (in base al valore in Validation), genera automaticamente o uno dei valori aggiunti a, IsolateApps. L'aggiunta di IsolateApps indica a ASP.NET di usare un valore univoco per ogni applicazione. Il valore predefinito è AutoGenerate, IsolateApps. |

**Tabella 2**: attributi dell'elemento&gt; di &lt;machineKey

Una descrizione approfondita di queste opzioni di crittografia e convalida e i vantaggi e gli svantaggi dei vari algoritmi esulano dall'ambito di questa esercitazione. Per una descrizione approfondita di questi problemi, incluse informazioni aggiuntive sugli algoritmi di crittografia e convalida da usare, sulle lunghezze delle chiavi da usare e sul modo migliore per generare queste chiavi, vedere la sezione relativa alla *sicurezza, all'appartenenza e alla gestione dei ruoli di Professional ASP.NET 2,0*.

Per impostazione predefinita, le chiavi usate per la crittografia e la convalida vengono generate automaticamente per ogni applicazione e le chiavi vengono archiviate nell'autorità di sicurezza locale (LSA). In breve, le impostazioni predefinite garantiscono chiavi univoche in base al server Web e all'applicazione. Di conseguenza, questo comportamento predefinito non funzionerà per i due scenari seguenti:

- **Web farm** : in uno scenario di [Web farm](http://en.wikipedia.org/wiki/Web_farm) , una singola applicazione Web è ospitata in più server Web per finalità di scalabilità e ridondanza. Ogni richiesta in ingresso viene inviata a un server nella farm, vale a dire che, per la durata della sessione di un utente, possono essere usati server diversi per gestire le varie richieste. Ogni server deve quindi utilizzare le stesse chiavi di crittografia e convalida, in modo che il ticket di autenticazione basata su form creato, crittografato e convalidato in un server possa essere decrittografato e convalidato in un server diverso della farm.
- **Condivisione di ticket tra applicazioni** : un singolo server Web può ospitare più applicazioni ASP.NET. Se è necessario che le diverse applicazioni condividano un singolo ticket di autenticazione basata su form, è essenziale che le chiavi di crittografia e convalida corrispondano.

Quando si utilizza un'impostazione Web farm o si condividono i ticket di autenticazione tra le applicazioni sullo stesso server, sarà necessario configurare l'elemento&gt; &lt;machineKey nelle applicazioni interessate in modo che i valori decryptionKey e validationKey corrispondano.

Sebbene nessuno degli scenari descritti in precedenza venga applicato all'applicazione di esempio, è comunque possibile specificare valori decryptionKey e validationKey espliciti e definire gli algoritmi da usare. Aggiungere un'impostazione di&gt; &lt;machineKey al file Web. config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Per altre informazioni, vedere [procedura: configurare MachineKey in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> I valori decryptionKey e validationKey sono stati presi dalla [pagina Web delle password perfette](https://www.grc.com/passwords.htm)di [Steve Gibson](http://www.grc.com/stevegibson.htm), che genera 64 caratteri esadecimali casuali in ogni pagina visitata. Per ridurre la probabilità che queste chiavi si trovino nelle applicazioni di produzione, è consigliabile sostituire le chiavi sopra elencate con quelle generate in modo casuale dalla pagina delle password perfette.

## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Passaggio 4: archiviazione di dati utente aggiuntivi nel ticket

In molte applicazioni Web vengono visualizzate informazioni sulla visualizzazione della pagina o sulla base dell'utente attualmente connesso. Ad esempio, una pagina Web potrebbe visualizzare il nome dell'utente e la data dell'ultimo accesso nell'angolo superiore di ogni pagina. Il ticket di autenticazione basata su form archivia il nome utente dell'utente attualmente connesso, ma quando sono necessarie altre informazioni, è necessario che la pagina venga inviata all'archivio utenti, in genere un database, per cercare le informazioni non archiviate nel ticket di autenticazione.

Con un po' di codice è possibile archiviare informazioni aggiuntive sull'utente nel ticket di autenticazione basata su form. Tali dati possono essere espressi tramite la [Proprietà UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx)della [classe FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx). Si tratta di una posizione utile per inserire piccole quantità di informazioni sull'utente in genere necessarie. Il valore specificato nella proprietà UserData è incluso come parte del cookie del ticket di autenticazione e, analogamente agli altri campi del ticket, viene crittografato e convalidato in base alla configurazione del sistema di autenticazione basata su form. Per impostazione predefinita, UserData è una stringa vuota.

Per archiviare i dati utente nel ticket di autenticazione, è necessario scrivere un po' di codice nella pagina di accesso che acquisisce le informazioni specifiche dell'utente e le archivia nel ticket. Poiché UserData è una proprietà di tipo String, i dati archiviati in tale proprietà devono essere serializzati correttamente sotto forma di stringa. Si supponga, ad esempio, che il nostro archivio utenti includa la data di nascita di ogni utente e il nome del proprio datore di lavoro e che si desideri archiviare questi due valori di proprietà nel ticket di autenticazione. È possibile serializzare questi valori in una stringa concatenando la data della stringa di nascita dell'utente con una pipe (|), seguita dal nome del datore di lavoro. Per un utente Nato il 15 agosto 1974 che funziona per Northwind Traders, la proprietà UserData viene assegnata alla stringa: 1974-08-15 | Northwind Traders.

Ogni volta che è necessario accedere ai dati archiviati nel ticket, è possibile farlo afferrando la FormsAuthenticationTicket della richiesta corrente e deserializzando la proprietà UserData. Nel caso della data di nascita e del nome del datore di lavoro, si suddividerebbe la stringa UserData in due sottostringhe in base al delimitatore (|).

[![informazioni aggiuntive sull'utente possono essere archiviate nel ticket di autenticazione](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Figura 04**: le informazioni sull'utente aggiuntive possono essere archiviate nel ticket[di autenticazione (fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))

### <a name="writing-information-to-userdata"></a>Scrittura di informazioni in UserData

Sfortunatamente, l'aggiunta di informazioni specifiche dell'utente a un ticket di autenticazione basata su form non è così semplice come si può aspettarsi. La proprietà UserData della classe FormsAuthenticationTicket è di sola lettura e può essere specificata solo tramite il costruttore della classe FormsAuthenticationTicket. Quando si specifica la proprietà UserData nel costruttore, è necessario specificare anche gli altri valori del ticket: il nome utente, la data del problema, la scadenza e così via. Quando è stata creata la pagina di accesso nell'esercitazione precedente, questa operazione è stata gestita automaticamente dalla classe FormsAuthentication. Quando si aggiunge UserData a FormsAuthenticationTicket, è necessario scrivere il codice per replicare gran parte della funzionalità già fornita dalla classe FormsAuthentication.

Esaminare il codice necessario per l'utilizzo di UserData aggiornando la pagina login. aspx per registrare informazioni aggiuntive sull'utente sul ticket di autenticazione. Tenere presente che l'archivio dell'utente contiene informazioni sull'azienda a cui l'utente lavora e il relativo titolo e che si desidera acquisire tali informazioni nel ticket di autenticazione. Aggiornare il gestore dell'evento Click LoginButton della pagina login. aspx in modo che il codice risulti analogo al seguente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Esaminiamo il codice una riga alla volta. Il metodo inizia definendo quattro matrici di stringhe: Users, passwords, companyName e titleAtCompany. Questi array contengono i nomi utente, le password, i nomi delle società e i titoli per gli account utente nel sistema, di cui sono presenti tre: Scott, Jisun e Sam. In un'applicazione reale, questi valori vengono sottoposti a query dall'archivio utenti, non hardcoded nel codice sorgente della pagina.

Nell'esercitazione precedente, se le credenziali specificate erano valide, abbiamo semplicemente chiamato FormsAuthentication. RedirectFromLoginPage (UserName. text, RememberMe. Checked), che ha eseguito i passaggi seguenti:

1. Creazione del ticket di autenticazione basata su form
2. Ha scritto il ticket nell'archivio appropriato. Per i ticket di autenticazione basati su cookie, viene utilizzata la raccolta di cookie del browser. per i ticket di autenticazione senza cookie, i dati del ticket vengono serializzati nell'URL
3. Reindirizzare l'utente alla pagina appropriata

Questi passaggi vengono replicati nel codice riportato sopra. In primo luogo, la stringa che verrà archiviata nella proprietà UserData viene formata combinando il nome della società e il titolo, delimitando i due valori con un carattere barra verticale (|).

string userDataString = string.Concat(companyName[i], "|", titleAtCompany[i]);

Viene quindi richiamato il metodo FormsAuthentication. GetAuthCookie, che crea il ticket di autenticazione, ne esegue la crittografia e la convalida in base alle impostazioni di configurazione e lo inserisce in un oggetto HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked);

Per lavorare con FormAuthenticationTicket Embedded all'interno del cookie, è necessario chiamare il [metodo Decrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx)della classe FormAuthentication, passando il valore del cookie.

FormsAuthenticationTicket ticket = FormsAuthentication.Decrypt(authCookie.Value);

Viene quindi creata una *nuova* istanza di FormsAuthenticationTicket in base ai valori di FormsAuthenticationTicket esistenti. Tuttavia, questo nuovo ticket include le informazioni specifiche dell'utente (userDataString).

FormsAuthenticationTicket newTicket = new FormsAuthenticationTicket(ticket.Version, ticket.Name, ticket.IssueDate, ticket.Expiration, ticket.IsPersistent, userDataString);

Viene quindi crittografata (e convalidata) la nuova istanza di FormsAuthenticationTicket chiamando il [metodo Encrypt](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)e vengono inseriti i dati crittografati (e convalidati) in authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Infine, authCookie viene aggiunto alla raccolta Response. cookies e viene chiamato il metodo GetRedirectUrl per determinare la pagina appropriata per l'invio dell'utente.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Tutto questo codice è necessario perché la proprietà UserData è di sola lettura e la classe FormsAuthentication non fornisce metodi per specificare le informazioni di UserData nei metodi GetAuthCookie, SetAuthCookie o RedirectFromLoginPage.

> [!NOTE]
> Il codice appena esaminato archivia le informazioni specifiche dell'utente in un ticket di autenticazione basato su cookie. Le classi responsabili della serializzazione del ticket di autenticazione basata su form per l'URL sono interne al .NET Framework. A breve, non è possibile archiviare i dati utente in un ticket di autenticazione basata su form senza cookie.

### <a name="accessing-the-userdata-information"></a>Accesso alle informazioni di UserData

A questo punto, il nome e il titolo della società di ogni utente vengono archiviati nella proprietà UserData del ticket di autenticazione basata su form al momento dell'accesso. È possibile accedere a queste informazioni dal ticket di autenticazione in qualsiasi pagina senza richiedere un viaggio all'archivio utenti. Per illustrare il modo in cui queste informazioni possono essere recuperate dalla proprietà UserData, è necessario aggiornare default. aspx in modo che il messaggio di benvenuto includa non solo il nome dell'utente, ma anche la società a cui lavorano e il titolo.

Attualmente, default. aspx contiene un pannello AuthenticatedMessagePanel con un controllo Label denominato WelcomeBackMessage. Questo pannello viene visualizzato solo per gli utenti autenticati. Aggiornare il codice nella pagina default. aspx\_caricare il gestore eventi in modo che abbia un aspetto simile al seguente:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Se request. IsAuthenticated è true, la proprietà Text di WelcomeBackMessage è impostata per la prima volta su Welcome back, *username*. Quindi, viene eseguito il cast della proprietà User. Identity a un oggetto FormsIdentity in modo che sia possibile accedere al FormsAuthenticationTicket sottostante. Una volta ottenuto il FormsAuthenticationTicket, deserializzare la proprietà UserData nel nome e nel titolo della società. Questa operazione viene eseguita suddividendo la stringa sul carattere barra verticale. Il nome e il titolo della società vengono quindi visualizzati nell'etichetta WelcomeBackMessage.

Nella figura 5 viene illustrata una schermata di questa visualizzazione in azione. Quando si accede a Scott, viene visualizzato un messaggio di benvenuto che include la società e il titolo di Scott.

[![viene visualizzata la società e il titolo dell'utente attualmente connesso](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Figura 05**: vengono visualizzate la società e il titolo dell'utente attualmente connesso ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))

> [!NOTE]
> La proprietà UserData del ticket di autenticazione funge da cache per l'archivio utente. Analogamente a qualsiasi cache, è necessario aggiornarla quando vengono modificati i dati sottostanti. Se, ad esempio, è presente una pagina Web da cui gli utenti possono aggiornare il proprio profilo, è necessario aggiornare i campi memorizzati nella cache della proprietà UserData in modo da riflettere le modifiche apportate dall'utente.

## <a name="step-5-using-a-custom-principal"></a>Passaggio 5: uso di un'entità personalizzata

Per ogni richiesta in ingresso, il FormsAuthenticationModule tenta di autenticare l'utente. Se è presente un ticket di autenticazione non scaduto, FormsAuthenticationModule assegna la proprietà HttpContext. User a un nuovo oggetto GenericPrincipal. Questo oggetto GenericPrincipal ha un'identità di tipo FormsIdentity, che include un riferimento al ticket di autenticazione basata su form. La classe GenericPrincipal contiene le funzionalità minime minime richieste da una classe che implementa IPrincipal, ma ha solo una proprietà Identity e un metodo IsInRole.

L'oggetto principal dispone di due responsabilità: per indicare i ruoli a cui appartiene l'utente e per fornire informazioni sull'identità. Questa operazione viene eseguita tramite il metodo IsInRole (*roleName*) e la proprietà Identity dell'interfaccia IPrincipal, rispettivamente. La classe GenericPrincipal consente di specificare una matrice di stringhe di nomi di ruoli tramite il relativo costruttore. il metodo IsInRole (*roleName*) verifica semplicemente se il passato *roleName* è presente nella matrice di stringhe. Quando FormsAuthenticationModule crea il GenericPrincipal, passa una matrice di stringhe vuota al costruttore del GenericPrincipal. Di conseguenza, qualsiasi chiamata a IsInRole restituirà sempre false.

La classe GenericPrincipal soddisfa le esigenze per la maggior parte degli scenari di autenticazione basata su form in cui non vengono utilizzati i ruoli. Per le situazioni in cui la gestione dei ruoli predefinita non è sufficiente o quando è necessario associare un oggetto IIdentity personalizzato all'utente, è possibile creare un oggetto IPrincipal personalizzato durante il flusso di lavoro di autenticazione e assegnarlo alla proprietà HttpContext. User.

> [!NOTE]
> Come si vedrà nelle esercitazioni future, quando ASP. Il Framework dei ruoli di NET è abilitato. crea un oggetto Principal personalizzato di tipo [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) e sovrascrive l'oggetto GenericPrincipal creato dall'autenticazione basata su form. Questa operazione viene eseguita in modo da personalizzare il metodo IsInRole dell'entità per l'interfaccia con l'API del Framework dei ruoli.

Poiché non ci preoccupiamo ancora dei ruoli, l'unico motivo per creare un'entità personalizzata in questo frangente è associare un oggetto IIdentity personalizzato al server principale. Nel passaggio 4 è stata esaminata l'archiviazione di informazioni utente aggiuntive nella proprietà UserData del ticket di autenticazione, in particolare il nome della società e il titolo dell'utente. Tuttavia, le informazioni di UserData sono accessibili solo tramite il ticket di autenticazione e solo in seguito come stringa serializzata, vale a dire che ogni volta che si desidera visualizzare le informazioni utente archiviate nel ticket è necessario analizzare la proprietà UserData.

Per migliorare l'esperienza dello sviluppatore, è possibile creare una classe che implementi IIdentity e includa proprietà CompanyName e title. In questo modo, uno sviluppatore può accedere al nome della società e al titolo dell'utente attualmente connesso direttamente tramite le proprietà CompanyName e title senza che sia necessario per comprendere come analizzare la proprietà UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Creazione delle classi Identity e Principal personalizzate

Per questa esercitazione, è ora di creare gli oggetti Principal e Identity personalizzati nella cartella app\_code. Per iniziare, aggiungere un'app\_codice alla cartella del progetto, fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni, selezionare l'opzione Aggiungi cartella ASP.NET e scegliere app\_codice. La cartella del codice\_app è una cartella ASP.NET speciale che include i file di classe specifici del sito Web.

> [!NOTE]
> La cartella del codice dell'app\_deve essere usata solo quando si gestisce il progetto tramite il modello di progetto del sito Web. Se si usa il [modello di progetto applicazione Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), creare una cartella standard e aggiungervi le classi. Ad esempio, è possibile aggiungere una nuova cartella denominata Classis e inserire il codice qui.

Aggiungere quindi due nuovi file di classe alla cartella del codice dell'app\_, uno denominato CustomIdentity.cs e uno denominato CustomPrincipal.cs.

[![aggiungere le classi CustomIdentity e CustomPrincipal al progetto](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Figura 06**: aggiungere le classi CustomIdentity e CustomPrincipal al progetto ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))

La classe CustomIdentity è responsabile dell'implementazione dell'interfaccia IIdentity, che definisce le proprietà AuthenticationType, IsAuthenticated e Name. Oltre alle proprietà obbligatorie, è possibile esporre il ticket di autenticazione basata su form sottostante, nonché le proprietà per il nome e il titolo della società dell'utente. Immettere il codice seguente nella classe CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Si noti che la classe include una variabile membro FormsAuthenticationTicket (ticket\_) e che queste informazioni del ticket devono essere fornite tramite il costruttore. Questi dati del ticket vengono usati per restituire il nome dell'identità; viene analizzata la relativa proprietà UserData per restituire i valori per le proprietà CompanyName e title.

Successivamente, creare la classe CustomPrincipal. Poiché in questo frangente non sono interessati i ruoli, il costruttore della classe CustomPrincipal accetta solo un oggetto CustomIdentity. il metodo IsInRole restituisce sempre false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Assegnazione di un oggetto CustomPrincipal al contesto di sicurezza della richiesta in ingresso

È ora disponibile una classe che estende la specifica IIdentity predefinita per includere le proprietà CompanyName e title, oltre a una classe Principal personalizzata che usa l'identità personalizzata. È ora possibile passare alla pipeline ASP.NET e assegnare l'oggetto entità personalizzata al contesto di sicurezza della richiesta in ingresso.

La pipeline ASP.NET accetta una richiesta in ingresso e la elabora attraverso una serie di passaggi. In ogni fase viene generato un particolare evento, che consente agli sviluppatori di accedere alla pipeline ASP.NET e modificare la richiesta in determinati punti del ciclo di vita. FormsAuthenticationModule, ad esempio, attende che ASP.NET generi l' [evento AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx). a questo punto controlla la richiesta in ingresso per un ticket di autenticazione. Se viene trovato un ticket di autenticazione, viene creato un oggetto GenericPrincipal che viene assegnato alla proprietà HttpContext. User.

Dopo l'evento AuthenticateRequest, la pipeline ASP.NET genera l' [evento PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), in cui è possibile sostituire l'oggetto GenericPrincipal creato da FormsAuthenticationModule con un'istanza dell'oggetto CustomPrincipal. Nella figura 7 viene illustrato questo flusso di lavoro.

[![GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Figura 07**: il GenericPrincipal viene sostituito da un CustomPrincipal nell'evento PostAuthenticationRequest ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))

Per eseguire il codice in risposta a un evento della pipeline ASP.NET, è possibile creare il gestore eventi appropriato in Global. asax o creare un modulo HTTP personalizzato. Per questa esercitazione verrà creato il gestore eventi in Global. asax. Per iniziare, aggiungere Global. asax al sito Web. Fare clic con il pulsante destro del mouse sul nome del progetto in Esplora soluzioni e aggiungere un elemento di tipo Global Application Class denominato Global. asax.

[![aggiungere un file Global. asax al sito Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Figura 08**: aggiungere un file Global. asax al sito Web ([fare clic per visualizzare l'immagine con dimensioni complete](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))

Il modello Global. asax predefinito include i gestori eventi per un numero di eventi della pipeline ASP.NET, tra cui l'evento di inizio, di fine e di [errore](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), tra gli altri. È possibile rimuovere questi gestori eventi, perché non sono necessari per questa applicazione. L'evento a cui si è interessati è PostAuthenticateRequest. Aggiornare il file Global. asax in modo che il relativo markup risulti simile al seguente:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Il Metodo OnPostAuthenticateRequest dell'applicazione\_viene eseguito ogni volta che il runtime di ASP.NET genera l'evento PostAuthenticateRequest, che si verifica una volta per ogni richiesta di pagina in arrivo. Il gestore eventi inizia controllando se l'utente è autenticato ed è stato autenticato tramite l'autenticazione basata su form. In tal caso, viene creato un nuovo oggetto CustomIdentity e viene passato il ticket di autenticazione della richiesta corrente nel relativo costruttore. In seguito, viene creato un oggetto CustomPrincipal e viene passato l'oggetto CustomIdentity appena creato nel relativo costruttore. Infine, il contesto di sicurezza della richiesta corrente viene assegnato all'oggetto CustomPrincipal appena creato.

Si noti che l'ultimo passaggio, che associa l'oggetto CustomPrincipal al contesto di sicurezza della richiesta, assegna l'entità a due proprietà: HttpContext. User e thread. CurrentPrincipal. Queste due assegnazioni sono necessarie a causa del modo in cui i contesti di sicurezza vengono gestiti in ASP.NET. Il .NET Framework associa un contesto di sicurezza a ogni thread in esecuzione; Queste informazioni sono disponibili come oggetto IPrincipal tramite la [proprietà CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx)dell' [oggetto thread](https://msdn.microsoft.com/library/system.threading.thread.aspx). Si tratta di una confusione che ASP.NET dispone di proprie informazioni sul contesto di sicurezza (HttpContext. User).

In alcuni scenari, la proprietà thread. CurrentPrincipal viene esaminata quando si determina il contesto di sicurezza. in altri scenari, viene usato HttpContext. User. In .NET, ad esempio, sono disponibili funzionalità di sicurezza che consentono agli sviluppatori di dichiarare in modo dichiarativo quali utenti o ruoli possono creare un'istanza di una classe o richiamare metodi specifici (vedere [aggiunta di regole di autorizzazione ai livelli business e dati tramite PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Dietro le quinte, queste tecniche dichiarative determinano il contesto di sicurezza tramite la proprietà thread. CurrentPrincipal.

In altri scenari, viene utilizzata la proprietà HttpContext. User. Nell'esercitazione precedente, ad esempio, è stata usata questa proprietà per visualizzare il nome utente dell'utente attualmente connesso. Ovviamente, è fondamentale che le informazioni sul contesto di sicurezza nelle proprietà thread. CurrentPrincipal e HttpContext. User corrispondano.

Il runtime di ASP.NET sincronizza automaticamente questi valori di proprietà. Questa sincronizzazione viene tuttavia eseguita dopo l'evento AuthenticateRequest, ma *prima* dell'evento PostAuthenticateRequest. Di conseguenza, quando si aggiunge un'entità personalizzata nell'evento PostAuthenticateRequest, è necessario essere certi di assegnare manualmente il thread. CurrentPrincipal o else thread. CurrentPrincipal e HttpContext. User non saranno sincronizzati. Per una descrizione più dettagliata di questo problema, vedere [context. User vs. thread. CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) .

### <a name="accessing-the-companyname-and-title-properties"></a>Accesso alle proprietà CompanyName e title

Ogni volta che una richiesta arriva e viene inviata al motore ASP.NET, l'applicazione\_gestore dell'evento OnPostAuthenticateRequest in Global. asax viene attivato. Se la richiesta è stata autenticata correttamente da FormsAuthenticationModule, il gestore eventi creerà un nuovo oggetto CustomPrincipal con un oggetto CustomIdentity basato sul ticket di autenticazione basata su form. Con questa logica, l'accesso alle informazioni relative al nome e al titolo della società dell'utente attualmente connesso è estremamente semplice.

Tornare al gestore eventi di caricamento della pagina\_in default. aspx, dove nel passaggio 4 è stato scritto il codice per recuperare il ticket di autenticazione del modulo e analizzare la proprietà UserData per visualizzare il nome e il titolo della società dell'utente. Con gli oggetti CustomPrincipal e CustomIdentity attualmente in uso, non è necessario analizzare i valori fuori dalla proprietà UserData del ticket. È invece sufficiente ottenere un riferimento all'oggetto CustomIdentity e utilizzare le proprietà CompanyName e title:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Riepilogo

In questa esercitazione è stato esaminato come personalizzare le impostazioni del sistema di autenticazione basata su form tramite Web. config. È stato esaminato il modo in cui viene gestita la scadenza del ticket di autenticazione e il modo in cui vengono usate le misure di sicurezza per la crittografia e la convalida per proteggere il ticket dall'ispezione e dalla modifica. Infine, è stato illustrato l'uso della proprietà UserData del ticket di autenticazione per archiviare informazioni aggiuntive sull'utente nel ticket stesso e come usare oggetti Principal e Identity personalizzati per esporre queste informazioni in modo più semplice per gli sviluppatori.

Questa esercitazione conclude l'esame dell'autenticazione basata su form in ASP.NET. L'esercitazione successiva avvia il passaggio al framework delle appartenenze.

Buona programmazione!

### <a name="further-reading"></a>Ulteriori informazioni

Per ulteriori informazioni sugli argomenti trattati in questa esercitazione, fare riferimento alle risorse seguenti:

- [Dissezione dell'autenticazione basata su form](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Spiegazione: autenticazione basata su form in ASP.NET 2,0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Procedura: proteggere l'autenticazione basata su form in ASP.NET 2,0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Sicurezza, appartenenza e gestione dei ruoli Professional ASP.NET 2,0](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Protezione dei controlli di accesso](https://msdn.microsoft.com/library/ms178346.aspx)
- [Elemento&gt; di autenticazione &lt;](https://msdn.microsoft.com/library/532aee0e.aspx)
- [Elemento &lt;Forms&gt; per &lt;Authentication&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [Elemento&gt; machineKey &lt;](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Informazioni sul ticket e sul cookie di autenticazione basata su form](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Formazione video sugli argomenti contenuti in questa esercitazione

- [Come modificare le proprietà di autenticazione basata su form](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Come configurare e usare l'autenticazione senza cookie in un'applicazione ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Rilocazione dell'accesso ai moduli ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Configurazione della chiave personalizzata di accesso ai moduli](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Aggiungere dati personalizzati al metodo di autenticazione](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Usare oggetti Principal personalizzati](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informazioni sull'autore

Scott Mitchell, autore di più libri ASP/ASP. NET e fondatore di 4GuysFromRolla.com, collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è *[Sams Teach Yourself ASP.NET 2,0 in 24 ore](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)* . Scott può essere raggiunto in [mitchell@4guysfromrolla.com](mailto:mitchell@4guysfromrolla.com) o tramite il suo blog al [http://ScottOnWriting.NET](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore responsabile di questa esercitazione è Alicja Maziarz. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Precedente](an-overview-of-forms-authentication-cs.md)
> [Successivo](security-basics-and-asp-net-support-vb.md)
