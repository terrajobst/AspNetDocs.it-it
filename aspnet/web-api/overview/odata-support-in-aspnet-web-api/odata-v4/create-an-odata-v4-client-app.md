---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
title: Creare un'app client OData V4 (C#) | Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 06/26/2014
ms.assetid: 47202362-3808-4add-9a69-c9d1f91d5e4e
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-client-app
msc.type: authoredcontent
ms.openlocfilehash: a0016cf2cc7bffe6268664395ccb38e140090310
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/06/2020
ms.locfileid: "78556294"
---
# <a name="create-an-odata-v4-client-app-c"></a><span data-ttu-id="68335-102">Creare un'app client OData v4 (C#)</span><span class="sxs-lookup"><span data-stu-id="68335-102">Create an OData v4 Client App (C#)</span></span>

<span data-ttu-id="68335-103">di [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="68335-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="68335-104">Nell'esercitazione precedente è stato creato un servizio OData di base che supporta operazioni CRUD.</span><span class="sxs-lookup"><span data-stu-id="68335-104">In the previous tutorial, you created a basic OData service that supports CRUD operations.</span></span> <span data-ttu-id="68335-105">A questo punto è possibile creare un client per il servizio.</span><span class="sxs-lookup"><span data-stu-id="68335-105">Now let's create a client for the service.</span></span>

<span data-ttu-id="68335-106">Avviare una nuova istanza di Visual Studio e creare un nuovo progetto di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="68335-106">Start a new instance of Visual Studio and create a new console application project.</span></span> <span data-ttu-id="68335-107">Nella finestra di dialogo **nuovo progetto** selezionare **installato** &gt; **modelli** &gt; **Visual C#**  &gt; **desktop di Windows**e selezionare il modello **applicazione console** .</span><span class="sxs-lookup"><span data-stu-id="68335-107">In the **New Project** dialog, select **Installed** &gt; **Templates** &gt; **Visual C#** &gt; **Windows Desktop**, and select the **Console Application** template.</span></span> <span data-ttu-id="68335-108">Denominare il progetto &quot;ProductsApp&quot;.</span><span class="sxs-lookup"><span data-stu-id="68335-108">Name the project &quot;ProductsApp&quot;.</span></span>

![](create-an-odata-v4-client-app/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="68335-109">È anche possibile aggiungere l'app console alla stessa soluzione di Visual Studio che contiene il servizio OData.</span><span class="sxs-lookup"><span data-stu-id="68335-109">You can also add the console app to the same Visual Studio solution that contains the OData service.</span></span>

## <a name="install-the-odata-client-code-generator"></a><span data-ttu-id="68335-110">Installare il generatore di codice client OData</span><span class="sxs-lookup"><span data-stu-id="68335-110">Install the OData Client Code Generator</span></span>

<span data-ttu-id="68335-111">Scegliere **Estensioni e aggiornamenti** dal menu **Strumenti**.</span><span class="sxs-lookup"><span data-stu-id="68335-111">From the **Tools** menu, select **Extensions and Updates**.</span></span> <span data-ttu-id="68335-112">Selezionare **Online** &gt; **Visual Studio Gallery**.</span><span class="sxs-lookup"><span data-stu-id="68335-112">Select **Online** &gt; **Visual Studio Gallery**.</span></span> <span data-ttu-id="68335-113">Nella casella di ricerca cercare &quot;&quot;generatore di codice client OData.</span><span class="sxs-lookup"><span data-stu-id="68335-113">In the search box, search for &quot;OData Client Code Generator&quot;.</span></span> <span data-ttu-id="68335-114">Fare clic su **download** per installare il progetto VSIX.</span><span class="sxs-lookup"><span data-stu-id="68335-114">Click **Download** to install the VSIX.</span></span> <span data-ttu-id="68335-115">Potrebbe essere richiesto di riavviare Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68335-115">You might be prompted to restart Visual Studio.</span></span>

[![](create-an-odata-v4-client-app/_static/image3.png)](create-an-odata-v4-client-app/_static/image2.png)

## <a name="run-the-odata-service-locally"></a><span data-ttu-id="68335-116">Eseguire il servizio OData localmente</span><span class="sxs-lookup"><span data-stu-id="68335-116">Run the OData Service Locally</span></span>

<span data-ttu-id="68335-117">Eseguire il progetto ProductService da Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="68335-117">Run the ProductService project from Visual Studio.</span></span> <span data-ttu-id="68335-118">Per impostazione predefinita, Visual Studio avvia un browser per la radice dell'applicazione.</span><span class="sxs-lookup"><span data-stu-id="68335-118">By default, Visual Studio launches a browser to the application root.</span></span> <span data-ttu-id="68335-119">Prendere nota dell'URI. Questa operazione sarà necessaria nel passaggio successivo.</span><span class="sxs-lookup"><span data-stu-id="68335-119">Note the URI; you will need this in the next step.</span></span> <span data-ttu-id="68335-120">Lasciare l'applicazione in esecuzione.</span><span class="sxs-lookup"><span data-stu-id="68335-120">Leave the application running.</span></span>

![](create-an-odata-v4-client-app/_static/image4.png)

> [!NOTE]
> <span data-ttu-id="68335-121">Se si inseriscono entrambi i progetti nella stessa soluzione, assicurarsi di eseguire il progetto ProductService senza debug.</span><span class="sxs-lookup"><span data-stu-id="68335-121">If you put both projects in the same solution, make sure to run the ProductService project without debugging.</span></span> <span data-ttu-id="68335-122">Nel passaggio successivo sarà necessario eseguire il servizio durante la modifica del progetto di applicazione console.</span><span class="sxs-lookup"><span data-stu-id="68335-122">In the next step, you will need to keep the service running while you modify the console application project.</span></span>

## <a name="generate-the-service-proxy"></a><span data-ttu-id="68335-123">Generazione del proxy del servizio</span><span class="sxs-lookup"><span data-stu-id="68335-123">Generate the Service Proxy</span></span>

<span data-ttu-id="68335-124">Il proxy del servizio è una classe .NET che definisce i metodi per accedere al servizio OData.</span><span class="sxs-lookup"><span data-stu-id="68335-124">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="68335-125">Il proxy converte le chiamate al metodo nelle richieste HTTP.</span><span class="sxs-lookup"><span data-stu-id="68335-125">The proxy translates method calls into HTTP requests.</span></span> <span data-ttu-id="68335-126">Si creerà la classe proxy eseguendo un [modello T4](https://msdn.microsoft.com/library/bb126445.aspx).</span><span class="sxs-lookup"><span data-stu-id="68335-126">You will create the proxy class by running a [T4 template](https://msdn.microsoft.com/library/bb126445.aspx).</span></span>

<span data-ttu-id="68335-127">Fare clic con il pulsante destro del mouse sul progetto.</span><span class="sxs-lookup"><span data-stu-id="68335-127">Right-click the project.</span></span> <span data-ttu-id="68335-128">Selezionare **aggiungi** &gt; **nuovo elemento**.</span><span class="sxs-lookup"><span data-stu-id="68335-128">Select **Add** &gt; **New Item**.</span></span>

![](create-an-odata-v4-client-app/_static/image5.png)

<span data-ttu-id="68335-129">Nella finestra di dialogo **Aggiungi nuovo elemento** selezionare **elementi C# visivi** &gt; **codice** &gt; **client OData**.</span><span class="sxs-lookup"><span data-stu-id="68335-129">In the **Add New Item** dialog, select **Visual C# Items** &gt; **Code** &gt; **OData Client**.</span></span> <span data-ttu-id="68335-130">Denominare il modello &quot;ProductClient.tt&quot;.</span><span class="sxs-lookup"><span data-stu-id="68335-130">Name the template &quot;ProductClient.tt&quot;.</span></span> <span data-ttu-id="68335-131">Fare clic su **Aggiungi** e quindi sull'avviso di sicurezza.</span><span class="sxs-lookup"><span data-stu-id="68335-131">Click **Add** and click through the security warning.</span></span>

[![](create-an-odata-v4-client-app/_static/image7.png)](create-an-odata-v4-client-app/_static/image6.png)

<span data-ttu-id="68335-132">A questo punto, si riceverà un errore, che può essere ignorato.</span><span class="sxs-lookup"><span data-stu-id="68335-132">At this point, you'll get an error, which you can ignore.</span></span> <span data-ttu-id="68335-133">Visual Studio esegue automaticamente il modello, ma il modello richiede prima alcune impostazioni di configurazione.</span><span class="sxs-lookup"><span data-stu-id="68335-133">Visual Studio automatically runs the template, but the template needs some configuration settings first.</span></span>

[![](create-an-odata-v4-client-app/_static/image9.png)](create-an-odata-v4-client-app/_static/image8.png)

<span data-ttu-id="68335-134">Aprire il file ProductClient. OData. config. Nell'elemento `Parameter` incollare l'URI dal progetto ProductService (passaggio precedente).</span><span class="sxs-lookup"><span data-stu-id="68335-134">Open the file ProductClient.odata.config. In the `Parameter` element, paste in the URI from the ProductService project (previous step).</span></span> <span data-ttu-id="68335-135">Esempio:</span><span class="sxs-lookup"><span data-stu-id="68335-135">For example:</span></span>

[!code-xml[Main](create-an-odata-v4-client-app/samples/sample1.xml)]

[![](create-an-odata-v4-client-app/_static/image11.png)](create-an-odata-v4-client-app/_static/image10.png)

<span data-ttu-id="68335-136">Eseguire nuovamente il modello.</span><span class="sxs-lookup"><span data-stu-id="68335-136">Run the template again.</span></span> <span data-ttu-id="68335-137">In Esplora soluzioni, fare clic con il pulsante destro del mouse sul file ProductClient.tt e scegliere **Esegui strumento personalizzato**.</span><span class="sxs-lookup"><span data-stu-id="68335-137">In Solution Explorer, right click the ProductClient.tt file and select **Run Custom Tool**.</span></span>

<span data-ttu-id="68335-138">Il modello crea un file di codice denominato ProductClient.cs che definisce il proxy.</span><span class="sxs-lookup"><span data-stu-id="68335-138">The template creates a code file named ProductClient.cs that defines the proxy.</span></span> <span data-ttu-id="68335-139">Quando si sviluppa l'app, se si modifica l'endpoint OData, eseguire di nuovo il modello per aggiornare il proxy.</span><span class="sxs-lookup"><span data-stu-id="68335-139">As you develop your app, if you change the OData endpoint, run the template again to update the proxy.</span></span>

![](create-an-odata-v4-client-app/_static/image12.png)

## <a name="use-the-service-proxy-to-call-the-odata-service"></a><span data-ttu-id="68335-140">Usare il proxy del servizio per chiamare il servizio OData</span><span class="sxs-lookup"><span data-stu-id="68335-140">Use the Service Proxy to Call the OData Service</span></span>

<span data-ttu-id="68335-141">Aprire il file Program.cs e sostituire il codice standard con il codice seguente.</span><span class="sxs-lookup"><span data-stu-id="68335-141">Open the file Program.cs and replace the boilerplate code with the following.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample2.cs)]

<span data-ttu-id="68335-142">Sostituire il valore di *ServiceUri* con l'URI del servizio precedente.</span><span class="sxs-lookup"><span data-stu-id="68335-142">Replace the value of *serviceUri* with the service URI from earlier.</span></span>

[!code-csharp[Main](create-an-odata-v4-client-app/samples/sample3.cs)]

<span data-ttu-id="68335-143">Quando si esegue l'app, deve restituire quanto segue:</span><span class="sxs-lookup"><span data-stu-id="68335-143">When you run the app, it should output the following:</span></span>

[!code-console[Main](create-an-odata-v4-client-app/samples/sample4.cmd)]
