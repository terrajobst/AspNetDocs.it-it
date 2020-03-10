---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
title: Autenticazione degli utenti con l'autenticazione basataC#su form () | Microsoft Docs
author: microsoft
description: Informazioni su come usare l'attributo [autorizzate] per proteggere le pagine specifiche nell'applicazione MVC. Si apprenderà anche come usare l'amministrazione del sito Web...
ms.author: riande
ms.date: 01/27/2009
ms.assetid: 239fd3ca-5630-4b8d-bc4b-2f906b1d3504
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: bed2eafa47fec25ac04cb07e0037f596494bb7d9
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78541510"
---
# <a name="authenticating-users-with-forms-authentication-c"></a>Autenticazione degli utenti con l'autenticazione basata su form (C#)

[Microsoft](https://github.com/microsoft)

> Informazioni su come usare l'attributo [autorizzate] per proteggere le pagine specifiche nell'applicazione MVC. Viene illustrato come utilizzare lo strumento Amministrazione sito Web per creare e gestire utenti e ruoli. Viene inoltre illustrato come configurare la posizione in cui vengono archiviati gli account utente e le informazioni sui ruoli.

L'obiettivo di questa esercitazione è spiegare come è possibile usare l'autenticazione basata su form per proteggere le visualizzazioni nelle applicazioni MVC ASP.NET. Viene illustrato come utilizzare lo strumento Amministrazione sito Web per creare utenti e ruoli. Viene inoltre illustrato come impedire agli utenti non autorizzati di richiamare le azioni del controller. Infine, si apprenderà come configurare la posizione in cui vengono archiviati i nomi utente e le password.

#### <a name="using-the-web-site-administration-tool"></a>Utilizzo dello strumento Amministrazione sito Web

Prima di eseguire altre operazioni, è necessario iniziare creando alcuni utenti e ruoli. Il modo più semplice per creare nuovi utenti e ruoli consiste nell'avvalersi dello strumento Amministrazione sito Web di Visual Studio 2008. È possibile avviare questo strumento selezionando l'opzione di menu **progetto, configurazione ASP.NET**. In alternativa, è possibile avviare lo strumento Amministrazione sito Web facendo clic sull'icona (piuttosto spaventosa) del martello che colpisce il mondo visualizzato nella parte superiore della finestra di Esplora soluzioni (vedere la figura 1).

**Figura 1: avvio dello strumento Amministrazione sito Web**

![clip_image002](authenticating-users-with-forms-authentication-cs/_static/image1.jpg)

All'interno dello strumento Amministrazione sito Web è possibile creare nuovi utenti e ruoli selezionando la scheda sicurezza. fare clic sul collegamento **Crea utente** per creare un nuovo utente denominato Stephen (vedere la figura 2). Fornire all'utente Stephen la password desiderata (ad esempio, *Secret*).

**Figura 2: creazione di un nuovo utente**

![clip_image004](authenticating-users-with-forms-authentication-cs/_static/image2.jpg)

Per creare nuovi ruoli, è innanzitutto necessario abilitare i ruoli e definire uno o più ruoli. Per abilitare i ruoli, fare clic sul collegamento **Abilita ruoli** . Successivamente, creare un ruolo denominato *Administrators* facendo clic sul collegamento **Crea o Gestisci ruoli** (vedere la figura 3).

**Figura 3: creazione di un nuovo ruolo**

![clip_image006](authenticating-users-with-forms-authentication-cs/_static/image3.jpg)

Infine, creare un nuovo utente denominato Sally e associare Sally al ruolo Administrators facendo clic sul collegamento Crea utente e selezionando Administrators quando si crea Sally (vedere la figura 4).

**Figura 4-aggiunta di un utente a un ruolo**

![clip_image008](authenticating-users-with-forms-authentication-cs/_static/image4.jpg)

Quando viene detto tutto, è necessario avere due nuovi utenti denominati Stephen e Sally. È inoltre necessario disporre di un nuovo ruolo denominato Administrators. Sally è un membro del ruolo Administrators e Stephen non lo è.

#### <a name="requiring-authorization"></a>Richiesta dell'autorizzazione

È possibile richiedere l'autenticazione di un utente prima che l'utente richiami un'azione del controller aggiungendo l'attributo [autorizzate] all'azione. È possibile applicare l'attributo [autorizzate] a una singola azione del controller oppure è possibile applicare questo attributo a un'intera classe controller.

Il controller nel listato 1, ad esempio, espone un'azione denominata CompanySecrets (). Poiché questa azione è decorata con l'attributo [autorizzate], questa azione non può essere richiamata a meno che un utente non sia autenticato.

**Listato 1 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample1.cs)]

Se si richiama l'azione CompanySecrets () immettendo l'URL/Home/CompanySecrets nella barra degli indirizzi del browser e l'utente non è autenticato, si verrà reindirizzati automaticamente alla visualizzazione di accesso (vedere la figura 5).

**Figura 5: visualizzazione dell'account di accesso**

![clip_image010](authenticating-users-with-forms-authentication-cs/_static/image5.jpg)

È possibile utilizzare la visualizzazione di accesso per immettere il nome utente e la password. Se non si è un utente registrato, è possibile fare clic sul collegamento **Register (registra** ) per passare alla visualizzazione Register (vedere la figura 6). Per creare un nuovo account utente, è possibile usare la visualizzazione Register.

**Figura 6: visualizzazione del registro**

![clip_image012](authenticating-users-with-forms-authentication-cs/_static/image6.jpg)

Una volta eseguito correttamente l'accesso, è possibile visualizzare la vista CompanySecrets (vedere la figura 7). Per impostazione predefinita, si continuerà a essere connessi fino a quando non si chiude la finestra del browser.

**Figura 7: visualizzazione CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-cs/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizzazione per nome utente o ruolo utente

È possibile utilizzare l'attributo [autorizzate] per limitare l'accesso a un'azione del controller a un particolare gruppo di utenti o a un particolare set di ruoli utente. Ad esempio, il controller Home modificato nel listato 2 contiene due nuove azioni denominate StephenSecrets () e AdministratorSecrets ().

**Listato 2 – Controllers\HomeController.cs**

[!code-csharp[Main](authenticating-users-with-forms-authentication-cs/samples/sample2.cs)]

Solo un utente con il nome utente Stephen può richiamare l'azione StephenSecrets (). Tutti gli altri utenti vengono reindirizzati alla visualizzazione di accesso. La proprietà Users accetta un elenco delimitato da virgole di nomi di account utente.

Solo gli utenti del ruolo Administrators possono richiamare l'azione AdministratorSecrets (). Ad esempio, poiché Sally è un membro del gruppo Administrators, può richiamare l'azione AdministratorSecrets (). Tutti gli altri utenti vengono reindirizzati alla visualizzazione di accesso. La proprietà Roles accetta un elenco delimitato da virgole di nomi di ruoli.

#### <a name="configuring-authentication"></a>Configurazione dell'autenticazione

A questo punto, è possibile chiedersi dove archiviare le informazioni sull'account utente e sui ruoli. Per impostazione predefinita, le informazioni vengono archiviate in un database di SQL Express (Letizia), denominato ASPNETDB. MDF, che si trova nella cartella app\_data dell'applicazione MVC. Questo database viene generato automaticamente dal framework ASP.NET quando si inizia a usare l'appartenenza.

Per visualizzare il database ASPNETDB. mdf nella finestra di Esplora soluzioni, è prima di tutto necessario selezionare l'opzione di menu progetto, Mostra tutti i file.

L'utilizzo del database SQL Express predefinito è ottimale durante lo sviluppo di un'applicazione. Probabilmente, tuttavia, non si vuole usare il database ASPNETDB. MDF predefinito per un'applicazione di produzione. In tal caso, è possibile modificare la posizione in cui vengono archiviate le informazioni dell'account utente completando i due passaggi seguenti:

1. Aggiungere gli oggetti di database Servizi per le applicazioni al database di produzione: modificare la stringa di connessione dell'applicazione in modo che punti al database di produzione

Il primo passaggio consiste nell'aggiungere tutti gli oggetti di database (tabelle e stored procedure) necessari al database di produzione. Il modo più semplice per aggiungere questi oggetti a un nuovo database consiste nell'usare l'installazione guidata di ASP.NET SQL Server (vedere la figura 8). È possibile avviare questo strumento aprendo il prompt dei comandi di Visual Studio 2008 dal gruppo di programmi Microsoft Visual Studio 2008 ed eseguendo il comando seguente dal prompt dei comandi:

ASPNET\_RegSql

**Figura 8: installazione guidata di ASP.NET SQL Server**

![clip_image016](authenticating-users-with-forms-authentication-cs/_static/image8.jpg)

L'installazione guidata di ASP.NET SQL Server consente di selezionare un database SQL Server nella rete e di installare tutti gli oggetti di database necessari per i servizi delle applicazioni ASP.NET. Non è necessario che il server di database si trovi nel computer locale.

> [!NOTE] 
> 
> Se non si vuole usare l'installazione guidata di ASP.NET SQL Server, è possibile trovare gli script SQL per l'aggiunta degli oggetti di database dei servizi dell'applicazione nella cartella seguente:
> 
> > C:\Windows\Microsoft.NET\Framework\v2.0.50727

Dopo aver creato gli oggetti di database necessari, è necessario modificare la connessione al database usata dall'applicazione MVC. Modificare la stringa di connessione ApplicationServices nel file di configurazione Web (Web. config) in modo che punti al database di produzione. Ad esempio, la connessione modificata nel listato 3 punta a un database denominato MyProductionDB (la stringa di connessione ApplicationServices originale è stata impostata come commento).

**Listato 3-Web. config**

[!code-xml[Main](authenticating-users-with-forms-authentication-cs/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurazione delle autorizzazioni per il database

Se si utilizza la sicurezza integrata per la connessione al database, sarà necessario aggiungere l'account utente di Windows corretto come account di accesso al database. L'account corretto dipende dal fatto che si usi il Server di sviluppo ASP.NET o Internet Information Services come server Web. L'account utente corretto dipende anche dal sistema operativo.

Se si usa il Server di sviluppo ASP.NET (il server Web predefinito usato da Visual Studio), l'applicazione viene eseguita nel contesto dell'account utente di Windows. In tal caso, è necessario aggiungere l'account utente di Windows come account di accesso al server di database.

In alternativa, se si utilizza Internet Information Services, è necessario aggiungere l'account ASPNET o l'account del servizio di rete/autorità NT come account di accesso del server di database. Se si utilizza Windows XP, aggiungere l'account ASPNET come account di accesso al database. Se si utilizza un sistema operativo più recente, ad esempio Windows Vista o Windows Server 2008, aggiungere l'account NT AUTHORITY/NETWORK SERVICE come account di accesso al database.

È possibile aggiungere un nuovo account utente al database usando Microsoft SQL Server Management Studio (vedere la figura 9).

**Figura 9-Creazione di un nuovo account di accesso Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-cs/_static/image9.jpg)

Dopo aver creato l'account di accesso richiesto, è necessario eseguire il mapping dell'account di accesso a un utente del database con i ruoli di database corretti. Fare doppio clic sull'account di accesso e selezionare la scheda mapping utenti. Selezionare uno o più ruoli del database dei servizi dell'applicazione. Per autenticare gli utenti, ad esempio, è necessario abilitare l'appartenenza\_ASPNET\_ruolo del database BasicAccess. Per creare nuovi utenti, è necessario abilitare l'appartenenza ASPNET\_\_ruolo del database FullAccess (vedere la figura 10).

**Figura 10-aggiunta di Servizi per le applicazioni ruoli del database**

![clip_image020](authenticating-users-with-forms-authentication-cs/_static/image10.jpg)

#### <a name="summary"></a>Riepilogo

In questa esercitazione si è appreso come usare l'autenticazione basata su form quando si compila un'applicazione MVC ASP.NET. In primo luogo, si è appreso come creare nuovi utenti e ruoli sfruttando lo strumento Amministrazione sito Web. Si è quindi appreso come usare l'attributo [autorizzate] per impedire a utenti non autorizzati di richiamare le azioni del controller. Infine, si è appreso come configurare l'applicazione MVC per archiviare le informazioni su utenti e ruoli in un database di produzione.

> [!div class="step-by-step"]
> [avanti](authenticating-users-with-windows-authentication-cs.md)
