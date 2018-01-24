---
title: "Zpracování žádostí s řadiči v ASP.NET MVC jádra"
author: ardalis
description: 
ms.author: riande
manager: wpickett
ms.date: 07/03/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/actions
ms.openlocfilehash: cef493fc2010d1c82e5c1dfec85864539252b817
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="handling-requests-with-controllers-in-aspnet-core-mvc"></a><span data-ttu-id="bc0c5-102">Zpracování žádostí s řadiči v ASP.NET MVC jádra</span><span class="sxs-lookup"><span data-stu-id="bc0c5-102">Handling requests with controllers in ASP.NET Core MVC</span></span>

<span data-ttu-id="bc0c5-103">Podle [Steve Smith](https://ardalis.com/) a [Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="bc0c5-103">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="bc0c5-104">Řadiče, akce a výsledky akce jsou základní součástí jak vývojářům vytvářet aplikace pomocí ASP.NET MVC jádra.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-104">Controllers, actions, and action results are a fundamental part of how developers build apps using ASP.NET Core MVC.</span></span>

## <a name="what-is-a-controller"></a><span data-ttu-id="bc0c5-105">Co je řadič?</span><span class="sxs-lookup"><span data-stu-id="bc0c5-105">What is a Controller?</span></span>

<span data-ttu-id="bc0c5-106">Řadič se používá k definování a seskupení sady akcí.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-106">A controller is used to define and group a set of actions.</span></span> <span data-ttu-id="bc0c5-107">Akce (nebo *metoda akce*) je metoda na řadič, která zpracovává požadavky.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-107">An action (or *action method*) is a method on a controller which handles requests.</span></span> <span data-ttu-id="bc0c5-108">Řadiče logicky seskupit podobné akce.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-108">Controllers logically group similar actions together.</span></span> <span data-ttu-id="bc0c5-109">Tato agregace akce umožňuje společné sady pravidel, například směrování, ukládání do mezipaměti a autorizace, které se má použít společně.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-109">This aggregation of actions allows common sets of rules, such as routing, caching, and authorization, to be applied collectively.</span></span> <span data-ttu-id="bc0c5-110">Požadavky jsou namapované na akce prostřednictvím [směrování](xref:mvc/controllers/routing).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-110">Requests are mapped to actions through [routing](xref:mvc/controllers/routing).</span></span>

<span data-ttu-id="bc0c5-111">Podle konvence řadiče třídy:</span><span class="sxs-lookup"><span data-stu-id="bc0c5-111">By convention, controller classes:</span></span>
* <span data-ttu-id="bc0c5-112">Jsou umístěné v kořenové úrovni projektu *řadiče* složky</span><span class="sxs-lookup"><span data-stu-id="bc0c5-112">Reside in the project's root-level *Controllers* folder</span></span>
* <span data-ttu-id="bc0c5-113">Dědit z`Microsoft.AspNetCore.Mvc.Controller`</span><span class="sxs-lookup"><span data-stu-id="bc0c5-113">Inherit from `Microsoft.AspNetCore.Mvc.Controller`</span></span>

<span data-ttu-id="bc0c5-114">Řadič je instantiable třída, ve kterém je aspoň jeden z následujících podmínek true:</span><span class="sxs-lookup"><span data-stu-id="bc0c5-114">A controller is an instantiable class in which at least one of the following conditions is true:</span></span>
* <span data-ttu-id="bc0c5-115">Název třídy je na konci "Controller"</span><span class="sxs-lookup"><span data-stu-id="bc0c5-115">The class name is suffixed with "Controller"</span></span>
* <span data-ttu-id="bc0c5-116">Třídy dědí z třídy, jejíž název je na konci "Controller"</span><span class="sxs-lookup"><span data-stu-id="bc0c5-116">The class inherits from a class whose name is suffixed with "Controller"</span></span>
* <span data-ttu-id="bc0c5-117">Třída je upraven pomocí `[Controller]` atribut</span><span class="sxs-lookup"><span data-stu-id="bc0c5-117">The class is decorated with the `[Controller]` attribute</span></span>

<span data-ttu-id="bc0c5-118">Třída kontroleru nesmí mít přiřazený `[NonController]` atribut.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-118">A controller class must not have an associated `[NonController]` attribute.</span></span>

<span data-ttu-id="bc0c5-119">Postupujte podle řadiče [explicitní závislosti Princip](http://deviq.com/explicit-dependencies-principle/).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-119">Controllers should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/).</span></span> <span data-ttu-id="bc0c5-120">Existuje několik přístupů k implementaci této zásady.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-120">There are a couple approaches to implementing this principle.</span></span> <span data-ttu-id="bc0c5-121">Pokud více akce kontroleru vyžadují stejnou službu, zvažte použití [konstruktor vkládání](xref:mvc/controllers/dependency-injection#constructor-injection) požádat o těchto závislostí.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-121">If multiple controller actions require the same service, consider using [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to request those dependencies.</span></span> <span data-ttu-id="bc0c5-122">Pokud služba je nutná metodou jenom jednu akci, zvažte použití [akce vkládání](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) požádat o závislost.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-122">If the service is needed by only a single action method, consider using [Action Injection](xref:mvc/controllers/dependency-injection#action-injection-with-fromservices) to request the dependency.</span></span>

<span data-ttu-id="bc0c5-123">V rámci **M**odelu -**V**obrazit -**C**ontroller vzor řadič je odpovědná za počáteční zpracování žádosti a vytvoření instance modelu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-123">Within the **M**odel-**V**iew-**C**ontroller pattern, a controller is responsible for the initial processing of the request and instantiation of the model.</span></span> <span data-ttu-id="bc0c5-124">Obecně platí obchodní rozhodnutí, která je třeba provést v rámci modelu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-124">Generally, business decisions should be performed within the model.</span></span>

<span data-ttu-id="bc0c5-125">Řadičem trvá výsledek zpracování modelu (pokud existuje) a vrátí správné zobrazení a jeho přidružené zobrazení dat nebo výsledek volání rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-125">The controller takes the result of the model's processing (if any) and returns either the proper view and its associated view data or the result of the API call.</span></span> <span data-ttu-id="bc0c5-126">Další informace na [přehled ASP.NET Core MVC](xref:mvc/overview) a [Začínáme s ASP.NET MVC jádra a sady Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-126">Learn more at [Overview of ASP.NET Core MVC](xref:mvc/overview) and [Getting started with ASP.NET Core MVC and Visual Studio](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="bc0c5-127">Je řadič *uživatelského rozhraní úrovni* abstrakce.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-127">The controller is a *UI-level* abstraction.</span></span> <span data-ttu-id="bc0c5-128">Jsou jeho zodpovědnosti zajistit data požadavku jsou platné a vybrat, které zobrazení (nebo výsledek rozhraní API) má být vrácen.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-128">Its responsibilities are to ensure request data is valid and to choose which view (or result for an API) should be returned.</span></span> <span data-ttu-id="bc0c5-129">V dobře započítané aplikace nebude obsahovat přímo data přístupu nebo obchodní logiku.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-129">In well-factored apps, it does not directly include data access or business logic.</span></span> <span data-ttu-id="bc0c5-130">Místo toho řadičem deleguje zpracování těchto odpovědnosti Services.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-130">Instead, the controller delegates to services handling these responsibilities.</span></span>

## <a name="defining-actions"></a><span data-ttu-id="bc0c5-131">Definování akcí</span><span class="sxs-lookup"><span data-stu-id="bc0c5-131">Defining Actions</span></span>

<span data-ttu-id="bc0c5-132">Veřejné metody na řadič, s výjimkou označených pomocí `[NonAction]` atribut, akce.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-132">Public methods on a controller, except those decorated with the `[NonAction]` attribute, are actions.</span></span> <span data-ttu-id="bc0c5-133">Parametry akce je vázána na data žádosti a se ověřují pomocí [model vazby](xref:mvc/models/model-binding).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-133">Parameters on actions are bound to request data and are validated using [model binding](xref:mvc/models/model-binding).</span></span> <span data-ttu-id="bc0c5-134">Pro vše, co je model vázán dojde k ověření modelu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-134">Model validation occurs for everything that's model-bound.</span></span> <span data-ttu-id="bc0c5-135">`ModelState.IsValid` Hodnota vlastnosti označuje, zda vazby modelu a ověření bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-135">The `ModelState.IsValid` property value indicates whether model binding and validation succeeded.</span></span>

<span data-ttu-id="bc0c5-136">Metody akce by měl obsahovat logiku pro mapování požadavek na obchodní problém.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-136">Action methods should contain logic for mapping a request to a business concern.</span></span> <span data-ttu-id="bc0c5-137">Problémů obchodního charakteru by měl být obvykle reprezentován jako služby, které řadičem přistupuje prostřednictvím [vkládání závislostí](xref:mvc/controllers/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-137">Business concerns should typically be represented as services that the controller accesses through [dependency injection](xref:mvc/controllers/dependency-injection).</span></span> <span data-ttu-id="bc0c5-138">Akce se mapují pak výsledek akce obchodní do stavu aplikace.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-138">Actions then map the result of the business action to an application state.</span></span>

<span data-ttu-id="bc0c5-139">Akce vrátí nic, ale často vrátit instanci třídy `IActionResult` (nebo `Task<IActionResult>` pro asynchronní metody), která vyvolává odpověď.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-139">Actions can return anything, but frequently return an instance of `IActionResult` (or `Task<IActionResult>` for async methods) that produces a response.</span></span> <span data-ttu-id="bc0c5-140">Metoda akce odpovídá pro výběr *jaký druh odpovědi*.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-140">The action method is responsible for choosing *what kind of response*.</span></span> <span data-ttu-id="bc0c5-141">Výsledek akce *nemá neodpovídá*.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-141">The action result *does the responding*.</span></span>

### <a name="controller-helper-methods"></a><span data-ttu-id="bc0c5-142">Metody Helper kontroleru</span><span class="sxs-lookup"><span data-stu-id="bc0c5-142">Controller Helper Methods</span></span>

<span data-ttu-id="bc0c5-143">Řadiče obvykle dědí [řadič](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), i když to není potřeba.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-143">Controllers usually inherit from [Controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller), although this is not required.</span></span> <span data-ttu-id="bc0c5-144">Odvozování z `Controller` poskytuje přístup k tří kategorií pomocné metody:</span><span class="sxs-lookup"><span data-stu-id="bc0c5-144">Deriving from `Controller` provides access to three categories of helper methods:</span></span>

#### <a name="1-methods-resulting-in-an-empty-response-body"></a><span data-ttu-id="bc0c5-145">1. Metody, což vede prázdný odpovědi</span><span class="sxs-lookup"><span data-stu-id="bc0c5-145">1. Methods resulting in an empty response body</span></span>

<span data-ttu-id="bc0c5-146">Ne `Content-Type` hlavičku HTTP odpovědi je zahrnuté, protože text odpovědi chybí obsah k popisu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-146">No `Content-Type` HTTP response header is included, since the response body lacks content to describe.</span></span>

<span data-ttu-id="bc0c5-147">Existují dva typy výsledků v rámci této kategorie: přesměrování a stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-147">There are two result types within this category: Redirect and HTTP Status Code.</span></span>

* <span data-ttu-id="bc0c5-148">**Kód stavu HTTP**</span><span class="sxs-lookup"><span data-stu-id="bc0c5-148">**HTTP Status Code**</span></span>

    <span data-ttu-id="bc0c5-149">Tento typ vrátí stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-149">This type returns an HTTP status code.</span></span> <span data-ttu-id="bc0c5-150">Několik pomocné metody tohoto typu jsou `BadRequest`, `NotFound`, a `Ok`.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-150">A couple helper methods of this type are `BadRequest`, `NotFound`, and `Ok`.</span></span> <span data-ttu-id="bc0c5-151">Například `return BadRequest();` vytváří 400 stavový kód při spuštění.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-151">For example, `return BadRequest();` produces a 400 status code when executed.</span></span> <span data-ttu-id="bc0c5-152">Když metody, jako `BadRequest`, `NotFound`, a `Ok` jsou přetížený, už kvalifikují jako respondéry stavový kód protokolu HTTP, protože probíhající vyjednávání obsahu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-152">When methods such as `BadRequest`, `NotFound`, and `Ok` are overloaded, they no longer qualify as HTTP Status Code responders, since content negotiation is taking place.</span></span>

* <span data-ttu-id="bc0c5-153">**Redirect**</span><span class="sxs-lookup"><span data-stu-id="bc0c5-153">**Redirect**</span></span>

    <span data-ttu-id="bc0c5-154">Tento typ vrátí přesměrování na akci nebo cílové (pomocí `Redirect`, `LocalRedirect`, `RedirectToAction`, nebo `RedirectToRoute`).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-154">This type returns a redirect to an action or destination (using `Redirect`, `LocalRedirect`, `RedirectToAction`, or `RedirectToRoute`).</span></span> <span data-ttu-id="bc0c5-155">Například `return RedirectToAction("Complete", new {id = 123});` přesměruje na `Complete`, předá anonymní objekt.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-155">For example, `return RedirectToAction("Complete", new {id = 123});` redirects to `Complete`, passing an anonymous object.</span></span>

    <span data-ttu-id="bc0c5-156">Typ výsledku přesměrování se liší od typu stavový kód HTTP především v přidání `Location` hlavičku HTTP odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-156">The Redirect result type differs from the HTTP Status Code type primarily in the addition of a `Location` HTTP response header.</span></span>

#### <a name="2-methods-resulting-in-a-non-empty-response-body-with-a-predefined-content-type"></a><span data-ttu-id="bc0c5-157">2. Metody, které jsou výsledkem neprázdný odpovědi s předdefinované typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-157">2. Methods resulting in a non-empty response body with a predefined content type</span></span>

<span data-ttu-id="bc0c5-158">Většina pomocné metody této kategorie patří `ContentType` vlastnosti, což umožňuje, abyste nastavili `Content-Type` hlavičku odpovědi k popisu text odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-158">Most helper methods in this category include a `ContentType` property, allowing you to set the `Content-Type` response header to describe the response body.</span></span>

<span data-ttu-id="bc0c5-159">Existují dva typy výsledků v rámci této kategorie: [zobrazení](xref:mvc/views/overview) a [formátu odpovědi](xref:mvc/models/formatting).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-159">There are two result types within this category: [View](xref:mvc/views/overview) and [Formatted Response](xref:mvc/models/formatting).</span></span>

* <span data-ttu-id="bc0c5-160">**View**</span><span class="sxs-lookup"><span data-stu-id="bc0c5-160">**View**</span></span>

    <span data-ttu-id="bc0c5-161">Tento typ vrací zobrazení, která používá model k vykreslení HTML.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-161">This type returns a view which uses a model to render HTML.</span></span> <span data-ttu-id="bc0c5-162">Například `return View(customer);` předá zobrazení pro datové vazby modelu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-162">For example, `return View(customer);` passes a model to the view for data-binding.</span></span>

* <span data-ttu-id="bc0c5-163">**Formátovaný odpovědi**</span><span class="sxs-lookup"><span data-stu-id="bc0c5-163">**Formatted Response**</span></span>

    <span data-ttu-id="bc0c5-164">Tento typ vrátí JSON nebo podobné formát exchange dat reprezentovat objekt konkrétním způsobem.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-164">This type returns JSON or a similar data exchange format to represent an object in a specific manner.</span></span> <span data-ttu-id="bc0c5-165">Například `return Json(customer);` serializuje zadaný objekt do formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-165">For example, `return Json(customer);` serializes the provided object into JSON format.</span></span>
    
    <span data-ttu-id="bc0c5-166">Zahrnout další běžné metody tohoto typu `File`, `PhysicalFile`, a `VirtualFile`.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-166">Other common methods of this type include `File`, `PhysicalFile`, and `VirtualFile`.</span></span> <span data-ttu-id="bc0c5-167">Například `return PhysicalFile(customerFilePath, "text/xml");` vrátí popsaného souboru XML `Content-Type` hodnotu hlavičky odpovědi "text/xml".</span><span class="sxs-lookup"><span data-stu-id="bc0c5-167">For example, `return PhysicalFile(customerFilePath, "text/xml");` returns an XML file described by a `Content-Type` response header value of "text/xml".</span></span>

#### <a name="3-methods-resulting-in-a-non-empty-response-body-formatted-in-a-content-type-negotiated-with-the-client"></a><span data-ttu-id="bc0c5-168">3. Metody, které jsou výsledkem text odpovědi neprázdné hodnoty ve formátu v vyjedná se klient pro typ obsahu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-168">3. Methods resulting in a non-empty response body formatted in a content type negotiated with the client</span></span>

<span data-ttu-id="bc0c5-169">Tato kategorie se nazývá lépe **vyjednávání obsahu**.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-169">This category is better known as **Content Negotiation**.</span></span> <span data-ttu-id="bc0c5-170">[Vyjednávání obsahu](xref:mvc/models/formatting#content-negotiation) platí vždy, když akce vrátí [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) typu nebo něco jiného než [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementace.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-170">[Content negotiation](xref:mvc/models/formatting#content-negotiation) applies whenever an action returns an [ObjectResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.objectresult) type or something other than an [IActionResult](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iactionresult) implementation.</span></span> <span data-ttu-id="bc0c5-171">Akce, která vrátí jinou hodnotu než`IActionResult` implementace (například `object`) také vrátí hodnotu formátu odpovědi.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-171">An action that returns a non-`IActionResult` implementation (for example, `object`) also returns a Formatted Response.</span></span>

<span data-ttu-id="bc0c5-172">Některé metody helper tohoto typu zahrnují `BadRequest`, `CreatedAtRoute`, a `Ok`.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-172">Some helper methods of this type include `BadRequest`, `CreatedAtRoute`, and `Ok`.</span></span> <span data-ttu-id="bc0c5-173">Příklady těchto metod `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, a `return Ok(value);`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-173">Examples of these methods include `return BadRequest(modelState);`, `return CreatedAtRoute("routename", values, newobject);`, and `return Ok(value);`, respectively.</span></span> <span data-ttu-id="bc0c5-174">Všimněte si, že `BadRequest` a `Ok` provedení vyjednávání obsahu pouze v případě, že je předaná hodnota; bez předávány hodnotu, místo toho slouží jako typy výsledků stavový kód HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-174">Note that `BadRequest` and `Ok` perform content negotiation only when passed a value; without being passed a value, they instead serve as HTTP Status Code result types.</span></span> <span data-ttu-id="bc0c5-175">`CreatedAtRoute` Metody na druhé straně vždy provede vyjednávání obsahu od jeho přetížení všechny vyžadují předat hodnotu.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-175">The `CreatedAtRoute` method, on the other hand, always performs content negotiation since its overloads all require that a value be passed.</span></span>

### <a name="cross-cutting-concerns"></a><span data-ttu-id="bc0c5-176">Mezi vyjímání otázky</span><span class="sxs-lookup"><span data-stu-id="bc0c5-176">Cross-Cutting Concerns</span></span>

<span data-ttu-id="bc0c5-177">Aplikace obvykle sdílet části svých pracovních postupů.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-177">Applications typically share parts of their workflow.</span></span> <span data-ttu-id="bc0c5-178">Mezi příklady patří aplikaci, která vyžaduje ověřování pro přístup k nákupní košík nebo aplikaci, která ukládá data na některé stránky do mezipaměti.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-178">Examples include an app that requires authentication to access the shopping cart, or an app that caches data on some pages.</span></span> <span data-ttu-id="bc0c5-179">Pokud chcete provést logiku před nebo po metody akce, použijte *filtru*.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-179">To perform logic before or after an action method, use a *filter*.</span></span> <span data-ttu-id="bc0c5-180">Pomocí [filtry](xref:mvc/controllers/filters) na otázky mezi vyjímání, můžete snížit duplikace, což jim umožní postupujte podle [nemáte opakujte sami (suchého) Princip](http://deviq.com/don-t-repeat-yourself/).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-180">Using [Filters](xref:mvc/controllers/filters) on cross-cutting concerns can reduce duplication, allowing them to follow the [Don't Repeat Yourself (DRY) principle](http://deviq.com/don-t-repeat-yourself/).</span></span>

<span data-ttu-id="bc0c5-181">Většina filtrovat atributy, jako například `[Authorize]`, lze použít na úrovni kontroler nebo akce v závislosti na požadované úrovni členitosti.</span><span class="sxs-lookup"><span data-stu-id="bc0c5-181">Most filter attributes, such as `[Authorize]`, can be applied at the controller or action level depending upon the desired level of granularity.</span></span>

<span data-ttu-id="bc0c5-182">Zpracování chyb a ukládání odpovědí do mezipaměti, jsou často mezi vyjímání otázky:</span><span class="sxs-lookup"><span data-stu-id="bc0c5-182">Error handling and response caching are often cross-cutting concerns:</span></span>
   * [<span data-ttu-id="bc0c5-183">Zpracování chyb</span><span class="sxs-lookup"><span data-stu-id="bc0c5-183">Error handling</span></span>](xref:mvc/controllers/filters#exception-filters)
   * [<span data-ttu-id="bc0c5-184">Ukládání odpovědí do mezipaměti</span><span class="sxs-lookup"><span data-stu-id="bc0c5-184">Response Caching</span></span>](xref:performance/caching/response)

<span data-ttu-id="bc0c5-185">Mnoho mezi vyjímání otázky můžete ke zpracování pomocí filtrů nebo vlastní [middleware](xref:fundamentals/middleware).</span><span class="sxs-lookup"><span data-stu-id="bc0c5-185">Many cross-cutting concerns can be handled using filters or custom [middleware](xref:fundamentals/middleware).</span></span>