---
uid: whitepapers/ms03-32-issue
title: Correzione per l'errore "applicazione server non disponibile" dopo l'applicazione dell'aggiornamento della sicurezza per IE | Microsoft Docs
author: rick-anderson
description: In questo documento viene descritta la patch che corregge un problema relativo all'aggiornamento della sicurezza MS03-32 per Internet Explorer che influiscono sulle applicazioni ASP.NET 1,0 in esecuzione su Wi...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: 1365eebb-bdf7-4a05-8d18-7f200531be55
msc.legacyurl: /whitepapers/ms03-32-issue
msc.type: content
ms.openlocfilehash: e0b6776cbfe22e341ac7105f03daac5074b480fc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78574186"
---
# <a name="fix-for-server-application-unavailable-error-after-applying-security-update-for-ie"></a>Correggere l'errore 'Applicazione server non disponibile' dopo l'applicazione di un aggiornamento della sicurezza per Internet Explorer

> In questo documento viene descritta la patch che corregge un problema relativo all'aggiornamento della sicurezza MS03-32 per Internet Explorer che interessa le applicazioni ASP.NET 1,0 in esecuzione su Windows XP Professional.
> 
> Si applica a ASP.NET 1,0 e Windows XP Professional.

Microsoft ha identificato un problema relativo all'aggiornamento della sicurezza MS03-32 per la patch di protezione di Internet Explorer e ASP.NET 1,0 in esecuzione su Windows XP. Questa patch può essere installata manualmente o ottenendo aggiornamenti critici recenti dal sito Windows Update.

Il sintomo di questo problema è che dopo l'installazione della patch in un computer con Windows XP, tutte le richieste alle applicazioni ASP.NET in esecuzione sul server Web IIS 5,1 locale generano un messaggio di errore che indica che l'applicazione server non è disponibile. Le richieste ai server Web remoti non sono interessate.

Questo problema influisca solo sulle installazioni che eseguono ASP.NET 1,0 in Windows XP. Non ha alcun effetto sui computer che eseguono Windows 2000 o Windows Server 2003. Non influisca inoltre sui computer che eseguono Windows XP con ASP.NET 1,1 installato.

Si noti che questo problema **non è** un bug di sicurezza con ASP.NET. Non **apre né** consente attacchi dannosi contro un'applicazione o un server ASP.NET. È invece puramente un bug funzionale causato dalla patch stessa.

Ci stiamo impegnando in una soluzione permanente per questo problema. Nel frattempo, è possibile eseguire il file batch seguente come soluzione alternativa per il problema. Il file batch esegue le operazioni seguenti:

1. Arresta i servizi di stato IIS e ASP.NET
2. Elimina e ricrea l'account ASPNET con una password temporanea nota
3. Usa il comando Windows `runas` per avviare un eseguibile che crea un profilo utente ASPNET
4. Esegue nuovamente la registrazione di ASP.NET. Verrà creata una nuova password casuale per l'account e verranno applicate le impostazioni predefinite del controllo di accesso ASP.NET
5. Riavvia il servizio IIS

Il file batch contiene una password temporanea hardcoded "<strong>1pass\@Word</strong>", a cui verrà richiesto di immettere il comando RunAs durante l'esecuzione del file batch. Al termine del comando RunAs, la password dell'account ASPNET viene ricreata con un valore casuale sicuro. Si noti che il file batch potrebbe non riuscire se la password hardcoded non soddisfa i requisiti di complessità delle password nell'ambiente in uso. In tal caso, è possibile modificarlo in un altro valore appropriato per l'ambiente in uso.

*> [!IMPORTANT]* Se sono state aggiunte le impostazioni di controllo di accesso o le autorizzazioni dell'account di database personalizzate per l'account ASPNET, sarà necessario ricrearle al termine del file batch. Questo perché, quando l'account viene ricreato, otterrà un nuovo ID di sicurezza (SID).

*> [!IMPORTANT]* Se si esegue il processo di lavoro ASP.NET con un account personalizzato diverso dall'account ASPNET, non eseguire questo file batch. In alternativa, è consigliabile eseguire l'accesso in modo interattivo o usare il comando RunAs con l'account che creerà un profilo utente per tale account.

Il file batch è incluso nell'archivio autoestraente riportato di seguito. Per usarla:

1. È necessario eseguire un account con privilegi di amministratore
2. [Scaricare e aprire il file eseguibile autoestraente](ms03-32-issue/_static/fixup1.exe)
3. Estrarre il contenuto in c:\
4. Selezionare Esegui... dal menu Start immettere `cmd.exe`
5. Nelle finestre di comando aperte digitare `c:\fixup.cmd`.
6. Quando richiesto, immettere <strong>1pass\@Word</strong> come password.
7. Se sono già state apportate impostazioni di controllo di accesso personalizzate o di account di database per l'account ASPNET, sarà necessario applicare nuovamente queste impostazioni.

Molte scuse per l'inconveniente causato da questo problema. Verranno pubblicate informazioni aggiuntive non appena saranno disponibili.

La matrice riportata di seguito descrive le piattaforme e le versioni interessate da questo problema.

| .NET Framework | Piattaforma | Colpiti |
| --- | --- | --- |
| Versione 1,0 | Windows 2000 Professional | No |
| Versione 1,0 | Windows 2000 Server | No |
| Versione 1,0 | Windows XP Professional | Yes |
| Versione 1,0 | Windows Server 2003 | No |
| Versione 1,0 | Windows XP Home con Cassini | No |
| Versione 1.1 | Windows 2000 Professional | No |
| Versione 1.1 | Windows 2000 Server | No |
| Versione 1.1 | Windows XP Professional | No |
| Versione 1.1 | Windows Server 2003 | No |
| Versione 1.1 | Windows XP Home con Cassini | No |

Grazie,   
 Il team di ASP.NET
