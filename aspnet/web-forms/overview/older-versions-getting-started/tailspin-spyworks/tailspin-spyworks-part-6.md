---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
title: 'Parte 6: appartenenza a ASP.NET | Microsoft Docs'
author: JoeStagner
description: Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 6 aggiunge l'appartenenza a ASP.NET.
ms.author: riande
ms.date: 07/21/2010
ms.assetid: f70a310c-9557-4743-82cb-655265676d39
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-6
msc.type: authoredcontent
ms.openlocfilehash: b0caa89dc9ffb5bb7451fa2d9d346c7db2bf1466
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78564183"
---
# <a name="part-6-aspnet-membership"></a>Parte 6: appartenenza a ASP.NET

di [Joe Stagner spiega](https://github.com/JoeStagner)

> In tilt SpyWorks viene illustrato il modo in cui è estremamente semplice creare applicazioni scalabili e potenti per la piattaforma .NET. Viene illustrato come utilizzare le nuove eccezionali funzionalità di ASP.NET 4 per creare un negozio online, inclusi acquisti, estrazione e amministrazione.
> 
> Questa serie di esercitazioni illustra tutti i passaggi necessari per compilare l'applicazione di esempio Tilt SpyWorks. La parte 6 aggiunge l'appartenenza a ASP.NET.

## <a id="_Toc260221672"></a>Uso dell'appartenenza a ASP.NET

![](tailspin-spyworks-part-6/_static/image1.png)

Fare clic su sicurezza

![](tailspin-spyworks-part-6/_static/image1.jpg)

Assicurarsi di usare l'autenticazione basata su form.

![](tailspin-spyworks-part-6/_static/image2.jpg)

Usare il collegamento "Crea utente" per creare un paio di utenti.

![](tailspin-spyworks-part-6/_static/image3.jpg)

Al termine, fare riferimento alla finestra Esplora soluzioni e aggiornare la visualizzazione.

![](tailspin-spyworks-part-6/_static/image2.png)

Si noti che ASPNETDB. La fine di MDF è stata creata. Questo file contiene le tabelle per supportare i servizi ASP.NET di base come l'appartenenza.

A questo punto è possibile iniziare a implementare il processo di estrazione.

Per iniziare, creare una pagina CheckOut. aspx.

La pagina CheckOut. aspx dovrebbe essere disponibile solo per gli utenti che hanno eseguito l'accesso, in modo da limitare l'accesso agli utenti connessi e reindirizzare gli utenti che non sono connessi alla pagina di accesso.

A tale scopo, aggiungere quanto segue alla sezione di configurazione del file Web. config.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample1.xml)]

Il modello per le applicazioni Web Form ASP.NET aggiunge automaticamente una sezione di autenticazione al file Web. config e stabilisce la pagina di accesso predefinita.

[!code-xml[Main](tailspin-spyworks-part-6/samples/sample2.xml)]

È necessario modificare il file code-behind login. aspx per eseguire la migrazione di un carrello acquisti anonimo quando l'utente esegue l'accesso. Modificare la pagina\_evento Load come indicato di seguito.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample3.cs)]

Aggiungere quindi un gestore dell'evento "login" come questo per impostare il nome della sessione sull'utente appena connesso e modificare l'ID sessione temporanea nel carrello acquisti con quello dell'utente chiamando il metodo MigrateCart nella classe MyShoppingCart. (Implementato nel file con estensione CS)

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample4.cs)]

Implementare il metodo MigrateCart () come questo.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample5.cs)]

In checkout. aspx verrà usato un oggetto EntityDataSource e un GridView nella pagina di estrazione, così come nella pagina del carrello della spesa.

[!code-aspx[Main](tailspin-spyworks-part-6/samples/sample6.aspx)]

Si noti che il controllo GridView specifica un gestore eventi "OnDataBound" denominato elenco\_RowDataBound, quindi si implementerà questo gestore eventi come questo.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample7.cs)]

Questo metodo mantiene un totale parziale del carrello acquisti quando ogni riga è associata e aggiorna la riga inferiore di GridView.

In questa fase è stata implementata una presentazione di "revisione" dell'ordine da inserire.

Per gestire uno scenario di carrello vuoto, è possibile aggiungere alcune righe di codice alla pagina\_evento Load:

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample8.cs)]

Quando l'utente fa clic sul pulsante "Submit", verrà eseguito il codice seguente nel gestore dell'evento click del pulsante Submit.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample9.cs)]

La "carne" del processo di invio dell'ordine deve essere implementata nel metodo SubmitOrder () della classe MyShoppingCart.

SubmitOrder:

- Prendere tutte le voci del carrello acquisti e usarle per creare un nuovo record di ordine e i record OrderDetails associati.
- Calcolare la data di spedizione.
- Cancellare il carrello della spesa.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample10.cs)]

Ai fini di questa applicazione di esempio, verrà calcolata una data di spedizione semplicemente aggiungendo due giorni alla data corrente.

[!code-csharp[Main](tailspin-spyworks-part-6/samples/sample11.cs)]

L'esecuzione dell'applicazione consente ora di testare il processo di acquisto dall'inizio alla fine.

> [!div class="step-by-step"]
> [Precedente](tailspin-spyworks-part-5.md)
> [Successivo](tailspin-spyworks-part-7.md)
