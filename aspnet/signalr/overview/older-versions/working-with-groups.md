---
uid: signalr/overview/older-versions/working-with-groups
title: Uso dei gruppi in SignalR 1. x | Microsoft Docs
author: bradygaster
description: Questo argomento descrive come salvare in modo permanente le informazioni sull'appartenenza ai gruppi con l'API Hub.
ms.author: bradyg
ms.date: 10/21/2013
ms.assetid: 22929efd-68c9-4609-b76d-f8ba42fda01e
msc.legacyurl: /signalr/overview/older-versions/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 5f50dc162d6cdcfbf2261e6a751f5f99078d5c54
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78579366"
---
# <a name="working-with-groups-in-signalr-1x"></a>Uso dei gruppi in SignalR 1.x

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come aggiungere utenti ai gruppi e salvare in modo permanente le informazioni sull'appartenenza al gruppo.

## <a name="overview"></a>Panoramica

I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a subset specificati di client connessi. Un gruppo può avere un numero qualsiasi di client e un client può essere un membro di un numero qualsiasi di gruppi. Non è necessario creare gruppi in modo esplicito. In effetti, un gruppo viene creato automaticamente la prima volta che si specifica il nome in una chiamata a groups. Add e viene eliminato quando si rimuove l'ultima connessione dall'appartenenza al gruppo. Per un'introduzione all'uso dei gruppi, vedere [come gestire l'appartenenza a un gruppo dalla classe Hub](index.md) nella Guida dell'API Hub-server.

Non è disponibile alcuna API per ottenere un elenco di appartenenza a un gruppo o un elenco di gruppi. SignalR invia messaggi a client e gruppi basati su un modello di pubblicazione/sottoscrizione e il server non gestisce gli elenchi di gruppi o appartenenze a gruppi. Ciò consente di massimizzare la scalabilità, perché ogni volta che si aggiunge un nodo a un Web farm, qualsiasi stato mantenuto da SignalR deve essere propagato al nuovo nodo.

Quando si aggiunge un utente a un gruppo utilizzando il metodo `Groups.Add`, l'utente riceve i messaggi indirizzati a tale gruppo per la durata della connessione corrente, ma l'appartenenza dell'utente al gruppo non viene salvata in modo permanente oltre la connessione corrente. Se si desidera mantenere in modo permanente le informazioni sui gruppi e l'appartenenza a un gruppo, è necessario archiviare i dati in un repository, ad esempio un database o l'archiviazione tabelle di Azure. Quindi, ogni volta che un utente si connette all'applicazione, si recupera dal repository a cui appartiene l'utente e si aggiunge manualmente tale utente a tali gruppi.

Quando si esegue la riconnessione dopo un'ininterrottità temporanea, l'utente aggiunge automaticamente i gruppi assegnati in precedenza. Il ricollegamento automatico di un gruppo si applica solo quando si esegue la riconnessione, non quando si stabilisce una nuova connessione. Un token firmato digitalmente viene passato dal client che contiene l'elenco dei gruppi assegnati in precedenza. Se si desidera verificare se l'utente appartiene ai gruppi richiesti, è possibile eseguire l'override del comportamento predefinito.

Questo argomento include le sezioni seguenti:

- [Aggiunta e rimozione di utenti](#add)
- [Chiamata di membri di un gruppo](#call)
- [Archiviazione dell'appartenenza a un gruppo in un database](#storedatabase)
- [Archiviazione dell'appartenenza al gruppo nell'archiviazione tabelle di Azure](#storeazuretable)
- [Verifica dell'appartenenza al gruppo durante la riconnessione](#verify)

<a id="add"></a>

## <a name="adding-and-removing-users"></a>Aggiunta e rimozione di utenti

Per aggiungere o rimuovere utenti da un gruppo, chiamare i metodi di [aggiunta](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.add(v=vs.111).aspx) o [rimozione](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.igroupmanager.remove(v=vs.111).aspx) e passare il nome dell'ID di connessione e del gruppo dell'utente come parametri. Non è necessario rimuovere manualmente un utente da un gruppo al termine della connessione.

Nell'esempio seguente vengono illustrati i metodi `Groups.Add` e `Groups.Remove` usati nei metodi dell'hub.

[!code-csharp[Main](working-with-groups/samples/sample1.cs?highlight=5,10)]

I metodi `Groups.Add` e `Groups.Remove` vengono eseguiti in modo asincrono.

Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo groups. Add completi prima di tutto. Gli esempi di codice seguenti illustrano come eseguire questa operazione, una usando il codice che funziona in .NET 4,5 e uno usando il codice che funziona in .NET 4.

#### <a name="asynchronous-net-45-example"></a>Esempio .NET 4,5 asincrono

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

#### <a name="asynchronous-net-4-example"></a>Esempio .NET 4 asincrono

[!code-csharp[Main](working-with-groups/samples/sample3.cs?highlight=3-4)]

In generale, non è consigliabile includere `await` quando si chiama il metodo `Groups.Remove` perché l'ID connessione che si sta tentando di rimuovere potrebbe non essere più disponibile. In tal caso, `TaskCanceledException` viene generata dopo il timeout della richiesta. Se l'applicazione deve verificare che l'utente sia stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima dei gruppi. rimuovere, quindi rilevare l'eccezione `TaskCanceledException` che potrebbe essere generata.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Chiamata di membri di un gruppo

È possibile inviare messaggi a tutti i membri di un gruppo o solo ai membri specificati del gruppo, come illustrato negli esempi seguenti.

- **Tutti** i client connessi in un gruppo specificato. 

    [!code-css[Main](working-with-groups/samples/sample4.css)]
- Tutti i client connessi in un gruppo specificato, **ad eccezione dei client specificati**, identificati dall'ID connessione. 

    [!code-csharp[Main](working-with-groups/samples/sample5.cs)]
- Tutti i client connessi in un gruppo specificato **eccetto il client chiamante**. 

    [!code-css[Main](working-with-groups/samples/sample6.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Archiviazione dell'appartenenza a un gruppo in un database

Negli esempi seguenti viene illustrato come mantenere le informazioni sui gruppi e sugli utenti in un database. È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework. Questi modelli di entità corrispondono a tabelle e campi di database. La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione. Questo esempio include una classe denominata `ConversationRoom` che sarebbe univoca per un'applicazione che consente agli utenti di partecipare a conversazioni su argomenti diversi, ad esempio Sports o Gardening. Questo esempio include anche una classe per le connessioni. La classe Connection non è assolutamente necessaria per tenere traccia dell'appartenenza al gruppo, ma fa spesso parte di una soluzione affidabile per tenere traccia degli utenti.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

Quindi, nell'hub è possibile recuperare le informazioni sul gruppo e sull'utente dal database e aggiungere manualmente l'utente ai gruppi appropriati. Nell'esempio non è incluso il codice per tenere traccia delle connessioni utente. In questo esempio, la parola chiave `await` non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo. Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo aver aggiunto il nuovo membro, è opportuno applicare la parola chiave `await` per assicurarsi che l'operazione asincrona sia stata completata.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Archiviazione dell'appartenenza al gruppo nell'archiviazione tabelle di Azure

L'uso dell'archiviazione tabelle di Azure per archiviare informazioni sui gruppi e sugli utenti è simile all'uso di un database. Nell'esempio seguente viene illustrata un'entità Table che archivia il nome utente e il nome del gruppo.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

Nell'hub è possibile recuperare i gruppi assegnati quando l'utente si connette.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Verifica dell'appartenenza al gruppo durante la riconnessione

Per impostazione predefinita, SignalR assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Le informazioni sul gruppo dell'utente vengono passate a un token durante la riconnessione e il token viene verificato nel server. Per informazioni sul processo di verifica per il join di utenti ai gruppi, vedere [riunione di gruppi durante la riconnessione](index.md).

In generale, è consigliabile usare il comportamento predefinito di riunione automatica dei gruppi durante la riconnessione. I gruppi SignalR non sono intesi come meccanismo di sicurezza per limitare l'accesso ai dati sensibili. Tuttavia, se l'applicazione deve controllare l'appartenenza a un gruppo di un utente durante la riconnessione, è possibile eseguire l'override del comportamento predefinito. La modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché è necessario recuperare l'appartenenza a un gruppo di un utente per ogni riconnessione anziché solo quando l'utente si connette.

Se è necessario verificare l'appartenenza al gruppo per la riconnessione, creare un nuovo modulo della pipeline Hub che restituisca un elenco di gruppi assegnati, come illustrato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample11.cs)]

Aggiungere quindi il modulo alla pipeline dell'hub, come illustrato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample12.cs?highlight=10)]
