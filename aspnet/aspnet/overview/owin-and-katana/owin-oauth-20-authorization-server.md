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
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617110"
---
# <a name="owin-oauth-20-authorization-server"></a><span data-ttu-id="4702f-104">Server di autorizzazione OAuth 2.0 OWIN</span><span class="sxs-lookup"><span data-stu-id="4702f-104">OWIN OAuth 2.0 Authorization Server</span></span>

<span data-ttu-id="4702f-105">Il [Framework OAuth 2,0](http://tools.ietf.org/html/rfc6749) consente a un'app di terze parti di ottenere accesso limitato a un servizio http.</span><span class="sxs-lookup"><span data-stu-id="4702f-105">The [OAuth 2.0 framework](http://tools.ietf.org/html/rfc6749) enables a third-party app to obtain limited access to an HTTP service.</span></span> <span data-ttu-id="4702f-106">Invece di usare le credenziali del proprietario della risorsa per accedere a una risorsa protetta, il client ottiene un token di accesso, ovvero una stringa che indica un ambito, una durata e altri attributi di accesso specifici.</span><span class="sxs-lookup"><span data-stu-id="4702f-106">Instead of using the resource owner's credentials to access a protected resource, the client obtains an access token (which is a string denoting a specific scope, lifetime, and other access attributes).</span></span> <span data-ttu-id="4702f-107">I token di accesso vengono rilasciati a client di terze parti da un server di autorizzazione con l'approvazione del proprietario della risorsa.</span><span class="sxs-lookup"><span data-stu-id="4702f-107">Access tokens are issued to third-party clients by an authorization server with the approval of the resource owner.</span></span>

<span data-ttu-id="4702f-108">OWIN definisce un'interfaccia standard tra i server Web .NET e le applicazioni Web.</span><span class="sxs-lookup"><span data-stu-id="4702f-108">OWIN defines a standard interface between .NET web servers and web applications.</span></span> <span data-ttu-id="4702f-109">L'obiettivo dell'interfaccia OWIN è separare il server e l'applicazione, incoraggiare lo sviluppo di moduli semplici per lo sviluppo Web .NET e, essendo uno standard aperto, stimolare l'ecosistema open source degli strumenti di sviluppo Web .NET.</span><span class="sxs-lookup"><span data-stu-id="4702f-109">The goal of the OWIN interface is to decouple server and application, encourage the development of simple modules for .NET web development, and, by being an open standard, stimulate the open source ecosystem of .NET web development tools.</span></span>

<span data-ttu-id="4702f-110">Per ulteriori informazioni, vedere [OWIN](http://owin.org/)</span><span class="sxs-lookup"><span data-stu-id="4702f-110">For more information, see [OWIN](http://owin.org/)</span></span>
