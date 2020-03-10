---
uid: web-api/overview/security/basic-authentication
title: Autenticazione di base in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo dell'autenticazione di base in API Web ASP.NET.
ms.author: riande
ms.date: 10/02/2014
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1470bd4b5abd5199b9a5105973b053812d643351
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78555727"
---
# <a name="basic-authentication-in-aspnet-web-api"></a>Autenticazione di base in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

L'autenticazione di base è definita in [RFC 2617, autenticazione http: autenticazione dell'accesso di base e digest](http://www.ietf.org/rfc/rfc2617.txt).

Svantaggi

- Le credenziali utente vengono inviate nella richiesta.
- Le credenziali vengono inviate come testo non crittografato.
- Le credenziali vengono inviate con ogni richiesta.
- Non è possibile disconnettersi, tranne terminando la sessione del browser.
- Vulnerabile a richieste intersito false (CSRF); richiede misure anti-CSRF.

Vantaggi

- Internet standard.
- Supportato da tutti i browser principali.
- Protocollo relativamente semplice.

L'autenticazione di base funziona nel modo seguente:

1. Se per una richiesta è necessaria l'autenticazione, il server restituisce 401 (non autorizzato). La risposta include un'intestazione WWW-Authenticate, che indica che il server supporta l'autenticazione di base.
2. Il client invia un'altra richiesta con le credenziali client nell'intestazione dell'autorizzazione. Le credenziali vengono formattate come stringa "Name: password", con codifica Base64. Le credenziali non vengono crittografate.

L'autenticazione di base viene eseguita nel contesto di un'area di autenticazione. Il server include il nome dell'area di autenticazione nell'intestazione WWW-Authenticate. Le credenziali dell'utente sono valide all'interno dell'area di autenticazione. L'ambito esatto di un'area di autenticazione è definito dal server. È ad esempio possibile definire più aree di autenticazione per partizionare le risorse.

![](basic-authentication/_static/image1.png)

Poiché le credenziali vengono inviate senza crittografia, l'autenticazione di base è protetta solo tramite HTTPS. Vedere [uso di SSL nell'API Web](working-with-ssl-in-web-api.md).

L'autenticazione di base è anche vulnerabile agli attacchi CSRF. Dopo l'immissione delle credenziali da parte dell'utente, il browser le invia automaticamente alle richieste successive allo stesso dominio per la durata della sessione. Sono incluse le richieste AJAX. Vedere [prevenzione degli attacchi di richiesta intersito falsificazione (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

## <a name="basic-authentication-with-iis"></a>Autenticazione di base con IIS

IIS supporta l'autenticazione di base, ma è presente un avvertimento: l'utente viene autenticato con le credenziali di Windows. Ciò significa che l'utente deve disporre di un account nel dominio del server. Per un sito Web pubblico, in genere si desidera eseguire l'autenticazione in base a un provider di appartenenze ASP.NET.

Per abilitare l'autenticazione di base usando IIS, impostare la modalità di autenticazione su "Windows" nel file Web. config del progetto ASP.NET:

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

In questa modalità, IIS utilizza le credenziali di Windows per l'autenticazione. Inoltre, è necessario abilitare l'autenticazione di base in IIS. In Gestione IIS passare a visualizzazione funzionalità, selezionare autenticazione e Abilita autenticazione di base.

![](basic-authentication/_static/image2.png)

Nel progetto API Web aggiungere l'attributo `[Authorize]` per le azioni del controller che richiedono l'autenticazione.

Un client esegue l'autenticazione impostando l'intestazione di autorizzazione nella richiesta. I client del browser eseguono questo passaggio automaticamente. I client non browser dovranno impostare l'intestazione.

## <a name="basic-authentication-with-custom-membership"></a>Autenticazione di base con appartenenza personalizzata

Come indicato in precedenza, l'autenticazione di base incorporata in IIS utilizza le credenziali di Windows. Ciò significa che è necessario creare account per gli utenti nel server di hosting. Per un'applicazione Internet, tuttavia, gli account utente vengono in genere archiviati in un database esterno.

Il codice seguente illustra come un modulo HTTP che esegue l'autenticazione di base. È possibile inserire facilmente un provider di appartenenze ASP.NET sostituendo il metodo `CheckPassword`, che in questo esempio è un metodo fittizio.

Nell'API Web 2 è consigliabile scrivere un filtro di [autenticazione](authentication-filters.md) o un [middleware OWIN](../../../aspnet/overview/owin-and-katana/index.md)anziché un modulo HTTP.

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

Per abilitare il modulo HTTP, aggiungere il codice seguente al file Web. config nella sezione **System. webserver** :

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

Sostituire "YourAssemblyName" con il nome dell'assembly (esclusa l'estensione "dll").

È necessario disabilitare altri schemi di autenticazione, ad esempio moduli o autenticazione di Windows.
