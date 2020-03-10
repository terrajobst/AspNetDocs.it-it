---
uid: web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
title: Rendering dei siti Pagine Web ASP.NET (Razor) per i dispositivi mobili | Microsoft Docs
author: Rick-Anderson
description: 'Questo articolo descrive come creare pagine in un sito di Pagine Web ASP.NET (Razor) che eseguirà il rendering appropriato nei dispositivi mobili. Che cosa apprenderai: come te...'
ms.author: riande
ms.date: 02/17/2014
ms.assetid: f15ab392-c05e-4269-83bf-7c6d2b8c8ec8
msc.legacyurl: /web-pages/overview/mobile/rendering-aspnet-web-pages-sites-for-mobile-devices
msc.type: authoredcontent
ms.openlocfilehash: c012348d65e48a275cb0e4808fef2a7f31e5fb33
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563567"
---
# <a name="rendering-aspnet-web-pages-razor-sites-for-mobile-devices"></a>Rendering dei siti di Pagine Web ASP.NET (Razor) per i dispositivi mobili

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come creare pagine in un sito di Pagine Web ASP.NET (Razor) che eseguirà il rendering appropriato nei dispositivi mobili.
> 
> Contenuto dell'esercitazione:
> 
> - Come usare una convenzione di denominazione per specificare che una pagina è progettata in modo specifico per i dispositivi mobili.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

Pagine Web ASP.NET consente di creare visualizzazioni personalizzate per il rendering del contenuto in dispositivi mobili o in altri dispositivi.

Il modo più semplice per creare una pagina specifica del dispositivo in un sito Pagine Web ASP.NET consiste nell'usare un modello di denominazione dei file simile al seguente: *filename. mobile. cshtml*. È possibile creare due versioni di una pagina (ad esempio, una denominata *MyFile. cshtml* e una denominata *MyFile. mobile. cshtml*). In fase di esecuzione, quando un dispositivo mobile richiede *MyFile. cshtml*, ASP.NET esegue il rendering del contenuto da *MyFile. mobile. cshtml*. In caso contrario, viene eseguito il rendering di *MyFile. cshtml* .

Nell'esempio seguente viene illustrato come abilitare il rendering mobile aggiungendo una pagina di contenuto per i dispositivi mobili. *Pagina1. cshtml* contiene contenuto più una barra laterale di navigazione. *Pagina1. mobile. cshtml* contiene lo stesso contenuto, ma omette l'intestazione laterale.

1. In un sito di Pagine Web ASP.NET creare un file denominato *Pagina1. cshtml* e sostituire il contenuto corrente con il markup seguente.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample1.html)]
2. Creare un file denominato *Pagina1. mobile. cshtml* e sostituire il contenuto esistente con il markup seguente. Si noti che la versione per dispositivi mobili della pagina omette la sezione di navigazione per un migliore rendering in una schermata più piccola.

    [!code-html[Main](rendering-aspnet-web-pages-sites-for-mobile-devices/samples/sample2.html)]
3. Eseguire un browser desktop e passare a *Pagina1. cshtml*. ![mobilesites-1](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image1.png)
4. Eseguire un browser per dispositivi mobili (o un emulatore di dispositivo mobile) e passare a *Pagina1. cshtml*. Si noti che non è incluso *. mobile.* come parte dell'URL. Anche se la richiesta è a *Pagina1. cshtml*, ASP.NET esegue il rendering di *Pagina1. mobile. cshtml*.

    ![mobilesites-2](rendering-aspnet-web-pages-sites-for-mobile-devices/_static/image2.png)

> [!NOTE]
> Per testare le pagine per dispositivi mobili, è possibile usare un simulatore di dispositivi mobili che viene eseguito in un computer desktop. Questo strumento consente di testare le pagine Web in modo analogo ai dispositivi mobili, ovvero in genere con un'area di visualizzazione molto più piccola. Un esempio di simulatore è il [componente aggiuntivo User Agent Switcher](http://addons.mozilla.org/firefox/addon/user-agent-switcher/) per Mozilla Firefox, che consente di emulare vari browser per dispositivi mobili da una versione desktop di Firefox.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Risorse aggiuntive

[Emulatore Windows Phone](https://msdn.microsoft.com/library/ff402563(v=VS.92).aspx)
