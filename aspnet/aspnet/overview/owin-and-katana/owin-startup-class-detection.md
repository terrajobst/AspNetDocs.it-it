---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: Rilevamento della classe di avvio OWIN | Microsoft Docs
author: Praburaj
description: Questa esercitazione illustra come configurare la classe di avvio OWIN caricata. Per altre informazioni su OWIN, vedere una panoramica del progetto Katana. Questa esercitazione è stata...
ms.author: riande
ms.date: 01/28/2019
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: e1670c32049ec33dd4d1941a091a429d3929c452
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78617047"
---
# <a name="owin-startup-class-detection"></a>Rilevamento della classe di avvio OWIN

> Questa esercitazione illustra come configurare la classe di avvio OWIN caricata. Per altre informazioni su OWIN, vedere [una panoramica del progetto Katana](an-overview-of-project-katana.md). Questa esercitazione è stata scritta da Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), bibanunicu Lorenzo e Howard Dierking ( [@howard\_Dierking](https://twitter.com/howard_dierking) ).
>
> ## <a name="prerequisites"></a>Prerequisiti
>
> [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)

## <a name="owin-startup-class-detection"></a>Rilevamento della classe di avvio OWIN

 Ogni applicazione OWIN dispone di una classe startup in cui è possibile specificare i componenti per la pipeline dell'applicazione. Esistono diversi modi in cui è possibile connettere la classe di avvio al runtime, a seconda del modello di hosting scelto (OwinHost, IIS e IIS-Express). La classe Startup illustrata in questa esercitazione può essere utilizzata in tutte le applicazioni di hosting. È possibile connettere la classe Startup al runtime host usando uno degli approcci seguenti:

1. **Convenzione di denominazione**: la Katana Cerca una classe denominata `Startup` nello spazio dei nomi corrispondente al nome dell'assembly o allo spazio dei nomi globale.
2. **Attributo OwinStartup**: questo è l'approccio che la maggior parte degli sviluppatori importerà per specificare la classe Startup. L'attributo seguente consente di impostare la classe Startup sulla classe `TestStartup` nello spazio dei nomi `StartupDemo`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

   L'attributo `OwinStartup` sostituisce la convenzione di denominazione. È anche possibile specificare un nome descrittivo con questo attributo, tuttavia, se si usa un nome descrittivo, è necessario usare anche l'elemento `appSetting` nel file di configurazione.
3. **Elemento appSetting nel file di configurazione**: l'elemento `appSetting` esegue l'override dell'attributo `OwinStartup` e della convenzione di denominazione. È possibile avere più classi di avvio (ognuna con un attributo `OwinStartup`) e configurare la classe di avvio che verrà caricata in un file di configurazione usando un markup simile al seguente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

   La chiave seguente, che specifica in modo esplicito la classe di avvio e l'assembly, può essere usata anche:

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

   Il codice XML seguente nel file di configurazione specifica un nome di classe di avvio descrittivo di `ProductionConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

   Il markup precedente deve essere usato con l'attributo `OwinStartup` seguente che specifica un nome descrittivo e fa in modo che la classe `ProductionStartup2` venga eseguita.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. Per disabilitare OWIN Startup Discovery, aggiungere la `appSetting owin:AutomaticAppStartup` con un valore `"false"` nel file Web. config.

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a>Creare un'app Web ASP.NET con l'avvio di OWIN

1. Creare un'applicazione Web Asp.Net vuota e denominarla **StartupDemo**. -Installare `Microsoft.Owin.Host.SystemWeb` usando Gestione pacchetti NuGet. Dal menu **strumenti** selezionare **Gestione pacchetti NuGet**e quindi **console di gestione pacchetti**. Immettere il comando seguente:

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
2. Aggiungere una classe di avvio OWIN. In Visual Studio 2017 fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi classe**. nella finestra di dialogo **Aggiungi nuovo elemento** immettere *OWIN* nel campo di ricerca e modificare il nome in startup.cs e quindi selezionare **Aggiungi**.

     ![](owin-startup-class-detection/_static/image1.png)

   La volta successiva che si desidera aggiungere una *classe di avvio Owin*, questa sarà disponibile dal menu **Aggiungi** .

     ![](owin-startup-class-detection/_static/image2.png)

   In alternativa, è possibile fare clic con il pulsante destro del mouse sul progetto e scegliere **Aggiungi**, quindi selezionare **nuovo elemento**e quindi selezionare la **classe di avvio Owin**.

     ![](owin-startup-class-detection/_static/image3.png)

- Sostituire il codice generato nel file *Startup.cs* con il codice seguente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]

  L'espressione lambda `app.Use` viene utilizzata per registrare il componente middleware specificato nella pipeline OWIN. In questo caso viene configurata la registrazione delle richieste in ingresso prima di rispondere alla richiesta in ingresso. Il `next` parametro è il delegato ( [Func](https://msdn.microsoft.com/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/library/dd321424(v=vs.100).aspx) &gt;) al componente successivo nella pipeline. L'espressione lambda `app.Run` associa la pipeline alle richieste in ingresso e fornisce il meccanismo di risposta.
     > [!NOTE]
     > Nel codice precedente è stato impostato come commento l'attributo `OwinStartup` e si basa sulla convenzione di esecuzione della classe denominata `Startup`.-Premere ***F5*** per eseguire l'applicazione. Fare clic su Aggiorna alcune volte.

    ![](owin-startup-class-detection/_static/image4.png) Nota: il numero visualizzato nelle immagini in questa esercitazione non corrisponderà al numero visualizzato. La stringa in millisecondi viene utilizzata per visualizzare una nuova risposta quando si aggiorna la pagina.
  È possibile visualizzare le informazioni di traccia nella finestra **output** .

    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a>Aggiungi altre classi di avvio

In questa sezione verrà aggiunta un'altra classe Startup. È possibile aggiungere più classe di avvio OWIN all'applicazione. È possibile, ad esempio, creare classi di avvio per lo sviluppo, il test e la produzione.

1. Creare una nuova classe di avvio OWIN e denominarla `ProductionStartup`.
2. Sostituire il codice generato con il seguente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. Premere CTRL F5 per eseguire l'app. L'attributo `OwinStartup` specifica che la classe Startup di produzione viene eseguita.

    ![](owin-startup-class-detection/_static/image6.png)
4. Creare un'altra classe di avvio OWIN e denominarla `TestStartup`.
5. Sostituire il codice generato con il seguente:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

   L'overload dell'attributo `OwinStartup` precedente specifica `TestingConfiguration` come nome *descrittivo* della classe Startup.
6. Aprire il file *Web. config* e aggiungere la chiave di avvio dell'app OWIN che specifica il nome descrittivo della classe Startup:

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. Premere CTRL F5 per eseguire l'app. L'elemento impostazioni app ha la precedenza e viene eseguita la configurazione di test.

    ![](owin-startup-class-detection/_static/image7.png)
8. Rimuovere il nome *descrittivo* dall'attributo `OwinStartup` nella classe `TestStartup`.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. Sostituire la chiave di avvio dell'app OWIN nel file *Web. config* con il codice seguente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. Ripristinare l'attributo `OwinStartup` in ogni classe al codice dell'attributo predefinito generato da Visual Studio:

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

    Ognuna delle chiavi di avvio dell'app OWIN riportata di seguito farà sì che la classe di produzione venga eseguita.

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

    L'ultima chiave di avvio specifica il metodo di configurazione dell'avvio. La chiave di avvio dell'app OWIN seguente consente di modificare il nome della classe di configurazione in `MyConfiguration`.

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a>Utilizzo di Owinhost. exe

1. Sostituire il file Web. config con il markup seguente:

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

   L'ultima chiave prevale, quindi in questo caso `TestStartup` viene specificato.
2. Installare Owinhost dalla console di gestione pacchetti:

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. Passare alla cartella dell'applicazione (la cartella contenente il file *Web. config* ) e al prompt dei comandi e digitare:

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

   Nella finestra di comando sarà visualizzato:

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. Avviare un browser con l'URL `http://localhost:5000/`.

    ![](owin-startup-class-detection/_static/image8.png)

   OwinHost ha rispettato le convenzioni di avvio sopra elencate.
5. Nella finestra di comando premere INVIO per uscire da OwinHost.
6. Nella classe `ProductionStartup` aggiungere il seguente attributo OwinStartup che specifica un nome descrittivo di *ProductionConfiguration*.

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. Nel prompt dei comandi e digitare:

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

   Viene caricata la classe di avvio Production.
    ![](owin-startup-class-detection/_static/image9.png)

   L'applicazione dispone di più classi di avvio e in questo esempio è stata posticipata la classe di avvio da caricare fino al runtime.
8. Testare le seguenti opzioni di avvio di runtime:

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
