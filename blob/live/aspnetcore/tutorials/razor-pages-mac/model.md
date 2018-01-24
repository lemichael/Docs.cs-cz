---
title: "Přidání modelu do aplikace pro stránky Razor pomocí sady Visual Studio pro Mac"
author: rick-anderson
description: "Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 7b1b2d54e9c68b0a6f2b1355726d0d1cb484f69e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="876dd-103">Přidání modelu do aplikace stránky Razor ve ASP.NET Core pomocí sady Visual Studio pro Mac</span><span class="sxs-lookup"><span data-stu-id="876dd-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="876dd-104">Přidat datový model</span><span class="sxs-lookup"><span data-stu-id="876dd-104">Add a data model</span></span>

* <span data-ttu-id="876dd-105">V Průzkumníku řešení klikněte pravým tlačítkem myši **RazorPagesMovie** projektu a potom vyberte **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="876dd-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="876dd-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="876dd-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="876dd-107">Klikněte pravým tlačítkem myši *modely* složku a potom vyberte **přidat** > **nový soubor**.</span><span class="sxs-lookup"><span data-stu-id="876dd-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="876dd-108">V **nový soubor** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="876dd-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="876dd-109">Vyberte **Obecné** v levém podokně.</span><span class="sxs-lookup"><span data-stu-id="876dd-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="876dd-110">Vyberte **prázdné třídy** v problémové center.</span><span class="sxs-lookup"><span data-stu-id="876dd-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="876dd-111">Název třídy **film** a vyberte **nový**.</span><span class="sxs-lookup"><span data-stu-id="876dd-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="876dd-112">Klikněte pravým tlačítkem na červenou vlnovkou řádek, například `MovieContext` v řádku `services.AddDbContext<MovieContext>(options =>`.</span><span class="sxs-lookup"><span data-stu-id="876dd-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="876dd-113">Vyberte **Quick Fix > pomocí RazorPagesMovie.Models;**.</span><span class="sxs-lookup"><span data-stu-id="876dd-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="876dd-114">Visual studio. přidá na pomocí příkazu.</span><span class="sxs-lookup"><span data-stu-id="876dd-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="876dd-115">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="876dd-115">Build the project to verify you don't have any errors.</span></span>

![Vytvoření stránky](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="876dd-117">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="876dd-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="876dd-118">Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="876dd-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="876dd-119">K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="876dd-119">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="876dd-120">**Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="876dd-120">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="876dd-121">Chcete-li upravit *.csproj* souboru:</span><span class="sxs-lookup"><span data-stu-id="876dd-121">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="876dd-122">Vyberte **soubor** > **otevřete**a pak vyberte *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="876dd-122">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="876dd-123">Vyberte **možnosti**.</span><span class="sxs-lookup"><span data-stu-id="876dd-123">Select **Options**.</span></span>
* <span data-ttu-id="876dd-124">Změna **Otevřít protokolem** k **editoru zdrojového kódu**.</span><span class="sxs-lookup"><span data-stu-id="876dd-124">Change **Open with** to **Source Code Editor**.</span></span>

![Upravte soubor csproj](model/csproj.png)

<span data-ttu-id="876dd-126">Přidat `Microsoft.EntityFrameworkCore.Tools.DotNet` nástroj odkaz na druhý  **\<ItemGroup >**:</span><span class="sxs-lookup"><span data-stu-id="876dd-126">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="876dd-127">Do projektu přidejte tyto stránek nebo filmy soubory</span><span class="sxs-lookup"><span data-stu-id="876dd-127">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="876dd-128">V sadě Visual Studio, klikněte pravým tlačítkem myši *stránky* složky a vyberte **Přidat > Přidat existující složku**.</span><span class="sxs-lookup"><span data-stu-id="876dd-128">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="876dd-129">Vyberte *filmy* složky.</span><span class="sxs-lookup"><span data-stu-id="876dd-129">Select the *Movies* folder.</span></span>
* <span data-ttu-id="876dd-130">V *Chosse soubory pro zahrnutí do projektu* dialogovém okně, vyberte **zahrnují všechny**.</span><span class="sxs-lookup"><span data-stu-id="876dd-130">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="876dd-131">Další kurz vysvětluje souborů vytvořených pomocí generování uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="876dd-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="876dd-132">[Předchozí: Začínáme](xref:tutorials/razor-pages-mac/razor-pages-start)
[Další: vygeneroval stránky Razor](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="876dd-132">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>