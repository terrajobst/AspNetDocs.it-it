---
uid: aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
title: Abilitazione dell'autenticazione di Windows in Katana | Microsoft Docs
author: MikeWasson
description: "Questo articolo illustra come abilitare l'autenticazione di Windows in Katana. Vengono illustrati due scenari: l'utilizzo di IIS per ospitare la Katana e l'utilizzo di HttpListener per l'hosting automatico di Kat..."
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 82324ef0-3b75-4f63-a217-76ef4036ec93
msc.legacyurl: /aspnet/overview/owin-and-katana/enabling-windows-authentication-in-katana
msc.type: authoredcontent
ms.openlocfilehash: 3d81e7e1bf13ab63417378fba0c5ab80213f404b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617180"
---
# <a name="enabling-windows-authentication-in-katana"></a>Abilitazione dell'autenticazione di Windows in Katana

di [Mike Wasson](https://github.com/MikeWasson)

> Questo articolo illustra come abilitare l'autenticazione di Windows in Katana. Vengono illustrati due scenari: l'uso di IIS per ospitare la Katana e l'uso di HttpListener per ospitare in modo automatico la katana in un processo personalizzato. Grazie a Barry Dorrans, David Matson e Chris Ross per la revisione di questo articolo.

Katana è l'implementazione Microsoft di [OWIN](http://owin.org/), l'interfaccia Web aperta per .NET. È possibile leggere un'introduzione a OWIN e Katana [qui](an-overview-of-project-katana.md). L'architettura OWIN presenta diversi livelli:

- Host: gestisce il processo in cui viene eseguita la pipeline OWIN.
- Server: apre un socket di rete e resta in attesa di richieste.
- Middleware: elabora la richiesta e la risposta HTTP.

Katana fornisce attualmente due server, entrambi supportati dall'autenticazione integrata di Windows:

- **Microsoft. Owin. host. systemWeb**. Usa IIS con la pipeline ASP.NET.
- **Microsoft. Owin. host. HttpListener**. USA [System .NET. HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx). Questo server è attualmente l'opzione predefinita quando si esegue l'hosting automatico di katana.

> [!NOTE]
> Katana non fornisce attualmente il middleware OWIN per l'autenticazione di Windows, perché questa funzionalità è già disponibile nei server.

## <a name="windows-authentication-in-iis"></a>Autenticazione di Windows in IIS

Con Microsoft. Owin. host. SystemWeb è possibile abilitare semplicemente l'autenticazione di Windows in IIS.

Per iniziare, creare una nuova applicazione ASP.NET usando il modello di progetto "applicazione Web vuota ASP.NET".

![](enabling-windows-authentication-in-katana/_static/image1.png)

Aggiungere quindi i pacchetti NuGet. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample1.cmd)]

A questo punto aggiungere una classe denominata `Startup` con il codice seguente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample2.cs)]

Questo è tutto ciò che serve per creare un'applicazione "Hello World" per OWIN, in esecuzione in IIS. ‎Premere F5 per eseguire il debug dell'applicazione. Dovrebbe essere visualizzato "Hello World!" nella finestra del browser.

![](enabling-windows-authentication-in-katana/_static/image2.png)

Successivamente, si Abilita l'autenticazione di Windows in IIS Express. Scegliere **Proprietà**dal menu **Visualizza** . Per visualizzare le proprietà del progetto, fare clic sul nome del progetto in Esplora soluzioni.

Nella finestra **Proprietà** impostare **autenticazione anonima** su **disabilitato** e impostare **autenticazione di Windows** su **abilitato**.

![](enabling-windows-authentication-in-katana/_static/image3.png)

Quando l'applicazione viene eseguita da Visual Studio, IIS Express richiederà le credenziali di Windows dell'utente. Per visualizzarlo, è possibile usare [Fiddler](http://fiddler2.com/home) o un altro strumento di debug http. Di seguito è riportato un esempio di risposta HTTP:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample3.cmd?highlight=1,5-6)]

Le intestazioni WWW-Authenticate in questa risposta indicano che il server supporta il protocollo [Negotiate](http://www.ietf.org/rfc/rfc4559.txt) , che usa Kerberos o NTLM.

Successivamente, quando si distribuisce l'applicazione in un server, attenersi alla [procedura](https://www.iis.net/configreference/system.webserver/security/authentication/windowsauthentication) seguente per abilitare l'autenticazione di Windows in IIS su tale server.

## <a name="windows-authentication-in-httplistener"></a>Autenticazione di Windows in HttpListener

Se si usa Microsoft. Owin. host. HttpListener per ospitare in modo autonomo la katana, è possibile abilitare l'autenticazione di Windows direttamente nell'istanza di **HttpListener** .

Per prima cosa, creare una nuova applicazione console. Aggiungere quindi i pacchetti NuGet. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**, quindi selezionare Console di **Gestione pacchetti**. Nella finestra Console di gestione pacchetti immettere il comando seguente:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample4.cmd)]

A questo punto aggiungere una classe denominata `Startup` con il codice seguente:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample5.cs)]

Questa classe implementa lo stesso esempio "Hello World" di prima, ma imposta anche l'autenticazione di Windows come schema di autenticazione.

All'interno della funzione `Main`, avviare la pipeline OWIN:

[!code-csharp[Main](enabling-windows-authentication-in-katana/samples/sample6.cs)]

È possibile inviare una richiesta in Fiddler per confermare che l'applicazione usa l'autenticazione di Windows:

[!code-console[Main](enabling-windows-authentication-in-katana/samples/sample7.cmd?highlight=1,4-5)]

## <a name="related-topics"></a>Argomenti correlati

[Panoramica del progetto Katana](an-overview-of-project-katana.md)

[System.Net.HttpListener](https://msdn.microsoft.com/library/system.net.httplistener.aspx)

[Informazioni sull'autenticazione basata su form OWIN in MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/07/03/understanding-owin-forms-authentication-in-mvc-5.aspx)
