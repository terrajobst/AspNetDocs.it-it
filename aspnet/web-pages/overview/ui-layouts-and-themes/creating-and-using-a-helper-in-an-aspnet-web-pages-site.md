---
uid: web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
title: Creazione e uso di un helper in un sito di Pagine Web ASP.NET (Razor) | Microsoft Docs
author: Rick-Anderson
description: Questo articolo descrive come creare un helper in un sito Web di Pagine Web ASP.NET (Razor). Un helper è un componente riutilizzabile che include codice e markup per le prestazioni...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: 46bff772-01e0-40f0-9ae6-9e18c5442ee6
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/creating-and-using-a-helper-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 380663951094c9fc7d5f0601e30995fa073a204b
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78563511"
---
# <a name="creating-and-using-a-helper-in-an-aspnet-web-pages-razor-site"></a>Creazione e utilizzo di un helper in un sito di Pagine Web ASP.NET (Razor)

di [Tom FitzMacken](https://github.com/tfitzmac)

> Questo articolo descrive come creare un helper in un sito Web di Pagine Web ASP.NET (Razor). Un *Helper* è un componente riutilizzabile che include codice e markup per eseguire un'attività che può essere noiosa o complessa.
> 
> **Cosa si apprenderà:** 
> 
> - Come creare e usare un helper semplice.
> 
> Queste sono le funzionalità di ASP.NET introdotte nell'articolo:
> 
> - Sintassi del `@helper`.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versioni del software usate nell'esercitazione
> 
> 
> - Pagine Web ASP.NET (Razor) 3
>   
> 
> Questa esercitazione funziona anche con Pagine Web ASP.NET 2.

## <a name="overview-of-helpers"></a>Panoramica degli helper

Se è necessario eseguire le stesse attività in pagine diverse del sito, è possibile usare un helper. Pagine Web ASP.NET include un numero di helper e molti altri possono essere scaricati e installati. Un elenco degli Helper incorporati in Pagine Web ASP.NET è elencato nel [riferimento rapido all'API ASP.NET](https://go.microsoft.com/fwlink/?LinkId=202907). Se nessuno degli helper esistenti soddisfa le proprie esigenze, è possibile creare un proprio helper.

Un helper consente di usare un blocco di codice comune su più pagine. Si supponga che nella pagina venga spesso creato un elemento della nota che è stato impostato oltre ai paragrafi normali. È possibile che la nota venga creata come un elemento `<div>` con stile come una casella con un bordo. Anziché aggiungere questo stesso markup a una pagina ogni volta che si desidera visualizzare una nota, è possibile creare un pacchetto del markup come helper. È quindi possibile inserire la nota con una sola riga di codice ovunque sia necessario.

L'uso di un helper in questo modo rende più semplice e leggibile il codice in ogni pagina. Semplifica inoltre la manutenzione del sito, perché se è necessario modificare l'aspetto delle note, è possibile modificare il markup in un'unica posizione.

## <a name="creating-a-helper"></a>Creazione di un helper

Questa procedura illustra come creare l'helper che crea la nota, come appena descritto. Si tratta di un esempio semplice, ma l'helper personalizzato può includere qualsiasi markup e codice ASP.NET necessario.

1. Nella cartella radice del sito Web creare una cartella denominata *App\_codice*. Si tratta di un nome di cartella riservata in ASP.NET in cui è possibile inserire il codice per i componenti come gli helper.
2. Nella cartella del *codice dell'App\_* creare un nuovo file con *estensione cshtml* e denominarlo *Helper. cshtml*.
3. Sostituire il contenuto esistente con il codice seguente:

    [!code-cshtml[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample1.cshtml)]

    Il codice usa la sintassi `@helper` per dichiarare un nuovo Helper denominato `MakeNote`. Questo helper specifico consente di passare un parametro denominato `content` che può contenere una combinazione di testo e markup. L'helper inserisce la stringa nel corpo della nota usando la variabile `@content`.

    Si noti che il file è denominato *Helper. cshtml*, ma l'helper è denominato `MakeNote`. È possibile inserire più helper personalizzati in un singolo file.
4. Salvare e chiudere il file.

## <a name="using-the-helper-in-a-page"></a>Uso dell'helper in una pagina

1. Nella cartella radice creare un nuovo file vuoto denominato *TestHelper. cshtml*.
2. Aggiungere al file il codice seguente:

    [!code-html[Main](creating-and-using-a-helper-in-an-aspnet-web-pages-site/samples/sample2.html)]

    Per chiamare l'helper creato, usare `@` seguito dal nome del file in cui si trova l'helper, un punto e quindi il nome dell'helper. Se sono presenti più cartelle nell' *App\_* cartella del codice, è possibile usare la sintassi `@FolderName.FileName.HelperName` per chiamare l'helper in qualsiasi livello di cartella nidificata). Il testo aggiunto tra virgolette tra parentesi è il testo che verrà visualizzato dall'helper nell'ambito della nota nella pagina Web.
3. Salvare la pagina ed eseguirla in un browser. L'helper genera l'elemento della nota nella posizione in cui è stato chiamato l'helper: tra i due paragrafi.

    ![Screenshot che mostra la pagina nel browser e il modo in cui l'helper ha generato il markup che inserisce una casella intorno al testo specificato.](creating-and-using-a-helper-in-an-aspnet-web-pages-site/_static/image1.png)

## <a name="additional-resources"></a>Risorse aggiuntive

[Menu orizzontale come helper Razor](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2341). Questo post di Blog di Mike Pope Mostra come creare un menu orizzontale come helper usando markup, CSS e codice.

Uso [di HTML5 in pagine Web ASP.NET helper per WebMatrix e ASP.NET MvC3](http://geekswithblogs.net/wildturtle/archive/2010/11/08/html5-in-asp.net-web-pages-helpers-for-webmatrix-and_aspnet_mvc3.aspx). Questo post di Blog di Sam Abraham Mostra un helper che esegue il rendering di un elemento `Canvas` HTML5.

[Differenza tra @Helpers e @Functions in WebMatrix](http://www.mikesdotnetting.com/Article/173/The-Difference-Between-@Helpers-and-@Functions-In-WebMatrix). Questo post di Blog di Mike Brind descrive `@helper` sintassi e `@function` sintassi e quando usarli.
