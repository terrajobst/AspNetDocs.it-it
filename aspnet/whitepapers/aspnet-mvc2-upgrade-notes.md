---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Aggiornamento di un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Questo documento descrive come eseguire l'aggiornamento manualmente e con una procedura guidata un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2. Questo documento è disponibile anche per d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 27589f1b1c9d5038118e5ff0cc2e7cecae17d5ed
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78637018"
---
# <a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Aggiornamento di un'applicazione ASP.NET MVC 1.0 ad ASP.NET MVC 2

> Questo documento descrive come eseguire l'aggiornamento manualmente e con una procedura guidata un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2. Questo documento è disponibile anche per il [download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)

## <a name="introduction"></a>Introduzione

ASP.NET MVC 2 può essere installato side-by-side con ASP.NET MVC 1,0 nello stesso server. Questo consente agli sviluppatori di applicazioni di scegliere quando eseguire l'aggiornamento di un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2.

Visual Studio 2010 include una procedura guidata che consente di aggiornare i progetti ASP.NET MVC 1,0 esistenti compilati con Visual Studio 2008 a ASP.NET MVC 2. L'aggiornamento guidato viene avviato aprendo un progetto ASP.NET MVC 1,0 in Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Aggiornamento guidato per ASP.NET MVC 1,0 in Visual Studio 2008 SP1

Per aggiornare un'applicazione ASP.NET MVC 1,0 a ASP.NET MVC 2 in Visual Studio 2008 SP1, usare l'applicazione MvcAppConverter (non supportata). È possibile scaricare l'applicazione dall'URL seguente:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Aggiornamento manuale di un progetto MVC 1,0 di ASP.NET

Per aggiornare manualmente un'applicazione ASP.NET MVC 1,0 esistente alla versione 2, seguire questa procedura:

1. Eseguire un backup del progetto esistente.
2. In un editor di testo aprire il file di progetto (il file con estensione csproj o vbproj) e trovare l'elemento ProjectTypeGuid. Come valore di tale elemento, sostituire il GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}. Al termine, il valore dell'elemento deve essere il seguente: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Nella cartella radice dell'applicazione Web modificare il file Web. config. Cercare System. Web. MVC, Version = 1.0.0.0 e sostituire tutte le istanze con System. Web. MVC, Version = 2.0.0.0.
4. Ripetere il passaggio precedente per il file Web. config che si trova nella cartella views.
5. Aprire il progetto con Visual Studio e in **Esplora soluzioni**espandere il nodo **riferimenti** . Eliminare il riferimento a System. Web. MVC, che fa riferimento all'assembly della versione 1,0. Aggiungere un riferimento a System. Web. Mvc (v 2.0.0.0).
6. Aggiungere l'elemento bindingRedirect seguente al file Web. config nella radice dell'applicazione nella sezione configuraton:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Creare una nuova applicazione MVC 2 ASP.NET vuota. Copiare i file dalla cartella Scripts della nuova applicazione nella cartella Scripts dell'applicazione esistente.
8. Aggiornare il file CSS ApplicationA™ s esistente con le definizioni di stile CSS nel file site. CSS.
9. Compilare l'applicazione ed eseguirla. Se si verificano errori, vedere la sezione relativa alle modifiche di rilievo nella pagina Novità di [ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) .
