---
uid: aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
title: Server di autorizzazione OAuth 2,0 di OWIN | Microsoft Docs
author: hongyes
description: Questa esercitazione illustra come implementare un server di autorizzazione OAuth 2,0 usando il middleware OAuth OWIN. Si tratta di un'esercitazione avanzata che outlin...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 20acee16-c70c-41e9-b38f-92bfcf9a4c1c
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-oauth-20-authorization-server
msc.type: authoredcontent
ms.openlocfilehash: 39c78ee57f791a94af7a5fb66c3ac42d7239760f
ms.sourcegitcommit: b95316530fa51087d6c400ff91814fe37e73f7e8
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/23/2019
ms.locfileid: "70000676"
---
# <a name="owin-oauth-20-authorization-server"></a>Server di autorizzazione OAuth 2.0 OWIN

Il [Framework OAuth 2,0](http://tools.ietf.org/html/rfc6749) consente a un'app di terze parti di ottenere accesso limitato a un servizio http. Invece di usare le credenziali del proprietario della risorsa per accedere a una risorsa protetta, il client ottiene un token di accesso, ovvero una stringa che indica un ambito, una durata e altri attributi di accesso specifici. I token di accesso vengono rilasciati a client di terze parti da un server di autorizzazione con l'approvazione del proprietario della risorsa.

OWIN definisce un'interfaccia standard tra i server Web .NET e le applicazioni Web. L'obiettivo dell'interfaccia OWIN è separare il server e l'applicazione, incoraggiare lo sviluppo di moduli semplici per lo sviluppo Web .NET e, essendo uno standard aperto, stimolare l'ecosistema open source degli strumenti di sviluppo Web .NET.

Per ulteriori informazioni, vedere [OWIN](http://owin.org/)
