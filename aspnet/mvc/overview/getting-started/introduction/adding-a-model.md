---
uid: mvc/overview/getting-started/introduction/adding-a-model
title: Aggiunta di un modello | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/28/2015
ms.assetid: 276552b5-f349-4fcf-8f40-6d042f7aa88e
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-model
msc.type: authoredcontent
ms.openlocfilehash: 0d926c7a8bd99c56820208921c10e609da56d236
ms.sourcegitcommit: 88fc80e3f65aebdf61ec9414810ddbc31c543f04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/22/2020
ms.locfileid: "76519037"
---
# <a name="adding-a-model"></a><span data-ttu-id="e4b4b-102">Aggiunta di un modello</span><span class="sxs-lookup"><span data-stu-id="e4b4b-102">Adding a Model</span></span>

<span data-ttu-id="e4b4b-103">di [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="e4b4b-103">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

[!INCLUDE [Tutorial Note](index.md)]

<span data-ttu-id="e4b4b-104">In questa sezione verranno aggiunte alcune classi per la gestione dei film in un database.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-104">In this section you'll add some classes for managing movies in a database.</span></span> <span data-ttu-id="e4b4b-105">Queste classi saranno il modello di &quot;&quot; parte dell'app MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-105">These classes will be the &quot;model&quot; part of the ASP.NET MVC app.</span></span>

<span data-ttu-id="e4b4b-106">Si utilizzerà una tecnologia di accesso ai dati .NET Framework nota come [Entity Framework](https://docs.microsoft.com/ef/) per definire e utilizzare queste classi di modelli.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-106">You'll use a .NET Framework data-access technology known as the [Entity Framework](https://docs.microsoft.com/ef/) to define and work with these model classes.</span></span> <span data-ttu-id="e4b4b-107">Il Entity Framework (spesso definito EF) supporta un paradigma di sviluppo denominato *Code First*.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-107">The Entity Framework (often referred to as EF) supports a development paradigm called *Code First*.</span></span> <span data-ttu-id="e4b4b-108">Code First consente di creare oggetti modello scrivendo classi semplici.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-108">Code First allows you to create model objects by writing simple classes.</span></span> <span data-ttu-id="e4b4b-109">Sono note anche come classi POCO, da oggetti CLR &quot;Plain Old.&quot;) È quindi possibile fare in modo che il database venga creato immediatamente dalle classi, che consente un flusso di lavoro di sviluppo molto pulito e rapido.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-109">(These are also known as POCO classes, from &quot;plain-old CLR objects.&quot;) You can then have the database created on the fly from your classes, which enables a very clean and rapid development workflow.</span></span> <span data-ttu-id="e4b4b-110">Se è necessario creare prima il database, è comunque possibile seguire questa esercitazione per informazioni sullo sviluppo di app MVC e EF.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-110">If you are required to create the database first, you can still follow this tutorial to learn about MVC and EF app development.</span></span> <span data-ttu-id="e4b4b-111">È quindi possibile seguire l'esercitazione sull' [impalcatura](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) di Tom Fizmakens ASP.NET, che illustra il primo approccio al database.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-111">You can then follow Tom Fizmakens [ASP.NET Scaffolding](xref:visual-studio/overview/2013/aspnet-scaffolding-overview) tutorial, which covers the database first approach.</span></span>

## <a name="adding-model-classes"></a><span data-ttu-id="e4b4b-112">Aggiunta di classi di modelli</span><span class="sxs-lookup"><span data-stu-id="e4b4b-112">Adding Model Classes</span></span>

<span data-ttu-id="e4b4b-113">In **Esplora soluzioni**, fare clic con il pulsante destro del mouse sulla cartella *modelli* , scegliere **Aggiungi**e quindi selezionare **classe**.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-113">In **Solution Explorer**, right click the *Models* folder, select **Add**, and then select **Class**.</span></span>

![](adding-a-model/_static/image1.png)

<span data-ttu-id="e4b4b-114">Immettere il nome della *classe* &quot;Movie&quot;.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-114">Enter the *class* name &quot;Movie&quot;.</span></span>

<span data-ttu-id="e4b4b-115">Aggiungere le cinque proprietà seguenti alla classe `Movie`:</span><span class="sxs-lookup"><span data-stu-id="e4b4b-115">Add the following five properties to the `Movie` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample1.cs)]

<span data-ttu-id="e4b4b-116">Si userà la classe `Movie` per rappresentare i film in un database.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-116">We'll use the `Movie` class to represent movies in a database.</span></span> <span data-ttu-id="e4b4b-117">Ogni istanza di un oggetto `Movie` corrisponderà a una riga all'interno di una tabella di database e ogni proprietà della classe `Movie` eseguirà il mapping a una colonna della tabella.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-117">Each instance of a `Movie` object will correspond to a row within a database table, and each property of the `Movie` class will map to a column in the table.</span></span>

<span data-ttu-id="e4b4b-118">Nota: per usare System. Data. Entity e la classe correlata, è necessario installare il [pacchetto NuGet Entity Framework](https://www.nuget.org/packages/EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="e4b4b-118">Note: In order to use System.Data.Entity, and the related class, you need to install the [Entity Framework NuGet Package](https://www.nuget.org/packages/EntityFramework/).</span></span> <span data-ttu-id="e4b4b-119">Per altre istruzioni, seguire il collegamento.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-119">Follow the link for further instructions.</span></span>

<span data-ttu-id="e4b4b-120">Nello stesso file aggiungere la classe `MovieDBContext` seguente:</span><span class="sxs-lookup"><span data-stu-id="e4b4b-120">In the same file, add the following `MovieDBContext` class:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample2.cs?highlight=2,15-18)]

<span data-ttu-id="e4b4b-121">La classe `MovieDBContext` rappresenta il contesto del database di Entity Framework Movie, che gestisce il recupero, l'archiviazione e l'aggiornamento delle istanze della classe `Movie` in un database.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-121">The `MovieDBContext` class represents the Entity Framework movie database context, which handles fetching, storing, and updating `Movie` class instances in a database.</span></span> <span data-ttu-id="e4b4b-122">Il `MovieDBContext` deriva dalla classe di base `DbContext` fornita dal Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-122">The `MovieDBContext` derives from the `DbContext` base class provided by the Entity Framework.</span></span>

<span data-ttu-id="e4b4b-123">Per poter fare riferimento `DbContext` e `DbSet`, è necessario aggiungere l'istruzione `using` seguente all'inizio del file:</span><span class="sxs-lookup"><span data-stu-id="e4b4b-123">In order to be able to reference `DbContext` and `DbSet`, you need to add the following `using` statement at the top of the file:</span></span>

[!code-csharp[Main](adding-a-model/samples/sample3.cs)]

<span data-ttu-id="e4b4b-124">È possibile eseguire questa operazione aggiungendo manualmente l'istruzione using oppure è possibile passare il mouse sulle righe rosse ondulate, fare clic su `Show potential fixes` e fare clic su `using System.Data.Entity;`</span><span class="sxs-lookup"><span data-stu-id="e4b4b-124">You can do this by manually adding the using statement, or you can hover over the red squiggly lines, click `Show potential fixes` and click `using System.Data.Entity;`</span></span>

![](adding-a-model/_static/image2.png)

<span data-ttu-id="e4b4b-125">Nota: sono state rimosse diverse istruzioni `using` inutilizzate.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-125">Note: Several unused `using` statements have been removed.</span></span> <span data-ttu-id="e4b4b-126">Visual Studio mostrerà le dipendenze inutilizzate come grigio.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-126">Visual Studio will show unused dependencies as gray.</span></span> <span data-ttu-id="e4b4b-127">È possibile rimuovere le dipendenze non utilizzate passando il mouse sulle dipendenze grigie, fare clic su `Show potential fixes` e quindi su **Rimuovi using inutilizzate.**</span><span class="sxs-lookup"><span data-stu-id="e4b4b-127">You can remove unused dependencies by hovering over the gray dependencies, click `Show potential fixes` and click **Remove Unused Usings.**</span></span>

![](adding-a-model/_static/image3.png)

<span data-ttu-id="e4b4b-128">È stato infine aggiunto un modello (il M in MVC).</span><span class="sxs-lookup"><span data-stu-id="e4b4b-128">We've finally added a model (the M in MVC).</span></span> <span data-ttu-id="e4b4b-129">Nella sezione successiva verrà utilizzata la stringa di connessione del database.</span><span class="sxs-lookup"><span data-stu-id="e4b4b-129">In the next section you'll work with the database connection string.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e4b4b-130">[Precedente](adding-a-view.md)
> [Successivo](creating-a-connection-string.md)</span><span class="sxs-lookup"><span data-stu-id="e4b4b-130">[Previous](adding-a-view.md)
[Next](creating-a-connection-string.md)</span></span>
