---
uid: web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
title: Convalida del modello in API Web ASP.NET-ASP.NET 4. x
author: MikeWasson
description: Panoramica della convalida del modello in API Web ASP.NET per ASP.NET 4. x.
ms.author: riande
ms.date: 07/20/2012
ms.custom: seoapril2019
ms.assetid: 7d061207-22b8-4883-bafa-e89b1e7749ca
msc.legacyurl: /web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 531a66b7ab642bd012663517640f2766f1917f25
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78557239"
---
# <a name="model-validation-in-aspnet-web-api"></a>Convalida di modelli in API Web ASP.NET

di [Mike Wasson](https://github.com/MikeWasson)

Questo articolo illustra come annotare i modelli, usare le annotazioni per la convalida dei dati e gestire gli errori di convalida nell'API Web. Quando un client invia dati all'API Web, spesso si vuole convalidare i dati prima di eseguire qualsiasi elaborazione. 

## <a name="data-annotations"></a>Annotazioni dei dati

In API Web ASP.NET, è possibile utilizzare gli attributi dello spazio dei nomi [System. ComponentModel. DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) per impostare le regole di convalida per le proprietà del modello. Si consideri il modello seguente:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample1.cs)]

Se è stata usata la convalida del modello in ASP.NET MVC, l'aspetto dovrebbe essere familiare. L'attributo **required** indica che la proprietà `Name` non deve essere null. L'attributo **Range** indica che `Weight` deve essere compreso tra 0 e 999.

Si supponga che un client invii una richiesta POST con la rappresentazione JSON seguente:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample2.json)]

È possibile notare che il client non include la proprietà `Name`, che è contrassegnata come obbligatoria. Quando l'API Web converte il codice JSON in un'istanza di `Product`, convalida l'`Product` rispetto agli attributi di convalida. Nell'azione del controller è possibile verificare se il modello è valido:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample3.cs)]

La convalida del modello non garantisce che i dati client siano sicuri. Potrebbe essere necessaria una convalida aggiuntiva in altri livelli dell'applicazione. Ad esempio, il livello dati potrebbe applicare vincoli di chiave esterna. L'esercitazione sull' [uso dell'API Web con Entity Framework](../data/using-web-api-with-entity-framework/part-1.md) esamina alcuni di questi problemi.

**"In**-posting": il sottoposting si verifica quando il client lascia alcune proprietà. Si supponga, ad esempio, che il client invii quanto segue:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample4.json)]

In questo caso, il client non ha specificato valori per `Price` o `Weight`. Il formattatore JSON assegna un valore predefinito pari a zero alle proprietà mancanti.

![](model-validation-in-aspnet-web-api/_static/image1.png)

Lo stato del modello è valido perché zero è un valore valido per queste proprietà. Il fatto che si tratti di un problema dipende dallo scenario. In un'operazione di aggiornamento, ad esempio, potrebbe essere necessario distinguere tra "zero" e "not set". Per forzare i client a impostare un valore, rendere Nullable la proprietà e impostare l'attributo **required** :

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample5.cs?highlight=1-2)]

**"Overposting"** : un client può anche inviare *più* dati del previsto. Esempio:

[!code-json[Main](model-validation-in-aspnet-web-api/samples/sample6.json)]

In questo caso, il codice JSON include una proprietà ("color") che non esiste nel modello di `Product`. In questo caso, il formattatore JSON ignora semplicemente questo valore. Il formattatore XML esegue la stessa operazione. La sovrascrittura causa problemi se il modello dispone di proprietà che si desidera essere di sola lettura. Esempio:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample7.cs)]

Non si vuole che gli utenti aggiornino la proprietà `IsAdmin` e si elevano agli amministratori. La strategia più sicura consiste nell'usare una classe modello che corrisponda esattamente a ciò che il client è autorizzato a inviare:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample8.cs)]

> [!NOTE]
> Il post di Blog di Brad Wilson "convalida dell'[input e convalida dei modelli in ASP.NET MVC](http://bradwilson.typepad.com/blog/2010/01/input-validation-vs-model-validation-in-aspnet-mvc.html)" presenta una descrizione approfondita di sottoposta e overposting. Sebbene il post sia relativo a ASP.NET MVC 2, i problemi sono ancora rilevanti per l'API Web.

## <a name="handling-validation-errors"></a>Gestione degli errori di convalida

L'API Web non restituisce automaticamente un errore al client quando la convalida ha esito negativo. Spetta all'azione del controller controllare lo stato del modello e rispondere in modo appropriato.

È inoltre possibile creare un filtro azione per verificare lo stato del modello prima che venga richiamata l'azione del controller. Il codice seguente mostra un esempio:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample9.cs)]

Se la convalida del modello ha esito negativo, questo filtro restituisce una risposta HTTP che contiene gli errori di convalida. In tal caso, l'azione del controller non viene richiamata.

[!code-console[Main](model-validation-in-aspnet-web-api/samples/sample10.cmd)]

Per applicare questo filtro a tutti i controller API Web, aggiungere un'istanza del filtro alla raccolta **HttpConfiguration. filters** durante la configurazione:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample11.cs)]

Un'altra opzione consiste nell'impostare il filtro come attributo per singoli controller o azioni del controller:

[!code-csharp[Main](model-validation-in-aspnet-web-api/samples/sample12.cs)]
