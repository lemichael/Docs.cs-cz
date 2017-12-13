---
title: "Přidání ověřování"
author: rick-anderson
description: "Postup přidání ověření do stránky Razor"
keywords: "ASP.NET Core, ověření, DataAnnotations, Razor, stránky Razor"
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 302e3077e8cf1cc3b145fcb4ba2ff677023d1524
ms.sourcegitcommit: c9658c0db446f7cb2e443f62b00cf773bed545fa
ms.translationtype: HT
ms.contentlocale: cs-CZ
ms.lasthandoff: 09/30/2017
---
# <a name="adding-validation-to-a-razor-page"></a><span data-ttu-id="abcda-104">Přidání ověřování na stránku Razor</span><span class="sxs-lookup"><span data-stu-id="abcda-104">Adding validation to a Razor Page</span></span>

<span data-ttu-id="abcda-105">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="abcda-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="abcda-106">V této části ověření logiku přidán `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="abcda-106">In this section validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="abcda-107">Pravidla ověřování se vynucují vždy, když uživatel vytvoří nebo upraví film.</span><span class="sxs-lookup"><span data-stu-id="abcda-107">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="abcda-108">Ověřování</span><span class="sxs-lookup"><span data-stu-id="abcda-108">Validation</span></span>

<span data-ttu-id="abcda-109">Klíčovým principem vývoj softwaru nazývá [SUCHÝCH](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**neměnit **R**epeat **Y**ourself").</span><span class="sxs-lookup"><span data-stu-id="abcda-109">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="abcda-110">Stránky Razor podporuje vývoj funkce je zadán jednou, kde se projeví v celé aplikaci.</span><span class="sxs-lookup"><span data-stu-id="abcda-110">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="abcda-111">SUCHÉHO může pomoci snížit objem kódu v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="abcda-111">DRY can help reduce the amount of code in an app.</span></span> <span data-ttu-id="abcda-112">SUCHÉHO díky kód náchylné k chybám a usnadňuje testování a udržovat méně chyby.</span><span class="sxs-lookup"><span data-stu-id="abcda-112">DRY makes the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="abcda-113">Podpora ověřování poskytované stránky Razor a Entity Framework je dobrým příkladem SUCHÝCH zásadu.</span><span class="sxs-lookup"><span data-stu-id="abcda-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="abcda-114">Ověřovací pravidla jsou deklarativně určené v jednom místě (ve třídě modelu) a pravidla jsou vynucována everywhere v aplikaci.</span><span class="sxs-lookup"><span data-stu-id="abcda-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

### <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="abcda-115">Přidávání pravidel ověřování modelu filmu</span><span class="sxs-lookup"><span data-stu-id="abcda-115">Adding validation rules to the movie model</span></span>

<span data-ttu-id="abcda-116">Otevřete *Movie.cs* souboru.</span><span class="sxs-lookup"><span data-stu-id="abcda-116">Open the *Movie.cs* file.</span></span> <span data-ttu-id="abcda-117">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) poskytuje integrovanou sadu atributů ověření, které se použijí deklarativně pro třídu nebo vlastnost.</span><span class="sxs-lookup"><span data-stu-id="abcda-117">[DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) provides a built-in set of validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="abcda-118">DataAnnotations také obsahuje formátování atributů, například `DataType` který usnadní formátování a neposkytují ověření.</span><span class="sxs-lookup"><span data-stu-id="abcda-118">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide validation.</span></span>

<span data-ttu-id="abcda-119">Aktualizace `Movie` třída využívat výhod `Required`, `StringLength`, `RegularExpression`, a `Range` atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="abcda-119">Update the `Movie` class to take advantage of the `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc//sample/MvcMovie/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="abcda-120">Atributy ověření Určete chování, která je vynucená na vlastnosti modelu:</span><span class="sxs-lookup"><span data-stu-id="abcda-120">Validation attributes specify behavior that's enforced on model properties:</span></span>

* <span data-ttu-id="abcda-121">`Required` a `MinimumLength` atributy znamená, že vlastnost musí mít hodnotu.</span><span class="sxs-lookup"><span data-stu-id="abcda-121">The `Required` and `MinimumLength` attributes indicate that a property must have a value.</span></span> <span data-ttu-id="abcda-122">Ale nic zabraňuje uživateli v zadávání prázdné znaky, které by vyhovovaly omezení ověření pro typ s možnou hodnotou Null.</span><span class="sxs-lookup"><span data-stu-id="abcda-122">However, nothing prevents a user from entering whitespace to satisfy the validation constraint for a nullable type.</span></span> <span data-ttu-id="abcda-123">Hodnotu Null [typů hodnot](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (například `decimal`, `int`, `float`, a `DateTime`) jsou ze své podstaty potřeba a nepotřebujete `Required` atribut.</span><span class="sxs-lookup"><span data-stu-id="abcda-123">Non-nullable [value types](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span>
* <span data-ttu-id="abcda-124">`RegularExpression` Atribut omezuje znaky, které může uživatel zadat.</span><span class="sxs-lookup"><span data-stu-id="abcda-124">The `RegularExpression` attribute limits the characters that the user can enter.</span></span> <span data-ttu-id="abcda-125">V předchozí kód `Genre` a `Rating` musí používat pouze písmena (prázdné znaky, číslice a speciální znaky nejsou povoleny).</span><span class="sxs-lookup"><span data-stu-id="abcda-125">In the preceding code, `Genre` and `Rating` must use only letters (whitespace, numbers, and special characters aren't allowed).</span></span>
* <span data-ttu-id="abcda-126">`Range` Atribut omezuje hodnotu na zadaném rozsahu.</span><span class="sxs-lookup"><span data-stu-id="abcda-126">The `Range` attribute constrains a value to a specified range.</span></span>
* <span data-ttu-id="abcda-127">`StringLength` Atribut Nastaví maximální délku řetězce a volitelně minimální délku.</span><span class="sxs-lookup"><span data-stu-id="abcda-127">The `StringLength` attribute sets the maximum length of a string, and optionally the minimum length.</span></span> 

<span data-ttu-id="abcda-128">S ověřovacích pravidel automaticky vynucováno ASP.NET Core pomáhá zkontrolujte aplikace robustnější.</span><span class="sxs-lookup"><span data-stu-id="abcda-128">Having validation rules automatically enforced by ASP.NET Core helps make an app more robust.</span></span> <span data-ttu-id="abcda-129">Automatické ověření u modelů pomáhá chránit aplikace, protože nemáte mějte na paměti, aby se projevily při přidání nového kódu.</span><span class="sxs-lookup"><span data-stu-id="abcda-129">Automatic validation on models helps protect the app because you don't have to remember to apply them when new code is added.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="abcda-130">Chyba ověření uživatelského rozhraní v stránky Razor</span><span class="sxs-lookup"><span data-stu-id="abcda-130">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="abcda-131">Spusťte aplikaci a přejděte na stránkách nebo filmy.</span><span class="sxs-lookup"><span data-stu-id="abcda-131">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="abcda-132">Vyberte **vytvořit nový** odkaz.</span><span class="sxs-lookup"><span data-stu-id="abcda-132">Select the **Create New** link.</span></span> <span data-ttu-id="abcda-133">Vyplňte formulář s některé neplatné hodnoty.</span><span class="sxs-lookup"><span data-stu-id="abcda-133">Complete the form with some invalid values.</span></span> <span data-ttu-id="abcda-134">Při ověřování na straně klienta jQuery zjistí chybu, zobrazí chybovou zprávu.</span><span class="sxs-lookup"><span data-stu-id="abcda-134">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![Film zobrazit formulář s několika chybami jQuery ověřování na straně klienta](validation/_static/val.png)

> [!NOTE]
> <span data-ttu-id="abcda-136">Nelze zadat desetinných míst nebo čárkami v `Price` pole.</span><span class="sxs-lookup"><span data-stu-id="abcda-136">You may not be able to enter decimal points or commas in the `Price` field.</span></span> <span data-ttu-id="abcda-137">Pro podporu [k ověřování jQuery](https://jqueryvalidation.org/) v neanglická národní prostředí, které používají čárkou (",") pro desetinné čárky a formát data neanglických USA, musíte provést kroky globalizace aplikace.</span><span class="sxs-lookup"><span data-stu-id="abcda-137">To support [jQuery validation](https://jqueryvalidation.org/) in non-English locales that use a comma (",") for a decimal point, and non US-English date formats, you must take steps to globalize your app.</span></span> <span data-ttu-id="abcda-138">V tématu [další prostředky](#additional-resources) Další informace.</span><span class="sxs-lookup"><span data-stu-id="abcda-138">See [Additional resources](#additional-resources) for more information.</span></span> <span data-ttu-id="abcda-139">Teď zadejte jenom celá čísla, jako je 10.</span><span class="sxs-lookup"><span data-stu-id="abcda-139">For now, just enter whole numbers like 10.</span></span>

<span data-ttu-id="abcda-140">Všimněte si, jak formulář automaticky vykreslí chybovou zprávu ověření v každé pole, která obsahuje neplatnou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="abcda-140">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="abcda-141">Chyby se vynucují straně klienta (pomocí jazyka JavaScript a jQuery) a na straně serveru (Pokud má uživatel JavaScript zakázaná).</span><span class="sxs-lookup"><span data-stu-id="abcda-141">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="abcda-142">Významné výhodou je, že **žádné** změn kódu byly nezbytné na stránkách vytvořit nebo upravit.</span><span class="sxs-lookup"><span data-stu-id="abcda-142">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="abcda-143">Jakmile DataAnnotations byly použité pro model, bylo povoleno ověření uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="abcda-143">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="abcda-144">Stránky Razor vytvořené v tomto kurzu automaticky převzata pravidla ověřování (pomocí atributů ověření ve vlastnostech `Movie` třída modelu).</span><span class="sxs-lookup"><span data-stu-id="abcda-144">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="abcda-145">Ověření testovacího pomocí stránce Upravit stejné ověření platí.</span><span class="sxs-lookup"><span data-stu-id="abcda-145">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="abcda-146">Data formuláře není odeslány na server, dokud nejsou žádné chyby ověření na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="abcda-146">The form data is not posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="abcda-147">Ověření formuláře, které dat není vystavil jeden nebo více z následujících postupů:</span><span class="sxs-lookup"><span data-stu-id="abcda-147">Verify form data is not posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="abcda-148">Chápat přerušení `OnPostAsync` metoda.</span><span class="sxs-lookup"><span data-stu-id="abcda-148">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="abcda-149">Odeslání formuláře (vyberte **vytvořit** nebo **Uložit**).</span><span class="sxs-lookup"><span data-stu-id="abcda-149">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="abcda-150">Bod rozdělení se nikdy přístupů.</span><span class="sxs-lookup"><span data-stu-id="abcda-150">The break point is never hit.</span></span>
* <span data-ttu-id="abcda-151">Použití [nástroj Fiddler](http://www.telerik.com/fiddler).</span><span class="sxs-lookup"><span data-stu-id="abcda-151">Use the [Fiddler tool](http://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="abcda-152">Pomocí nástrojů pro vývojáře prohlížeče pro monitorování síťového provozu.</span><span class="sxs-lookup"><span data-stu-id="abcda-152">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="abcda-153">Ověřování na straně serveru</span><span class="sxs-lookup"><span data-stu-id="abcda-153">Server-side validation</span></span>

<span data-ttu-id="abcda-154">Pokud je zakázáno JavaScript v prohlížeči, odesláním formuláře s chybami bude odeslána na server.</span><span class="sxs-lookup"><span data-stu-id="abcda-154">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="abcda-155">Volitelné, ověřování na straně serveru testu:</span><span class="sxs-lookup"><span data-stu-id="abcda-155">Optional, test server-side validation:</span></span>

* <span data-ttu-id="abcda-156">Zakážete JavaScript v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="abcda-156">Disable JavaScript in the browser.</span></span> <span data-ttu-id="abcda-157">Pokud zakážete nelze JavaScript v prohlížeči, zkuste jiný prohlížeč.</span><span class="sxs-lookup"><span data-stu-id="abcda-157">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="abcda-158">Nastavit bod přerušení `OnPostAsync` metoda vytvoření nebo úprava stránky.</span><span class="sxs-lookup"><span data-stu-id="abcda-158">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="abcda-159">Odeslání formuláře se chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="abcda-159">Submit a form with validation errors.</span></span>
* <span data-ttu-id="abcda-160">Ověřte, zda že stav modelu, který je neplatný:</span><span class="sxs-lookup"><span data-stu-id="abcda-160">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

<span data-ttu-id="abcda-161">Následující kód ukazuje část *Create.cshtml* stránky, který vygeneroval dříve v tomto kurzu.</span><span class="sxs-lookup"><span data-stu-id="abcda-161">The following code shows a portion of the *Create.cshtml* page that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="abcda-162">Použije se stránkami vytvořit a upravit k zobrazení původního formuláře a znovu zobrazit formulář v případě chyby.</span><span class="sxs-lookup"><span data-stu-id="abcda-162">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="abcda-163">[Vstupní značka pomocná](xref:mvc/views/working-with-forms) používá [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) atributů a atributů HTML, které jsou potřebné pro architekturu jQuery ověření na straně klienta vytváří.</span><span class="sxs-lookup"><span data-stu-id="abcda-163">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](https://docs.microsoft.com/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="abcda-164">[Pomocná rutina pro ověření značky](xref:mvc/views/working-with-forms#the-validation-tag-helpers) zobrazí chyby ověření.</span><span class="sxs-lookup"><span data-stu-id="abcda-164">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="abcda-165">V tématu [ověření](xref:mvc/models/validation) Další informace.</span><span class="sxs-lookup"><span data-stu-id="abcda-165">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="abcda-166">Vytvořit a upravit stránky obsahují žádné ověřovací pravidla.</span><span class="sxs-lookup"><span data-stu-id="abcda-166">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="abcda-167">Ověřovací pravidla a řetězce chyby se zadávají pouze v `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="abcda-167">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="abcda-168">Tato pravidla ověření budou automaticky použita pro stránky Razor, který upravit `Movie` modelu.</span><span class="sxs-lookup"><span data-stu-id="abcda-168">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="abcda-169">Když logiku ověření je nutné změnit, se provádí pouze v modelu.</span><span class="sxs-lookup"><span data-stu-id="abcda-169">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="abcda-170">Ověření konzistentně platí v celé aplikaci (logiku ověření je definována v jednom místě).</span><span class="sxs-lookup"><span data-stu-id="abcda-170">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="abcda-171">Ověřování na jednom místě vám pomůže uchovávat kód vyčištění a je jednodušší a aktualizace.</span><span class="sxs-lookup"><span data-stu-id="abcda-171">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="abcda-172">Pomocí atributů datový typ</span><span class="sxs-lookup"><span data-stu-id="abcda-172">Using DataType Attributes</span></span>

<span data-ttu-id="abcda-173">Zkontrolujte `Movie` třídy.</span><span class="sxs-lookup"><span data-stu-id="abcda-173">Examine the `Movie` class.</span></span> <span data-ttu-id="abcda-174">`System.ComponentModel.DataAnnotations` Obor názvů poskytuje atributy formátování kromě integrovanou sadu atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="abcda-174">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="abcda-175">`DataType` Je použit atribut `ReleaseDate` a `Price` vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="abcda-175">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="abcda-176">`DataType` Atributy pouze poskytovat pro modul zobrazení pro zobrazení dat (a poskytuje atributy, jako `<a>` pro adresy URL a `<a href="mailto:EmailAddress.com">` e-mailu).</span><span class="sxs-lookup"><span data-stu-id="abcda-176">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="abcda-177">Použití `RegularExpression` atribut pro ověření formátu dat.</span><span class="sxs-lookup"><span data-stu-id="abcda-177">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="abcda-178">`DataType` Atribut slouží k určení datový typ, který je specifičtější než vnitřní typ databáze.</span><span class="sxs-lookup"><span data-stu-id="abcda-178">The `DataType` attribute is used to specify a data type that is more specific than the database intrinsic type.</span></span> <span data-ttu-id="abcda-179">`DataType`atributy nejsou atributů ověření.</span><span class="sxs-lookup"><span data-stu-id="abcda-179">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="abcda-180">V ukázkové aplikaci se zobrazí pouze datum bez čas.</span><span class="sxs-lookup"><span data-stu-id="abcda-180">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="abcda-181">`DataType` Výčtu poskytuje pro mnoho typů dat, jako je například datum, čas, telefonní číslo, měny, EmailAddress a další.</span><span class="sxs-lookup"><span data-stu-id="abcda-181">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="abcda-182">`DataType` Atributu můžete také povolit aplikace automaticky poskytnout konkrétní typ funkce.</span><span class="sxs-lookup"><span data-stu-id="abcda-182">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="abcda-183">Například `mailto:` může vytvořit odkaz pro `DataType.EmailAddress`.</span><span class="sxs-lookup"><span data-stu-id="abcda-183">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="abcda-184">Selektor datum lze zadat pro `DataType.Date` v prohlížečích podporujících HTML5.</span><span class="sxs-lookup"><span data-stu-id="abcda-184">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="abcda-185">`DataType` Atributy vysílá standardu HTML 5 `data-` (výrazný data dash) atributy, které využívají standardu HTML 5 prohlížeče.</span><span class="sxs-lookup"><span data-stu-id="abcda-185">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="abcda-186">`DataType` Atributy provést **není** žádné ověřování.</span><span class="sxs-lookup"><span data-stu-id="abcda-186">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="abcda-187">`DataType.Date`neurčuje formát data, které se zobrazí.</span><span class="sxs-lookup"><span data-stu-id="abcda-187">`DataType.Date` does not specify the format of the date that is displayed.</span></span> <span data-ttu-id="abcda-188">Ve výchozím nastavení, zobrazí se pole dat podle výchozích formátů podle serveru `CultureInfo`.</span><span class="sxs-lookup"><span data-stu-id="abcda-188">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="abcda-189">`DisplayFormat` Atribut slouží k explicitnímu zadání formát data:</span><span class="sxs-lookup"><span data-stu-id="abcda-189">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="abcda-190">`ApplyFormatInEditMode` Nastavení určuje, že formátování, které bude použito při hodnota je zobrazena pro úpravy.</span><span class="sxs-lookup"><span data-stu-id="abcda-190">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="abcda-191">Toto chování není vhodné pro některá pole.</span><span class="sxs-lookup"><span data-stu-id="abcda-191">You might not want that behavior for some fields.</span></span> <span data-ttu-id="abcda-192">Například v hodnoty měny, pravděpodobně nechcete symbolu měny v úpravy uživatelského rozhraní.</span><span class="sxs-lookup"><span data-stu-id="abcda-192">For example, in currency values, you probably do not want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="abcda-193">`DisplayFormat` Atribut lze použít samostatně, ale obecně je vhodné používat `DataType` atribut.</span><span class="sxs-lookup"><span data-stu-id="abcda-193">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="abcda-194">`DataType` Atribut přenese tak sémantika data a jak vykreslit ho na obrazovce a nabízí následující výhody, které není dostupná s DisplayFormat:</span><span class="sxs-lookup"><span data-stu-id="abcda-194">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="abcda-195">V prohlížeči můžete povolit funkce HTML5 (např. k zobrazení ovládacího prvku kalendář, symbolu měny vhodné národního prostředí, e-mailu odkazy, atd.)</span><span class="sxs-lookup"><span data-stu-id="abcda-195">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="abcda-196">Ve výchozím nastavení bude v prohlížeči vykreslovat data ve správném formátu podle národního prostředí.</span><span class="sxs-lookup"><span data-stu-id="abcda-196">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="abcda-197">`DataType` Atributu můžete povolit rozhraní ASP.NET Core vybrat šablonu pravé pole k vykreslení data.</span><span class="sxs-lookup"><span data-stu-id="abcda-197">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="abcda-198">`DisplayFormat` Pokud používá samotné používá šablonu řetězec.</span><span class="sxs-lookup"><span data-stu-id="abcda-198">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="abcda-199">Poznámka: k ověřování jQuery nefunguje s `Range` atribut a `DateTime`.</span><span class="sxs-lookup"><span data-stu-id="abcda-199">Note: jQuery validation does not work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="abcda-200">Například následující kód vždy zobrazí chybu ověřování na straně klienta i v případě, že datum je v zadaném rozsahu:</span><span class="sxs-lookup"><span data-stu-id="abcda-200">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="abcda-201">Je obecně není vhodné zkompilovat pevného data v modely, takže pomocí `Range` atribut a `DateTime` se nedoporučuje.</span><span class="sxs-lookup"><span data-stu-id="abcda-201">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="abcda-202">Následující kód ukazuje kombinování atributy na jeden řádek:</span><span class="sxs-lookup"><span data-stu-id="abcda-202">The following code shows combining attributes on one line:</span></span>

[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDAmult.cs?name=snippet1)]

### <a name="publish-to-azure"></a><span data-ttu-id="abcda-203">Publikování v Azure</span><span class="sxs-lookup"><span data-stu-id="abcda-203">Publish to Azure</span></span>

<span data-ttu-id="abcda-204">V tématu [publikování webové aplikace ASP.NET Core Azure App Service pomocí sady Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) pokyny, jak publikovat tuto aplikaci do Azure.</span><span class="sxs-lookup"><span data-stu-id="abcda-204">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on how to publish this app to Azure.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="abcda-205">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="abcda-205">Additional resources</span></span>

* [<span data-ttu-id="abcda-206">Práce s formuláři</span><span class="sxs-lookup"><span data-stu-id="abcda-206">Working with Forms</span></span>](xref:mvc/views/working-with-forms)
* [<span data-ttu-id="abcda-207">Globalizace a lokalizace</span><span class="sxs-lookup"><span data-stu-id="abcda-207">Globalization and localization</span></span>](xref:fundamentals/localization)
* [<span data-ttu-id="abcda-208">Úvod do pomocné rutiny značky</span><span class="sxs-lookup"><span data-stu-id="abcda-208">Introduction to Tag Helpers</span></span>](xref:mvc/views/tag-helpers/intro)
* [<span data-ttu-id="abcda-209">Vytváření Pomocníci značky</span><span class="sxs-lookup"><span data-stu-id="abcda-209">Authoring Tag Helpers</span></span>](xref:mvc/views/tag-helpers/authoring)

>[!div class="step-by-step"]
<span data-ttu-id="abcda-210">[Předchozí: Přidání nové pole](xref:tutorials/razor-pages/new-field)
[Další: nahrávání souborů](xref:tutorials/razor-pages/uploading-files)</span><span class="sxs-lookup"><span data-stu-id="abcda-210">[Previous: Adding a new field](xref:tutorials/razor-pages/new-field)
[Next: Uploading files](xref:tutorials/razor-pages/uploading-files)</span></span>