---
title: Introduzione alle autorizzazioni in ASP.NET Core
author: rick-anderson
description: Informazioni di base del funzionamento dell'autorizzazione nelle App ASP.NET Core e l'autorizzazione.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/introduction
ms.openlocfilehash: 5465eb7875ebecd77b628376ef886db0ddd05025
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57046368"
---
# <a name="introduction-to-authorization-in-aspnet-core"></a><span data-ttu-id="e8db6-103">Introduzione alle autorizzazioni in ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8db6-103">Introduction to authorization in ASP.NET Core</span></span>

<a name="security-authorization-introduction"></a>

<span data-ttu-id="e8db6-104">Autorizzazione si riferisce al processo che determina ciò che un utente è in grado di eseguire.</span><span class="sxs-lookup"><span data-stu-id="e8db6-104">Authorization refers to the process that determines what a user is able to do.</span></span> <span data-ttu-id="e8db6-105">Ad esempio, un utente amministratore è consentito per creare una raccolta documenti aggiungere i documenti, modificare i documenti ed eliminarli.</span><span class="sxs-lookup"><span data-stu-id="e8db6-105">For example, an administrative user is allowed to create a document library, add documents, edit documents, and delete them.</span></span> <span data-ttu-id="e8db6-106">Un utente non amministratore, utilizzare la libreria solo è autorizzato a leggere i documenti.</span><span class="sxs-lookup"><span data-stu-id="e8db6-106">A non-administrative user working with the library is only authorized to read the documents.</span></span>

<span data-ttu-id="e8db6-107">L'autorizzazione è ortogonale e indipendente dall'autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e8db6-107">Authorization is orthogonal and independent from authentication.</span></span> <span data-ttu-id="e8db6-108">Tuttavia, autorizzazione, è necessario un meccanismo di autenticazione.</span><span class="sxs-lookup"><span data-stu-id="e8db6-108">However, authorization requires an authentication mechanism.</span></span> <span data-ttu-id="e8db6-109">L'autenticazione è il processo di accerta l'identità è un utente.</span><span class="sxs-lookup"><span data-stu-id="e8db6-109">Authentication is the process of ascertaining who a user is.</span></span> <span data-ttu-id="e8db6-110">L'autenticazione può creare una o più identità per l'utente corrente.</span><span class="sxs-lookup"><span data-stu-id="e8db6-110">Authentication may create one or more identities for the current user.</span></span>

## <a name="authorization-types"></a><span data-ttu-id="e8db6-111">Tipi di autorizzazione</span><span class="sxs-lookup"><span data-stu-id="e8db6-111">Authorization types</span></span>

<span data-ttu-id="e8db6-112">Autorizzazione di ASP.NET Core fornisce un semplice e dichiarativo [ruolo](xref:security/authorization/roles) e un ricco [basata su criteri](xref:security/authorization/policies) modello.</span><span class="sxs-lookup"><span data-stu-id="e8db6-112">ASP.NET Core authorization provides a simple, declarative [role](xref:security/authorization/roles) and a rich [policy-based](xref:security/authorization/policies) model.</span></span> <span data-ttu-id="e8db6-113">Autorizzazione viene espresso in requisiti e i gestori di valutare le attestazioni dell'utente in base ai requisiti.</span><span class="sxs-lookup"><span data-stu-id="e8db6-113">Authorization is expressed in requirements, and handlers evaluate a user's claims against requirements.</span></span> <span data-ttu-id="e8db6-114">Controlli imperativi possono basarsi su simple o criteri di cui valutano l'identità dell'utente e le proprietà della risorsa a cui l'utente sta provando ad accedere.</span><span class="sxs-lookup"><span data-stu-id="e8db6-114">Imperative checks can be based on simple policies or policies which evaluate both the user identity and properties of the resource that the user is attempting to access.</span></span>

## <a name="namespaces"></a><span data-ttu-id="e8db6-115">Spazi dei nomi</span><span class="sxs-lookup"><span data-stu-id="e8db6-115">Namespaces</span></span>

<span data-ttu-id="e8db6-116">I componenti di autorizzazione, inclusi i `AuthorizeAttribute` e `AllowAnonymousAttribute` gli attributi, si trovano nel `Microsoft.AspNetCore.Authorization` dello spazio dei nomi.</span><span class="sxs-lookup"><span data-stu-id="e8db6-116">Authorization components, including the `AuthorizeAttribute` and `AllowAnonymousAttribute` attributes, are found in the `Microsoft.AspNetCore.Authorization` namespace.</span></span>

<span data-ttu-id="e8db6-117">Consultare la documentazione sul [autorizzazione semplice](xref:security/authorization/simple).</span><span class="sxs-lookup"><span data-stu-id="e8db6-117">Consult the documentation on [simple authorization](xref:security/authorization/simple).</span></span>
