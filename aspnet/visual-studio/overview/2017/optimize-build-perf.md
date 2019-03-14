---
uid: visual-studio/overview/2017/optimize-build-perf
title: Ottimizzare le prestazioni di compilazione per la soluzione
author: AngelosP
description: Ottimizzare le prestazioni di compilazione per la soluzione
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/01/2019
ms.locfileid: "57050508"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="f7e88-103">Ottimizzare le prestazioni di compilazione per la soluzione</span><span class="sxs-lookup"><span data-stu-id="f7e88-103">Optimize build performance for solution</span></span>

<span data-ttu-id="f7e88-104">Visual Studio 2017 15.8 o versioni successive includono una voce di menu: **Compilare** > **compilazione ASP.NET** > **ottimizzare le prestazioni di compilazione per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="f7e88-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![schermata della nuova voce di menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="f7e88-106">ASP.NET consente di compilare le relative visualizzazioni in fase di esecuzione, il che significa che un progetto ASP.NET è caratterizzata da una copia del compilatore.</span><span class="sxs-lookup"><span data-stu-id="f7e88-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="f7e88-107">Tuttavia in un computer di sviluppo durante la copia del compilatore non corrisponde la copia di Visual Studio, le prestazioni di compilazione sono interessata all'ordine di secondi da 1 a 3 per la compilazione incrementale.</span><span class="sxs-lookup"><span data-stu-id="f7e88-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="f7e88-108">Questa funzionalità Aggiorna la copia del progetto del compilatore in modo che corrispondano Visual Studio, che in genere consente di velocizzare le compilazioni incrementali.</span><span class="sxs-lookup"><span data-stu-id="f7e88-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="f7e88-109">**Questo è applicabile a ASP.NET Framework 4.7.1 o versioni successive solo progetti, non è applicabile ad ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="f7e88-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
