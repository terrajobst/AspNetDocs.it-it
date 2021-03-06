---
uid: web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
title: Uso di CascadingDropDown con un database (VB) | Microsoft Docs
author: wenz
description: Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altr...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 97a3d33c-c856-43f3-8acb-f1ccbc48221a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/cascadingdropdown/using-cascadingdropdown-with-a-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 4482aa18c4446ec8f5f160c423008398ea2e1d0d
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78535917"
---
# <a name="using-cascadingdropdown-with-a-database-vb"></a>Uso di CascadingDropDown con un database (VB)

di [Christian Wenz](https://github.com/wenz)

[Scarica codice](https://download.microsoft.com/download/9/0/7/907760b1-2c60-4f81-aeb6-ca416a573b0d/cascadingdropdown1.vb.zip) o [Scarica PDF](https://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/cascadingdropdown1VB.pdf)

> Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Per consentire il funzionamento di questo, è necessario creare un servizio Web speciale.

## <a name="overview"></a>Panoramica

Il controllo CascadingDropDown in AJAX Control Toolkit estende un controllo DropDownList in modo che le modifiche in un controllo DropDownList carichino i valori associati in un altro DropDownList. Ad esempio, un elenco include un elenco di stati degli Stati Uniti e l'elenco successivo viene quindi riempito con le città principali in tale stato. Per consentire il funzionamento di questo, è necessario creare un servizio Web speciale.

## <a name="steps"></a>Passaggi

Prima di tutto, è necessaria un'origine dati. In questo esempio vengono utilizzati il database AdventureWorks e il Microsoft SQL Server 2005 Express Edition. Il database è una parte facoltativa di un'installazione di Visual Studio (inclusa l'edizione Express) ed è disponibile anche come download separato in [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064). Il database AdventureWorks fa parte degli esempi SQL Server 2005 e dei database di esempio (scaricare [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)). Il modo più semplice per impostare il database consiste nell'usare il Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;D isplaylang = en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) e alleghi il file di database `AdventureWorks.mdf`.

Per questo esempio si presuppone che l'istanza del SQL Server 2005 Express Edition venga chiamata `SQLEXPRESS` e che risiede nello stesso computer del server Web. si tratta anche della configurazione predefinita. Se la configurazione è diversa, è necessario adattare le informazioni di connessione per il database.

Per attivare la funzionalità di ASP.NET AJAX e di Control Toolkit, il controllo `ScriptManager` deve essere inserito in un punto qualsiasi della pagina, ma all'interno dell'elemento &lt;`form`&gt;:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample1.aspx)]

Nel passaggio successivo sono necessari due controlli DropDownList. In questo esempio vengono usate le informazioni relative al fornitore e al contatto da AdventureWorks, quindi viene creato un elenco per i fornitori disponibili e uno per i contatti disponibili:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample2.aspx)]

Quindi, è necessario aggiungere due Extender CascadingDropDown alla pagina. Una riempie il primo elenco (fornitori) e l'altra riempie il secondo elenco (contatti). È necessario impostare i seguenti attributi:

- `ServicePath`: URL di un servizio Web che fornisce le voci dell'elenco
- `ServiceMethod`: metodo Web che fornisce le voci dell'elenco
- `TargetControlID`: ID dell'elenco a discesa
- `Category`: informazioni sulla categoria inviate al metodo Web quando viene chiamata
- `PromptText`: testo visualizzato quando si caricano in modo asincrono i dati dell'elenco dal server
- `ParentControlID`: (facoltativo) elenco a discesa padre che attiva il caricamento dell'elenco corrente

A seconda del linguaggio di programmazione utilizzato, il nome del servizio Web in questione cambia, ma tutti gli altri valori di attributo sono gli stessi. Ecco l'elemento CascadingDropDown per il primo elenco a discesa:

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample3.aspx)]

Gli Extender del controllo per il secondo elenco devono impostare l'attributo `ParentControlID` in modo che la selezione di una voce nell'elenco dei fornitori attivi il caricamento degli elementi associati nell'elenco contatti.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample4.aspx)]

Il lavoro effettivo viene quindi eseguito nel servizio Web, configurato come indicato di seguito. Si noti che viene usato l'attributo `[ScriptService]`, in caso contrario ASP.NET AJAX non è in grado di creare il proxy JavaScript per accedere ai metodi Web dal codice di script sul lato client.

[!code-aspx[Main](using-cascadingdropdown-with-a-database-vb/samples/sample5.aspx)]

La firma dei metodi Web chiamati da CascadingDropDown è la seguente:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample6.vb)]

Il valore restituito deve quindi essere una matrice di tipo `CascadingDropDownNameValue` definita da Control Toolkit. Il metodo `GetVendors()` è piuttosto semplice da implementare: il codice si connette al database AdventureWorks ed esegue una query sui primi 25 fornitori. Il primo parametro nel costruttore `CascadingDropDownNameValue` è la didascalia della voce dell'elenco, il secondo valore (attributo value nell'elemento &lt;`option`&gt; di HTML). Il codice è il seguente:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample7.vb)]

Il recupero dei contatti associati per un fornitore (nome metodo: `GetContactsForVendor()`) è un po' più complicato. Prima di tutto, è necessario determinare il fornitore selezionato nel primo elenco a discesa. Il Toolkit di controllo definisce un metodo helper per l'attività: il metodo `ParseKnownCategoryValuesString()` restituisce un elemento `StringDictionary` con i dati dell'elenco a discesa:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample8.vb)]

Per motivi di sicurezza, questi dati devono essere convalidati per primi. Quindi, se è presente una voce del fornitore, perché la proprietà `Category` del primo elemento CascadingDropDown è impostata su `"Vendor"`), è possibile che venga recuperato l'ID del fornitore selezionato:

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample9.vb)]

Il resto del metodo è piuttosto semplice, quindi. L'ID del fornitore viene utilizzato come parametro per una query SQL che recupera tutti i contatti associati per tale fornitore. Ancora una volta, il metodo restituisce una matrice di tipo `CascadingDropDownNameValue`.

[!code-vb[Main](using-cascadingdropdown-with-a-database-vb/samples/sample10.vb)]

Caricare la pagina ASP.NET e, dopo un breve periodo di tempo, l'elenco dei fornitori viene riempito con 25 voci. Selezionare una voce e notare il modo in cui il secondo elenco a discesa viene riempito con i dati.

[![il primo elenco viene compilato automaticamente](using-cascadingdropdown-with-a-database-vb/_static/image2.png)](using-cascadingdropdown-with-a-database-vb/_static/image1.png)

Il primo elenco viene compilato automaticamente ([fare clic per visualizzare l'immagine con dimensioni complete](using-cascadingdropdown-with-a-database-vb/_static/image3.png))

[![il secondo elenco viene compilato in base alla selezione nel primo elenco](using-cascadingdropdown-with-a-database-vb/_static/image5.png)](using-cascadingdropdown-with-a-database-vb/_static/image4.png)

Il secondo elenco viene compilato in base alla selezione nel primo elenco ([fare clic per visualizzare l'immagine con dimensioni complete](using-cascadingdropdown-with-a-database-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Precedente](filling-a-list-using-cascadingdropdown-vb.md)
> [Successivo](presetting-list-entries-with-cascadingdropdown-vb.md)
