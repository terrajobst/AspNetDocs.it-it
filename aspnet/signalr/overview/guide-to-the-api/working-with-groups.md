---
uid: signalr/overview/guide-to-the-api/working-with-groups
title: Uso dei gruppi in SignalR | Microsoft Docs
author: bradygaster
description: Questo argomento descrive come salvare in modo permanente le informazioni sull'appartenenza ai gruppi con l'API Hub.
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: cd378ecd-3e9e-4236-b902-65916d85a048
msc.legacyurl: /signalr/overview/guide-to-the-api/working-with-groups
msc.type: authoredcontent
ms.openlocfilehash: 46dd952fc4902b37c981a111dfa344dad79bb668
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78558583"
---
# <a name="working-with-groups-in-signalr"></a>Uso dei gruppi in SignalR

di [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> In questo argomento viene descritto come aggiungere utenti ai gruppi e salvare in modo permanente le informazioni sull'appartenenza al gruppo.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versioni del software usate in questo argomento
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR versione 2
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versioni precedenti di questo argomento
>
> Per informazioni sulle versioni precedenti di SignalR, vedere [SignalR versioni precedenti](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Domande e commenti
>
> Inviare commenti e suggerimenti su come questa esercitazione è stata apprezzata e su cosa è possibile migliorare nei commenti nella parte inferiore della pagina. In caso di domande non direttamente correlate all'esercitazione, è possibile pubblicarle nel [Forum di ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o in [StackOverflow.com](http://stackoverflow.com/).

## <a name="overview"></a>Panoramica

I gruppi in SignalR forniscono un metodo per la trasmissione di messaggi a subset specificati di client connessi. Un gruppo può avere un numero qualsiasi di client e un client può essere un membro di un numero qualsiasi di gruppi. Non è necessario creare gruppi in modo esplicito. In effetti, un gruppo viene creato automaticamente la prima volta che si specifica il nome in una chiamata a groups. Add e viene eliminato quando si rimuove l'ultima connessione dall'appartenenza al gruppo. Per un'introduzione all'uso dei gruppi, vedere [come gestire l'appartenenza a un gruppo dalla classe Hub](hubs-api-guide-server.md#groupsfromhub) nella Guida dell'API Hub-server.

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

Se si desidera aggiungere un client a un gruppo e inviare immediatamente un messaggio al client tramite il gruppo, è necessario assicurarsi che il metodo groups. Add completi prima di tutto. Negli esempi di codice seguenti viene illustrato come eseguire questa operazione.

[!code-csharp[Main](working-with-groups/samples/sample2.cs?highlight=1,3)]

In generale, non è consigliabile includere `await` quando si chiama il metodo `Groups.Remove` perché l'ID connessione che si sta tentando di rimuovere potrebbe non essere più disponibile. In tal caso, `TaskCanceledException` viene generata dopo il timeout della richiesta. Se l'applicazione deve verificare che l'utente sia stato rimosso dal gruppo prima di inviare un messaggio al gruppo, è possibile aggiungere `await` prima `Groups.Remove`, quindi intercettare l'eccezione `TaskCanceledException` che potrebbe essere generata.

<a id="call"></a>

## <a name="calling-members-of-a-group"></a>Chiamata di membri di un gruppo

È possibile inviare messaggi a tutti i membri di un gruppo o solo ai membri specificati del gruppo, come illustrato negli esempi seguenti.

- **Tutti** i client connessi in un gruppo specificato.

    [!code-css[Main](working-with-groups/samples/sample3.css)]
- Tutti i client connessi in un gruppo specificato, **ad eccezione dei client specificati**, identificati dall'ID connessione.

    [!code-csharp[Main](working-with-groups/samples/sample4.cs)]
- Tutti i client connessi in un gruppo specificato **eccetto il client chiamante**.

    [!code-css[Main](working-with-groups/samples/sample5.css)]

<a id="storedatabase"></a>

## <a name="storing-group-membership-in-a-database"></a>Archiviazione dell'appartenenza a un gruppo in un database

Negli esempi seguenti viene illustrato come mantenere le informazioni sui gruppi e sugli utenti in un database. È possibile utilizzare qualsiasi tecnologia di accesso ai dati; Nell'esempio seguente viene tuttavia illustrato come definire i modelli utilizzando Entity Framework. Questi modelli di entità corrispondono a tabelle e campi di database. La struttura dei dati potrebbe variare notevolmente in base ai requisiti dell'applicazione. Questo esempio include una classe denominata `ConversationRoom` che sarebbe univoca per un'applicazione che consente agli utenti di partecipare a conversazioni su argomenti diversi, ad esempio Sports o Gardening. Questo esempio include anche una classe per le connessioni. La classe Connection non è assolutamente necessaria per tenere traccia dell'appartenenza al gruppo, ma fa spesso parte di una soluzione affidabile per tenere traccia degli utenti.

[!code-csharp[Main](working-with-groups/samples/sample6.cs)]

Quindi, nell'hub è possibile recuperare le informazioni sul gruppo e sull'utente dal database e aggiungere manualmente l'utente ai gruppi appropriati. Nell'esempio non è incluso il codice per tenere traccia delle connessioni utente. In questo esempio, la parola chiave `await` non viene applicata prima `Groups.Add` perché un messaggio non viene inviato immediatamente ai membri del gruppo. Se si desidera inviare un messaggio a tutti i membri del gruppo immediatamente dopo aver aggiunto il nuovo membro, è opportuno applicare la parola chiave `await` per assicurarsi che l'operazione asincrona sia stata completata.

[!code-csharp[Main](working-with-groups/samples/sample7.cs)]

<a id="storeazuretable"></a>

## <a name="storing-group-membership-in-azure-table-storage"></a>Archiviazione dell'appartenenza al gruppo nell'archiviazione tabelle di Azure

L'uso dell'archiviazione tabelle di Azure per archiviare informazioni sui gruppi e sugli utenti è simile all'uso di un database. Nell'esempio seguente viene illustrata un'entità Table che archivia il nome utente e il nome del gruppo.

[!code-csharp[Main](working-with-groups/samples/sample8.cs)]

Nell'hub è possibile recuperare i gruppi assegnati quando l'utente si connette.

[!code-csharp[Main](working-with-groups/samples/sample9.cs)]

<a id="verify"></a>

## <a name="verifying-group-membership-when-reconnecting"></a>Verifica dell'appartenenza al gruppo durante la riconnessione

Per impostazione predefinita, SignalR assegna automaticamente un utente ai gruppi appropriati quando si riconnette da un'interruzione temporanea, ad esempio quando una connessione viene eliminata e ristabilita prima del timeout della connessione. Le informazioni sul gruppo dell'utente vengono passate a un token durante la riconnessione e il token viene verificato nel server. Per informazioni sul processo di verifica per il join di utenti ai gruppi, vedere [riunione di gruppi durante la riconnessione](../security/introduction-to-security.md#rejoingroup).

In generale, è consigliabile usare il comportamento predefinito di riunione automatica dei gruppi durante la riconnessione. I gruppi SignalR non sono intesi come meccanismo di sicurezza per limitare l'accesso ai dati sensibili. Tuttavia, se l'applicazione deve controllare l'appartenenza a un gruppo di un utente durante la riconnessione, è possibile eseguire l'override del comportamento predefinito. La modifica del comportamento predefinito può aggiungere un carico di lavoro al database perché è necessario recuperare l'appartenenza a un gruppo di un utente per ogni riconnessione anziché solo quando l'utente si connette.

Se è necessario verificare l'appartenenza al gruppo per la riconnessione, creare un nuovo modulo della pipeline Hub che restituisca un elenco di gruppi assegnati, come illustrato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample10.cs)]

Aggiungere quindi il modulo alla pipeline dell'hub, come illustrato di seguito.

[!code-csharp[Main](working-with-groups/samples/sample11.cs?highlight=4)]
