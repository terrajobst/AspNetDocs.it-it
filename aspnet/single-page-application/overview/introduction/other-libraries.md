---
uid: single-page-application/overview/introduction/other-libraries
title: Librerie diverse da Knockout | Microsoft Docs
author: madskristensen
description: Librerie diverse da Knockout
ms.author: riande
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 64a4ad1fb411f7291a5cba634afdf4d2fdb16d55
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78578547"
---
# <a name="know-a-library-other-than-knockout"></a>Librerie diverse da Knockout

di [Mads Kristensen](https://github.com/madskristensen)

Il [modello di applicazione a pagina singola (Spa)](knockoutjs-template.md) è un ottimo modo per iniziare a scrivere applicazioni a singola pagina. Il modello utilizza [Knockout](http://knockoutjs.com/) per associare i dati dell'applicazione agli elementi DOM.

Ma Knockout non è l'unica libreria JavaScript per la creazione di applicazioni rich client. Altre librerie risolvono problemi simili in modi diversi. È possibile che si preferisca una libreria rispetto a un'altra, quindi sono stati resi disponibili per il download diversi modelli creati dalla community. Ognuno di questi modelli USA una combinazione diversa di librerie JavaScript client.

Per installare un modello creato dalla community, visitare una delle pagine del modello elencate di seguito e fare clic sul pulsante download. I modelli vengono forniti come file VSIX.

## <a name="backbonejs"></a>BackboneJS

[Modello di Spa backbone. js](../templates/backbonejs-template.md). Questo modello fornisce uno scheletro iniziale per lo sviluppo di un'applicazione [backbone. js](http://backbonejs.org/) in ASP.NET MVC. Fornisce funzionalità di base per l'accesso degli utenti, tra cui l'iscrizione, l'accesso, la reimpostazione della password e la conferma dell'utente con i modelli di posta elettronica di base.

## <a name="breezejs"></a>BreezeJS

[Breezejs](http://www.breezejs.com/?utm_source=ms-spa) è una libreria open source per la gestione di dati avanzati in un client JavaScript. Breeze gestisce l'esecuzione di query, la memorizzazione nella cache, il rilevamento delle modifiche, la convalida e altro ancora. Funzionalità di due modelli Breeze:

- Il modello [Breeze/Knockout](../templates/breezeknockout-template.md) estende il modello di applicazione a singola pagina, mostrando con quale facilità è possibile creare un'applicazione a singola pagina con Breeze per la gestione dei dati e knockout per data binding.
- Il modello [Breeze/angolare](../templates/breezeangular-template.md) estende anche il modello Spa Knockout con Breeze, ma usando la libreria [AngularJS](http://angularjs.org) per data binding, l'inserimento di dipendenze e la gestione dello schermo.

Inoltre, il [modello Hot scaldasalviette Spa](../templates/hottowel-template.md) USA breezejs.

## <a name="emberjs"></a>EmberJS

[Modello di EMBERJS Spa](../templates/emberjs-template.md). Questo modello USA [Brac](http://emberjs.com/), una potente libreria JavaScript MVC che risolve un'ampia gamma di problemi per la creazione di applicazioni rich client.

Il modello di Brac SPA è una nuova implementazione del modello di applicazione a singola pagina, che usa EmberJS e i modelli manubri.

## <a name="hot-towel"></a>Asciugamano caldo

[Modello Hot asciugamano Spa](../templates/hottowel-template.md). Questo modello offre diverse librerie JavaScript, tra cui Breeze, Knockout, RequireJS e Twitter bootstrap.

Rispetto agli altri modelli elencati di seguito, il modello Hot asciugamano fornisce un'applicazione più completa da cui è possibile creare i propri. Ci sono altri concetti da conoscere, ma dopo averli capiti, questo modello potrebbe essere solo quello che si sta cercando. Se si vuole creare un'applicazione a singola pagina, ma non è possibile decidere dove iniziare, usare l'asciugamano caldo e in pochi secondi sarà presente una SPA e tutti gli strumenti necessari per compilarlo.

## <a name="feature-table"></a>Tabella delle funzionalità

Di seguito sono riportate le funzionalità fornite da ogni modello di SPA:

|                        | ASP.NET SPA | Backbone | Breeze/angolare | Breeze/KO |  Ember   | Asciugamano caldo |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Esempio ToDo       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Modello bare      |             | &#10003; |                |           |          | &#10003;  |
| Esplorazione e cronologia |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Librerie       |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Backbone     |             | &#10003; |                |           |          |           |
|         Breeze         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |
