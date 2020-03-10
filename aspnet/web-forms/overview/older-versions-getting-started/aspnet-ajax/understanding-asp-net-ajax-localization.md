---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Informazioni sulla localizzazione di ASP.NET AJAX | Microsoft Docs
author: scottcate
description: La localizzazione è il processo di progettazione e integrazione del supporto per una lingua e impostazioni cultura specifiche in un'applicazione o in un componente dell'applicazione. Il MIC...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 003e7939accd7a68dab97441b3d999bca835b85a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566220"
---
# <a name="understanding-aspnet-ajax-localization"></a>Informazioni sulla localizzazione in ASP.NET AJAX

di [Scott Cate](https://github.com/scottcate)

[Scaricare PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> La localizzazione è il processo di progettazione e integrazione del supporto per una lingua e impostazioni cultura specifiche in un'applicazione o in un componente dell'applicazione. La piattaforma Microsoft ASP.NET offre un ampio supporto per la localizzazione per le applicazioni ASP.NET standard integrando il modello di localizzazione .NET standard. Microsoft AJAX Framework utilizza il modello integrato per supportare i diversi scenari in cui è possibile eseguire la localizzazione.

## <a name="introduction"></a>Introduzione

La tecnologia ASP.NET di Microsoft offre un modello di programmazione orientato agli oggetti e basato sugli eventi e lo unisce ai vantaggi del codice compilato. Tuttavia, il modello di elaborazione sul lato server presenta diversi svantaggi inerenti alla tecnologia, molti dei quali possono essere trattati dalle nuove funzionalità incluse nello spazio dei nomi System. Web. Extensions, che incapsula i servizi Microsoft AJAX nella .NET Framework 3,5. Queste estensioni abilitano molte funzionalità rich client, precedentemente disponibili come parte delle estensioni ASP.NET 2,0 AJAX, ma ora fanno parte della libreria di classi base del Framework. I controlli e le funzionalità in questo spazio dei nomi includono il rendering parziale delle pagine senza richiedere un aggiornamento completo della pagina, la possibilità di accedere ai servizi Web tramite script client (inclusa l'API di profilatura ASP.NET) e un'API lato client completa progettata per il mirroring di molti schemi di controllo visualizzati nel set di controlli lato server di ASP.NET.

Questo white paper esamina le funzionalità di localizzazione presenti in Microsoft AJAX Framework e Microsoft AJAX script Library, nel contesto delle esigenze aziendali per il supporto della localizzazione e la revisione del supporto già integrato per la localizzazione nel Web applicazioni fornite dal .NET Framework. Microsoft AJAX script Library usa il formato di file. resx già usato dalle applicazioni .NET, che fornisce il supporto IDE integrato e un tipo di risorsa condivisibile.

Questo white paper è basato sulla versione beta 2 di Microsoft Visual Studio 2008. In questo white paper si presuppone inoltre che si lavorerà con Visual Studio 2008, non con Visual Web Developer Express, e verranno fornite procedure dettagliate in base all'interfaccia utente di Visual Studio. In alcuni esempi di codice verranno utilizzati modelli di progetto che potrebbero non essere disponibili in Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La necessità di localizzazione*

In particolare per gli sviluppatori di applicazioni aziendali e per gli sviluppatori di componenti, la possibilità di creare strumenti che possono essere consapevoli delle differenze tra culture e linguaggi è diventata sempre più necessaria. La progettazione di componenti con la possibilità di adattarsi alle impostazioni locali del client aumenta la produttività degli sviluppatori e riduce la quantità di lavoro necessaria per l'adattamento di un componente a una funzione globale.

La localizzazione è il processo di progettazione e integrazione del supporto per una lingua e impostazioni cultura specifiche in un'applicazione o in un componente dell'applicazione. La piattaforma Microsoft ASP.NET offre un ampio supporto per la localizzazione per le applicazioni ASP.NET standard integrando il modello di localizzazione .NET standard. Microsoft AJAX Framework utilizza il modello integrato per supportare i diversi scenari in cui è possibile eseguire la localizzazione. Con il Framework Microsoft AJAX è possibile localizzare gli script tramite la distribuzione in assembly satellite o utilizzando una struttura di file system statica.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incorporamento di script con assembly satellite*

Coerentemente con la strategia di localizzazione .NET Framework standard, le risorse possono essere incluse in assembly satellite. Gli assembly satellite offrono diversi vantaggi rispetto all'inclusione di risorse tradizionale nei file binari. ogni localizzazione può essere aggiornata senza aggiornare l'immagine più grande. è possibile distribuire altre localizzazioni installando semplicemente assembly satellite in la cartella del progetto e gli assembly satellite possono essere distribuiti senza causare un ricaricamento dell'assembly del progetto principale. In particolare nei progetti ASP.NET, questo è vantaggioso perché può ridurre significativamente la quantità di risorse di sistema usate dagli aggiornamenti incrementali e interferisce almeno con l'utilizzo del sito Web di produzione.

Gli script sono incorporati in assembly, inclusi nei file gestiti. resx (o compilati. Resources), inclusi nell'assembly in fase di compilazione. Le risorse vengono quindi rese disponibili all'applicazione script tramite il codice generato dal runtime AJAX, tramite gli attributi a livello di assembly

*Convenzioni di denominazione per i file di script incorporati*

La gestione degli script di Microsoft AJAX Framework supporta un'ampia gamma di opzioni per la distribuzione e il testing degli script e fornisce linee guida per semplificare queste opzioni.

*Per semplificare il debug:*

Gli script di versione (produzione) non devono includere il qualificatore `.debug` nel nome file. Gli script progettati per il debug devono includere `.debug` nel nome file.

*Per facilitare la localizzazione:*

Gli script di impostazioni cultura neutre non devono includere alcun identificatore di impostazioni cultura nel nome del file. Per gli script che contengono risorse localizzate, il codice della lingua ISO deve essere specificato nel nome file. Ad esempio, `es-CO` sta per spagnolo, Columbia.

Nella tabella seguente sono riepilogate le convenzioni di denominazione dei file con esempi:

| Nomefile | Significato |
| --- | --- |
| Script.js | Script indipendente dalle impostazioni cultura della versione di rilascio. |
| Script.debug.js | Uno script di debug della versione indipendente dalle impostazioni cultura. |
| Script.en-US.js | Versione in lingua inglese, Stati Uniti script. |
| Script.debug.es-CO.js | Script di debug-version spagnolo, Columbia. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Procedura dettagliata: creare uno script incorporato localizzato

*Nota: per questa procedura dettagliata è necessario usare Visual Studio 2008 perché Visual Web Developer Express non include un modello di progetto per i progetti di libreria di classi.*

1. Creare un nuovo progetto di sito Web con ASP.NET AJAX Extensions Integrated. Creare un altro progetto, un progetto di libreria di classi, all'interno della soluzione denominata LocalizingResources.
2. Aggiungere un file JScript denominato VerifyDeletion. js al progetto LocalizingResources, oltre ai file di risorse. resx denominati DeletionResources. resx e DeletionResources. es. resx. Il primo conterrà le risorse indipendenti dalle impostazioni cultura; quest'ultimo conterrà le risorse in lingua spagnola.
3. Aggiungere il codice seguente a VerifyDeletion. js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Per coloro che non hanno familiarità con la sintassi di Regex JavaScript, il testo all'interno di barre singole (nell'esempio precedente,/FILENAME/è un esempio) denota un oggetto RegExp. MSDN Library contiene un riferimento a JavaScript completo e le risorse su oggetti nativi JavaScript sono disponibili online.

1. Aggiungere le stringhe di risorse seguenti a DeletionResources. resx: 

    **VerifyDelete**: eliminare il nome file?

    **Deleted**: il nome del file è stato eliminato.

1. Aggiungere le stringhe di risorse seguenti a DeletionResources. es. resx: 

    **VerifyDelete**: est Seguro que Deset nomefile quite?

    **Eliminato**: filename se ha quitado.
2. Aggiungere le righe di codice seguenti al file AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Aggiungere riferimenti a System. Web e System. Web. Extensions al progetto LocalizingResources.
2. Aggiungere un riferimento al progetto LocalizingResources dal progetto di sito Web.
3. In default. aspx, nel progetto di sito Web, aggiornare il controllo ScriptManager con il markup aggiuntivo seguente:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. In default. aspx, in qualsiasi punto della pagina, includere il markup seguente:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Premere F5. Se richiesto, abilitare il debug. Quando la pagina viene caricata, premere il pulsante Elimina. Si noti che viene richiesta la lingua inglese (a meno che il computer non sia impostato su preferisci le risorse in lingua spagnola per impostazione predefinita) per la conferma.
2. Chiudere la finestra del browser e tornare a default. aspx. Nella direttiva di intestazione @Page sostituire auto per culture e UICulture con es-ES. Premere di nuovo F5 per avviare l'applicazione Web nel browser. Questa volta, si noti che viene richiesto di eliminare il file in spagnolo:

[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-localization/_static/image3.png))

[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Fare clic per visualizzare l'immagine con dimensioni complete](understanding-asp-net-ajax-localization/_static/image6.png))

Si noti che per questa procedura dettagliata sono disponibili diverse varianti. Ad esempio, gli script possono essere registrati con il controllo ScriptManager a livello di codice durante il caricamento della pagina.

## <a name="including-a-static-script-file-structure"></a>*Inclusione di una struttura di file script statici*

Quando si utilizzano file di script statici per la distribuzione, si perdono alcuni dei vantaggi derivanti dall'utilizzo dello schema di localizzazione .NET intrinseco. Principalmente visibile è che si perde il tipo automatico generato da inclusi i file di risorse script; nella procedura dettagliata precedente, ad esempio, le risorse sono state esposte da un tipo generato automaticamente chiamato Message dal controllo ScriptManager.

Esistono, tuttavia, alcuni vantaggi dell'utilizzo di una struttura di file script statici. Gli aggiornamenti possono essere eseguiti senza ricompilare e ridistribuire gli assembly satellite e l'utilizzo di una struttura di file statica può essere eseguito anche per eseguire l'override di uno script incorporato, per integrare una parte secondaria di funzionalità che potrebbero non essere state fornite con un componente.

Microsoft consiglia di evitare un problema di controllo della versione generando automaticamente le risorse di script durante la compilazione del progetto. Quando si mantiene una base di codice di script estesa, può diventare sempre più difficile assicurarsi che le modifiche al codice vengano riflesse in ogni script localizzato. In alternativa, è possibile gestire semplicemente uno script per la logica e più script di localizzazione, unendo i file durante la compilazione del progetto.

Poiché non sono presenti risorse da includere in modo dichiarativo, è necessario fare riferimento ai file di script statici aggiungendo `<asp:ScriptElement>` elementi come figlio del tag `<Scripts>` del controllo ScriptManager o aggiungendo a livello di codice `ScriptReference` oggetti alla proprietà `Scripts` del controllo `ScriptManager` nella pagina in fase di esecuzione.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*ScriptManager e relativo ruolo nella localizzazione*

ScriptManager consente diversi comportamenti automatici per le applicazioni localizzate:

- Individua automaticamente i file di script in base alle impostazioni e alle convenzioni di denominazione. ad esempio, carica gli script abilitati per il debug in modalità di debug e carica gli script localizzati in base alla selezione dell'interfaccia utente del browser.
- Consente la definizione delle impostazioni cultura, incluse le impostazioni cultura personalizzate.
- Consente la compressione dei file di script tramite HTTP.
- Consente di memorizzare nella cache gli script per gestire in modo efficiente molte richieste.
- Aggiunge un livello di riferimento indiretto agli script tramite il piping tramite un URL crittografato.

I riferimenti agli script possono essere aggiunti al controllo ScriptManager a livello di codice o tramite markup dichiarativo. Il markup dichiarativo è particolarmente utile quando si utilizzano script incorporati in assembly diversi dal progetto di sito Web stesso, in quanto il nome dello script probabilmente non cambierà quando viene eseguito il push delle revisioni.

## <a name="summary"></a>Riepilogo

Man mano che le applicazioni Web crescono per raggiungere un gruppo di destinatari più ampio, la necessità di essere in grado di raggiungere culture e community più ampie diventa essenziale per un modello aziendale; le applicazioni Web di e-commerce devono essere in grado di gestire le valute straniere, i sistemi di gestione dei contenuti devono essere in grado di presentare solo il contenuto, ma anche gli hint di navigazione e i campi dei moduli in altre lingue e le aziende devono tenere presente che questa esigenza è accessibile.

Il .NET Framework supporta intrinsecamente un Framework di localizzazione completo, utilizzando assembly satellite e file di risorse XML (con estensione resx) per presentare un modo uniforme per cercare stringhe di risorse e immagini. Le estensioni ASP.NET AJAX, tra cui Microsoft AJAX Framework e Microsoft AJAX script Library, forniscono supporto per questo modello di programmazione nel codice sul lato client, abilitando ricerche di stringhe di risorse semplici. Gli assembly satellite supportano l'inclusione automatica delle risorse di script (file con estensione js effettivi) tramite ScriptResource. axd, purché i nomi file seguano uno schema di denominazione specificato. Grazie a questo supporto, le estensioni ASP.NET AJAX semplificano la localizzazione degli script e la globalizzazione delle applicazioni.

## <a name="bio"></a>*Biografia*

Scott Cate collabora con le tecnologie Web Microsoft a partire dal 1997 ed è il Presidente di myKB.com ([www.myKB.com](http://www.myKB.com)), dove si specializza nella scrittura di applicazioni basate su ASP.NET incentrate sulle soluzioni software della Knowledge base. Scott può essere contattato tramite posta elettronica all' [scott.cate@myKB.com](mailto:scott.cate@myKB.com) o al suo blog all' [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Precedente](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Successivo](understanding-asp-net-ajax-web-services.md)
