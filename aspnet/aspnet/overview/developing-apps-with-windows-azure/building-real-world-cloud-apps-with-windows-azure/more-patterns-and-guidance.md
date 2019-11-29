---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
title: Altri modelli e linee guida (creazione di app Cloud reali con Azure) | Microsoft Docs
author: MikeWasson
description: La creazione di app cloud del mondo reale con l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che possono essere...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: 7e97cfc3-d830-4002-8ff7-5790d1ff49e6
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/more-patterns-and-guidance
msc.type: authoredcontent
ms.openlocfilehash: afade34477d1136883e7543d09e73dfbe435690e
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/28/2019
ms.locfileid: "74585352"
---
# <a name="more-patterns-and-guidance-building-real-world-cloud-apps-with-azure"></a>Altri modelli e linee guida (creazione di app Cloud reali con Azure)

di [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Scarica il progetto di correzione it](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [Scarica l'E-Book](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> La **creazione di app cloud del mondo reale con** l'e-book di Azure si basa su una presentazione sviluppata da Scott Guthrie. Vengono illustrati 13 modelli e procedure che consentono di sviluppare correttamente app Web per il cloud. Per informazioni sull'e-book, vedere [il primo capitolo](introduction.md).

A questo punto sono stati rilevati 13 modelli che forniscono indicazioni su come avere esito positivo in cloud computing. Questi sono solo alcuni dei modelli che si applicano alle app cloud. Di seguito sono riportate altre cloud computing argomenti e risorse utili:

- Migrazione di applicazioni locali esistenti nel cloud. 

    - [Trasferimento delle applicazioni nel cloud](https://msdn.microsoft.com/library/ff728592.aspx). E-book di Microsoft Patterns and Practices. Disponibile anche come brossura per la [copia di dischi rigidi](https://www.amazon.com/dp/1621140202).
    - [Migrazione di ASP.NET e IIS.NET di Microsoft](https://go.microsoft.com/fwlink/?LinkId=400656). Case Study di Robert McMurray.
    - [Trasferimento di 4 &amp; Mayor a siti Web di Azure](http://www.jeff.wilcox.name/2013/04/4thandmayor-azure-websites/). Post di Blog di Jeff Wilcox che racconta la sua esperienza nello spostando un'app Web da Amazon Web Services ad app Web nel servizio app Azure.
    - [Trasferimento di app in Azure: quali modifiche?](https://azure.microsoft.com/documentation/videos/web-sites-internals-and-the-file-system/) Breve video di Stefan Schackow, che illustra file system l'accesso alle app Web nel servizio app Azure.
    - [Cloud ibrido di Azure](https://www.amazon.com/dp/B00EOP4UQW). Libro cartaceo o e-book di Danny Garber, Jamal Malik e Adam Fazio.
- Problemi di sicurezza, autenticazione e autorizzazione univoci per le applicazioni cloud

    - [Guida alla sicurezza di Azure](https://azure.microsoft.com/blog/2014/02/10/best-practices-windows-azure-websites-waws/)
    - [Modelli e procedure Microsoft: informazioni aggiuntive su Azure](https://msdn.microsoft.com/library/dn568099.aspx). Vedere modello di gatekeeper, modello di identità federata.
    - [Sicurezza di rete di Azure](https://download.microsoft.com/download/4/3/9/43902EC9-410E-4875-8800-0788BE146A3D/Windows%20Azure%20Network%20Security%20Whitepaper%20-%20FINAL.docx). White paper di Ashin Pecchia.

Vedere anche modelli di cloud computing aggiuntivi e linee guida in [Microsoft Patterns and Practices-Azure guidance](https://msdn.microsoft.com/library/dn568099.aspx).

<a id="resources"></a>
## <a name="resources"></a>Risorse

Ognuno dei capitoli di questo e-book fornisce collegamenti alle risorse per ulteriori informazioni su tale argomento specifico. L'elenco seguente contiene i collegamenti alle panoramiche delle procedure consigliate e ai modelli consigliati per lo sviluppo nel cloud con Azure.

Documentation

- [Procedure consigliate per la progettazione di servizi su larga scala nei servizi cloud di Azure](https://msdn.microsoft.com/library/windowsazure/jj717232.aspx). White paper di Mark Simms e Michael Thomassy.
- [Failsafe: linee guida per architetture cloud resilienti](https://msdn.microsoft.com/library/windowsazure/jj853352.aspx). White paper di Marc Mercuri, Ulrich Homann e Andrew Townhill. Versione di pagina Web della serie di video FailSafe.
- [Informazioni aggiuntive su Azure](https://azure.microsoft.com/develop/net/guidance/) Pagina del portale per la documentazione ufficiale relativa allo sviluppo di applicazioni per Azure.

Videos

- [Creazione di app Cloud nel mondo reale con Azure-parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324) e [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325). Video della presentazione di Scott Guthrie su cui si basa questo e-book. Presentata presso Tech ed Australia nel settembre 2013. Una versione precedente della stessa presentazione è stata recapitata a Norwegian Developers Conference (NDC) nel giugno 2013: [NDC parte 1](http://vimeo.com/68215538), [NDC parte 2](http://vimeo.com/68215602).
- [Failsafe: compilazione di servizi cloud scalabili e resilienti](https://channel9.msdn.com/Series/FailSafe). Serie di video in nove parti di Ulrich Homann, Marc Mercuri e Mark Simms. Viene presentata una visualizzazione a livello di 400 di come progettare app cloud. Questa serie è incentrata sulla teoria e sui motivi alla base degli schemi consigliati; per altre procedure dettagliate, vedere la pagina relativa alla creazione di grandi serie di Mark Simms.
- [Creazione di grandi dimensioni: lezioni apprese dai clienti di Azure-parte 1](https://channel9.msdn.com/Events/Build/2012/3-029) e [parte 2](https://channel9.msdn.com/Events/Build/2012/3-030). Serie di video in due parti di Simon Davies e Mark Simms, analogamente alla serie FailSafe ma orientata maggiormente all'implementazione pratica.

Esempio di codice

- [L'applicazione di correzione it che accompagna questo e-book](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4?cdn_id=2013-12-03-002).
- [Nozioni fondamentali sui servizi cloud in C# Azure in per Visual Studio 2012](https://aka.ms/csf). Il progetto scaricabile nel sito della raccolta di codici Microsoft include sia il codice che la documentazione sviluppati dal team di consulenza clienti Microsoft (CAT). Vengono illustrate molte delle procedure consigliate per il FailSafe e la creazione di serie di video di grandi dimensioni e il white paper FailSafe. Nella pagina della raccolta di codici sono inoltre disponibili collegamenti a una documentazione completa da parte degli autori del progetto. vedere in particolare il collegamento alla [raccolta wiki dei concetti fondamentali del servizio cloud](https://social.technet.microsoft.com/wiki/contents/articles/17987.cloud-service-fundamentals.aspx) nella casella blu nella parte superiore della descrizione del progetto. Questo progetto e la relativa documentazione è ancora in fase di sviluppo, rendendola una scelta migliore per le informazioni su molti argomenti rispetto ai white paper più vecchi.

Libri cartacei

- [Bibbia sul cloud computing](https://www.amazon.com/dp/0470903562). Di Barrie Sosinsky.
- [Rilascia! progettare e distribuire software pronto per la produzione](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Di Michael T. Nygard.
- [Modelli di architettura cloud: uso di Microsoft Azure](http://shop.oreilly.com/product/0636920023777.do). Di Bill Wilder.
- [Piattaforma Windows Azure](https://www.amazon.com/dp/1430235632). Di Tejaswi Redkar.
- [Modelli di programmazione di Windows Azure per start-up](https://www.amazon.com/dp/1849685606). Di Riccardo Becker.
- [Cookbook per lo sviluppo per Microsoft Windows Azure](https://www.amazon.com/dp/1849682224). Di Neil Mackenzie.

Infine, quando si inizia a creare app reali ed eseguirle in Azure, prima o dopo sarà probabilmente necessario assistenza da esperti. È possibile porre domande nei siti della community, ad esempio [Forum di Azure o StackOverflow](https://azure.microsoft.com/support/forums/), oppure contattare direttamente Microsoft per il supporto tecnico di Azure. Microsoft offre diversi livelli di supporto tecnico di Azure: per un riepilogo e un confronto delle opzioni, vedere [supporto di Azure](https://azure.microsoft.com/support/plans/).

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>Ringraziamenti

Questo contenuto è stato scritto da Tom Dykstra, Rick Anderson e Mike Wasson. La maggior parte del contenuto originale proviene da [Scott Guthrie](https://weblogs.asp.net/scottgu/)e ha a sua volta tratto dal materiale di Mark Simms e dal team di consulenza clienti Microsoft (cat).

Molti altri colleghi Microsoft hanno esaminato e commentato le bozze e il codice:

- Tim Ammann-revisione del capitolo relativo all'automazione.
- Christopher Bennage-ha esaminato e testato il codice di correzione it.
- Ryan Berry-revisione del capitolo CD/CI.
- Vittorio Bertocci-revisione del capitolo SSO.
- Chris Clayton-ha aiutato a risolvere i problemi tecnici negli script di PowerShell.
- Conor Cunningham-revisione del capitolo opzioni di archiviazione dati.
- Carlos Farre: ha esaminato e testato il codice per risolvere i problemi di sicurezza.
- Larry Franks-revisione del capitolo telemetria e monitoraggio.
- Jonathan Gao-sezioni rivedute di Hadoop e MapReduce del capitolo opzioni di archiviazione dati.
- Sidney Higa-rivisto tutti i capitoli.
- Gordon Hogenson-revisione del capitolo del controllo del codice sorgente.
- Tamra Myers: sono stati rivisti i capitoli opzioni, BLOB e code di archiviazione dati.
- Rastogi di recapito SSO-revisione del capitolo SSO.
- June Blender Rogers-ha aggiunto la gestione degli errori e la guida per gli script di automazione di PowerShell.
- Mani per la revisione del codice e il processo di test per la correzione del codice it.
- Shaun Tinline-Jones: è stato esaminato il capitolo sul partizionamento dei dati.
- Selcin Tukarslan-sono stati rivisti i capitoli che coprono il database SQL e SQL Server.
- Edward Wu ha fornito il codice di esempio per il capitolo SSO.
- Guang Yang: sono stati scritti gli script di automazione di PowerShell.

I membri di [Microsoft Developer guidance Advisory Council](https://aka.ms/DGAC) (DGAC) hanno anche esaminato e commentato le bozze:

- Jean-Luc Boucho
- Gheorghiu catalin
- Stato di una mappa.
- Carlos Dos Santos
- Neil Mackenzie
- Dennis Persson
- Sunil Sabat
- [Sinyagin di Aleksey](http://www.linkedin.com/in/sinyagin)
- Fattura Wagner
- Michael Wood

Altri membri di DGAC hanno esaminato e commentato il contorno preliminare:

- Damir ARH
- Di Edward Bakker
- Bozovic di questo
- Ming Man Chan
- Gianni Rosa Gallina
- Paulo Morgado
- Jason Oliveira
- Alberto Poblacion
- Ryan Riley
- Perez Jones Tsisah
- Roger Whitehead
- Wilkosz come animatore Pawel

> [!div class="step-by-step"]
> [Precedente](queue-centric-work-pattern.md)
> [Successivo](the-fix-it-sample-application.md)
