---
uid: visual-studio/overview/2017/optimize-build-perf
title: Ottimizzare le prestazioni di compilazione per la soluzione
author: AngelosP
description: Ottimizzare le prestazioni di compilazione per la soluzione
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78622640"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="b74fd-103">Ottimizzare le prestazioni di compilazione per la soluzione</span><span class="sxs-lookup"><span data-stu-id="b74fd-103">Optimize build performance for solution</span></span>

<span data-ttu-id="b74fd-104">Visual Studio 2017 15,8 o versione successiva include una voce di **menu: compilazione** > **compilazione ASP.NET** > **ottimizzare le prestazioni di compilazione per la soluzione**.</span><span class="sxs-lookup"><span data-stu-id="b74fd-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![Screenshot della nuova voce di menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="b74fd-106">ASP.NET compila le visualizzazioni in fase di esecuzione, il che significa che un progetto ASP.NET trasporta una copia del compilatore.</span><span class="sxs-lookup"><span data-stu-id="b74fd-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="b74fd-107">Tuttavia, in un computer di sviluppo quando la copia del compilatore non corrisponde alla copia di Visual Studio, le prestazioni di compilazione sono influenzate dall'ordine di 1-3 secondi per ogni compilazione incrementale.</span><span class="sxs-lookup"><span data-stu-id="b74fd-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="b74fd-108">Questa funzionalità Aggiorna la copia del progetto del compilatore in modo che corrisponda a Visual Studio, che in genere velocizza le compilazioni incrementali.</span><span class="sxs-lookup"><span data-stu-id="b74fd-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="b74fd-109">**Questa operazione si applica solo ai progetti ASP.NET Framework 4.7.1 o versioni successive, ma non a ASP.NET Core.**</span><span class="sxs-lookup"><span data-stu-id="b74fd-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
