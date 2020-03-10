---
uid: mvc/overview/getting-started/database-first-development/customizing-a-view
title: "Esercitazione: personalizzare la visualizzazione per Database First EF con l'app MVC ASP.NET"
description: Questa esercitazione è incentrata sulla modifica delle visualizzazioni generate automaticamente per migliorare la presentazione.
author: Rick-Anderson
ms.author: riande
ms.date: 01/24/2019
ms.topic: tutorial
ms.assetid: 269380ff-d7e1-4035-8ad1-fe1316a25f76
msc.legacyurl: /mvc/overview/getting-started/database-first-development/customizing-a-view
msc.type: authoredcontent
ms.openlocfilehash: 89b8a0eb84b6e287c45bc141c68a2c76e63b0e41
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78583594"
---
# <a name="tutorial-customize-view-for-ef-database-first-with-aspnet-mvc-app"></a>Esercitazione: personalizzare la visualizzazione per Database First EF con l'app MVC ASP.NET

Usando l'impalcatura MVC, Entity Framework e ASP.NET, è possibile creare un'applicazione Web che fornisce un'interfaccia a un database esistente. Questa serie di esercitazioni illustra come generare automaticamente il codice che consente agli utenti di visualizzare, modificare, creare ed eliminare dati che si trovano in una tabella di database. Il codice generato corrisponde alle colonne nella tabella di database.

Questa esercitazione è incentrata sulla modifica delle visualizzazioni generate automaticamente per migliorare la presentazione.

Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiungere corsi alla pagina dei dettagli degli studenti
> * Verificare che i corsi vengano aggiunti alla pagina

## <a name="prerequisites"></a>Prerequisiti

* [Modificare il database](changing-the-database.md)

## <a name="add-courses-to-student-detail"></a>Aggiungere corsi ai dettagli degli studenti

Il codice generato fornisce un valido punto di partenza per l'applicazione, ma non fornisce necessariamente tutte le funzionalità necessarie nell'applicazione. È possibile personalizzare il codice per soddisfare i requisiti specifici dell'applicazione. Attualmente, l'applicazione non Visualizza i corsi registrati per lo studente selezionato. In questa sezione vengono aggiunti i corsi registrati per ogni studente alla visualizzazione **Dettagli** per lo studente.

Aprire **Views** > **students** > *Details. cshtml*. Al di sotto dell'ultimo &lt;tag di&gt;/DL, ma prima del tag di chiusura &lt;/div&gt; aggiungere il codice seguente.

[!code-cshtml[Main](customizing-a-view/samples/sample1.cshtml)]

Questo codice consente di creare una tabella in cui viene visualizzata una riga per ogni record della tabella di registrazione per lo studente selezionato. Il metodo di **visualizzazione** esegue il rendering del codice HTML per l'oggetto (ModelItem) che rappresenta l'espressione. Si usa il metodo di visualizzazione (anziché semplicemente incorporare il valore della proprietà nel codice) per assicurarsi che il valore sia formattato correttamente in base al tipo e al modello per quel tipo. In questo esempio, ogni espressione restituisce una singola proprietà del record corrente nel ciclo e i valori sono tipi primitivi di cui viene eseguito il rendering come testo.

## <a name="confirm-courses-are-added"></a>Sono stati aggiunti i corsi Confirm

Eseguire la soluzione. Fare clic su **elenco di studenti** e selezionare i **Dettagli** per uno degli studenti. Si noterà che i corsi registrati sono stati inclusi nella vista.

![studente con registrazione](customizing-a-view/_static/image1.png)

## <a name="next-steps"></a>Passaggi successivi
Le attività di questa esercitazione sono le seguenti:

> [!div class="checklist"]
> * Aggiunta di corsi alla pagina dei dettagli degli studenti
> * Confermato che i corsi vengono aggiunti alla pagina

Passare all'esercitazione successiva per apprendere come aggiungere annotazioni dei dati per specificare i requisiti di convalida e la formattazione della visualizzazione.
> [!div class="nextstepaction"]
> [Miglioramento della convalida dei dati](enhancing-data-validation.md)
