---
uid: mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
title: Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 1 | Microsoft Docs
author: Rick-Anderson
description: Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un ASP.NET MV...
ms.author: riande
ms.date: 08/29/2011
ms.assetid: c23d27f7-b0cf-44f2-8445-fb69e045c674
msc.legacyurl: /mvc/overview/older-versions/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc/using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1
msc.type: authoredcontent
ms.openlocfilehash: c1c2380f24c72f6aabaaacaf975e95288a384ff1
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457622"
---
# <a name="using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc---part-1"></a>Uso del calendario popup per HTML5 e jQuery UI DatePicker con ASP.NET MVC-parte 1

di [Rick Anderson](https://twitter.com/RickAndMSFT)

> Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il calendario popup DatePicker dell'interfaccia utente di jQuery in un'applicazione Web MVC ASP.NET.

Questa esercitazione illustra le nozioni di base su come usare i modelli di editor, i modelli di visualizzazione e il [calendario popup DatePicker dell'interfaccia utente](http://plugins.jquery.com/project/datepicker) di jQuery in un'applicazione Web MVC ASP.NET. Per questa esercitazione, è possibile utilizzare Microsoft Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer&quot;), che è una versione gratuita di Microsoft Visual Studio oppure è possibile utilizzare Visual Studio 2010 SP1 se questa operazione è già disponibile.

Prima di iniziare, verificare di aver installato i prerequisiti elencati di seguito. È possibile installarli tutti facendo clic sul collegamento seguente: [installazione guidata piattaforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). In alternativa, è possibile installare singolarmente il software necessario usando i collegamenti seguenti:

- [Prerequisiti di Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
- [Aggiornamento degli strumenti di ASP.NET MVC 3](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
- [SQL Server Compact 4,0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(supporto di runtime + Tools)

Se si usa Visual Studio 2010 anziché Visual Web Developer, installare i prerequisiti facendo clic sul collegamento seguente: [prerequisiti di Visual studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).

Questa esercitazione presuppone che sia stata completata l'esercitazione [Introduzione con MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o che si abbia familiarità con lo sviluppo di ASP.NET MVC. Questa esercitazione inizia con il progetto completato dall'esercitazione [Introduzione con MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) .

In questa esercitazione viene illustrato C#il codice in. Tuttavia, il [progetto iniziale](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800) e il progetto completato sono disponibili anche in Visual Basic.

È disponibile un progetto di C# Visual Studio con e Visual Basic codice sorgente per accompagnare questo argomento: [download](https://archive.msdn.microsoft.com/Project/Download/FileDownload.aspx?ProjectName=aspnetmvcsamples&amp;DownloadId=15800).

### <a name="what-youll-build"></a>Scopo dell'esercitazione

È possibile aggiungere modelli (in particolare, modificare e visualizzare modelli) alla semplice applicazione per la visualizzazione di filmati creata nell'esercitazione [Introduzione con MVC 3](../getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) . Si aggiungerà anche un calendario popup [DatePicker dell'interfaccia utente jQuery](http://jqueryui.com/demos/datepicker/) per semplificare il processo di immissione delle date. Lo screenshot seguente mostra l'applicazione modificata con il calendario popup dell'interfaccia utente di jQuery visualizzato.

![jQuery completato](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image1.png)

### <a name="skills-youll-learn"></a>Acquisizione di competenze

In questa esercitazione si apprenderà:

- Come usare gli attributi dello spazio dei nomi [DataAnnotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) per controllare il formato dei dati quando viene visualizzato e quando è in modalità di modifica.
- Come creare modelli (modificare e visualizzare modelli) per controllare la formattazione dei dati.
- Come aggiungere l' [interfaccia utente di jQuery](http://jqueryui.com/demos/datepicker/) come modo per immettere i campi data.

### <a name="getting-started"></a>Guida introduttiva

Se non si ha già l'applicazione per la visualizzazione dei film dal progetto iniziale, scaricarla: 

* [Scaricare](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).
* In Esplora risorse fare clic con il pulsante destro del mouse sul file *MvcMovie. zip* e selezionare **Proprietà**. 
* Nella finestra di dialogo **Proprietà MvcMovie. zip** selezionare **Sblocca**. L'operazione di sblocco impedisce la visualizzazione dell'avviso di sicurezza quando si tenta di utilizzare un file *ZIP* scaricato dal Web.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image2.png)

Fare clic con il pulsante destro del mouse sul file *MvcMovie. zip* e selezionare **Estrai tutto** per decomprimere il file. In Visual Web Developer o Visual Studio 2010 aprire il file *MvcMovieCS\_tu. sln* .

In **Esplora soluzioni**fare doppio clic su *Views\Shared\\_Layout. cshtml* per aprirlo. Modificare l'intestazione `H1` da **MVC Movie app** a **Movie jQuery**. Premere CTRL + F5 per eseguire l'applicazione e fare clic sulla scheda **Home** , che consente di passare al metodo `Index` del controller di film. Per provare l'applicazione, selezionare il collegamento **modifica** e il collegamento **Dettagli** per uno dei filmati. Si noti che nelle visualizzazioni index, Edit e Details la data di rilascio e il prezzo sono formattati correttamente:

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image3.png)

La formattazione per la data e il prezzo è il risultato dell'utilizzo dell'attributo [DisplayFormat](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) sulle proprietà della classe `Movie`.

Aprire il file *Movie.cs* e impostare come commento l'attributo `DisplayFormat` sulle proprietà `ReleaseDate` e `Price`. La classe `Movie` risultante ha un aspetto simile al seguente:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample1.cs)]

Premere di nuovo CTRL + F5 per eseguire l'applicazione e selezionare la scheda **Home** per visualizzare l'elenco di film. Questa volta la data di rilascio Visualizza la data e l'ora e il campo Price non Visualizza più il simbolo di valuta. La modifica apportata alla classe `Movie` ha annullato la formattazione che si è visto in precedenza, ma questa operazione verrà risolta tra qualche minuto.

![](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/_static/image4.png)

### <a name="using-the-dataannotations-datatype-attribute-to-specify-the-data-type"></a>Utilizzo dell'attributo DataType DataAnnotations per specificare il tipo di dati

Sostituire l'attributo `DisplayFormat` impostato come commento per la proprietà `ReleaseDate` con l'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) usando l'enumerazione `Date`. Sostituire l'attributo `DisplayFormat` per la proprietà `Price` con l'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) , questa volta usando l'enumerazione `Currency`. Ecco il codice completato:

[!code-csharp[Main](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-1/samples/sample2.cs)]

Eseguire l'applicazione. La data di rilascio e le proprietà del prezzo sono ora formattate correttamente, ovvero usando formati di data e valuta appropriati. L'attributo [DataType](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) fornisce i metadati del tipo per i modelli MVC ASP.NET incorporati in modo che i campi vengano sottoposte a rendering nel formato corretto. L'utilizzo dell'attributo `DataType` è preferibile all'utilizzo dell'attributo `DisplayFormat` originariamente nel codice, poiché l'attributo `DataType` rende il modello più pulito e più flessibile per scopi come l'internazionalizzazione.

Nella sezione successiva si vedrà come creare modelli personalizzati per visualizzare i campi data.

> [!div class="step-by-step"]
> [avanti](using-the-html5-and-jquery-ui-datepicker-popup-calendar-with-aspnet-mvc-part-2.md)
