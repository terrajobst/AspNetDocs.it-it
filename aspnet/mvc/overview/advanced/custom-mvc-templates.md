---
uid: mvc/overview/advanced/custom-mvc-templates
title: Modello MVC personalizzato | Microsoft Docs
author: joeloff
description: Creare un modello come estensione VSIX.
ms.author: riande
ms.date: 12/10/2012
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 0603bc24e070e223551813f66a75889a2e46fd35
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78616354"
---
# <a name="custom-mvc-template"></a>Modello MVC personalizzato

di [Jacques Eloff](https://github.com/joeloff)

La versione di MVC 3 Tools Update per Visual Studio 2010 ha introdotto una procedura guidata di progetto separata per i progetti MVC. La modifica è stata determinata da due fattori. Prima di tutto, l'introduzione di nuovi modelli in MVC 3 e il supporto per altri motori di visualizzazione, come Razor lead, per la creazione di una finestra di dialogo nuovo progetto in Visual Studio. In secondo luogo, i clienti chiedevano i punti di estendibilità e la creazione guidata nuovo progetto MVC ci offriva la possibilità di rispondere a queste richieste.

L'aggiunta di modelli personalizzati è un processo arduo basato sull'utilizzo del registro di sistema per rendere visibili i nuovi modelli per la creazione guidata progetto MVC. L'autore di un nuovo modello doveva eseguirne il wrapping all'interno di un'identità del servizio gestito per assicurarsi che le voci del registro di sistema necessarie venissero create in fase di installazione. In alternativa, è possibile creare un file ZIP contenente il modello disponibile e fare in modo che l'utente finale crei manualmente le voci del registro di sistema richieste.

Nessuno degli approcci sopra indicati è ideale, pertanto abbiamo deciso di sfruttare alcune delle infrastrutture esistenti fornite dalle estensioni [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) per semplificare la creazione, la distribuzione e l'installazione di modelli MVC personalizzati a partire da MVC 4 per Visual Studio 2012. Ecco alcuni dei vantaggi offerti da questo approccio:

- Un'estensione VSIX può contenere più modelli che supportano lingue diverse (C# e Visual Basic) e più motori di visualizzazione (aspx e Razor).
- Un'estensione VSIX può essere destinata a più SKU di Visual Studio, inclusi gli SKU rapidi.
- [Visual Studio Gallery](https://visualstudiogallery.msdn.microsoft.com/) semplifica la distribuzione dell'estensione a un vasto pubblico.
- È possibile aggiornare le estensioni VSIX semplificando la creazione di correzioni e aggiornamenti per i modelli personalizzati.

## <a name="prerequisites"></a>Prerequisiti

- Gli utenti devono avere familiarità con la creazione di modelli di progetto, incluso il markup necessario per i file vstemplate e così via.
- Gli utenti dovranno avere installato Visual Studio Professional e versioni successive. Gli SKU Express non supportano la creazione di progetti VSIX.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) installato.

## <a name="example"></a>Esempio

Il primo passaggio consiste nel creare un nuovo progetto VSIX usando C# o Visual Basic. Selezionare **File > nuovo progetto**, quindi fare clic su **estensibilità** nel riquadro sinistro e selezionare il **progetto VSIX**.

![Nuovo progetto](custom-mvc-templates/_static/image1.jpg)

Dopo la creazione del progetto, verrà aperta la finestra di progettazione VSIX.

![Metadati di progettazione progetti](custom-mvc-templates/_static/image2.jpg)

La finestra di progettazione può essere usata per modificare alcune delle proprietà generali dell'estensione che verranno visualizzate agli utenti quando installano l'estensione o sfogliano le estensioni installate in Visual Studio (**strumenti > estensioni e aggiornamenti**). Dopo aver completato le informazioni generali, fare clic sulla **scheda destinazioni di installazione**.

![Destinazioni di installazione di progettazione progetti](custom-mvc-templates/_static/image3.jpg)

Questa scheda viene usata per specificare gli SKU e le versioni di Visual Studio supportate dall'estensione. Selezionare la casella di controllo per **questo progetto VSIX è installato per tutti gli utenti** per abilitare le installazioni per computer di VSIX. Fare clic sul pulsante **nuovo** a destra per aggiungere SKU aggiuntivi, ad esempio Web Developer Express (VWD).

![Aggiungi nuova destinazione di installazione](custom-mvc-templates/_static/image4.jpg)

Se si intende supportare tutti gli SKU Professional e versioni successive (Professional, Premium e Ultimate), è sufficiente selezionare lo SKU minimo nella famiglia **Microsoft.VisualStudio.Pro**. Ricordarsi di salvare tutte le modifiche dopo aver completato le destinazioni di installazione.

![Destinazioni di installazione di progettazione progetti](custom-mvc-templates/_static/image5.jpg)

La scheda **assets** viene usata per aggiungere tutti i file di contenuto a VSIX. Poiché MVC richiede metadati personalizzati, è necessario modificare il codice XML non elaborato del file manifesto VSIX invece di usare la scheda **Asset** per aggiungere contenuto. Per iniziare, aggiungere il contenuto del modello al progetto VSIX. È importante che la struttura della cartella e il contenuto rispecchino il layout del progetto. Nell'esempio seguente sono contenuti quattro modelli di progetto derivati dal modello di progetto MVC di base. Assicurarsi che tutti i file che includono il modello di progetto (tutti gli elementi sotto la cartella ProjectTemplates) vengano aggiunti al **contenuto** ItemGroup nel file di progetto VSIX e che ogni elemento contenga il set di metadati **CopyToOutputDirectory** e **IncludeInVsix** , come illustrato nell'esempio riportato di seguito.

&lt;contenuto includono =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;sempre&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/content&gt;

In caso contrario, l'IDE tenterà di compilare il contenuto del modello quando si compila il progetto VSIX e probabilmente verrà visualizzato un errore. I file di codice nei modelli contengono spesso [parametri di modello](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) speciali usati da Visual Studio quando viene creata un'istanza del modello di progetto e pertanto non possono essere compilati nell'IDE.

![Esplora soluzioni](custom-mvc-templates/_static/image6.jpg)

Chiudere la finestra di progettazione VSIX, fare clic con il pulsante destro del mouse sul file **source. Extension. manifest** in **Esplora soluzioni** e selezionare **Apri con** e scegliere l'opzione **editor XML (testo)** .

![Apri con finestra di dialogo](custom-mvc-templates/_static/image7.jpg)

Creare un elemento **&lt;assets&gt;** e aggiungere un elemento **&lt;asset&gt;** per ogni file che deve essere incluso in VSIX. L'attributo **Type** di ogni elemento **&lt;asset&gt;** deve essere impostato su **Microsoft. VisualStudio. Mvc. template**. Si tratta di uno spazio dei nomi personalizzato che viene riconosciuto solo dalla procedura guidata del progetto MVC. Vedere la documentazione dello schema VSIX 2,0 per ulteriori informazioni sulla struttura e il layout del file manifesto.

Semplicemente aggiungere i file al progetto VSIX non è sufficiente per registrare i modelli con la procedura guidata MVC. È necessario fornire informazioni quali il nome del modello, la descrizione, i motori di visualizzazione e il linguaggio di programmazione supportati alla procedura guidata di MVC. Queste informazioni vengono trasferite negli attributi personalizzati associati all'elemento **&lt;Asset&gt;** per ogni file **vstemplate** .

&lt;asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Tipo =&quot;Microsoft. VisualStudio. Mvc. template&quot;

d:Source =&quot;file&quot;

Path =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC&quot;

Lingua =&quot;C#&quot;

ViewEngine =&quot;aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Title =&quot;applicazione Web di base personalizzata&quot;

Description =&quot;un modello personalizzato derivato da un'applicazione Web MVC di base (Razor)&quot;

Version =&quot;4,0&quot;/&gt;

Di seguito è riportata una spiegazione degli attributi personalizzati che devono essere presenti:

- **ProjectType** deve essere impostato su MVC.
- Il **linguaggio** designa il linguaggio di sviluppo supportato dal modello. I valori validi sono C# o VB.
- **ViewEngine** designa il motore di visualizzazione supportato dal modello, ad esempio aspx o Razor. È possibile specificare un valore personalizzato per questo campo.
- **TemplateID** viene utilizzato per raggruppare i modelli. Se il valore corrisponde a un ID modello esistente, verranno sostituiti i modelli registrati in precedenza con la procedura guidata MVC.
- **Titolo** designa la breve descrizione visualizzata nella procedura guidata MVC sotto ogni modello di progetto.
- **Description** indica una descrizione più dettagliata del modello.

Dopo aver aggiunto tutti i file al manifesto e averlo salvato, si noterà che nella scheda **Asset** della finestra di progettazione vengono visualizzati tutti i file, ma non gli attributi personalizzati aggiunti al **&lt;asset&gt;** elementi per i file **vstemplate** .

![Asset di progettazione progetti](custom-mvc-templates/_static/image8.jpg)

Tutto ciò che rimane ora è compilare il progetto VSIX e installarlo.

Verificare che tutte le istanze di Visual Studio siano chiuse nel computer in cui si vuole testare l'estensione VSIX. Visual Studio analizza le nuove estensioni durante l'avvio, pertanto se l'IDE è aperto durante l'installazione di un progetto VSIX, sarà necessario riavviare Visual Studio. In Esplora, fare doppio clic sul file VSIX per avviare il **programma di installazione VSIX**, fare clic su **Installa** e quindi avviare Visual Studio.

![Programma di installazione VSIX](custom-mvc-templates/_static/image9.jpg)

Dal menu selezionare **strumenti > estensioni e aggiornamenti** per verificare che l'estensione sia stata installata. Se il programma di installazione VSIX ha segnalato errori durante l'installazione dell'estensione, è possibile visualizzare il log del programma di installazione VSIX per ulteriori informazioni. Il log viene in genere creato nella cartella **% Temp%** dell'utente che ha installato l'estensione, ad esempio **C:\Users\Bob\AppData\Local\Temp**.

![Estensioni e aggiornamenti](custom-mvc-templates/_static/image10.jpg)

Dopo aver chiuso la finestra, è possibile creare un progetto MVC 4 per verificare se i nuovi modelli sono visualizzati nella procedura guidata di MVC.

![Nuovo progetto MVC 4 ASP.NET](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitazioni

1. La procedura guidata MVC non supporta i modelli personalizzati localizzati.
2. Se non è possibile individuare modelli personalizzati, la procedura guidata non segnalerà alcun errore. Se uno degli attributi personalizzati richiesti è assente, il modello verrebbe semplicemente escluso dalla procedura guidata.
