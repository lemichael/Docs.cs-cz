---
title: "Kompilace zobrazení syntaxe Razor a předkompilace"
author: rick-anderson
description: "Odkaz na dokument vysvětlením, jak povolit kompilace MVC Razor zobrazení a předkompilaci v aplikacích ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="8e3b4-103">Kompilace zobrazení syntaxe Razor a předkompilaci v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8e3b4-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="8e3b4-104">Podle [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e3b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e3b4-105">Při vyvolání zobrazení zobrazení syntaxe Razor kompilované za běhu.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="8e3b4-106">ASP.NET základní 1.1.0 a vyšší můžete volitelně zkompilovat zobrazení syntaxe Razor a nasadit je do aplikace&mdash;tento proces se označuje jako předkompilaci.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="8e3b4-107">Šablony projektů ASP.NET Core 2.x povolit předkompilaci ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8e3b4-108">Předkompilace zobrazení syntaxe Razor není momentálně k dispozici při provádění [samostatná nasazení (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="8e3b4-109">Tato funkce bude k dispozici pro SCDs po vydání 2.1.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="8e3b4-110">Další informace najdete v tématu [zobrazení kompilace selže, když mezi kompilace pro Linux v systému Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="8e3b4-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="8e3b4-111">Předkompilace aspekty:</span><span class="sxs-lookup"><span data-stu-id="8e3b4-111">Precompilation considerations:</span></span>

* <span data-ttu-id="8e3b4-112">Předkompilace zobrazení následek menší publikované sady a rychlejší čas spuštění.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="8e3b4-113">Soubory Razor předkompilovat zobrazení nelze upravit.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="8e3b4-114">Upravená zobrazení nebudou existovat v publikované sady.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="8e3b4-115">Nasazení předkompilovaných zobrazení:</span><span class="sxs-lookup"><span data-stu-id="8e3b4-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="8e3b4-116">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="8e3b4-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="8e3b4-117">Pokud je cílem vašeho projektu je .NET Framework, obsahovat odkaz na balíček [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="8e3b4-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="8e3b4-118">Pokud je cílem vašeho projektu je .NET Core, jsou nezbytné žádné změny.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="8e3b4-119">Šablony projektů ASP.NET Core 2.x implicitně nastavit `MvcRazorCompileOnPublish` k `true` ve výchozím nastavení, což znamená, můžete z bezpečně odebrat tento uzel *.csproj* souboru.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="8e3b4-120">Pokud dáváte přednost lze odvodit přímo, je není škodu v nastavení `MvcRazorCompileOnPublish` vlastnost `true`.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="8e3b4-121">Následující *.csproj* ukázka označuje toto nastavení:</span><span class="sxs-lookup"><span data-stu-id="8e3b4-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="8e3b4-122">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="8e3b4-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="8e3b4-123">Nastavit `MvcRazorCompileOnPublish` k `true`a obsahovat odkaz na balíček `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="8e3b4-124">Následující *.csproj* ukázka označuje tato nastavení:</span><span class="sxs-lookup"><span data-stu-id="8e3b4-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="8e3b4-125">Příprava aplikace pro [nasazení závislé na framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) spuštěním příkazu například následující v kořenovém adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="8e3b4-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="8e3b4-126">A *< název_projektu >. PrecompiledViews.dll* obsahující kompilované zobrazení syntaxe Razor vytváří při předkompilaci úspěšné.</span><span class="sxs-lookup"><span data-stu-id="8e3b4-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="8e3b4-127">Například následující snímek obrazovky znázorňuje obsah *Index.cshtml* uvnitř *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="8e3b4-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Zobrazení syntaxe Razor uvnitř knihovny DLL](view-compilation/_static/razor-views-in-dll.png)