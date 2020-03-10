---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
title: Autenticazione degli utenti con l'autenticazione di Windows (VB) | Microsoft Docs
author: microsoft
description: Informazioni su come usare l'autenticazione di Windows nel contesto di un'applicazione MVC. Si apprenderà come abilitare l'autenticazione di Windows all'interno dell'applicazione Web Co...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 532fa051-7d5c-4d6d-87f6-339ce4b84c44
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-windows-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: aa64b1f9ef6461a81611ca066310dca2d545baa3
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78624117"
---
# <a name="authenticating-users-with-windows-authentication-vb"></a>Autenticazione degli utenti con l'autenticazione di Windows (VB)

[Microsoft](https://github.com/microsoft)

> Informazioni su come usare l'autenticazione di Windows nel contesto di un'applicazione MVC. Si apprenderà come abilitare l'autenticazione di Windows nel file di configurazione Web dell'applicazione e come configurare l'autenticazione con IIS. Viene infine illustrato come utilizzare l'attributo [autorizzate] per limitare l'accesso alle azioni del controller a specifici utenti o gruppi di Windows.

L'obiettivo di questa esercitazione è spiegare come sfruttare le funzionalità di sicurezza integrate in Internet Information Services per proteggere le visualizzazioni nelle applicazioni MVC. Si apprenderà come consentire le azioni del controller da richiamare solo da utenti o utenti di Windows specifici che sono membri di gruppi di Windows specifici.

L'uso dell'autenticazione di Windows è utile quando si crea un sito Web aziendale interno (un sito Intranet) e si vuole che gli utenti siano in grado di usare i nomi utente e le password standard di Windows per l'accesso al sito Web. Se si sta creando un sito Web esterno, ovvero un sito Web Internet, è consigliabile utilizzare l'autenticazione basata su form.

#### <a name="enabling-windows-authentication"></a>Abilitazione dell'autenticazione di Windows

Quando si crea una nuova applicazione MVC ASP.NET, l'autenticazione di Windows non è abilitata per impostazione predefinita. L'autenticazione basata su form è il tipo di autenticazione predefinito abilitato per le applicazioni MVC. È necessario abilitare l'autenticazione di Windows modificando il file di configurazione Web (Web. config) dell'applicazione MVC. Trovare la sezione &lt;Authentication&gt; e modificarla in modo da usare Windows anziché l'autenticazione basata su form, come indicato di seguito:

[!code-xml[Main](authenticating-users-with-windows-authentication-vb/samples/sample1.xml)]

Quando si Abilita l'autenticazione di Windows, il server Web diventa responsabile dell'autenticazione degli utenti. In genere, sono disponibili due diversi tipi di server Web che si usano per la creazione e la distribuzione di un'applicazione MVC ASP.NET.

Per prima cosa, durante lo sviluppo di un'applicazione MVC, si usa il server Web di sviluppo ASP.NET incluso in Visual Studio. Per impostazione predefinita, il server Web di sviluppo ASP.NET esegue tutte le pagine nel contesto dell'account di Windows corrente (indipendentemente dall'account usato per accedere a Windows).

Il server Web di sviluppo ASP.NET supporta anche l'autenticazione NTLM. È possibile abilitare l'autenticazione NTLM facendo clic con il pulsante destro del mouse sul nome del progetto nella finestra Esplora soluzioni e scegliendo Proprietà. Selezionare quindi la scheda Web e selezionare la casella di controllo NTLM (vedere la figura 1).

**Figura 1-Abilitazione dell'autenticazione NTLM per il server Web di sviluppo ASP.NET**

![clip_image002](authenticating-users-with-windows-authentication-vb/_static/image1.jpg)

Per un'applicazione Web di produzione, è possibile usare IIS come server Web. IIS supporta diversi tipi di autenticazione, tra cui:

- Autenticazione di base: definita come parte del protocollo HTTP 1,0. Invia nomi utente e password in testo non crittografato (con codifica Base64) su Internet. -Authentication digest: Invia un hash di una password, invece della password stessa, in Internet. -Autenticazione integrata di Windows (NTLM): il tipo di autenticazione migliore da utilizzare negli ambienti Intranet utilizzando Windows. -Autenticazione del certificato: consente l'autenticazione tramite un certificato sul lato client. Il certificato viene mappato a un account utente di Windows.

> [!NOTE] 
> 
> Per una panoramica più dettagliata di questi diversi tipi di autenticazione, vedere [https://msdn.microsoft.com/library/aa292114(VS.71).aspx](https://msdn.microsoft.com/library/aa292114(VS.71).aspx).

È possibile utilizzare Internet Information Services Manager per abilitare un particolare tipo di autenticazione. Tenere presente che tutti i tipi di autenticazione non sono disponibili nel caso di ogni sistema operativo. Inoltre, se si utilizza IIS 7,0 con Windows Vista, sarà necessario abilitare i diversi tipi di autenticazione di Windows prima che vengano visualizzati in Gestione Internet Information Services. Aprire il **Pannello di controllo, programmi, programmi e funzionalità, attivare o disattivare le funzionalità Windows**ed espandere il nodo Internet Information Services (vedere la figura 2).

**Figura 2: abilitazione delle funzionalità IIS di Windows**

![clip_image004](authenticating-users-with-windows-authentication-vb/_static/image2.jpg)

Utilizzando Internet Information Services, è possibile abilitare o disabilitare tipi diversi di autenticazione. Ad esempio, nella figura 3 viene illustrata la disabilitazione dell'autenticazione anonima e l'abilitazione dell'autenticazione integrata di Windows (NTLM) quando si utilizza IIS 7,0.

**Figura 3-Abilitazione dell'autenticazione integrata di Windows**

![clip_image006](authenticating-users-with-windows-authentication-vb/_static/image3.jpg)

#### <a name="authorizing-windows-users-and-groups"></a>Autorizzazione di utenti e gruppi di Windows

Dopo aver abilitato l'autenticazione di Windows, è possibile usare l'&lt;autorizzare&gt; attributo per controllare l'accesso ai controller o alle azioni del controller. Questo attributo può essere applicato a un intero controller MVC o a una particolare azione del controller.

Il controller Home nel listato 1, ad esempio, espone tre azioni denominate index (), CompanySecrets () e StephenSecrets (). Chiunque può richiamare l'azione index (). Tuttavia, solo i membri del gruppo gestione locale di Windows possono richiamare l'azione CompanySecrets (). Infine, solo l'utente di dominio di Windows denominato Stephen (nel dominio Redmond) può richiamare l'azione StephenSecrets ().

**Listato 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-windows-authentication-vb/samples/sample2.vb)]

> [!NOTE]
> A causa del controllo dell'account utente di Windows, quando si utilizza Windows Vista o Windows Server 2008, il gruppo Administrators locale si comporterà in modo diverso rispetto ad altri gruppi. Il &lt;autorizzazione&gt; attributo non riconosce correttamente un membro del gruppo Administrators locale, a meno che non si modifichino le impostazioni del controllo dell'account utente del computer.

Esattamente ciò che accade quando si tenta di richiamare un'azione del controller senza essere le autorizzazioni corrette dipende dal tipo di autenticazione abilitata. Per impostazione predefinita, quando si usa il Server di sviluppo ASP.NET, è sufficiente ottenere una pagina vuota. La pagina viene servita con uno stato di risposta http **401 non autorizzato** .

Se, d'altra parte, si usa IIS con l'autenticazione anonima disabilitata e l'autenticazione di base abilitata, si continua a ricevere una finestra di dialogo di accesso ogni volta che si richiede la pagina protetta (vedere la figura 4).

**Figura 4: finestra di dialogo di accesso con autenticazione di base**

![clip_image008](authenticating-users-with-windows-authentication-vb/_static/image4.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione è stato illustrato come è possibile usare l'autenticazione di Windows nel contesto di un'applicazione MVC ASP.NET. Si è appreso come abilitare l'autenticazione di Windows nel file di configurazione Web dell'applicazione e come configurare l'autenticazione con IIS. Infine, si è appreso come utilizzare il &lt;autorizzare&gt; attributo per limitare l'accesso alle azioni del controller a specifici utenti o gruppi di Windows.

> [!div class="step-by-step"]
> [Precedente](authenticating-users-with-forms-authentication-vb.md)
> [Successivo](preventing-javascript-injection-attacks-vb.md)
