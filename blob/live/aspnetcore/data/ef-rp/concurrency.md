---
title: "Stránky Razor EF základní - souběžnosti - 8 8"
author: rick-anderson
description: "Tento kurz ukazuje způsobu řešení konfliktů, když se více uživatelů aktualizace stejné entity ve stejnou dobu."
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/concurrency
ms.openlocfilehash: a980669d49d332d7ef2ff5a18c73e9b269281287
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
<span data-ttu-id="6c061-103">en-us /</span><span class="sxs-lookup"><span data-stu-id="6c061-103">en-us/</span></span>

# <a name="handling-concurrency-conflicts---ef-core-with-razor-pages-8-of-8"></a><span data-ttu-id="6c061-104">Zpracování konfliktů souběžnosti – základní EF s stránky Razor (8 8)</span><span class="sxs-lookup"><span data-stu-id="6c061-104">Handling concurrency conflicts - EF Core with Razor Pages (8 of 8)</span></span>

<span data-ttu-id="6c061-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT), [tní Dykstra](https://github.com/tdykstra), a [Jon P Smith](https://twitter.com/thereformedprog)</span><span class="sxs-lookup"><span data-stu-id="6c061-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra), and  [Jon P Smith](https://twitter.com/thereformedprog)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="6c061-106">Tento kurz ukazuje způsobu řešení konfliktů při více uživatelů aktualizovat entitu, souběžně (ve stejnou dobu).</span><span class="sxs-lookup"><span data-stu-id="6c061-106">This tutorial shows how to handle conflicts when multiple users update an entity concurrently (at the same time).</span></span> <span data-ttu-id="6c061-107">Pokud narazíte na problémy, které nelze vyřešit, stáhněte si [dokončené aplikace pro tuto fázi](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span><span class="sxs-lookup"><span data-stu-id="6c061-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part8).</span></span>

## <a name="concurrency-conflicts"></a><span data-ttu-id="6c061-108">Konflikty souběžnosti</span><span class="sxs-lookup"><span data-stu-id="6c061-108">Concurrency conflicts</span></span>

<span data-ttu-id="6c061-109">Dojde ke konfliktu souběžnosti při:</span><span class="sxs-lookup"><span data-stu-id="6c061-109">A concurrency conflict occurs when:</span></span>

* <span data-ttu-id="6c061-110">Uživatel přejde na stránku upravit pro entitu.</span><span class="sxs-lookup"><span data-stu-id="6c061-110">A user navigates to the edit page for an entity.</span></span>
* <span data-ttu-id="6c061-111">Jiný uživatel aktualizuje na stejnou entitu před první uživatel změnu je zapsán do databáze.</span><span class="sxs-lookup"><span data-stu-id="6c061-111">Another user updates the same entity before the first user's change is written to the DB.</span></span>

<span data-ttu-id="6c061-112">Pokud detekce souběžnosti není povolena, když dojde k Souběžná aktualizace:</span><span class="sxs-lookup"><span data-stu-id="6c061-112">If concurrency detection is not enabled, when concurrent updates occur:</span></span>

* <span data-ttu-id="6c061-113">Poslední aktualizace služby wins.</span><span class="sxs-lookup"><span data-stu-id="6c061-113">The last update wins.</span></span> <span data-ttu-id="6c061-114">To znamená poslední aktualizace hodnoty se uloží do databáze.</span><span class="sxs-lookup"><span data-stu-id="6c061-114">That is, the last update values are saved to the DB.</span></span>
* <span data-ttu-id="6c061-115">První z aktuální aktualizace budou ztraceny.</span><span class="sxs-lookup"><span data-stu-id="6c061-115">The first of the current updates are lost.</span></span>

### <a name="optimistic-concurrency"></a><span data-ttu-id="6c061-116">Optimistickou metodu souběžného zpracování</span><span class="sxs-lookup"><span data-stu-id="6c061-116">Optimistic concurrency</span></span>

<span data-ttu-id="6c061-117">Optimistickou metodu souběžného umožňuje konfliktů souběžnosti provést, a pak reaguje správně při dělají.</span><span class="sxs-lookup"><span data-stu-id="6c061-117">Optimistic concurrency allows concurrency conflicts to happen, and then reacts appropriately when they do.</span></span> <span data-ttu-id="6c061-118">Například Jana navštíví stránce Upravit oddělení a mění nároky pro angličtinu oddělení z $350,000.00 0,00 Kč.</span><span class="sxs-lookup"><span data-stu-id="6c061-118">For example, Jane visits the Department edit page and changes the budget for the English department from $350,000.00 to $0.00.</span></span>

![Změna nároky na 0](concurrency/_static/change-budget.png)

<span data-ttu-id="6c061-120">Před kliknutím na Jana **Uložit**, Jan navštíví stejné stránce a změny pole Počáteční datum 9/1/2013 z 9/1/2007.</span><span class="sxs-lookup"><span data-stu-id="6c061-120">Before Jane clicks **Save**, John visits the same page and changes the Start Date field from 9/1/2007 to 9/1/2013.</span></span>

![Změna počáteční datum na 2013](concurrency/_static/change-date.png)

<span data-ttu-id="6c061-122">Jana klikne **Uložit** první a zobrazí ji změnit, když prohlížeč zobrazí indexovou stránku.</span><span class="sxs-lookup"><span data-stu-id="6c061-122">Jane clicks **Save** first and sees her change when the browser displays the Index page.</span></span>

![Nároky změnit tak, aby nula.](concurrency/_static/budget-zero.png)

<span data-ttu-id="6c061-124">Jan klikne **Uložit** na stránku úpravy, která se zobrazuje nároky $350,000.00.</span><span class="sxs-lookup"><span data-stu-id="6c061-124">John clicks **Save** on an Edit page that still shows a budget of $350,000.00.</span></span> <span data-ttu-id="6c061-125">Co se stane dále je určen jak zpracování konfliktů souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="6c061-125">What happens next is determined by how you handle concurrency conflicts.</span></span>

<span data-ttu-id="6c061-126">Optimistickou metodu souběžného zahrnuje následující možnosti:</span><span class="sxs-lookup"><span data-stu-id="6c061-126">Optimistic concurrency includes the following options:</span></span>

* <span data-ttu-id="6c061-127">Můžete udržovat přehled o vlastností, které uživatel změnil a aktualizovat na odpovídající sloupce v databázi.</span><span class="sxs-lookup"><span data-stu-id="6c061-127">You can keep track of which property a user has modified and update only the corresponding columns in the DB.</span></span>

 <span data-ttu-id="6c061-128">Ve scénáři bude ztracena žádná data.</span><span class="sxs-lookup"><span data-stu-id="6c061-128">In the scenario, no data would be lost.</span></span> <span data-ttu-id="6c061-129">Různé vlastnosti byly aktualizovány dva uživatelé.</span><span class="sxs-lookup"><span data-stu-id="6c061-129">Different properties were updated by the two users.</span></span> <span data-ttu-id="6c061-130">Při příštím někdo umožňuje anglické oddělení, se zobrazí na Jana i Jan pro změny.</span><span class="sxs-lookup"><span data-stu-id="6c061-130">The next time someone browses the English department, they'll see both Jane's and John's changes.</span></span> <span data-ttu-id="6c061-131">Tato metoda aktualizace může snížit počet konfliktům, ke kterým může dojít ke ztrátě dat.</span><span class="sxs-lookup"><span data-stu-id="6c061-131">This method of updating can reduce the number of conflicts that could result in data loss.</span></span> <span data-ttu-id="6c061-132">Tento postup: \* nelze nedošlo ke ztrátě dat. Pokud konkurenční změn stejnou vlastnost.</span><span class="sxs-lookup"><span data-stu-id="6c061-132">This approach: \* Can't avoid data loss if competing changes are made to the same property.</span></span>
        <span data-ttu-id="6c061-133">\* Je obecně není praktické ve webové aplikaci.</span><span class="sxs-lookup"><span data-stu-id="6c061-133">\* Is generally not practical in a web app.</span></span> <span data-ttu-id="6c061-134">To vyžaduje údržbu významné stavu k uchovávání informací o všech načtených hodnoty a nové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="6c061-134">It requires maintaining significant state in order to keep track of all fetched values and new values.</span></span> <span data-ttu-id="6c061-135">Správa velkých objemů stavu může ovlivnit výkon aplikace.</span><span class="sxs-lookup"><span data-stu-id="6c061-135">Maintaining large amounts of state can affect app performance.</span></span>
        <span data-ttu-id="6c061-136">\* Může zvýšit složitost aplikace ve srovnání s detekce souběžnosti na entitu.</span><span class="sxs-lookup"><span data-stu-id="6c061-136">\* Can increase app complexity compared to concurrency detection on an entity.</span></span>

* <span data-ttu-id="6c061-137">Můžete je nechat Jan pro změnu Jana změna přepsána.</span><span class="sxs-lookup"><span data-stu-id="6c061-137">You can let John's change overwrite Jane's change.</span></span>

 <span data-ttu-id="6c061-138">Při příštím někdo umožňuje anglické oddělení, se zobrazí 9/1/2013 a načtených $350,000.00 hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6c061-138">The next time someone browses the English department, they'll see 9/1/2013 and the fetched $350,000.00 value.</span></span> <span data-ttu-id="6c061-139">Tato metoda se nazývá *klienta Wins* nebo *poslední ve službě Wins* scénář.</span><span class="sxs-lookup"><span data-stu-id="6c061-139">This approach is called a *Client Wins* or *Last in Wins* scenario.</span></span> <span data-ttu-id="6c061-140">(Všechny hodnoty z klienta přednost co je v úložišti.) Pokud neprovedete žádné kódování pro zpracování souběžnosti, Wins, klient se automaticky.</span><span class="sxs-lookup"><span data-stu-id="6c061-140">(All values from the client take precedence over what's in the data store.) If you don't do any coding for concurrency handling, Client Wins happens automatically.</span></span>

* <span data-ttu-id="6c061-141">Jan pro změnu může zabránit aktualizaci v databázi.</span><span class="sxs-lookup"><span data-stu-id="6c061-141">You can prevent John's change from being updated in the DB.</span></span> <span data-ttu-id="6c061-142">Obvykle se aplikace by: \* Zobrazí se chybová zpráva.</span><span class="sxs-lookup"><span data-stu-id="6c061-142">Typically, the app would: \* Display an error message.</span></span>
        <span data-ttu-id="6c061-143">\* Zobrazit aktuální stav data.</span><span class="sxs-lookup"><span data-stu-id="6c061-143">\* Show the current state of the data.</span></span>
        <span data-ttu-id="6c061-144">\* Povolí uživateli znovu použít změny.</span><span class="sxs-lookup"><span data-stu-id="6c061-144">\* Allow the user to reapply the changes.</span></span>

 <span data-ttu-id="6c061-145">Tento postup se nazývá *Wins úložiště* scénář.</span><span class="sxs-lookup"><span data-stu-id="6c061-145">This is called a *Store Wins* scenario.</span></span> <span data-ttu-id="6c061-146">(Hodnoty úložiště dat mají přednost před odeslané klientem hodnoty.) V tomto kurzu implementujete scénář úložiště služby Wins.</span><span class="sxs-lookup"><span data-stu-id="6c061-146">(The data-store values take precedence over the values submitted by the client.) You implement the Store Wins scenario in this tutorial.</span></span> <span data-ttu-id="6c061-147">Tato metoda zajišťuje, že žádné změny budou přepsána bez uživatele se zobrazí upozornění.</span><span class="sxs-lookup"><span data-stu-id="6c061-147">This method ensures that no changes are overwritten without a user being alerted.</span></span>

## <a name="handling-concurrency"></a><span data-ttu-id="6c061-148">Zpracování souběžnosti</span><span class="sxs-lookup"><span data-stu-id="6c061-148">Handling concurrency</span></span> 

<span data-ttu-id="6c061-149">Pokud je vlastnost nakonfigurovaný jako [token souběžnosti](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span><span class="sxs-lookup"><span data-stu-id="6c061-149">When a property is configured as a [concurrency token](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency):</span></span>

* <span data-ttu-id="6c061-150">EF základní ověřuje, že vlastnost neupravoval po se načetla.</span><span class="sxs-lookup"><span data-stu-id="6c061-150">EF Core verifies that property has not been modified after it was fetched.</span></span> <span data-ttu-id="6c061-151">Kontrola dojde při [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) nebo [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) je volána.</span><span class="sxs-lookup"><span data-stu-id="6c061-151">The check occurs when [SaveChanges](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechanges?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChanges) or [SaveChangesAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span>
* <span data-ttu-id="6c061-152">Pokud vlastnost byla změněna od jeho tabulky se načetla, [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="6c061-152">If the property has been changed after it was fetched, a [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception?view=efcore-2.0) is thrown.</span></span> 

<span data-ttu-id="6c061-153">Databáze a datového modelu musí být nakonfigurované pro podporu vyvolání `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6c061-153">The DB and data model must be configured to support throwing `DbUpdateConcurrencyException`.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-property"></a><span data-ttu-id="6c061-154">Zjišťování konfliktů souběžnosti u vlastnosti</span><span class="sxs-lookup"><span data-stu-id="6c061-154">Detecting concurrency conflicts on a property</span></span>

<span data-ttu-id="6c061-155">Může být zjistil konflikt souběžnosti na úrovni vlastnost s [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) atribut.</span><span class="sxs-lookup"><span data-stu-id="6c061-155">Concurrency conflicts can be detected at the property level with the [ConcurrencyCheck](https://docs.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.concurrencycheckattribute?view=netcore-2.0) attribute.</span></span> <span data-ttu-id="6c061-156">Atribut je použít pro více vlastností v modelu.</span><span class="sxs-lookup"><span data-stu-id="6c061-156">The attribute can be applied to multiple properties on the model.</span></span> <span data-ttu-id="6c061-157">Další informace najdete v tématu [Data poznámky-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span><span class="sxs-lookup"><span data-stu-id="6c061-157">For more information, see [Data Annotations-ConcurrencyCheck](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency#data-annotations).</span></span>

<span data-ttu-id="6c061-158">`[ConcurrencyCheck]` Atribut není použit v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="6c061-158">The `[ConcurrencyCheck]` attribute is not used in this tutorial.</span></span>

### <a name="detecting-concurrency-conflicts-on-a-row"></a><span data-ttu-id="6c061-159">Zjišťování konfliktů souběžnosti na řádek</span><span class="sxs-lookup"><span data-stu-id="6c061-159">Detecting concurrency conflicts on a row</span></span>

<span data-ttu-id="6c061-160">Ke zjišťování konfliktů souběžnosti, [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) sledování sloupce se přidá do modelu.</span><span class="sxs-lookup"><span data-stu-id="6c061-160">To detect concurrency conflicts, a [rowversion](https://docs.microsoft.com/sql/t-sql/data-types/rowversion-transact-sql) tracking column is added to the model.</span></span>  <span data-ttu-id="6c061-161">`rowversion` :</span><span class="sxs-lookup"><span data-stu-id="6c061-161">`rowversion` :</span></span>

* <span data-ttu-id="6c061-162">SQL Server se konkrétní.</span><span class="sxs-lookup"><span data-stu-id="6c061-162">Is SQL Server specific.</span></span> <span data-ttu-id="6c061-163">Ostatní databáze nemusí poskytují podobné funkce.</span><span class="sxs-lookup"><span data-stu-id="6c061-163">Other databases may not provide a similar feature.</span></span>
* <span data-ttu-id="6c061-164">Slouží k určení, že entita nebyla změněna od načtení z databáze.</span><span class="sxs-lookup"><span data-stu-id="6c061-164">Is used to determine that an entity has not been changed since it was fetched from the DB.</span></span> 

<span data-ttu-id="6c061-165">Databáze generuje sekvenční `rowversion` číslo, které se zvýší, řádek pokaždé, když se aktualizuje.</span><span class="sxs-lookup"><span data-stu-id="6c061-165">The DB generates a sequential `rowversion` number that's incremented each time the row is updated.</span></span> <span data-ttu-id="6c061-166">V `Update` nebo `Delete` příkaz, `Where` klauzule zahrnuje načtených hodnotu `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="6c061-166">In an `Update` or `Delete` command, the `Where` clause includes the fetched value of `rowversion`.</span></span> <span data-ttu-id="6c061-167">Pokud došlo ke změně řádku:</span><span class="sxs-lookup"><span data-stu-id="6c061-167">If the row being updated has changed:</span></span>

 * <span data-ttu-id="6c061-168">`rowversion`neodpovídá hodnotě načtených.</span><span class="sxs-lookup"><span data-stu-id="6c061-168">`rowversion` doesn't match the fetched value.</span></span>
 * <span data-ttu-id="6c061-169">`Update` Nebo `Delete` příkazů nelze nalézt řádek, protože `Where` klauzule zahrnuje načtených `rowversion`.</span><span class="sxs-lookup"><span data-stu-id="6c061-169">The `Update` or `Delete` commands don't find a row because the `Where` clause includes the fetched `rowversion`.</span></span>
 * <span data-ttu-id="6c061-170">A `DbUpdateConcurrencyException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="6c061-170">A `DbUpdateConcurrencyException` is thrown.</span></span>

<span data-ttu-id="6c061-171">V EF Core, když žádné řádky bylo aktualizováno správcem `Update` nebo `Delete` příkaz, je vyvolána výjimka souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="6c061-171">In EF Core, when no rows have been updated by an `Update` or `Delete` command, a concurrency exception is thrown.</span></span>

### <a name="add-a-tracking-property-to-the-department-entity"></a><span data-ttu-id="6c061-172">Přidat vlastnost sledování do oddělení entity</span><span class="sxs-lookup"><span data-stu-id="6c061-172">Add a tracking property to the Department entity</span></span>

<span data-ttu-id="6c061-173">V *Models/Department.cs*, přidejte sledování vlastnost s názvem RowVersion:</span><span class="sxs-lookup"><span data-stu-id="6c061-173">In *Models/Department.cs*, add a tracking property named RowVersion:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Department.cs?name=snippet_Final&highlight=26,27)]

<span data-ttu-id="6c061-174">[Časové razítko](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) atribut určuje, zda je tento sloupec součástí `Where` klauzuli `Update` a `Delete` příkazy.</span><span class="sxs-lookup"><span data-stu-id="6c061-174">The [Timestamp](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations.timestampattribute) attribute specifies that this column is included in the `Where` clause of `Update` and `Delete` commands.</span></span> <span data-ttu-id="6c061-175">Atribut se nazývá `Timestamp` protože předchozí verze systému SQL Server používá SQL `timestamp` datového typu než SQL `rowversion` typ nahradí ji.</span><span class="sxs-lookup"><span data-stu-id="6c061-175">The attribute is called `Timestamp` because previous versions of SQL Server used a SQL `timestamp` data type before the SQL `rowversion` type replaced it.</span></span>

<span data-ttu-id="6c061-176">Rozhraní fluent API můžete také určit vlastnosti sledování:</span><span class="sxs-lookup"><span data-stu-id="6c061-176">The fluent API can also specify the tracking property:</span></span>

```csharp
modelBuilder.Entity<Department>()
  .Property<byte[]>("RowVersion")
  .IsRowVersion();
```

<span data-ttu-id="6c061-177">Následující kód ukazuje část vytvářené EF jádra, když se aktualizuje název oddělení T-SQL:</span><span class="sxs-lookup"><span data-stu-id="6c061-177">The following code shows a portion of the T-SQL generated by EF Core when the Department name is updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=2-3)]

<span data-ttu-id="6c061-178">Podle předchozích zvýrazněná kód ukazuje `WHERE` obsahující klauzuli `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6c061-178">The preceding highlighted code shows the `WHERE` clause containing `RowVersion`.</span></span> <span data-ttu-id="6c061-179">Pokud databáze `RowVersion` se nerovná `RowVersion` parametr (`@p2`), jsou aktualizovány žádné řádky.</span><span class="sxs-lookup"><span data-stu-id="6c061-179">If the DB `RowVersion` doesn't equal the `RowVersion` parameter (`@p2`), no rows are updated.</span></span>

<span data-ttu-id="6c061-180">Následující zvýrazněný kód ukazuje T-SQL, která ověřuje, že byla aktualizována přesně jeden řádek:</span><span class="sxs-lookup"><span data-stu-id="6c061-180">The following highlighted code shows the T-SQL that verifies exactly one row was updated:</span></span>

[!code-sql[](intro/samples/sql.txt?highlight=4-6)]

<span data-ttu-id="6c061-181">[@@ROWCOUNT ](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) vrátí počet řádků, které jsou ovlivněné poslední příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c061-181">[@@ROWCOUNT](https://docs.microsoft.com/en-us/sql/t-sql/functions/rowcount-transact-sql) returns the number of rows affected by the last statement.</span></span> <span data-ttu-id="6c061-182">V žádné řádky jsou aktualizovány, vyvolá EF jádra `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="6c061-182">In no rows are updated, EF Core throws a `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="6c061-183">Uvidíte, že generuje základní EF T-SQL v okně výstupu sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c061-183">You can see the T-SQL EF Core generates in the output window of Visual Studio.</span></span>

### <a name="update-the-db"></a><span data-ttu-id="6c061-184">Aktualizace databáze</span><span class="sxs-lookup"><span data-stu-id="6c061-184">Update the DB</span></span>

<span data-ttu-id="6c061-185">Přidávání `RowVersion` změny vlastností DB modelu, který vyžaduje migrace.</span><span class="sxs-lookup"><span data-stu-id="6c061-185">Adding the `RowVersion` property changes the DB model, which requires a migration.</span></span>

<span data-ttu-id="6c061-186">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="6c061-186">Build the project.</span></span> <span data-ttu-id="6c061-187">V okně příkazového řádku zadejte následující údaje:</span><span class="sxs-lookup"><span data-stu-id="6c061-187">Enter the following in a command window:</span></span>

```console
dotnet ef migrations add RowVersion
dotnet ef database update
```

<span data-ttu-id="6c061-188">Předchozí příkazy:</span><span class="sxs-lookup"><span data-stu-id="6c061-188">The preceding commands:</span></span>

* <span data-ttu-id="6c061-189">Přidá *migrace / {čas stamp}_RowVersion.cs* souboru migrace.</span><span class="sxs-lookup"><span data-stu-id="6c061-189">Adds the *Migrations/{time stamp}_RowVersion.cs* migration file.</span></span>
* <span data-ttu-id="6c061-190">Aktualizace *Migrations/SchoolContextModelSnapshot.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="6c061-190">Updates the *Migrations/SchoolContextModelSnapshot.cs* file.</span></span> <span data-ttu-id="6c061-191">Aktualizace přidává následující zvýrazněný kód do `BuildModel` metoda:</span><span class="sxs-lookup"><span data-stu-id="6c061-191">The update adds the following highlighted code to the `BuildModel` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot2.cs?name=snippet&highlight=14-16)]

* <span data-ttu-id="6c061-192">Spustí migrace k aktualizaci databáze.</span><span class="sxs-lookup"><span data-stu-id="6c061-192">Runs migrations to update the DB.</span></span>

<a name="scaffold"></a>
## <a name="scaffold-the-departments-model"></a><span data-ttu-id="6c061-193">Vygenerované uživatelské rozhraní modelu oddělení</span><span class="sxs-lookup"><span data-stu-id="6c061-193">Scaffold the Departments model</span></span>

* <span data-ttu-id="6c061-194">Ukončete aplikaci Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c061-194">Exit Visual Studio.</span></span>
* <span data-ttu-id="6c061-195">Otevřete okno příkazového řádku v adresáři projektu (adresář, který obsahuje *Program.cs*, *Startup.cs*, a *.csproj* soubory).</span><span class="sxs-lookup"><span data-stu-id="6c061-195">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="6c061-196">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="6c061-196">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Department -dc SchoolContext -udl -outDir Pages\Departments --referenceScriptLibraries
 ```

<span data-ttu-id="6c061-197">Předchozí příkaz scaffold `Department` modelu.</span><span class="sxs-lookup"><span data-stu-id="6c061-197">The preceding command scaffolds the `Department` model.</span></span> <span data-ttu-id="6c061-198">Otevřete projekt v sadě Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6c061-198">Open the project in Visual Studio.</span></span>

<span data-ttu-id="6c061-199">Sestavte projekt.</span><span class="sxs-lookup"><span data-stu-id="6c061-199">Build the project.</span></span> <span data-ttu-id="6c061-200">Sestavení generuje chyby takto:</span><span class="sxs-lookup"><span data-stu-id="6c061-200">The build generates errors like the following:</span></span>

`1>Pages/Departments/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Department' and no extension method 'Department' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="6c061-201">Globálně změnit `_context.Department` k `_context.Departments` (tedy "s" přidat do `Department`).</span><span class="sxs-lookup"><span data-stu-id="6c061-201">Globally change `_context.Department` to `_context.Departments` (that is, add an "s" to `Department`).</span></span> <span data-ttu-id="6c061-202">7 výskytů jsou vyhledána a aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="6c061-202">7 occurrences are found and updated.</span></span>

### <a name="update-the-departments-index-page"></a><span data-ttu-id="6c061-203">Aktualizace oddělení indexovou stránku</span><span class="sxs-lookup"><span data-stu-id="6c061-203">Update the Departments Index page</span></span>

<span data-ttu-id="6c061-204">Modul generování uživatelského rozhraní, který je vytvořen `RowVersion` sloupec indexovou stránku, ale toto pole by se neměly zobrazovat.</span><span class="sxs-lookup"><span data-stu-id="6c061-204">The scaffolding engine created a `RowVersion` column for the Index page, but that field shouldn't be displayed.</span></span> <span data-ttu-id="6c061-205">V tomto kurzu, poslední bajt `RowVersion` se zobrazí pro lepší porozumění tomu souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="6c061-205">In this tutorial, the last byte of the `RowVersion` is displayed to help understand concurrency.</span></span> <span data-ttu-id="6c061-206">Poslední bajt není musí být jedinečný.</span><span class="sxs-lookup"><span data-stu-id="6c061-206">The last byte is not guaranteed to be unique.</span></span> <span data-ttu-id="6c061-207">Skutečné aplikaci, nezobrazí se `RowVersion` nebo poslední bajt `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6c061-207">A real app wouldn't display `RowVersion` or the last byte of `RowVersion`.</span></span>

<span data-ttu-id="6c061-208">Aktualizace indexovou stránku:</span><span class="sxs-lookup"><span data-stu-id="6c061-208">Update the Index page:</span></span>

* <span data-ttu-id="6c061-209">Nahraďte Index oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c061-209">Replace Index with Departments.</span></span>
* <span data-ttu-id="6c061-210">Nahraďte kód obsahující `RowVersion` s poslední bajt `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6c061-210">Replace the markup containing `RowVersion` with the last byte of `RowVersion`.</span></span>
* <span data-ttu-id="6c061-211">Nahraďte FirstMidName FullName.</span><span class="sxs-lookup"><span data-stu-id="6c061-211">Replace FirstMidName with FullName.</span></span>

<span data-ttu-id="6c061-212">Následující kód ukazuje aktualizovanou stránku:</span><span class="sxs-lookup"><span data-stu-id="6c061-212">The following markup shows the updated page:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Index.cshtml?highlight=5,8,29,47,50)]

### <a name="update-the-edit-page-model"></a><span data-ttu-id="6c061-213">Aktualizace modelu stránky úpravy</span><span class="sxs-lookup"><span data-stu-id="6c061-213">Update the Edit page model</span></span>

<span data-ttu-id="6c061-214">Aktualizace *pages\departments\edit.cshtml.cs* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6c061-214">Update *pages\departments\edit.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet)]

<span data-ttu-id="6c061-215">Ke zjištění problémem souběžnosti [původní hodnota](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) je aktualizován `rowVersion` hodnotu z entity se načetla.</span><span class="sxs-lookup"><span data-stu-id="6c061-215">To detect a concurrency issue, the [OriginalValue](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyentry.originalvalue?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyEntry_OriginalValue) is updated with the `rowVersion` value from the entity it was fetched.</span></span> <span data-ttu-id="6c061-216">Základní EF generuje příkazu SQL UPDATE s klauzulí WHERE, která obsahuje původní `RowVersion` hodnota.</span><span class="sxs-lookup"><span data-stu-id="6c061-216">EF Core generates a SQL UPDATE command with a WHERE clause containing the original `RowVersion` value.</span></span> <span data-ttu-id="6c061-217">Pokud se příkaz UPDATE žádné řádky (žádné řádky mají původní `RowVersion` hodnotu), `DbUpdateConcurrencyException` je vyvolána výjimka.</span><span class="sxs-lookup"><span data-stu-id="6c061-217">If no rows are affected by the UPDATE command (no rows have the original `RowVersion` value), a `DbUpdateConcurrencyException` exception is thrown.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_rv&highlight=24-)]

<span data-ttu-id="6c061-218">V předchozí kód `Department.RowVersion` je hodnota, když se načetla entity.</span><span class="sxs-lookup"><span data-stu-id="6c061-218">In the preceding code, `Department.RowVersion` is the value when the entity was fetched.</span></span> <span data-ttu-id="6c061-219">`OriginalValue`je hodnota v databázi při `FirstOrDefaultAsync` byla volána v této metodě.</span><span class="sxs-lookup"><span data-stu-id="6c061-219">`OriginalValue` is the value in the DB when `FirstOrDefaultAsync` was called in this method.</span></span>

<span data-ttu-id="6c061-220">Následující kód získá hodnoty klienta (hodnoty odeslány této metody) a hodnoty DB:</span><span class="sxs-lookup"><span data-stu-id="6c061-220">The following code gets the client values (the values posted to this method) and the DB values:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=9,18)]

<span data-ttu-id="6c061-221">Toto kód přidá vlastní chybové zprávy pro každý sloupec, který má DB hodnoty liší od co byla odeslána do `OnPostAsync`:</span><span class="sxs-lookup"><span data-stu-id="6c061-221">The follwing code adds a custom error message for each column that has DB values different from what was posted to `OnPostAsync`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_err)]

<span data-ttu-id="6c061-222">Následující zvýrazněnou sady kódu `RowVersion` hodnotu na novou hodnotu načíst z databáze.</span><span class="sxs-lookup"><span data-stu-id="6c061-222">The following highlighted code sets the `RowVersion` value to the new value retrieved from the DB.</span></span> <span data-ttu-id="6c061-223">Při příštím kliknutí na tlačítko **Uložit**, pouze souběžného zpracování chyb, které dojít, protože vzniká, poslední zobrazení stránky upravit.</span><span class="sxs-lookup"><span data-stu-id="6c061-223">The next time the user clicks **Save**, only concurrency errors that happen since the last display of the Edit page will be caught.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Edit.cshtml.cs?name=snippet_try&highlight=23)]

<span data-ttu-id="6c061-224">`ModelState.Remove` Příkaz není nutná, protože `ModelState` má starý `RowVersion` hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6c061-224">The `ModelState.Remove` statement is required because `ModelState` has the old `RowVersion` value.</span></span> <span data-ttu-id="6c061-225">Na stránce Razor `ModelState` hodnota pole má přednost před hodnoty vlastností modelu, pokud obě existuje.</span><span class="sxs-lookup"><span data-stu-id="6c061-225">In the Razor Page, the `ModelState` value for a field takes precedence over the model property values when both are present.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="6c061-226">Upravit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="6c061-226">Update the Edit page</span></span>

<span data-ttu-id="6c061-227">Aktualizace *Pages/Departments/Edit.cshtml* s následující kód:</span><span class="sxs-lookup"><span data-stu-id="6c061-227">Update *Pages/Departments/Edit.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Edit.cshtml?highlight=1,14,16-17,37-39)]

<span data-ttu-id="6c061-228">Předchozí kód:</span><span class="sxs-lookup"><span data-stu-id="6c061-228">The preceding markup:</span></span>

* <span data-ttu-id="6c061-229">Aktualizace `page` direktivy z `@page` k `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6c061-229">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="6c061-230">Přidá verze skrytá řádku.</span><span class="sxs-lookup"><span data-stu-id="6c061-230">Adds a hidden row version.</span></span> <span data-ttu-id="6c061-231">`RowVersion`je nutné přidat tak post zpět váže hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6c061-231">`RowVersion` must be added so post back binds the value.</span></span>
* <span data-ttu-id="6c061-232">Zobrazí poslední bajt `RowVersion` pro účely ladění.</span><span class="sxs-lookup"><span data-stu-id="6c061-232">Displays the last byte of `RowVersion` for debugging purposes.</span></span>
* <span data-ttu-id="6c061-233">Nahradí `ViewData` s silného typu `InstructorNameSL`.</span><span class="sxs-lookup"><span data-stu-id="6c061-233">Replaces `ViewData` with the strongly-typed `InstructorNameSL`.</span></span>

## <a name="test-concurrency-conflicts-with-the-edit-page"></a><span data-ttu-id="6c061-234">Test je v konfliktu s stránce Upravit souběžnosti</span><span class="sxs-lookup"><span data-stu-id="6c061-234">Test concurrency conflicts with the Edit page</span></span>

<span data-ttu-id="6c061-235">Otevřete dvě instance upravit prohlížeče, na angličtinu oddělení:</span><span class="sxs-lookup"><span data-stu-id="6c061-235">Open two browsers instances of Edit on the English department:</span></span>

* <span data-ttu-id="6c061-236">Spusťte aplikaci a vyberte oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c061-236">Run the app and select Departments.</span></span>
* <span data-ttu-id="6c061-237">Klikněte pravým tlačítkem myši **upravit** hypertextový odkaz pro angličtinu oddělení a vyberte **otevřít na nové kartě**.</span><span class="sxs-lookup"><span data-stu-id="6c061-237">Right-click the **Edit** hyperlink for the English department and select **Open in new tab**.</span></span>
* <span data-ttu-id="6c061-238">Na první kartě, klikněte **upravit** hypertextový odkaz pro angličtinu oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c061-238">In the first tab, click the **Edit** hyperlink for the English department.</span></span>

<span data-ttu-id="6c061-239">Karty dvě prohlížeč zobrazí stejné informace.</span><span class="sxs-lookup"><span data-stu-id="6c061-239">The two browser tabs display the same information.</span></span>

<span data-ttu-id="6c061-240">Změňte název první záložce prohlížeče a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c061-240">Change the name in the first browser tab and click **Save**.</span></span>

![Upravit oddělení stránka 1 po změně](concurrency/_static/edit-after-change-1.png)

<span data-ttu-id="6c061-242">Prohlížeč zobrazí indexovou stránku s změněné hodnoty a aktualizované rowVersion indikátoru.</span><span class="sxs-lookup"><span data-stu-id="6c061-242">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="6c061-243">Všimněte si aktualizované rowVersion indikátoru, se zobrazí na druhém zpětné volání na jiné kartě.</span><span class="sxs-lookup"><span data-stu-id="6c061-243">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="6c061-244">Změňte jiné pole druhý záložce prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6c061-244">Change a different field in the second browser tab.</span></span>

![Upravit oddělení stránka 2 po změně](concurrency/_static/edit-after-change-2.png)

<span data-ttu-id="6c061-246">Klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c061-246">Click **Save**.</span></span> <span data-ttu-id="6c061-247">Zobrazí chybové zprávy pro všechna pole, které se neshodují s hodnotami DB:</span><span class="sxs-lookup"><span data-stu-id="6c061-247">You see error messages for all fields that don't match the DB values:</span></span>

![Oddělení upravit stránku chybová zpráva](concurrency/_static/edit-error.png)

<span data-ttu-id="6c061-249">Chcete-li změnit název pole nechtěli tohoto okna prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="6c061-249">This browser window did not intend to change the Name field.</span></span> <span data-ttu-id="6c061-250">Zkopírujte a vložte aktuální hodnota (jazyky) do pole název.</span><span class="sxs-lookup"><span data-stu-id="6c061-250">Copy and paste the current value (Languages) into the Name field.</span></span> <span data-ttu-id="6c061-251">Klikněte na. Ověřování na straně klienta odebere chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="6c061-251">Tab out. Client-side validation removes the error message.</span></span>

![Oddělení upravit stránku chybová zpráva](concurrency/_static/cv.png)

<span data-ttu-id="6c061-253">Klikněte na tlačítko **Uložit** znovu.</span><span class="sxs-lookup"><span data-stu-id="6c061-253">Click **Save** again.</span></span> <span data-ttu-id="6c061-254">Hodnota, kterou jste zadali na kartě druhý prohlížeče je uložit.</span><span class="sxs-lookup"><span data-stu-id="6c061-254">The value you entered in the second browser tab is saved.</span></span> <span data-ttu-id="6c061-255">Zobrazí uložené hodnoty na stránce indexu.</span><span class="sxs-lookup"><span data-stu-id="6c061-255">You see the saved values in the Index page.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="6c061-256">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="6c061-256">Update the Delete page</span></span>

<span data-ttu-id="6c061-257">Aktualizace modelu odstranění stránek s následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6c061-257">Update the Delete page model with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Departments/Delete.cshtml.cs)]

<span data-ttu-id="6c061-258">Odstranit stránky rozpozná souběžnosti je v konfliktu, pokud entity se změnila po se načetla.</span><span class="sxs-lookup"><span data-stu-id="6c061-258">The Delete page detects concurrency conflicts when the entity has changed after it was fetched.</span></span> <span data-ttu-id="6c061-259">`Department.RowVersion`verze řádku je, když se načetla entity.</span><span class="sxs-lookup"><span data-stu-id="6c061-259">`Department.RowVersion` is the row version when the entity was fetched.</span></span> <span data-ttu-id="6c061-260">Když EF základní vytváří příkaz SQL odstranit, obsahuje klauzuli WHERE s `RowVersion`.</span><span class="sxs-lookup"><span data-stu-id="6c061-260">When EF Core creates the SQL DELETE command, it includes a WHERE clause with `RowVersion`.</span></span> <span data-ttu-id="6c061-261">Pokud vliv na výsledky příkazu SQL odstranit v nulový počet řádků:</span><span class="sxs-lookup"><span data-stu-id="6c061-261">If the SQL DELETE command results in zero rows affected:</span></span>

* <span data-ttu-id="6c061-262">`RowVersion` V SQL odstranit příkaz neodpovídá `RowVersion` v databázi.</span><span class="sxs-lookup"><span data-stu-id="6c061-262">The `RowVersion` in the SQL DELETE command doesn't match `RowVersion` in the DB.</span></span>
* <span data-ttu-id="6c061-263">Je vyvolána výjimka DbUpdateConcurrencyException.</span><span class="sxs-lookup"><span data-stu-id="6c061-263">A DbUpdateConcurrencyException exception is thrown.</span></span>
* <span data-ttu-id="6c061-264">`OnGetAsync`je volána `concurrencyError`.</span><span class="sxs-lookup"><span data-stu-id="6c061-264">`OnGetAsync` is called with the `concurrencyError`.</span></span>

### <a name="update-the-delete-page"></a><span data-ttu-id="6c061-265">Odstranit stránku aktualizace</span><span class="sxs-lookup"><span data-stu-id="6c061-265">Update the Delete page</span></span>

<span data-ttu-id="6c061-266">Aktualizace *Pages/Departments/Delete.cshtml* následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="6c061-266">Update *Pages/Departments/Delete.cshtml* with the following code:</span></span>

[!code-html[](intro/samples/cu/Pages/Departments/Delete.cshtml?highlight=1,10,36,51)]


<span data-ttu-id="6c061-267">Předchozí kód provede tyto změny:</span><span class="sxs-lookup"><span data-stu-id="6c061-267">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="6c061-268">Aktualizace `page` direktivy z `@page` k `@page "{id:int}"`.</span><span class="sxs-lookup"><span data-stu-id="6c061-268">Updates the `page` directive from `@page` to `@page "{id:int}"`.</span></span>
* <span data-ttu-id="6c061-269">Přidá chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="6c061-269">Adds an error message.</span></span>
* <span data-ttu-id="6c061-270">Nahradí FullName v FirstMidName **správce** pole.</span><span class="sxs-lookup"><span data-stu-id="6c061-270">Replaces FirstMidName with FullName in the **Administrator** field.</span></span>
* <span data-ttu-id="6c061-271">Změny `RowVersion` zobrazíte poslední bajt.</span><span class="sxs-lookup"><span data-stu-id="6c061-271">Changes `RowVersion` to display the last byte.</span></span>
* <span data-ttu-id="6c061-272">Přidá verze skrytá řádku.</span><span class="sxs-lookup"><span data-stu-id="6c061-272">Adds a hidden row version.</span></span> <span data-ttu-id="6c061-273">`RowVersion`je nutné přidat tak post zpět váže hodnotu.</span><span class="sxs-lookup"><span data-stu-id="6c061-273">`RowVersion` must be added so post back binds the value.</span></span>

### <a name="test-concurrency-conflicts-with-the-delete-page"></a><span data-ttu-id="6c061-274">Test je v konfliktu s stránce odstranit souběžnosti</span><span class="sxs-lookup"><span data-stu-id="6c061-274">Test concurrency conflicts with the Delete page</span></span>

<span data-ttu-id="6c061-275">Vytvořte testovací oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c061-275">Create a test department.</span></span>

<span data-ttu-id="6c061-276">Otevřete dvě instance prohlížeče odstranit na testovací oddělení:</span><span class="sxs-lookup"><span data-stu-id="6c061-276">Open two browsers instances of Delete on the test department:</span></span>

* <span data-ttu-id="6c061-277">Spusťte aplikaci a vyberte oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c061-277">Run the app and select Departments.</span></span>
* <span data-ttu-id="6c061-278">Klikněte pravým tlačítkem myši **odstranit** hypertextový odkaz pro test oddělení a vyberte **otevřít na nové kartě**.</span><span class="sxs-lookup"><span data-stu-id="6c061-278">Right-click the **Delete** hyperlink for the test department and select **Open in new tab**.</span></span>
* <span data-ttu-id="6c061-279">Klikněte **upravit** hypertextový odkaz pro test oddělení.</span><span class="sxs-lookup"><span data-stu-id="6c061-279">Click the **Edit** hyperlink for the test department.</span></span>

<span data-ttu-id="6c061-280">Karty dvě prohlížeč zobrazí stejné informace.</span><span class="sxs-lookup"><span data-stu-id="6c061-280">The two browser tabs display the same information.</span></span>

<span data-ttu-id="6c061-281">Nároky na první kartě prohlížeče a klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="6c061-281">Change the budget in the first browser tab and click **Save**.</span></span>

<span data-ttu-id="6c061-282">Prohlížeč zobrazí indexovou stránku s změněné hodnoty a aktualizované rowVersion indikátoru.</span><span class="sxs-lookup"><span data-stu-id="6c061-282">The browser shows the Index page with the changed value and updated rowVersion indicator.</span></span> <span data-ttu-id="6c061-283">Všimněte si aktualizované rowVersion indikátoru, se zobrazí na druhém zpětné volání na jiné kartě.</span><span class="sxs-lookup"><span data-stu-id="6c061-283">Note the updated rowVersion indicator, it is displayed on the second postback in the other tab.</span></span>

<span data-ttu-id="6c061-284">Odstraňte testovací oddělení z druhé karty. Chyba souběžnosti je zobrazení pomocí aktuálních hodnot z databáze.</span><span class="sxs-lookup"><span data-stu-id="6c061-284">Delete the test department from the second tab. A concurrency error is display with the current values from the DB.</span></span> <span data-ttu-id="6c061-285">Kliknutím na tlačítko **odstranit** odstraní entitu, pokud `RowVersion` byl updated.department byl odstraněn.</span><span class="sxs-lookup"><span data-stu-id="6c061-285">Clicking **Delete** deletes the entity, unless `RowVersion` has been updated.department has been deleted.</span></span>

<span data-ttu-id="6c061-286">V tématu [dědičnosti](xref:data/ef-mvc/inheritance) o tom, jak dědit datový model.</span><span class="sxs-lookup"><span data-stu-id="6c061-286">See [Inheritance](xref:data/ef-mvc/inheritance) on how to inherit a data model.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="6c061-287">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="6c061-287">Additional resources</span></span>

* [<span data-ttu-id="6c061-288">Tokeny souběžnosti v EF jádra</span><span class="sxs-lookup"><span data-stu-id="6c061-288">Concurrency Tokens in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/modeling/concurrency)
* [<span data-ttu-id="6c061-289">Zpracování souběžnost v EF jádra</span><span class="sxs-lookup"><span data-stu-id="6c061-289">Handling concurrency in EF Core</span></span>](https://docs.microsoft.com/en-us/ef/core/saving/concurrency)

>[!div class="step-by-step"]
[<span data-ttu-id="6c061-290">Předchozí</span><span class="sxs-lookup"><span data-stu-id="6c061-290">Previous</span></span>](xref:data/ef-rp/update-related-data)