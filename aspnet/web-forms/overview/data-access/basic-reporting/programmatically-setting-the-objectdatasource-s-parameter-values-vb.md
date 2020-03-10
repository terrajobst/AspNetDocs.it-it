---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
title: Impostazione a livello di programmazione dei valori dei parametri di ObjectDataSource (VB) | Microsoft Docs
author: rick-anderson
description: In questa esercitazione verrà illustrata l'aggiunta di un metodo all'oggetto DAL e BLL che accetta un singolo parametro di input e restituisce i dati. Questo parametro verrà impostato nell'esempio...
ms.author: riande
ms.date: 03/31/2010
ms.assetid: 0ecb03b6-52a0-4731-8c7a-436391d36838
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-vb
msc.type: authoredcontent
ms.openlocfilehash: f1dd50f46528e8dd51f85e503604d3f0dbc21ad2
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78577056"
---
# <a name="programmatically-setting-the-objectdatasources-parameter-values-vb"></a>Impostazione a livello di programmazione dei valori dei parametri di ObjectDataSource (VB)

di [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Scaricare l'app di esempio](https://download.microsoft.com/download/5/d/7/5d7571fc-d0b7-4798-ad4a-c976c02363ce/ASPNET_Data_Tutorial_6_VB.exe) o [scaricare il file PDF](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/datatutorial06vb1.pdf)

> In questa esercitazione verrà illustrata l'aggiunta di un metodo all'oggetto DAL e BLL che accetta un singolo parametro di input e restituisce i dati. Questo parametro viene impostato a livello di codice.

## <a name="introduction"></a>Introduzione

Come illustrato nell' [esercitazione precedente](declarative-parameters-vb.md), sono disponibili diverse opzioni per passare in modo dichiarativo i valori dei parametri ai metodi di ObjectDataSource. Se il valore del parametro è hardcoded, deriva da un controllo Web nella pagina o si trova in un'altra origine leggibile da un'origine dati `Parameter` oggetto, ad esempio, tale valore può essere associato al parametro di input senza scrivere una riga di codice.

In alcuni casi, tuttavia, il valore del parametro deriva da un'origine non ancora contabilizzata da una delle origini dati predefinite `Parameter` oggetti. Se il sito supporta gli account utente, potrebbe essere necessario impostare il parametro in base all'ID utente del visitatore attualmente connesso. In alternativa, potrebbe essere necessario personalizzare il valore del parametro prima di inviarlo insieme al metodo dell'oggetto sottostante di ObjectDataSource.

Ogni volta che viene richiamato il metodo di `Select` ObjectDataSource, ObjectDataSource genera innanzitutto l' [evento Selecting](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Viene quindi richiamato il metodo dell'oggetto sottostante di ObjectDataSource. Una volta completata l'attivazione dell' [evento selezionato](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) di ObjectDataSource (la figura 1 illustra questa sequenza di eventi). I valori dei parametri passati nel metodo dell'oggetto sottostante di ObjectDataSource possono essere impostati o personalizzati in un gestore eventi per l'evento `Selecting`.

[![l'oggetto di ObjectDataSource selezionato e selezionando gli eventi attivati prima e dopo che viene richiamato il metodo dell'oggetto sottostante](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image1.png)

**Figura 1**: gli eventi `Selected` e `Selecting` di ObjectDataSource vengono attivati prima e dopo che viene richiamato il metodo dell'oggetto sottostante ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image3.png))

In questa esercitazione verrà illustrata l'aggiunta di un metodo all'oggetto DAL e BLL che accetta un singolo parametro di input `Month`, di tipo `Integer` e restituisce un oggetto `EmployeesDataTable` popolato con i dipendenti che hanno il relativo anniversario di assunzione nel `Month`specificato. Questo parametro viene impostato a livello di codice in base al mese corrente, mostrando un elenco di "anniversari del dipendente questo mese".

Ecco come procedere.

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Passaggio 1: aggiunta di un metodo a`EmployeesTableAdapter`

Per il primo esempio è necessario aggiungere un mezzo per recuperare i dipendenti il cui `HireDate` si è verificato in un mese specificato. Per fornire questa funzionalità in base all'architettura è necessario innanzitutto creare un metodo in `EmployeesTableAdapter` che esegue il mapping all'istruzione SQL corretta. A tale scopo, iniziare aprendo il DataSet tipizzato Northwind. Fare clic con il pulsante destro del mouse sull'etichetta `EmployeesTableAdapter` e scegliere Aggiungi query.

[![aggiungere una nuova query a EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image4.png)

**Figura 2**: aggiungere una nuova Query al `EmployeesTableAdapter` ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image6.png))

Scegliere di aggiungere un'istruzione SQL che restituisce righe. Quando si raggiunge la schermata specificare un'istruzione `SELECT`, l'istruzione `SELECT` predefinita per il `EmployeesTableAdapter` verrà già caricata. È sufficiente aggiungere la clausola `WHERE`: `WHERE DATEPART(m, HireDate) = @Month`. [DatePart](https://msdn.microsoft.com/library/ms174420.aspx) è una funzione T-SQL che restituisce una particolare parte della data di un tipo di `datetime`; in questo caso si sta usando `DATEPART` per restituire il mese della colonna `HireDate`.

[![restituiscono solo le righe in cui la colonna Assunta è minore o uguale al parametro @HiredBeforeDate](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image7.png)

**Figura 3**: restituire solo le righe in cui la colonna `HireDate` è minore o uguale al parametro `@HiredBeforeDate` ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image9.png))

Infine, modificare i nomi dei metodi `FillBy` e `GetDataBy` rispettivamente in `FillByHiredDateMonth` e `GetEmployeesByHiredDateMonth`.

[![scegliere nomi di metodo più appropriati rispetto a FillBy e GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image10.png)

**Figura 4**: scegliere nomi di metodo più appropriati rispetto a `FillBy` e `GetDataBy` ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image12.png))

Fare clic su fine per completare la procedura guidata e tornare all'area di progettazione del set di dati. Il `EmployeesTableAdapter` dovrebbe ora includere un nuovo set di metodi per l'accesso ai dipendenti assunti in un mese specificato.

[![i nuovi metodi vengono visualizzati nella Area di progettazione del set di dati](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image13.png)

**Figura 5**: i nuovi metodi vengono visualizzati nel area di progettazione del set di dati ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image15.png))

## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Passaggio 2: aggiunta del metodo`GetEmployeesByHiredDateMonth(month)`al livello della logica di business

Poiché l'architettura dell'applicazione usa un livello separato per la logica di business e la logica di accesso ai dati, è necessario aggiungere un metodo al BLL che chiama il DAL per recuperare i dipendenti assunti prima di una data specificata. Aprire il file `EmployeesBLL.vb` e aggiungere il metodo seguente:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample1.vb)]

Come per gli altri metodi di questa classe, `GetEmployeesByHiredDateMonth(month)` semplicemente chiama il metodo DAL e restituisce i risultati.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Passaggio 3: visualizzazione di dipendenti il cui anniversario di assunzione è il mese

Il passaggio finale di questo esempio consiste nel visualizzare i dipendenti il cui anniversario di assunzione è il mese. Per iniziare, aggiungere un controllo GridView alla pagina `ProgrammaticParams.aspx` nella cartella `BasicReporting` e aggiungere un nuovo ObjectDataSource come origine dati. Configurare ObjectDataSource per usare la classe `EmployeesBLL` con la `SelectMethod` impostata su `GetEmployeesByHiredDateMonth(month)`.

[![usare la classe EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image16.png)

**Figura 6**: usare la classe `EmployeesBLL` ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image18.png))

[![selezionare il metodo GetEmployeesByHiredDateMonth (month)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image19.png)

**Figura 7**: effettuare una selezione dal metodo `GetEmployeesByHiredDateMonth(month)` ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image21.png))

La schermata finale chiede di specificare l'origine del valore del parametro `month`. Poiché questo valore verrà impostato a livello di codice, lasciare l'origine del parametro impostata sull'opzione None predefinita e fare clic su fine.

[![lasciare l'origine parametro impostata su None](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image22.png)

**Figura 8**: lasciare l'origine del parametro impostata su None ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image24.png))

Verrà creato un oggetto `Parameter` nella raccolta `SelectParameters` di ObjectDataSource che non ha un valore specificato.

[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample2.aspx)]

Per impostare questo valore a livello di codice, è necessario creare un gestore eventi per l'evento `Selecting` di ObjectDataSource. A tale scopo, passare alla visualizzazione progettazione e fare doppio clic su ObjectDataSource. In alternativa, selezionare ObjectDataSource, passare alla Finestra Proprietà e fare clic sull'icona fulmine Bolt. Fare quindi doppio clic nella casella di testo accanto all'evento `Selecting` o digitare il nome del gestore eventi che si desidera utilizzare. Come terza opzione, è possibile creare il gestore eventi selezionando ObjectDataSource e il relativo evento `Selecting` dai due elenchi a discesa nella parte superiore della classe code-behind della pagina.

![Fare clic sull'icona del fulmine nella finestra proprietà per elencare gli eventi di un controllo Web](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image25.png)

**Figura 9**: fare clic sull'icona a forma di fulmine nella finestra proprietà per elencare gli eventi di un controllo Web

Tutti e tre gli approcci aggiungono un nuovo gestore eventi per l'evento `Selecting` di ObjectDataSource alla classe code-behind della pagina. In questo gestore eventi è possibile leggere e scrivere nei valori dei parametri usando `e.InputParameters(parameterName)`, dove *`parameterName`* è il valore dell'attributo `Name` nel tag `<asp:Parameter>` (la raccolta di `InputParameters` può anche essere indicizzata in modo ordinale, come in `e.InputParameters(index)`). Per impostare il parametro `month` sul mese corrente, aggiungere il codice seguente al gestore dell'evento `Selecting`:

[!code-vb[Main](programmatically-setting-the-objectdatasource-s-parameter-values-vb/samples/sample3.vb)]

Quando si visita questa pagina tramite un browser, è possibile notare che solo un dipendente è stato assunto questo mese (marzo) Laura Callahan, che è stato presso la società dal 1994.

[![i dipendenti i cui mesi vengono visualizzati gli anniversari](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image26.png)

**Figura 10**: i dipendenti i cui ricorre questo mese sono visualizzati ([fare clic per visualizzare l'immagine con dimensioni complete](programmatically-setting-the-objectdatasource-s-parameter-values-vb/_static/image28.png))

## <a name="summary"></a>Riepilogo

Anche se i valori dei parametri di ObjectDataSource possono in genere essere impostati in modo dichiarativo, senza richiedere una riga di codice, è facile impostare i valori dei parametri a livello di programmazione. È sufficiente creare un gestore eventi per l'evento `Selecting` di ObjectDataSource, che viene attivato prima che venga richiamato il metodo dell'oggetto sottostante e impostare manualmente i valori per uno o più parametri tramite la raccolta di `InputParameters`.

Questa esercitazione conclude la sezione report di base. L' [esercitazione successiva](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md) avvia la sezione relativa agli scenari di filtro e Master-Details, in cui verranno analizzate le tecniche per consentire al visitatore di filtrare i dati ed eseguire il drill-down da un report master in un report dettagli.

Buona programmazione!

## <a name="about-the-author"></a>Informazioni sull'autore

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autore di sette ASP/ASP. NET Books e fondatore di [4GuysFromRolla.com](http://www.4guysfromrolla.com), collabora con le tecnologie Web Microsoft a partire da 1998. Scott lavora come consulente, trainer e writer indipendenti. Il suo ultimo libro è [*Sams Teach Yourself ASP.NET 2,0 in 24 ore*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Può essere raggiunto in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o tramite il suo Blog, disponibile in [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Grazie speciale

Questa serie di esercitazioni è stata esaminata da molti revisori utili. Il revisore principale di questa esercitazione è stato Hilton Giesenow. Sei interessato a esaminare i miei prossimi articoli MSDN? In tal caso, rilasciare una riga in [mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Precedente](declarative-parameters-vb.md)
