<span data-ttu-id="12c2e-101">Přidejte následující vlastnosti pro `Movie` třídy:</span><span class="sxs-lookup"><span data-stu-id="12c2e-101">Add the following properties to the `Movie` class:</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/MovieNoEF.cs?name=snippet_MovieNoEF)]

<span data-ttu-id="12c2e-102">`ID` Pole je vyžadováno pro databázi pro primární klíč.</span><span class="sxs-lookup"><span data-stu-id="12c2e-102">The `ID` field is required by the database for the primary key.</span></span>

<a name="dc"></a>
### <a name="add-a-database-context-class"></a><span data-ttu-id="12c2e-103">Přidání třídy kontextu databáze</span><span class="sxs-lookup"><span data-stu-id="12c2e-103">Add a database context class</span></span>

<span data-ttu-id="12c2e-104">Přidejte následující `DbContext` odvozené třídy s názvem *MovieContext.cs* k *modely* složky:[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span><span class="sxs-lookup"><span data-stu-id="12c2e-104">Add the following `DbContext` derived class named *MovieContext.cs* to the *Models* folder: [!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_sample/RazorPagesMovie/Models/MovieContext.cs)]</span></span>

<span data-ttu-id="12c2e-105">Předchozí kód vytvoří `DbSet` vlastnost pro sadu entit.</span><span class="sxs-lookup"><span data-stu-id="12c2e-105">The preceding code creates a `DbSet` property for the entity set.</span></span> <span data-ttu-id="12c2e-106">V terminologii rozhraní Entity Framework obvykle sadu entit, odpovídá do databázové tabulky a entity odpovídá na řádek v tabulce.</span><span class="sxs-lookup"><span data-stu-id="12c2e-106">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>