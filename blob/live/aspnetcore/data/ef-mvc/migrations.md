---
title: "Jádro ASP.NET MVC s EF Core - Migrations - 4 10"
author: tdykstra
description: "V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů v aplikaci MVC rozhraní ASP.NET Core."
keywords: ASP.NET Core Entity Framework Core, migrace
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 20b05801ac666feef29fd05dd3e4738b1bd50b86
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="32cdf-104">Migrace – základní EF s kurz k ASP.NET MVC jádra (4 10)</span><span class="sxs-lookup"><span data-stu-id="32cdf-104">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="32cdf-105">Podle [tní Dykstra](https://github.com/tdykstra) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="32cdf-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="32cdf-106">Contoso univerzity ukázkovou webovou aplikaci ukazuje, jak vytvářet webové aplikace ASP.NET MVC základní pomocí Entity Framework Core a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="32cdf-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="32cdf-107">Informace o kurzu řady najdete v tématu [z prvního kurzu řady](intro.md).</span><span class="sxs-lookup"><span data-stu-id="32cdf-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="32cdf-108">V tomto kurzu začnete používat funkci migrace EF jádra pro správu změn datových modelů.</span><span class="sxs-lookup"><span data-stu-id="32cdf-108">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="32cdf-109">V dalších kurzech přidáte více migrací mění datový model.</span><span class="sxs-lookup"><span data-stu-id="32cdf-109">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="32cdf-110">Úvod do migrace</span><span class="sxs-lookup"><span data-stu-id="32cdf-110">Introduction to migrations</span></span>

<span data-ttu-id="32cdf-111">Při vývoji nových aplikací datový model změní často a pokaždé, když změn modelu, získá synchronizována s databází.</span><span class="sxs-lookup"><span data-stu-id="32cdf-111">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="32cdf-112">Tyto kurzy jste spustili nakonfigurováním rozhraní Entity Framework pro vytvoření databáze, pokud neexistuje.</span><span class="sxs-lookup"><span data-stu-id="32cdf-112">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="32cdf-113">Pokaždé, když změníte datový model – přidat, odebrat, nebo změňte tříd entit nebo změnit vaší třídy DbContext – pak můžete odstranit databázi a EF vytvoří novou, který odpovídá modelu a doplňuje s testovacích datech.</span><span class="sxs-lookup"><span data-stu-id="32cdf-113">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="32cdf-114">Udržování databáze synchronizace s datovým modelem, tato metoda funguje dobře, dokud nasazení aplikace do produkčního prostředí.</span><span class="sxs-lookup"><span data-stu-id="32cdf-114">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="32cdf-115">Když aplikace běží v produkčním prostředí se obvykle ukládá data, která chcete zachovat a nechcete ztratit všechno pokaždé, když provedete změny, jako je například přidávání nové sloupce.</span><span class="sxs-lookup"><span data-stu-id="32cdf-115">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="32cdf-116">Funkce migrace základní EF tento problém řeší tím, že umožňuje EF aktualizace schématu databáze, místo vytvoření nové databáze.</span><span class="sxs-lookup"><span data-stu-id="32cdf-116">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="32cdf-117">Entity Framework Core NuGet balíčky pro migrace</span><span class="sxs-lookup"><span data-stu-id="32cdf-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="32cdf-118">Chcete-li pracovat s migrací, můžete použít **Konzola správce balíčků** (pomocí PMC) nebo rozhraní příkazového řádku (CLI).</span><span class="sxs-lookup"><span data-stu-id="32cdf-118">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="32cdf-119">Tyto kurzy ukazují, jak používat rozhraní příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="32cdf-119">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="32cdf-120">Informace o pomocí PMC je na [konci tohoto kurzu](#pmc).</span><span class="sxs-lookup"><span data-stu-id="32cdf-120">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="32cdf-121">Nástroje EF pro rozhraní příkazového řádku (CLI) jsou uvedeny v [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span><span class="sxs-lookup"><span data-stu-id="32cdf-121">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="32cdf-122">K instalaci tohoto balíčku, přidejte ho do `DotNetCliToolReference` kolekce v *.csproj* souboru, jak je vidět.</span><span class="sxs-lookup"><span data-stu-id="32cdf-122">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="32cdf-123">**Poznámka:** je nutné nainstalovat tento balíček úpravou *.csproj* souboru; nelze použít `install-package` příkaz nebo grafického uživatelského rozhraní Správce balíčků.</span><span class="sxs-lookup"><span data-stu-id="32cdf-123">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="32cdf-124">Můžete upravit *.csproj* kliknutím pravým tlačítkem myši na název projektu v souboru **Průzkumníku řešení** a výběrem **upravit ContosoUniversity.csproj**.</span><span class="sxs-lookup"><span data-stu-id="32cdf-124">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="32cdf-125">(Číslo verze v tomto příkladu byly aktuální v době kurzu byla zapsána.)</span><span class="sxs-lookup"><span data-stu-id="32cdf-125">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="32cdf-126">Změnit připojovací řetězec</span><span class="sxs-lookup"><span data-stu-id="32cdf-126">Change the connection string</span></span>

<span data-ttu-id="32cdf-127">V *appSettings.JSON určený* souboru, změňte název databáze v připojovacím řetězci ContosoUniversity2 nebo jiný název, který nepoužili na počítači, který používáte.</span><span class="sxs-lookup"><span data-stu-id="32cdf-127">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="32cdf-128">Tato změna nastaví projekt tak, aby první migrací vytvoří novou databázi.</span><span class="sxs-lookup"><span data-stu-id="32cdf-128">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="32cdf-129">Není to povinné pro zahájení práce s migrací, ale se zobrazí později proto je vhodné.</span><span class="sxs-lookup"><span data-stu-id="32cdf-129">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="32cdf-130">Jako alternativu k změnit název databáze můžete odstranit databázi.</span><span class="sxs-lookup"><span data-stu-id="32cdf-130">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="32cdf-131">Použití **Průzkumník objektů systému SQL Server** (SSOX) nebo `database drop` rozhraní příkazového řádku příkaz:</span><span class="sxs-lookup"><span data-stu-id="32cdf-131">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="32cdf-132">V následující části vysvětluje, jak spouštět příkazy příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="32cdf-132">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="32cdf-133">Vytvoření počáteční migrace</span><span class="sxs-lookup"><span data-stu-id="32cdf-133">Create an initial migration</span></span>

<span data-ttu-id="32cdf-134">Uložte změny a sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="32cdf-134">Save your changes and build the project.</span></span> <span data-ttu-id="32cdf-135">Potom otevřete okno příkazového řádku a přejděte do složky projektu.</span><span class="sxs-lookup"><span data-stu-id="32cdf-135">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="32cdf-136">Tady je rychlý způsob, jak to udělat:</span><span class="sxs-lookup"><span data-stu-id="32cdf-136">Here's a quick way to do that:</span></span>

* <span data-ttu-id="32cdf-137">V **Průzkumníku řešení**, klikněte pravým tlačítkem na projekt a vyberte možnost **otevřít v Průzkumníku souborů** v místní nabídce.</span><span class="sxs-lookup"><span data-stu-id="32cdf-137">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![Otevřít v Průzkumníkovi souborů položky nabídky](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="32cdf-139">Na panelu Adresa zadejte "cmd" a stiskněte klávesu Enter.</span><span class="sxs-lookup"><span data-stu-id="32cdf-139">Enter "cmd" in the address bar and press Enter.</span></span>

  ![Otevřete příkazové okno](migrations/_static/open-command-window.png)

<span data-ttu-id="32cdf-141">Do příkazového řádku zadejte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="32cdf-141">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="32cdf-142">Zobrazí výstup v příkazovém okně takto:</span><span class="sxs-lookup"><span data-stu-id="32cdf-142">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="32cdf-143">Pokud se zobrazí chybová zpráva *žádný spustitelný soubor nalezen odpovídající příkaz "dotnet-ef"*, najdete v části [tomto příspěvku na blogu](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) pomoc při řešení potíží.</span><span class="sxs-lookup"><span data-stu-id="32cdf-143">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="32cdf-144">Pokud se zobrazí chybová zpráva "*nemůže přistupovat k souboru... ContosoUniversity.dll vzhledem k tomu, že je stále používán jiným procesem.* ", najít na službě IIS Express ikonu na hlavním panelu systému Windows a pravým tlačítkem myši a pak klikněte na tlačítko **ContosoUniversity > Stop lokality**.</span><span class="sxs-lookup"><span data-stu-id="32cdf-144">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="32cdf-145">Zkontrolujte nahoru a dolů metody</span><span class="sxs-lookup"><span data-stu-id="32cdf-145">Examine the Up and Down methods</span></span>

<span data-ttu-id="32cdf-146">Při provedení `migrations add` příkaz EF generovaného kódu, který vytvoří databázi od začátku.</span><span class="sxs-lookup"><span data-stu-id="32cdf-146">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="32cdf-147">Tento kód je v *migrace* složku, v souboru s názvem  *\<časové razítko > _InitialCreate.cs*.</span><span class="sxs-lookup"><span data-stu-id="32cdf-147">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="32cdf-148">`Up` Metodu `InitialCreate` třída vytvoří databázové tabulky, které odpovídají sady dat modelu entity a `Down` metoda odstraní, jak je znázorněno v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="32cdf-148">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="32cdf-149">Migrace volání `Up` metody k implementaci změny modelu dat pro migraci.</span><span class="sxs-lookup"><span data-stu-id="32cdf-149">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="32cdf-150">Když zadáte příkaz k vrácení aktualizace, migrace volání `Down` metoda.</span><span class="sxs-lookup"><span data-stu-id="32cdf-150">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="32cdf-151">Tento kód je pro počáteční migrace, která byla vytvořena, když jste zadali `migrations add InitialCreate` příkaz.</span><span class="sxs-lookup"><span data-stu-id="32cdf-151">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="32cdf-152">Parametr name migrace ("InitialCreate" v příkladu) se používá pro název souboru a může být libovolně.</span><span class="sxs-lookup"><span data-stu-id="32cdf-152">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="32cdf-153">Doporučujeme vybrat slovo nebo frázi, která shrnuje, co probíhá při migraci.</span><span class="sxs-lookup"><span data-stu-id="32cdf-153">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="32cdf-154">Například můžete třeba pojmenovat novější migrace "AddDepartmentTable".</span><span class="sxs-lookup"><span data-stu-id="32cdf-154">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="32cdf-155">Pokud jste vytvořili počáteční migrace, když databáze již existuje, se vygeneruje kód pro vytvoření databáze, ale nemá spustit, protože databáze již odpovídá datový model.</span><span class="sxs-lookup"><span data-stu-id="32cdf-155">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="32cdf-156">Když nasadíte aplikaci do jiného prostředí, kde databáze ještě neexistuje, tento kód spustí k vytvoření databáze, proto je vhodné se nejdřív otestovat.</span><span class="sxs-lookup"><span data-stu-id="32cdf-156">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="32cdf-157">To je důvod, proč jste změnili název databáze v připojovacím řetězci dříve – tak, aby migrace můžete vytvořit novou od začátku.</span><span class="sxs-lookup"><span data-stu-id="32cdf-157">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="32cdf-158">Zkontrolujte snímku modelu dat</span><span class="sxs-lookup"><span data-stu-id="32cdf-158">Examine the data model snapshot</span></span>

<span data-ttu-id="32cdf-159">Migrace taky vytvoří *snímku* z aktuální schéma databáze v *Migrations/SchoolContextModelSnapshot.cs*.</span><span class="sxs-lookup"><span data-stu-id="32cdf-159">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="32cdf-160">Tady je tento kód vypadá takto:</span><span class="sxs-lookup"><span data-stu-id="32cdf-160">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="32cdf-161">Vzhledem k tomu, že aktuální schéma databáze je reprezentována v kódu, EF základní nemusí pracovat s databází pro vytvoření migrace.</span><span class="sxs-lookup"><span data-stu-id="32cdf-161">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="32cdf-162">Když přidáte migrace, EF Určuje, co se změnilo tak, že porovnáte datový model, který soubor snímku.</span><span class="sxs-lookup"><span data-stu-id="32cdf-162">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="32cdf-163">EF komunikuje s databází jenom v případě, že má k aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="32cdf-163">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="32cdf-164">Soubor snímku musí být synchronizovány s migrací, které vytvářejí, takže nelze odebrat migrace právě odstraněním soubor s názvem  *\<časové razítko > _\<migrationname > .cs*.</span><span class="sxs-lookup"><span data-stu-id="32cdf-164">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="32cdf-165">Pokud odstraníte tento soubor, zbývající migrace bude synchronizován s soubor snímku databáze.</span><span class="sxs-lookup"><span data-stu-id="32cdf-165">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="32cdf-166">Chcete-li odstranit poslední migrace, který jste přidali, použijte [odebrat dotnet ef migrace](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) příkaz.</span><span class="sxs-lookup"><span data-stu-id="32cdf-166">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="32cdf-167">Použití migrace k databázi</span><span class="sxs-lookup"><span data-stu-id="32cdf-167">Apply the migration to the database</span></span>

<span data-ttu-id="32cdf-168">V okně příkazového řádku zadejte následující příkaz pro vytvoření databáze a tabulky v ní.</span><span class="sxs-lookup"><span data-stu-id="32cdf-168">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="32cdf-169">Výstup z tohoto příkazu je podobná `migrations add` příkaz, s tím rozdílem, že pro příkazy SQL, která nastavení databáze naleznete v protokolech.</span><span class="sxs-lookup"><span data-stu-id="32cdf-169">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="32cdf-170">Většina protokoly byly vynechány v následující ukázce výstupu.</span><span class="sxs-lookup"><span data-stu-id="32cdf-170">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="32cdf-171">Pokud dáváte přednost není zobrazíte tato úroveň podrobností ve zprávách protokolu, můžete změnit úroveň protokolu v *appsettings. Development.JSON* souboru.</span><span class="sxs-lookup"><span data-stu-id="32cdf-171">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="32cdf-172">Další informace najdete v tématu [Úvod k protokolování](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="32cdf-172">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="32cdf-173">Použití **Průzkumník objektů systému SQL Server** Kontrola databáze, stejně jako v prvním kurzu.</span><span class="sxs-lookup"><span data-stu-id="32cdf-173">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="32cdf-174">Můžete si všimnout přidání tabulky __EFMigrationsHistory udržuje informace o migrace, které byly použity k databázi.</span><span class="sxs-lookup"><span data-stu-id="32cdf-174">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="32cdf-175">Zobrazení dat v této tabulce a zobrazí jeden řádek na první migraci.</span><span class="sxs-lookup"><span data-stu-id="32cdf-175">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="32cdf-176">(V poslední protokolu v předchozím příkladu výstupu rozhraní příkazového řádku se zobrazuje příkaz INSERT, která vytváří tento řádek.)</span><span class="sxs-lookup"><span data-stu-id="32cdf-176">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="32cdf-177">Spusťte aplikaci k ověření, že všechno funguje stále stejná jako předtím.</span><span class="sxs-lookup"><span data-stu-id="32cdf-177">Run the application to verify that everything still works the same as before.</span></span>

![Studenti, kteří indexovou stránku](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="32cdf-179">Rozhraní příkazového řádku (CLI) vs. Konzola správce balíčků (pomocí PMC)</span><span class="sxs-lookup"><span data-stu-id="32cdf-179">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="32cdf-180">EF nástrojů pro správu migrace je k dispozici z rozhraní .NET Core příkazového řádku nebo rutiny prostředí PowerShell v sadě Visual Studio **Konzola správce balíčků** okno (pomocí PMC).</span><span class="sxs-lookup"><span data-stu-id="32cdf-180">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="32cdf-181">Tento kurz ukazuje, jak používat rozhraní příkazového řádku, ale můžete pomocí PMC Pokud dáváte přednost.</span><span class="sxs-lookup"><span data-stu-id="32cdf-181">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="32cdf-182">EF příkazů pro příkazy pomocí PMC jsou v [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) balíčku.</span><span class="sxs-lookup"><span data-stu-id="32cdf-182">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="32cdf-183">Tento balíček je již součástí [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, takže není nutné ji nainstalovat.</span><span class="sxs-lookup"><span data-stu-id="32cdf-183">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="32cdf-184">**Důležité:** toto není stejného balíčku jako instalace pro rozhraní příkazového řádku úpravou *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="32cdf-184">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="32cdf-185">Název touto končí v `Tools`, na rozdíl od název balíčku rozhraní příkazového řádku, které končí na `Tools.DotNet`.</span><span class="sxs-lookup"><span data-stu-id="32cdf-185">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="32cdf-186">Další informace o rozhraní příkazového řádku najdete v tématu [.NET Core rozhraní příkazového řádku](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span><span class="sxs-lookup"><span data-stu-id="32cdf-186">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="32cdf-187">Další informace o příkazech pomocí PMC najdete v tématu [Konzola správce balíčků (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span><span class="sxs-lookup"><span data-stu-id="32cdf-187">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="32cdf-188">Souhrn</span><span class="sxs-lookup"><span data-stu-id="32cdf-188">Summary</span></span>

<span data-ttu-id="32cdf-189">V tomto kurzu jste viděli, jak vytvořit a použít první migrace.</span><span class="sxs-lookup"><span data-stu-id="32cdf-189">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="32cdf-190">V dalším kurzu brzy prohlížení pokročilejší témata rozšířením datový model.</span><span class="sxs-lookup"><span data-stu-id="32cdf-190">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="32cdf-191">Na této cestě můžete vytvářet a použijte další migrace.</span><span class="sxs-lookup"><span data-stu-id="32cdf-191">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="32cdf-192">[Předchozí](sort-filter-page.md)
[další](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="32cdf-192">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  