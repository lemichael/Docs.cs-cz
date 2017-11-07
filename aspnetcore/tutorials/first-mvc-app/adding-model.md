---
title: "Přidání modelu do aplikace ASP.NET MVC jádra"
author: rick-anderson
description: "Přidáte model do jednoduchou aplikaci ASP.NET Core."
keywords: "Jádro ASP.NET"
ms.author: riande
manager: wpickett
ms.date: 03/30/2017
ms.topic: get-started-article
ms.assetid: 8dc28498-00ee-4d66-b903-b593059e9f39
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/adding-model
ms.openlocfilehash: 7469546494ec54bfe36bc5bd2f5f9702889ddf4a
ms.sourcegitcommit: 2e61e287e220eddd5f3f4cd9147aa6417cfd9236
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/12/2017
---
[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model1.md)]

<span data-ttu-id="36ac4-104">Poznámka: Šablony ASP.NET 2.0 základní obsahují *modely* složky.</span><span class="sxs-lookup"><span data-stu-id="36ac4-104">Note: The ASP.NET Core 2.0 templates contain the *Models* folder.</span></span>

<span data-ttu-id="36ac4-105">V Průzkumníku řešení klikněte pravým tlačítkem **MvcMovie** Projekt > **přidat** > **novou složku**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-105">In Solution Explorer, right click the **MvcMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="36ac4-106">Název složky *modely*.</span><span class="sxs-lookup"><span data-stu-id="36ac4-106">Name the folder *Models*.</span></span>

<span data-ttu-id="36ac4-107">Klikněte pravým tlačítkem *modely* složky > **přidat** > **třída**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-107">Right click the *Models* folder > **Add** > **Class**.</span></span> <span data-ttu-id="36ac4-108">Název třídy **film** a přidejte následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="36ac4-108">Name the class **Movie** and add the following properties:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Models/MovieNoEF.cs?name=snippet_1)]

<span data-ttu-id="36ac4-109">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="36ac4-109">The `ID` field is required by the database for the primary key.</span></span> 

<span data-ttu-id="36ac4-110">Sestavte projekt a ověřte, že nemáte žádné chyby.</span><span class="sxs-lookup"><span data-stu-id="36ac4-110">Build the project to verify you don't have any errors.</span></span> <span data-ttu-id="36ac4-111">Nyní máte **M**odelu ve vaší **M**VC aplikace.</span><span class="sxs-lookup"><span data-stu-id="36ac4-111">You now have a **M**odel in your **M**VC app.</span></span>

## <a name="scaffolding-a-controller"></a><span data-ttu-id="36ac4-112">Řadič generování uživatelského rozhraní</span><span class="sxs-lookup"><span data-stu-id="36ac4-112">Scaffolding a controller</span></span>

<span data-ttu-id="36ac4-113">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > řadiče**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-113">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![zobrazení výše krok](adding-model/_static/add_controller.png)

<span data-ttu-id="36ac4-115">V **přidat závislosti MVC** dialogovém okně, vyberte **minimální závislosti**a vyberte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-115">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

![zobrazení výše krok](adding-model/_static/add_depend.png)

<span data-ttu-id="36ac4-117">Visual Studio přidává závislosti nutné chcete vygenerovat řadič, ale samotný řadič není vytvořena.</span><span class="sxs-lookup"><span data-stu-id="36ac4-117">Visual Studio adds the dependencies needed to scaffold a controller, but the controller itself is not created.</span></span> <span data-ttu-id="36ac4-118">Další vyvolat z **> Přidat > řadiče** vytvoří kontroler.</span><span class="sxs-lookup"><span data-stu-id="36ac4-118">The next invoke of **> Add > Controller** creates the controller.</span></span> 

<span data-ttu-id="36ac4-119">V **Průzkumníku řešení**, klikněte pravým tlačítkem myši *řadiče* složky **> Přidat > řadiče**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-119">In **Solution Explorer**, right-click the *Controllers* folder **> Add > Controller**.</span></span>

![zobrazení výše krok](adding-model/_static/add_controller.png)

<span data-ttu-id="36ac4-121">V **přidat vygenerované uživatelské rozhraní** dialogové okno, klepněte na **kontroler MVC se zobrazeními s využitím nástroje Entity Framework > Přidat**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-121">In the **Add Scaffold** dialog, tap **MVC Controller with views, using Entity Framework > Add**.</span></span>

![Přidat vygenerované uživatelské rozhraní – dialogové okno](adding-model/_static/add_scaffold2.png)

<span data-ttu-id="36ac4-123">Dokončení **přidat kontroler** dialogové okno:</span><span class="sxs-lookup"><span data-stu-id="36ac4-123">Complete the **Add Controller** dialog:</span></span>

* <span data-ttu-id="36ac4-124">**Třída modelu:** *Movie (MvcMovie.Models)*</span><span class="sxs-lookup"><span data-stu-id="36ac4-124">**Model class:** *Movie (MvcMovie.Models)*</span></span>
* <span data-ttu-id="36ac4-125">**Třída kontextu dat:** vyberte  **+**  ikonu a přidejte výchozí **MvcMovie.Models.MvcMovieContext**</span><span class="sxs-lookup"><span data-stu-id="36ac4-125">**Data context class:** Select the **+** icon and add the default **MvcMovie.Models.MvcMovieContext**</span></span>

![Přidat kontextu dat](adding-model/_static/dc.png)

* <span data-ttu-id="36ac4-127">**Zobrazení:** zachovat ve výchozím nastavení mají jednotlivé možnosti zaškrtnuto</span><span class="sxs-lookup"><span data-stu-id="36ac4-127">**Views:** Keep the default of each option checked</span></span>
* <span data-ttu-id="36ac4-128">**Název řadiče:** ponechat výchozí *MoviesController*</span><span class="sxs-lookup"><span data-stu-id="36ac4-128">**Controller name:** Keep the default *MoviesController*</span></span>
* <span data-ttu-id="36ac4-129">Klepněte na **přidat**</span><span class="sxs-lookup"><span data-stu-id="36ac4-129">Tap **Add**</span></span>

![Dialogové okno řadiče přidání](adding-model/_static/add_controller2.png)

<span data-ttu-id="36ac4-131">Visual Studio vytvoří:</span><span class="sxs-lookup"><span data-stu-id="36ac4-131">Visual Studio creates:</span></span>

* <span data-ttu-id="36ac4-132">Entity Framework Core [databáze třída kontextu](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span><span class="sxs-lookup"><span data-stu-id="36ac4-132">An Entity Framework Core [database context class](xref:data/ef-mvc/intro#create-the-database-context) (*Data/MvcMovieContext.cs*)</span></span>
* <span data-ttu-id="36ac4-133">Řadič filmy (*Controllers/MoviesController.cs*)</span><span class="sxs-lookup"><span data-stu-id="36ac4-133">A movies controller (*Controllers/MoviesController.cs*)</span></span>
* <span data-ttu-id="36ac4-134">Zobrazení syntaxe Razor soubory vytvořit, odstranit, podrobnosti, úpravy a Index stránky (*zobrazení nebo filmy nebo&ast;.cshtml*)</span><span class="sxs-lookup"><span data-stu-id="36ac4-134">Razor view files for Create, Delete, Details, Edit and Index pages (*Views/Movies/&ast;.cshtml*)</span></span>

<span data-ttu-id="36ac4-135">Automatické vytváření kontext databáze a [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (vytvářet, číst, aktualizovat a odstranit) metody akce a zobrazení se označuje jako *generování uživatelského rozhraní*.</span><span class="sxs-lookup"><span data-stu-id="36ac4-135">The automatic creation of the database context and [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) (create, read, update, and delete) action methods and views is known as *scaffolding*.</span></span> <span data-ttu-id="36ac4-136">Brzy budete mít funkční webovou aplikaci, která vám umožní spravovat film databáze.</span><span class="sxs-lookup"><span data-stu-id="36ac4-136">You'll soon have a fully functional web application that lets you manage a movie database.</span></span>

<span data-ttu-id="36ac4-137">Pokud spustíte aplikaci a klikněte na **Mvc film** odkazu, který budete dojde k chybě podobný následujícímu:</span><span class="sxs-lookup"><span data-stu-id="36ac4-137">If you run the app and click on the **Mvc Movie** link, you'll get an error similar to the following:</span></span>

```
An unhandled exception occurred while processing the request.

SqlException: Cannot open database "MvcMovieContext-<GUID removed>" requested by the login. The login failed.
Login failed for user 'Rick'.

System.Data.SqlClient.SqlInternalConnectionTds..ctor(DbConnectionPoolIdentity identity, SqlConnectionString 
```

<span data-ttu-id="36ac4-138">Musíte vytvořit databázi, a budete používat jádro EF [migrace](xref:data/ef-mvc/migrations) funkci tak, aby to udělat.</span><span class="sxs-lookup"><span data-stu-id="36ac4-138">You need to create the database, and you'll use the EF Core [Migrations](xref:data/ef-mvc/migrations) feature to do that.</span></span> <span data-ttu-id="36ac4-139">Migrace umožňuje vytvořit databázi, která odpovídá datový model a aktualizovat schéma databáze, když datový model změny.</span><span class="sxs-lookup"><span data-stu-id="36ac4-139">Migrations lets you create a database that matches your data model and update the database schema when your data model changes.</span></span>

## <a name="add-ef-tooling-and-perform-initial-migration"></a><span data-ttu-id="36ac4-140">Přidejte EF nástrojů a provedení počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="36ac4-140">Add EF tooling and perform initial migration</span></span>

<span data-ttu-id="36ac4-141">V této části pomocí konzoly Správce balíčků (PMC) na:</span><span class="sxs-lookup"><span data-stu-id="36ac4-141">In this section you'll use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="36ac4-142">Přidejte balíček základní nástroje Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="36ac4-142">Add the Entity Framework Core Tools package.</span></span> <span data-ttu-id="36ac4-143">Tento balíček je potřebné k přidání migrace a aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="36ac4-143">This package is required to add migrations and update the database.</span></span>
* <span data-ttu-id="36ac4-144">Přidáte počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="36ac4-144">Add an initial migration.</span></span>
* <span data-ttu-id="36ac4-145">Aktualizace databáze s počáteční migrace.</span><span class="sxs-lookup"><span data-stu-id="36ac4-145">Update the database with the initial migration.</span></span>

<span data-ttu-id="36ac4-146">Z **nástroje** nabídce vyberte možnost **Správce balíčků NuGet > Konzola správce balíčků**.</span><span class="sxs-lookup"><span data-stu-id="36ac4-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

<!-- following image shared with uid: tutorials/razor-pages/model -->
  ![Pomocí PMC nabídky](adding-model/_static/pmc.png)

<span data-ttu-id="36ac4-148">Pomocí PMC zadejte následující příkazy:</span><span class="sxs-lookup"><span data-stu-id="36ac4-148">In the PMC, enter the following commands:</span></span>

``` PMC
Install-Package Microsoft.EntityFrameworkCore.Tools
Add-Migration Initial
Update-Database
```

<span data-ttu-id="36ac4-149">**Poznámka:** Pokud obdržíte chybu s `Install-Package` příkaz, otevřete Správce balíčků NuGet a vyhledejte `Microsoft.EntityFrameworkCore.Tools` balíčku.</span><span class="sxs-lookup"><span data-stu-id="36ac4-149">**Note:** If you receive an error with the `Install-Package` command, open NuGet Package Manager and search for the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="36ac4-150">To vám umožňuje nainstalovat balíček nebo zkontrolujte, zda je již nainstalován.</span><span class="sxs-lookup"><span data-stu-id="36ac4-150">This allows you to install the package or check if it is already installed.</span></span> <span data-ttu-id="36ac4-151">Můžete si také přečíst [rozhraní příkazového řádku přístup](#cli) Pokud máte problémy s pomocí PMC.</span><span class="sxs-lookup"><span data-stu-id="36ac4-151">Alternatively, see the [CLI approach](#cli) if you have problems with the PMC.</span></span>

<span data-ttu-id="36ac4-152">`Add-Migration` Příkaz vytvoří kód k vytvoření schématu počáteční databáze.</span><span class="sxs-lookup"><span data-stu-id="36ac4-152">The `Add-Migration` command creates code to create the initial database schema.</span></span> <span data-ttu-id="36ac4-153">Schéma je založena na zadaný ve model `DbContext`(v *Data/MvcMovieContext.cs* souboru).</span><span class="sxs-lookup"><span data-stu-id="36ac4-153">The schema is based on the model specified in the `DbContext`(In the *Data/MvcMovieContext.cs* file).</span></span> <span data-ttu-id="36ac4-154">`Initial` Argument se používá k pojmenování byla migrace.</span><span class="sxs-lookup"><span data-stu-id="36ac4-154">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="36ac4-155">Můžete použít libovolný název, ale podle konvence zvolte název, který popisuje migraci.</span><span class="sxs-lookup"><span data-stu-id="36ac4-155">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="36ac4-156">V tématu [Úvod do migrace](xref:data/ef-mvc/migrations#introduction-to-migrations) Další informace.</span><span class="sxs-lookup"><span data-stu-id="36ac4-156">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="36ac4-157">`Update-Database` Příkaz spustí `Up` metoda v *migrace nebo\<časové razítko > _InitialCreate.cs* souboru, který vytvoří databázi.</span><span class="sxs-lookup"><span data-stu-id="36ac4-157">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

<span data-ttu-id="36ac4-158"><a name="cli"></a>Místo pomocí PMC můžete provést kroky předcházející pomocí rozhraní příkazového řádku (CLI):</span><span class="sxs-lookup"><span data-stu-id="36ac4-158"><a name="cli"></a> You can perform the preceeding steps using the command-line interface (CLI) rather than the PMC:</span></span>

* <span data-ttu-id="36ac4-159">Přidat [EF základní nástrojů](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) k *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="36ac4-159">Add [EF Core tooling](xref:data/ef-mvc/migrations#entity-framework-core-nuget-packages-for-migrations) to the *.csproj* file.</span></span>
* <span data-ttu-id="36ac4-160">Spusťte následující příkazy z konzoly (v adresáři projektu):</span><span class="sxs-lookup"><span data-stu-id="36ac4-160">Run the following commands from the console (in the project directory):</span></span>

  ```console
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```     
  

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model3.md)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?name=ConfigureServices&highlight=6-7)]

[!INCLUDE[adding-model](../../includes/mvc-intro/adding-model4.md)]

![Kontextové nabídky IntelliSense pro položku modelu, seznam dostupných vlastností pro ID, ceny, datum vydání a název](adding-model/_static/ints.png)

## <a name="additional-resources"></a><span data-ttu-id="36ac4-162">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="36ac4-162">Additional resources</span></span>

* [<span data-ttu-id="36ac4-163">Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="36ac4-163">Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="36ac4-164">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="36ac4-164">Globalization and localization</span></span>](xref:fundamentals/localization)

>[!div class="step-by-step"]
<span data-ttu-id="36ac4-165">[Předchozí přidávání zobrazení](adding-view.md)
[další práci s SQL](working-with-sql.md)</span><span class="sxs-lookup"><span data-stu-id="36ac4-165">[Previous Adding a View](adding-view.md)
[Next Working with SQL](working-with-sql.md)</span></span>  