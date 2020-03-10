---
uid: whitepapers/request-validation
title: Convalida delle richieste-prevenzione degli attacchi script | Microsoft Docs
author: rick-anderson
description: Questo documento descrive la funzionalità di convalida della richiesta di ASP.NET, in cui, per impostazione predefinita, l'applicazione non è in grado di elaborare il contenuto HTML non codificato inviato...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: fa429113-5f8f-4ef4-97c5-5c04900a19fa
msc.legacyurl: /whitepapers/request-validation
msc.type: content
ms.openlocfilehash: 807cccd6fe1acdd6359b014387abd3878840d4cd
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78640847"
---
# <a name="request-validation---preventing-script-attacks"></a>Richiedere la convalida - Prevenzione degli attacchi tramite script

> In questo documento viene descritta la funzionalità di convalida della richiesta di ASP.NET, in cui, per impostazione predefinita, l'applicazione non è in grado di elaborare contenuto HTML non codificato inviato al server. Questa funzionalità di convalida della richiesta può essere disabilitata quando l'applicazione è stata progettata per elaborare in modo sicuro i dati HTML.
> 
> Si applica a ASP.NET 1,1 e ASP.NET 2,0.

La convalida della richiesta, una funzionalità di ASP.NET sin dalla versione 1.1, impedisce al server di accettare contenuti che includono HTML non codificato. Questa funzionalità è progettata per impedire alcuni attacchi script injection in cui il codice script client o HTML può essere inconsapevolmente inviato a un server, archiviato e quindi presentato ad altri utenti. È tuttavia vivamente consigliabile convalidare tutti i dati di input e codificarli con HTML, se appropriato.

Si crea, ad esempio, una pagina Web che richiede l'indirizzo di posta elettronica di un utente e quindi archivia tale indirizzo di posta elettronica in un database. Se l'utente immette &lt;SCRIPT&gt;avviso ("Hello from script")&lt;/SCRIPT&gt; anziché un indirizzo di posta elettronica valido, quando i dati vengono presentati, questo script può essere eseguito se il contenuto non è stato codificato correttamente. La funzionalità di convalida della richiesta di ASP.NET impedisce che ciò accada.

## <a name="why-this-feature-is-useful"></a>Perché questa funzionalità è utile

Molti siti non sono consapevoli del fatto che sono aperti a attacchi di script injection semplici. Indipendentemente dal fatto che lo scopo di questi attacchi sia quello di rivolto al sito visualizzando il codice HTML o di eseguire potenzialmente uno script client per reindirizzare l'utente al sito di un pirata informatico, gli attacchi di script injection costituiscono un problema che gli sviluppatori Web devono affrontare.

Gli attacchi di script injection rappresentano un problema per tutti gli sviluppatori Web, che usano ASP.NET, ASP o altre tecnologie di sviluppo Web.

La funzionalità di convalida della richiesta ASP.NET impedisce in modo proattivo questi attacchi evitando che il contenuto HTML non codificato venga elaborato dal server, a meno che lo sviluppatore non decida di consentire tale contenuto.

## <a name="what-to-expect-error-page"></a>Cosa aspettarsi: pagina di errore

Lo screenshot seguente mostra alcuni esempi di codice ASP.NET:

![](request-validation/_static/image1.png)

Eseguendo questo codice si ottiene una pagina semplice che consente di immettere testo nella casella di testo, fare clic sul pulsante e visualizzare il testo nel controllo etichetta:

![](request-validation/_static/image2.png)

Tuttavia, era JavaScript, ad esempio `<script>alert("hello!")</script>` da immettere e inviare, si otterrebbe un'eccezione:

![](request-validation/_static/image3.png)

Il messaggio di errore indica che è stato rilevato un valore ' request. Form potenzialmente pericoloso ', che fornisce ulteriori dettagli nella descrizione di quanto si è verificato e come modificare il comportamento. Esempio:

La convalida della richiesta ha rilevato un valore di input client potenzialmente pericoloso e l'elaborazione della richiesta è stata interrotta. Questo valore può indicare un tentativo di compromettere la sicurezza dell'applicazione, ad esempio un attacco di scripting tra siti. È possibile disabilitare la convalida delle richieste impostando `validateRequest=false` nella direttiva della pagina o nella sezione di configurazione. Tuttavia, si consiglia vivamente all'applicazione di controllare in modo esplicito tutti gli input in questo caso.

## <a name="disabling-request-validation-on-a-page"></a>Disabilitazione della convalida delle richieste in una pagina

Per disabilitare la convalida delle richieste in una pagina, è necessario impostare l'attributo `validateRequest` della direttiva della pagina su `false`:

[!code-aspx[Main](request-validation/samples/sample1.aspx)]

> [!CAUTION]
> Quando la convalida delle richieste è disabilitata, il contenuto può essere inviato a una pagina; è responsabilità dello sviluppatore della pagina garantire che il contenuto sia codificato o elaborato correttamente.

## <a name="disabling-request-validation-for-your-application"></a>Disabilitazione della convalida delle richieste per l'applicazione

Per disabilitare la convalida delle richieste per l'applicazione, è necessario modificare o creare un file Web. config per l'applicazione e impostare l'attributo validateRequest della sezione `<pages />` su `false`:

[!code-xml[Main](request-validation/samples/sample2.xml)]

Se si desidera disabilitare la convalida delle richieste per tutte le applicazioni nel server, è possibile apportare questa modifica al file Machine. config.

> [!CAUTION]
> Quando la convalida delle richieste è disabilitata, il contenuto può essere inviato all'applicazione; è responsabilità dello sviluppatore dell'applicazione garantire che il contenuto sia codificato o elaborato correttamente.

Il codice seguente è stato modificato per disattivare la convalida delle richieste:

![](request-validation/_static/image4.png)

A questo punto, se il codice JavaScript seguente è stato immesso nella casella di testo `<script>alert("hello!")</script>` il risultato sarà:

![](request-validation/_static/image5.png)

Per evitare che ciò accada, quando la convalida delle richieste è disattivata, è necessario codificare in HTML il contenuto.

## <a name="how-to-html-encode-content"></a>Come codificare il contenuto HTML

Se è stata disabilitata la convalida delle richieste, è consigliabile codificare il contenuto HTML che verrà archiviato per un uso futuro. La codifica HTML sostituirà automaticamente qualsiasi elemento '&lt;' o '&gt;' (insieme a diversi altri simboli) con la relativa rappresentazione codificata HTML corrispondente. Ad esempio,'&lt;' viene sostituito da'&amp;lt;' è&gt;' viene sostituito da'&amp;gt;'. I browser usano questi codici speciali per visualizzare '&lt;' o '&gt;' nel browser.

Il contenuto può essere facilmente codificato in HTML nel server usando l'API `Server.HtmlEncode(string)`. Il contenuto può anche essere facilmente decodificato in formato HTML, ovvero ripristinato in HTML standard usando il metodo `Server.HtmlDecode(string)`.

![](request-validation/_static/image6.png)

Risultato:

![](request-validation/_static/image7.png)
