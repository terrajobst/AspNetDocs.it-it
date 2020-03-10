---
uid: web-api/overview/security/forms-authentication
title: Autenticazione basata su form in API Web ASP.NET | Microsoft Docs
author: MikeWasson
description: Viene descritto l'utilizzo dell'autenticazione basata su form in API Web ASP.NET.
ms.author: riande
ms.date: 12/12/2012
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 147bfab76e48497f35a72b28cd935f40ec4193bf
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78598595"
---
# <a name="forms-authentication-in-aspnet-web-api"></a>Autenticazione basata su form in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

L'autenticazione basata su form utilizza un form HTML per inviare le credenziali dell'utente al server. Non è uno standard Internet. L'autenticazione basata su form è appropriata solo per le API Web chiamate da un'applicazione Web, in modo che l'utente possa interagire con il form HTML.

| Vantaggi | Svantaggi |
| --- | --- |
| -Facile da implementare: incorporata in ASP.NET. : Usa il provider di appartenenze ASP.NET, che consente di gestire facilmente gli account utente. | -Non è un meccanismo di autenticazione HTTP standard. usa cookie HTTP anziché l'intestazione di autorizzazione standard. -Richiede un client browser. -Le credenziali vengono inviate come testo non crittografato. -Vulnerabile a richieste intersito false (CSRF); richiede misure anti-CSRF. -Difficile da utilizzare da client non browser. Per l'account di accesso è necessario un browser. -Le credenziali utente vengono inviate nella richiesta. -Alcuni utenti disabilitano i cookie. |

In breve, l'autenticazione basata su form in ASP.NET funziona come segue:

1. Il client richiede una risorsa che richiede l'autenticazione.
2. Se l'utente non è autenticato, il server restituisce HTTP 302 (trovato) e reindirizza a una pagina di accesso.
3. L'utente immette le credenziali e invia il modulo.
4. Il server restituisce un altro HTTP 302 che reindirizza all'URI originale. Questa risposta include un cookie di autenticazione.
5. Il client richiede nuovamente la risorsa. La richiesta include il cookie di autenticazione, quindi il server concede la richiesta.

![](forms-authentication/_static/image1.png)

Per ulteriori informazioni, vedere [Panoramica dell'autenticazione basata su form.](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>Uso dell'autenticazione basata su form con l'API Web

Per creare un'applicazione che utilizza l'autenticazione basata su form, selezionare il modello "applicazione Internet" nella creazione guidata progetto MVC 4. Questo modello crea controller MVC per la gestione degli account. È anche possibile usare il modello "applicazione a pagina singola", disponibile nell'aggiornamento ASP.NET Fall 2012.

Nei controller dell'API Web è possibile limitare l'accesso usando l'attributo `[Authorize]`, come descritto in [uso dell'attributo [autorizzate]](authentication-and-authorization-in-aspnet-web-api.md#auth3).

L'autenticazione basata su form usa un cookie di sessione per autenticare le richieste. I browser inviano automaticamente tutti i cookie pertinenti al sito Web di destinazione. Questa funzionalità rende l'autenticazione basata su form potenzialmente vulnerabile agli attacchi di richiesta intersito falsa (CSRF) vedere [prevenzione degli attacchi di richiesta intersito falsificazione (CSRF)](preventing-cross-site-request-forgery-csrf-attacks.md).

L'autenticazione basata su form non consente di crittografare le credenziali dell'utente. Pertanto, l'autenticazione basata su form non è protetta, a meno che non venga utilizzata con SSL. Vedere [uso di SSL nell'API Web](working-with-ssl-in-web-api.md).
