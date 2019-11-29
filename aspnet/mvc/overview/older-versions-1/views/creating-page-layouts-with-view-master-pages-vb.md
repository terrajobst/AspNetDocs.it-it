---
uid: mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
title: Creazione di layout di pagina con le pagine master di visualizzazione (VB) | Microsoft Docs
author: microsoft
description: In questa esercitazione si apprenderà come creare un layout di pagina comune per più pagine dell'applicazione sfruttando le pagine master di visualizzazione. È possibile usare...
ms.author: riande
ms.date: 10/16/2008
ms.assetid: d34f90a1-6de3-482a-a326-f87fdcbaaaff
msc.legacyurl: /mvc/overview/older-versions-1/views/creating-page-layouts-with-view-master-pages-vb
msc.type: authoredcontent
ms.openlocfilehash: 97c0ecf1953cc54030656dd710a5150243877110
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74594020"
---
# <a name="creating-page-layouts-with-view-master-pages-vb"></a>Creazione di layout di pagina con le pagine master di visualizzazione (VB)

[Microsoft](https://github.com/microsoft)

[Scaricare PDF](https://download.microsoft.com/download/e/f/3/ef3f2ff6-7424-48f7-bdaa-180ef64c3490/ASPNET_MVC_Tutorial_12_VB.pdf)

> In questa esercitazione si apprenderà come creare un layout di pagina comune per più pagine dell'applicazione sfruttando le pagine master di visualizzazione. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e utilizzare il layout a due colonne per tutte le pagine dell'applicazione Web.

## <a name="creating-page-layouts-with-view-master-pages"></a>Creazione di layout di pagina con le pagine master di visualizzazione

In questa esercitazione si apprenderà come creare un layout di pagina comune per più pagine dell'applicazione sfruttando le pagine master di visualizzazione. È possibile utilizzare una pagina master di visualizzazione, ad esempio, per definire un layout di pagina a due colonne e utilizzare il layout a due colonne per tutte le pagine dell'applicazione Web.

È anche possibile sfruttare le pagine master di visualizzazione per condividere contenuto comune tra più pagine dell'applicazione. Ad esempio, è possibile inserire il logo del sito Web, i collegamenti di navigazione e gli annunci di banner in una pagina di visualizzazione master. In questo modo, ogni pagina dell'applicazione visualizzerà automaticamente il contenuto.

In questa esercitazione si apprenderà come creare una nuova pagina di visualizzazione master e come creare una nuova pagina del contenuto della visualizzazione basata sulla pagina master.

### <a name="creating-a-view-master-page"></a>Creazione di una pagina master di visualizzazione

Iniziamo creando una pagina master di visualizzazione che definisce un layout a due colonne. Per aggiungere una nuova pagina master di visualizzazione a un progetto MVC, fare clic con il pulsante destro del mouse sulla cartella Views\Shared, selezionare l'opzione di menu **Aggiungi, nuovo elemento**e selezionare il modello di pagina master visualizzazione MVC (vedere la figura 1).

[![aggiunta di una pagina master di visualizzazione](creating-page-layouts-with-view-master-pages-vb/_static/image2.png)](creating-page-layouts-with-view-master-pages-vb/_static/image1.png)

**Figura 01**: aggiunta di una pagina master di visualizzazione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-page-layouts-with-view-master-pages-vb/_static/image3.png))

È possibile creare più di una pagina master di visualizzazione in un'applicazione. Ogni pagina master di visualizzazione può definire un layout di pagina diverso. Ad esempio, si potrebbe desiderare che alcune pagine dispongano di un layout a due colonne e di altre pagine per avere un layout di tre colonne.

Una pagina master di visualizzazione è simile a una visualizzazione MVC ASP.NET standard. Tuttavia, a differenza di una visualizzazione normale, una pagina master di visualizzazione contiene uno o più tag `<asp:ContentPlaceHolder>`. I tag `<contentplaceholder>` vengono usati per contrassegnare le aree della pagina master di cui è possibile eseguire l'override in una singola pagina di contenuto.

Ad esempio, nella pagina di visualizzazione master del listato 1 viene definito un layout a due colonne. Contiene due tag `<contentplaceholder>`. Una `<ContentPlaceHolder>` per ogni colonna.

**Listato 1-`Views\Shared\Site.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample1.aspx)]

Il corpo della pagina master di visualizzazione nel listato 1 contiene due tag `<div>` che corrispondono alle due colonne. La classe della colonna del foglio di stile CSS viene applicata a entrambi i tag `<div>`. Questa classe viene definita nel foglio di stile dichiarato nella parte superiore della pagina master. È possibile visualizzare in anteprima come verrà eseguito il rendering della pagina master di visualizzazione passando alla visualizzazione progettazione. Fare clic sulla scheda Progettazione nella parte inferiore sinistra dell'editor del codice sorgente (vedere la figura 2).

[![l'anteprima di una pagina master nella finestra di progettazione](creating-page-layouts-with-view-master-pages-vb/_static/image5.png)](creating-page-layouts-with-view-master-pages-vb/_static/image4.png)

**Figura 02**: anteprima di una pagina master nella finestra di progettazione ([fare clic per visualizzare l'immagine con dimensioni complete](creating-page-layouts-with-view-master-pages-vb/_static/image6.png))

### <a name="creating-a-view-content-page"></a>Creazione di una pagina Visualizza contenuto

Dopo aver creato una pagina master di visualizzazione, è possibile creare una o più pagine di contenuto della visualizzazione in base alla pagina di visualizzazione master. Ad esempio, è possibile creare una pagina di contenuto visualizzazione indice per il controller Home facendo clic con il pulsante destro del mouse sulla cartella Views\Home, scegliendo **Aggiungi, nuovo elemento**, selezionando il modello di **pagina contenuto visualizzazione MVC** , immettendo il nome index. aspx e facendo clic sul pulsante Aggiungi (vedere la figura 3).

[![l'aggiunta di una pagina Visualizza contenuto](creating-page-layouts-with-view-master-pages-vb/_static/image8.png)](creating-page-layouts-with-view-master-pages-vb/_static/image7.png)

**Figura 03**: aggiunta di una pagina Visualizza contenuto ([fare clic per visualizzare l'immagine con dimensioni complete](creating-page-layouts-with-view-master-pages-vb/_static/image9.png))

Dopo aver fatto clic sul pulsante Aggiungi, viene visualizzata una nuova finestra di dialogo che consente di selezionare una pagina master di visualizzazione da associare alla pagina Visualizza contenuto (vedere la figura 4). È possibile passare alla pagina master della visualizzazione site. master creata nella sezione precedente.

[![selezione di una pagina master](creating-page-layouts-with-view-master-pages-vb/_static/image11.png)](creating-page-layouts-with-view-master-pages-vb/_static/image10.png)

**Figura 04**: selezione di una pagina master ([fare clic per visualizzare l'immagine a dimensione intera](creating-page-layouts-with-view-master-pages-vb/_static/image12.png))

Dopo aver creato una nuova pagina del contenuto della visualizzazione basata sulla pagina master site. master, è possibile ottenere il file nel listato 2.

**Listato 2: `Views\Home\Index.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample2.aspx)]

Si noti che questa visualizzazione contiene un tag `<asp:Content>` corrispondente a ognuno dei tag `<asp:ContentPlaceHolder>` nella pagina di visualizzazione master. Ogni tag `<asp:Content>` include un attributo ContentPlaceHolderID che punta al `<asp:ContentPlaceHolder>` particolare che esegue l'override.

Si noti inoltre che la pagina visualizzazione contenuto del listato 2 non contiene i normali tag HTML di apertura e chiusura. Ad esempio, non contiene i tag di apertura e chiusura `<html>` o `<head>`. Tutti i normali tag di apertura e chiusura sono contenuti nella pagina di visualizzazione master.

I contenuti che si desidera visualizzare in una pagina di contenuto di visualizzazione devono essere inseriti in un tag `<asp:Content>`. Se si inseriscono contenuti HTML o altri contenuti al di fuori di questi tag, verrà visualizzato un errore quando si tenta di visualizzare la pagina.

Non è necessario eseguire l'override di ogni tag `<asp:ContentPlaceHolder>` da una pagina master in una pagina di visualizzazione del contenuto. È necessario eseguire l'override di un tag di `<asp:ContentPlaceHolder>` solo quando si vuole sostituire il tag con contenuto specifico.

Ad esempio, la vista index modificata nel listato 3 contiene solo due tag `<asp:Content>`. Ognuno dei tag `<asp:Content>` include un testo.

**Listato 3-`Views\Home\Index.aspx (modified)`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample3.aspx)]

Quando viene richiesta la visualizzazione nel listato 3, viene eseguito il rendering della pagina nella figura 5. Si noti che la visualizzazione esegue il rendering di una pagina con due colonne. Si noti inoltre che il contenuto della pagina Visualizza contenuto viene unito al contenuto della pagina di visualizzazione master.

[![pagina contenuto visualizzazione indice](creating-page-layouts-with-view-master-pages-vb/_static/image14.png)](creating-page-layouts-with-view-master-pages-vb/_static/image13.png)

**Figura 05**: pagina del contenuto della visualizzazione Index ([fare clic per visualizzare l'immagine con dimensioni complete](creating-page-layouts-with-view-master-pages-vb/_static/image15.png))

### <a name="modifying-view-master-page-content"></a>Modifica del contenuto della pagina master di visualizzazione

Un problema che si verifica quasi immediatamente quando si lavora con le pagine master di visualizzazione è il problema di modificare il contenuto della pagina master di visualizzazione quando sono richieste pagine di contenuto di visualizzazione diverse. Si desidera, ad esempio, che ogni pagina dell'applicazione Web disponga di un titolo univoco. Il titolo viene tuttavia dichiarato nella pagina di visualizzazione master e non nella pagina Visualizza contenuto. In che modo è possibile personalizzare il titolo della pagina per ogni pagina Visualizza contenuto?

Esistono due modi per modificare il titolo visualizzato da una pagina Visualizza contenuto. In primo luogo, è possibile assegnare un titolo di pagina all'attributo title della direttiva `<%@ page %>` dichiarata nella parte superiore di una pagina di visualizzazione del contenuto. Se ad esempio si vuole assegnare il titolo della pagina "Super Great Grand website" alla visualizzazione index, è possibile includere la direttiva seguente nella parte superiore della vista index:

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample4.aspx)]

Quando viene eseguito il rendering della visualizzazione indice nel browser, il titolo desiderato viene visualizzato nella barra del titolo del browser:

[barra del titolo del browser ![](creating-page-layouts-with-view-master-pages-vb/_static/image17.png)](creating-page-layouts-with-view-master-pages-vb/_static/image16.png)

Per il corretto funzionamento dell'attributo title è necessario che una pagina di visualizzazione Master soddisfi un requisito importante. La pagina master di visualizzazione deve contenere un tag `<head runat="server">` anziché un normale tag `<head>` per l'intestazione. Se il tag `<head>` non include l'attributo runat = "Server", il titolo non verrà visualizzato. La pagina master di visualizzazione predefinita include il tag `<head runat="server">` obbligatorio.

Un approccio alternativo alla modifica del contenuto della pagina master da una singola pagina del contenuto della visualizzazione consiste nel eseguire il wrapping dell'area che si desidera modificare in un tag `<asp:ContentPlaceHolder>`. Si supponga, ad esempio, di voler modificare non solo il titolo, ma anche i tag meta, sottoposti a rendering da una pagina di visualizzazione master. Nella pagina di visualizzazione master del listato 4 è contenuto un tag `<asp:ContentPlaceHolder>` all'interno del tag `<head>`.

**Listato 4-`Views\Shared\Site2.master`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample5.aspx)]

Si noti che il tag `<asp:ContentPlaceHolder>` nel listato 4 include il contenuto predefinito, ovvero un titolo predefinito e i tag meta predefiniti. Se non si esegue l'override di questo tag `<asp:ContentPlaceHolder>` in una singola pagina di visualizzazione del contenuto, verrà visualizzato il contenuto predefinito.

La pagina visualizzazione contenuto del listato 5 sostituisce il tag `<asp:ContentPlaceHolder>` per visualizzare un titolo personalizzato e tag meta personalizzati.

**Elenco 5-`Views\Home\Index2.aspx`**

[!code-aspx[Main](creating-page-layouts-with-view-master-pages-vb/samples/sample6.aspx)]

### <a name="summary"></a>Riepilogo

In questa esercitazione è stata fornita un'introduzione di base per visualizzare le pagine master e visualizzare le pagine di contenuto. Si è appreso come creare nuove pagine master di visualizzazione e creare pagine di contenuto di visualizzazione basate su di essi. È stato inoltre esaminato come è possibile modificare il contenuto di una pagina di visualizzazione master da una pagina di contenuto di visualizzazione particolare.

> [!div class="step-by-step"]
> [Precedente](using-the-tagbuilder-class-to-build-html-helpers-vb.md)
> [Successivo](passing-data-to-view-master-pages-vb.md)
