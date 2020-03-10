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
# <a name="optimize-build-performance-for-solution"></a>Ottimizzare le prestazioni di compilazione per la soluzione

Visual Studio 2017 15,8 o versione successiva include una voce di **menu: compilazione** > **compilazione ASP.NET** > **ottimizzare le prestazioni di compilazione per la soluzione**.

![Screenshot della nuova voce di menu](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET compila le visualizzazioni in fase di esecuzione, il che significa che un progetto ASP.NET trasporta una copia del compilatore. Tuttavia, in un computer di sviluppo quando la copia del compilatore non corrisponde alla copia di Visual Studio, le prestazioni di compilazione sono influenzate dall'ordine di 1-3 secondi per ogni compilazione incrementale. Questa funzionalità Aggiorna la copia del progetto del compilatore in modo che corrisponda a Visual Studio, che in genere velocizza le compilazioni incrementali.

**Questa operazione si applica solo ai progetti ASP.NET Framework 4.7.1 o versioni successive, ma non a ASP.NET Core.**
