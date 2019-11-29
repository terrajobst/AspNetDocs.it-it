---
uid: aspnet/mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
title: Creazione di helper HTML personalizzati (C#) | Microsoft Docs
author: microsoft
description: L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC. Sfruttando l'helper HTML...
ms.author: riande
ms.date: 10/07/2008
ms.assetid: e454c67d-a86e-4119-a858-eb04bbec2dff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-custom-html-helpers-cs
msc.type: authoredcontent
ms.openlocfilehash: 264ff9850bad397826b45649d52fbfefafc53a01
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594494"
---
# <a name="creating-custom-html-helpers-c"></a>Creazione di helper HTML personalizzati (C#)

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](https://download.microsoft.com/download/1/1/f/11f721aa-d749-4ed7-bb89-a681b68894e6/ASPNET_MVC_Tutorial_9_CS.pdf)

> L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC. Sfruttando gli helper HTML, è possibile ridurre la quantità di tipi noiosi di tag HTML che è necessario eseguire per creare una pagina HTML standard.

L'obiettivo di questa esercitazione è illustrare come creare helper HTML personalizzati che è possibile usare nelle visualizzazioni MVC. Sfruttando gli helper HTML, è possibile ridurre la quantità di tipi noiosi di tag HTML che è necessario eseguire per creare una pagina HTML standard.

Nella prima parte di questa esercitazione vengono descritti alcuni degli helper HTML esistenti inclusi in ASP.NET MVC Framework. Vengono quindi descritti due metodi per la creazione di helper HTML personalizzati: viene illustrato come creare helper HTML personalizzati creando un metodo statico e creando un metodo di estensione.

## <a name="understanding-html-helpers"></a>Informazioni sugli helper HTML

Un helper HTML è semplicemente un metodo che restituisce una stringa. La stringa può rappresentare qualsiasi tipo di contenuto desiderato. Ad esempio, è possibile usare gli helper HTML per eseguire il rendering di tag HTML standard come HTML `<input>` e tag di `<img>`. È anche possibile usare gli helper HTML per eseguire il rendering di contenuto più complesso, ad esempio una scheda o una tabella HTML di dati del database.

Il framework ASP.NET MVC include il seguente set di helper HTML standard (non è un elenco completo):

- HTML. ActionLink ()
- HTML. BeginForm ()
- HTML. CheckBox ()
- HTML. DropDownList ()
- HTML. EndForm ()
- HTML. Hidden ()
- HTML. ListBox ()
- HTML. password ()
- HTML. RadioButton ()
- HTML. TextArea ()
- HTML. TextBox ()

Si consideri, ad esempio, il modulo nel listato 1. Questo form viene sottoposto a rendering con l'aiuto di due degli helper HTML standard (vedere la figura 1). Questo modulo usa i metodi di supporto `Html.BeginForm()` e `Html.TextBox()` per eseguire il rendering di un semplice form HTML.

[![pagina sottoposta a rendering con helper HTML](creating-custom-html-helpers-cs/_static/image2.png)](creating-custom-html-helpers-cs/_static/image1.png)

**Figura 01**: rendering della pagina con helper HTML ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-html-helpers-cs/_static/image3.png))

**Listato 1-`Views\Home\Index.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample1.aspx)]

Il metodo helper HTML. BeginForm () viene usato per creare i tag HTML `<form>` di apertura e chiusura. Si noti che il metodo `Html.BeginForm()` viene chiamato all'interno di un'istruzione using. L'istruzione using garantisce che il tag `<form>` venga chiuso alla fine del blocco using.

Se si preferisce, anziché creare un blocco using, è possibile chiamare il metodo helper HTML. EndForm () per chiudere il tag `<form>`. Usare qualsiasi approccio per la creazione di un tag `<form>` di apertura e chiusura che sembra più intuitivo.

Il `Html.TextBox()` metodi helper vengono usati nel listato 1 per eseguire il rendering dei tag HTML `<input>`. Se si seleziona Visualizza origine nel browser, viene visualizzata l'origine HTML nel listato 2. Si noti che l'origine contiene tag HTML standard.

> [!IMPORTANT]
> Si noti che viene eseguito il rendering dell'helper `Html.TextBox()`-HTML con tag `<%= %>` invece dei tag `<% %>`. Se non si include il segno di uguale, non viene eseguito il rendering di alcun elemento nel browser.

Il Framework di MVC ASP.NET contiene un piccolo set di helper. È molto probabile che sia necessario estendere il framework MVC con gli helper HTML personalizzati. Nella parte restante di questa esercitazione vengono illustrati due metodi per la creazione di helper HTML personalizzati.

**Listato 2: `Index.aspx Source`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample2.aspx)]

### <a name="creating-html-helpers-with-static-methods"></a>Creazione di helper HTML con metodi statici

Il modo più semplice per creare un nuovo helper HTML consiste nel creare un metodo statico che restituisca una stringa. Si supponga, ad esempio, che si decida di creare un nuovo helper HTML che esegue il rendering di un tag HTML `<label>`. Per eseguire il rendering di una `<label>`, è possibile utilizzare la classe nel listato 2.

**Listato 2: `Helpers\LabelHelper.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample3.cs)]

Non c'è niente di speciale sulla classe nel listato 2. Il metodo `Label()` restituisce semplicemente una stringa.

La vista index modificata nel listato 3 usa il `LabelHelper` per eseguire il rendering dei tag HTML `<label>`. Si noti che la vista include una direttiva `<%@ imports %>` che importa lo spazio dei nomi `Application1.Helpers`.

**Listato 2: `Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample4.aspx)]

### <a name="creating-html-helpers-with-extension-methods"></a>Creazione di helper HTML con i metodi di estensione

Se si desidera creare helper HTML che funzionano esattamente come gli helper HTML standard inclusi nel framework ASP.NET MVC, è necessario creare i metodi di estensione. I metodi di estensione consentono di aggiungere nuovi metodi a una classe esistente. Quando si crea un metodo helper HTML, si aggiungono nuovi metodi alla classe HtmlHelper rappresentata dalla proprietà HTML di una visualizzazione.

La classe nel listato 3 aggiunge un metodo di estensione alla classe `HtmlHelper` denominata `Label()`. Ci sono un paio di aspetti da notare in merito a questa classe. Si noti prima di tutto che la classe è una classe statica. È necessario definire un metodo di estensione con una classe statica.

In secondo luogo, si noti che il primo parametro del `Label()` metodo è preceduto dalla parola chiave `this`. Il primo parametro di un metodo di estensione indica la classe estesa dal metodo di estensione.

**Listato 3-`Helpers\LabelExtensions.cs`**

[!code-csharp[Main](creating-custom-html-helpers-cs/samples/sample5.cs)]

Dopo aver creato un metodo di estensione e aver compilato correttamente l'applicazione, il metodo di estensione viene visualizzato in Visual Studio IntelliSense come tutti gli altri metodi di una classe (vedere la figura 2). L'unica differenza è che i metodi di estensione vengono visualizzati con un simbolo speciale accanto ad essi (icona di una freccia verso il basso).

[![usando il metodo di estensione HTML. Label ()](creating-custom-html-helpers-cs/_static/image5.png)](creating-custom-html-helpers-cs/_static/image4.png)

**Figura 02**: uso del metodo di estensione HTML. Label () ([fare clic per visualizzare l'immagine con dimensioni complete](creating-custom-html-helpers-cs/_static/image6.png))

La vista index modificata nel listato 4 usa il metodo di estensione HTML. Label () per eseguire il rendering di tutti i tag `<label>`.

**Listato 4-`Views\Home\Index3.aspx`**

[!code-aspx[Main](creating-custom-html-helpers-cs/samples/sample6.aspx)]

## <a name="summary"></a>Riepilogo

In questa esercitazione sono stati appresi due metodi per la creazione di helper HTML personalizzati. In primo luogo, si è appreso come creare un helper HTML `Label()` personalizzato creando un metodo statico che restituisce una stringa. A questo punto si è appreso come creare un metodo di helper HTML `Label()` personalizzato creando un metodo di estensione nella classe `HtmlHelper`.

In questa esercitazione si è concentrato sulla creazione di un metodo helper HTML estremamente semplice. Tenere presente che un helper HTML può essere complicato come si desidera. È possibile compilare helper HTML che eseguono il rendering di contenuti avanzati, ad esempio visualizzazioni ad albero, menu o tabelle di dati del database.

> [!div class="step-by-step"]
> [Precedente](asp-net-mvc-views-overview-cs.md)
> [Successivo](using-the-tagbuilder-class-to-build-html-helpers-cs.md)
