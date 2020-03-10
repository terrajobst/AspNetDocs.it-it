---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: Aggiunta della convalida al modello | Microsoft Docs
author: shanselman
description: Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Creare una semplice applicazione Web che legge e scrive da un database.
ms.author: riande
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 9403be574324c34edf93bef1e0e4fd7ba68a3a9d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78543645"
---
# <a name="adding-validation-to-the-model"></a>Aggiunta della convalida al modello

di [Scott hanseln](https://github.com/shanselman)

> Questa esercitazione introduttiva illustra le nozioni di base di ASP.NET MVC. Verrà creata una semplice applicazione Web che legge e scrive da un database. Visitare il [centro per l'apprendimento di ASP.NET MVC](../../../index.md) per trovare altre esercitazioni ed esempi di ASP.NET MVC.

In questa sezione verrà implementato il supporto necessario per abilitare la convalida dell'input all'interno dell'applicazione. Assicuriamo che il contenuto del database sia sempre corretto e che gli utenti finali forniscano messaggi di errore utili quando provano a immettere dati di film non validi. Si inizierà aggiungendo una piccola logica di convalida alla classe Movie.

Fare clic con il pulsante destro del mouse sulla cartella del modello e scegliere Aggiungi classe. Denominare il film della classe.

Quando il modello di entità Movie è stato creato in precedenza, l'IDE ha creato una classe Movie. Di fatto, parte della classe Movie può trovarsi in un file e parte in un altro. Si tratta di una classe parziale. La classe Movie verrà estesa da un altro file.

Verrà creata una classe di film parziale che punta a una "classe Buddy" con alcuni attributi che daranno suggerimenti di convalida al sistema. Il titolo e il prezzo verranno contrassegnati secondo le esigenze e anche il prezzo rientra in un determinato intervallo. Fare clic con il pulsante destro del mouse sulla cartella Models e scegliere Aggiungi classe. Denominare il film della classe e fare clic sul pulsante OK. Ecco la nostra classe di film parziale.

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

Eseguire nuovamente l'applicazione e provare a immettere un film con un prezzo superiore a 100. Si riceverà un errore dopo l'invio del modulo. L'errore viene rilevato sul lato server e si verifica dopo la pubblicazione del modulo. Si noti che gli helper HTML incorporati in ASP.NET MVC erano sufficientemente intelligenti per visualizzare il messaggio di errore e mantenere i valori per Microsoft negli elementi TextBox:

[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)

Si tratta di una soluzione ottimale, ma sarebbe interessante se avessimo potuto informare l'utente sul lato client, immediatamente prima che il server venisse occupato.

È ora abilitata la convalida lato client con JavaScript.

## <a name="adding-client-side-validation"></a>Aggiunta della convalida lato client

Poiché la classe Movie include già alcuni attributi di convalida, è sufficiente aggiungere alcuni file JavaScript al modello di vista Create. aspx e aggiungere una riga di codice per abilitare la convalida lato client.

Dall'interno della VWD andare alla cartella Views/Movie e aprire create. aspx.

Aprire la cartella Scripts nella Esplora soluzioni e trascinare i tre script seguenti in all'interno del tag Head&gt; &lt;.

- MicrosoftAjax.js
- MicrosoftMvcValidation.js

Si desidera che questi file di script vengano visualizzati in questo ordine.

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

Aggiungere anche questa riga singola sopra il codice HTML. BeginForm:

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

Ecco il codice visualizzato nell'IDE.

[![Movies-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)

Eseguire l'applicazione e visitare di nuovo/Movies/Create e fare clic su Crea senza immettere i dati. I messaggi di errore vengono visualizzati immediatamente senza il flash di pagina associato all'invio dei dati fino al server. Questo perché ASP.NET MVC sta convalidando l'input sia nel client (tramite JavaScript) che nel server.

[![creare-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)

Si tratta di un aspetto positivo. A questo punto, aggiungere una colonna aggiuntiva al database.

> [!div class="step-by-step"]
> [Precedente](getting-started-with-mvc-part6.md)
> [Successivo](getting-started-with-mvc-part8.md)
