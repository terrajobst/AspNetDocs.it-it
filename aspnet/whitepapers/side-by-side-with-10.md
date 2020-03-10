---
uid: whitepapers/side-by-side-with-10
title: Esecuzione side-by-side di ASP.NET .NET Framework 1,0 e 1,1 | Microsoft Docs
author: rick-anderson
description: Questo white paper descrive come installare .NET 1,0 e .NET 1,1 nel computer, consentendo l'esecuzione di un'applicazione Web ASP.NET in entrambe le versioni di Fram...
ms.author: riande
ms.date: 02/10/2010
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: c123545099013af71569bce4707f2b3eb732c344
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632972"
---
# <a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>Esecuzione side-by-side di .NET Framework 1.0 e 1.1 in ASP.NET

> Questo white paper descrive come installare .NET 1,0 e .NET 1,1 nel computer, consentendo l'esecuzione di un'applicazione Web ASP.NET in entrambe le versioni del Framework.
> 
> Si applica a ASP.NET 1,0 e ASP.NET 1,1.

In ASP.NET, le applicazioni vengono eseguite side-by-side quando sono installate nello stesso computer, ma usano versioni diverse del .NET Framework. Nell'argomento seguente viene descritto come configurare le applicazioni ASP.NET per l'esecuzione side-by-side e vengono fornite le procedure dettagliate per:

- [Mantenere il mapping dell'applicazione Web per .NET Framework versione 1,0 durante l'installazione](#1)
- [Eseguire il mapping di un'applicazione Web a una versione specifica del .NET Framework](#2)
- [Trovare la versione del .NET Framework utilizzata da un sito Web](#3)

Tradizionalmente, quando un componente o un'applicazione viene aggiornata in un computer, la versione precedente viene rimossa e sostituita con la versione più recente. Se la nuova versione non è compatibile con la versione precedente, questo in genere interrompe le altre applicazioni che usano il componente o l'applicazione. Il .NET Framework fornisce supporto per l'esecuzione side-by-Side, che consente l'installazione di più versioni di un assembly o di un'applicazione nello stesso computer nello stesso momento. Poiché è possibile installare più versioni contemporaneamente, le applicazioni gestite possono selezionare la versione da usare senza influire sulle applicazioni che usano una versione diversa.

Per impostazione predefinita, durante l'installazione del .NET Framework versione 1,1, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente per usare la versione più recente del .NET Framework. Se non si vuole che le applicazioni ASP.NET utilizzino per impostazione predefinita .NET Framework 1,1, fare clic [qui](#1) per informazioni su come evitare questo problema durante l'installazione.

Se si aggiorna il server Web a .NET Framework 1,1 e si desidera eseguire una o più applicazioni Web .NET Framework 1,0, è necessario aggiornare la mappa di script Internet Information Services (IIS). Il mapping degli script è il meccanismo per eseguire il mapping dell'estensione di file aspx per un'applicazione Web specifica a una versione del .NET Framework. Fare clic [qui](#2) per informazioni su come eseguire il mapping di un'applicazione Web a una versione specifica del .NET Framework.

È possibile utilizzare Internet Information Manager o lo strumento di registrazione IIS ASP.NET (ASPNET\_regiis. exe) per individuare la versione .NET Framework che esegue una particolare applicazione Web. Fare clic [qui](#3) per informazioni su come trovare la versione del .NET Framework utilizzata da un sito Web.

Una considerazione di importazione quando si esegue la migrazione a .NET Framework 1,1 è che ogni versione del .NET Framework usa il proprio file Machine. config. Di conseguenza, se un amministratore Web ha apportato modifiche al file Machine. config, è necessario eseguire la migrazione di tali modifiche al file Machine. config di .NET Framework 1,1.

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>Gestione del mapping dell'applicazione Web a .NET Framework 1,0 durante l'installazione

Per impostazione predefinita, tutte le applicazioni ASP.NET esistenti vengono riconfigurate automaticamente durante l'installazione in modo da usare la versione più recente del .NET Framework. Utilizzando la versione più recente del .NET Framework, le applicazioni possono sfruttare appieno i miglioramenti e le nuove funzionalità incluse nella nuova versione. Allo stesso tempo, l'amministratore Web, che potrebbe voler controllare in modo granulare quali applicazioni vengono aggiornate, può impedire il mapping automatico di tutte le applicazioni ASP.NET esistenti durante l'installazione del .NET Framework.

Per evitare che il mapping automatico dell'intera applicazione ASP.NET alla versione più recente del .NET Framework, l'amministratore Web può utilizzare l'opzione della riga di comando/noaspupgrade con il programma di installazione Dotnetfx. exe.

**Per impedire il rimapping totale dell'applicazione ASP.NET alla versione più recente**

1. Passare a **Start**.
2. Fare clic su **Esegui**.
3. Digitare **cmd**.
4. Fare clic su **OK**.  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. Al prompt dei comandi digitare la riga seguente per avviare l'installazione del .NET Framework: **Dotnetfx. exe/c: "install/NOASPUPGRADE?** .  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. Fare clic su **Sì** nell'installazione di Microsoft .NET Framework 1,1. Verrà avviato il processo di installazione del .NET Framework 1,1.  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>Eseguire il mapping di un'applicazione Web a una versione specifica del .NET Framework

Ogni versione del .NET Framework include una versione di ASP.NET IIS Registration Tool (ASPNET\_regiis. exe). Questo strumento consente agli amministratori di specificare che un'applicazione Web deve essere eseguita in una particolare versione del .NET Framework. Questa operazione viene definita come mapping di un'applicazione Web a una versione del .NET Framework. Gli amministratori devono selezionare ASPNET\_regiis. exe che corrisponde alla versione del .NET Framework che verrà associata all'applicazione Web. Ad esempio, un amministratore che desidera specificare che un sito Web utilizza .NET Framework 1,1 deve utilizzare ASPNET\_regiis. exe che viene fornita con .NET Framework 1,1.

Il\_ASPNET regiis. exe per la versione 1,0 si trova in:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.0.3705**\ASPNET\_regiis

Il\_ASPNET regiis. exe per la versione 1, 1 si trova in:

- C:\WINDOWS\Microsoft.NET\Framework\\**v 1.1.4322**\ASPNET\_regiis

Il\_ASPNET regiis. exe fornisce due opzioni per il mapping di script a un'applicazione Web:

- **-s** imposta la mappa di script nel percorso e nelle relative directory figlio.
- **-sn** imposta la mappa di script solo nel percorso.

Il percorso definisce il percorso dei metadati IIS dell'applicazione Web, definito nel formato W3SVC/ROOT/{WebSiteNumber}/{Application\_Name}. Ad esempio, per un'applicazione Web denominata portale che si trova nel sito Web predefinito, il percorso della metabase è W3SVC/1/ROOT/Portal.

![](side-by-side-with-10/_static/image4.gif)

Nota è inoltre possibile utilizzare uno strumento denominato Editor della metabase per ottenere il percorso della metabase. È possibile scaricare questo strumento dal sito supporto tecnico Microsoft in [https://support.microsoft.com/default.aspx?scid=kb; en-US; 232068.](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- Eseguire ASPNET\_regiis. exe-s W3SVC/1/ROOT/Portal per aggiornare la mappa di script di IIS del portale e la relativa sottoapplicazione.  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Eseguire ASPNET\_regiis. exe-sn W3SVC/1/ROOT/Portal per aggiornare la mappa di script di IIS del portale, senza influire sulle applicazioni nelle sottodirectory del portale.  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Trovare la versione .NET Framework utilizzata da un'applicazione Web

Un amministratore può utilizzare Internet Service Manager per individuare la versione del .NET Framework che esegue un sito Web. Diverse versioni del sistema operativo avviano Internet Service Manager in modo diverso. Per avviare Service Manager, attenersi alla procedura riportata di seguito.

**Per avviare Internet Service Manager**

1. Passare a **Start**.
2. Fare clic su **Esegui**.
3. Digitare **inetmgr**.  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. Da Internet Service Manager selezionare l'applicazione Web di cui si desidera tenere conto la versione del .NET Framework.  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Fare clic con il pulsante destro del mouse sull'applicazione Web e scegliere **Proprietà.**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. Nella finestra delle proprietà selezionare **configurazione.**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. Nella tabella mapping applicazioni selezionare **. aspx**, quindi fare clic su **modifica**.  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. Nella casella di testo **eseguibile** esaminare la directory Version scorrendo. Se la directory della versione è v. 1.1.4322, viene eseguito il mapping dell'applicazione a .NET Framework 1,1. Viceversa, se la directory della versione è v 1.0.3705, viene eseguito il mapping dell'applicazione a .NET Framework 1,0.  
  
    ![](side-by-side-with-10/_static/image12.gif)
