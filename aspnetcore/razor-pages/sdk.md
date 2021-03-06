---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: Zjistěte, jak v ASP.NET Core Razor Pages díky psaní kódu zaměřená na stránce scénáře jednodušší a produktivnější než pomocí MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196366"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>Přehled

[!INCLUDE[](~/includes/2.1-SDK.md)] Zahrnuje `Microsoft.NET.Sdk.Razor` sady MSBuild SDK (Razor SDK). Razor SDK:

* Standardizuje prostředí týkající se vytváření, balení a publikování projektů, které obsahují [Razor](xref:mvc/views/razor) souborů pro projekty ASP.NET Core MVC.
* Obsahuje sadu předdefinovaných cílů, vlastností a položek, které umožňují přizpůsobení kompilace souborech Razor.

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Sada Razor SDK zahrnuje `<Content>` element s `Include` atribut nastaven na `**\*.cshtml` model podpory zástupných znaků. Porovnávání souborů k publikování.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Sada Razor SDK zahrnuje `<Content>` prvky s `Include` nastavte atributy na `**\*.cshtml` a `**\*.razor` vzorů podpory zástupných znaků. Porovnávání souborů k publikování.

::: moniker-end

## <a name="prerequisites"></a>Požadavky

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>Použít syntaxi Razor SDK

Většina webových aplikací není nutné explicitně odkazují na sadu SDK Razor.

Použití sady SDK Razor k vytvoření knihovny tříd obsahující zobrazení syntaxe Razor nebo stránky Razor:

* Použití `Microsoft.NET.Sdk.Razor` místo `Microsoft.NET.Sdk`:

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* Obvykle, odkaz na balíček pro `Microsoft.AspNetCore.Mvc` se vyžaduje pro příjem Další závislosti, které jsou potřebné k sestavení a kompilace Razor Pages a zobrazeními Razor. Minimálně byste vašeho projektu přidat odkazy na balíčky do:

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  `Microsoft.AspNetCore.Razor.Design` Balíček poskytuje Razor kompilace úlohy a cíle projektu.

  Předchozí balíčky jsou součástí `Microsoft.AspNetCore.Mvc`. Následující kód ukazuje soubor projektu, který používá sada Razor SDK k sestavení souborů Razor pro aplikace ASP.NET Core Razor Pages:
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design` a `Microsoft.AspNetCore.Mvc.Razor.Extensions` jsou součástí balíčků [Microsoft.AspNetCore.App Microsoft.aspnetcore.all](xref:fundamentals/metapackage-app). Ale bez verze `Microsoft.AspNetCore.App` odkaz na balíček poskytuje Microsoft.aspnetcore.all aplikaci, která neobsahuje nejnovější verzi `Microsoft.AspNetCore.Razor.Design`. Projekty musí odkazovat na konzistentní verzi `Microsoft.AspNetCore.Razor.Design` (nebo `Microsoft.AspNetCore.Mvc`) tak, aby byly zahrnuty nejnovější opravy čas sestavení pro syntaxi Razor. Další informace najdete v tématu [tento problém Githubu](https://github.com/aspnet/Razor/issues/2553).

::: moniker-end

### <a name="properties"></a>Vlastnosti

Následující vlastnosti řídit chování sady SDK pro syntaxi Razor jako součást sestavení projektu:

* `RazorCompileOnBuild` &ndash; Když `true`, zkompiluje a vysílá Razor sestavení jako součást sestavení projektu. Výchozí hodnota je `true`.
* `RazorCompileOnPublish` &ndash; Když `true`, zkompiluje a vysílá Razor sestavení jako součást publikování projektu. Výchozí hodnota je `true`.

Vlastnosti a položky v následující tabulce se používají ke konfiguraci vstupů a výstupů Razor SDK.

| Položky | Popis |
| ----- | ----------- |
| `RazorGenerate` | Položka elementy ( *.cshtml* soubory), které jsou vstupy do cíle generování kódu. |
| `RazorCompile` | Položka elementy ( *.cs* soubory), které jsou vstupy do cíle kompilace Razor. Použijte tento `ItemGroup` zadat další soubory se zkompiluje do sestavení Razor. |
| `RazorTargetAssemblyAttribute` | Položka prvků, které slouží ke kódu generovat atributy pro sestavení Razor. Příklad:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | Položky elementy přidané jako vložené prostředky do generovaného sestavení Razor. |

| Vlastnost | Popis |
| -------- | ----------- |
| `RazorTargetName` | Název souboru (bez přípony) sestavení vytvořené metodou Razor. | 
| `RazorOutputPath` | Výstupní adresář Razor. |
| `RazorCompileToolset` | Slouží k určení sady nástrojů použitý k vytvoření sestavení Razor. Platné hodnoty jsou `Implicit`, `RazorSDK`, a `PrecompilationTool`. |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | Výchozí hodnota je `true`. Když `true`, zahrnuje *web.config*, *.json*, a *.cshtml* soubory jako obsah v projektu. Pokud je k dispozici prostřednictvím `Microsoft.NET.Sdk.Web`, soubory pod *wwwroot* a konfigurační soubory jsou zahrnuté také. |
| `EnableDefaultRazorGenerateItems` | Když `true`, zahrnuje *.cshtml* souborů z `Content` položky v `RazorGenerate` položky. |
| `GenerateRazorTargetAssemblyInfo` | Když `true`, generuje *.cs* soubor, který obsahuje atributy určené `RazorAssemblyAttribute` a obsahuje soubor ve výstupu kompilace. |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | Když `true`, přidá výchozí sadu atributů sestavení, které mají `RazorAssemblyAttribute`. |
| `CopyRazorGenerateFilesToPublishDirectory` | Když `true`, kopie `RazorGenerate` položky ( *.cshtml*) soubory do adresáře publikovat. Obvykle nejsou publikované aplikace vyžadovat Razor soubory, pokud se účastní v sestavování v době sestavení nebo publikovat čas. Výchozí hodnota je `false`. |
| `CopyRefAssembliesToPublishDirectory` | Když `true`, zkopírovat odkaz na sestavení položky do adresáře publikovat. Obvykle nejsou publikované aplikace vyžadovat referenční sestavení, dojde v okamžiku sestavení nebo publikovat čas kompilace Razor. Nastavte na `true` Pokud publikované aplikace vyžaduje kompilace modulu runtime. Například nastavte hodnotu na `true` Pokud aplikace změní *.cshtml* soubory za běhu nebo používá vložený zobrazení. Výchozí hodnota je `false`. |
| `IncludeRazorContentInPack` | Když `true`, všechny položky obsahu Razor ( *.cshtml* soubory) jsou označeny k zahrnutí vygenerovaný balíček NuGet. Výchozí hodnota je `false`. |
| `EmbedRazorGenerateSources` | Když `true`, přidá RazorGenerate ( *.cshtml*) položky jako vložené soubory do generovaného sestavení Razor. Výchozí hodnota je `false`. |
| `UseRazorBuildServer` | Když `true`, využívá proces serveru trvalé sestavení přesměrovat pracovní generování kódu. Výchozí hodnota je hodnota `UseSharedCompilation`. |

Další informace o vlastnostech najdete v tématu [vlastnosti nástroje MSBuild](/visualstudio/msbuild/msbuild-properties).

### <a name="targets"></a>Cíle

Sada Razor SDK definuje dva hlavní cíle:

* `RazorGenerate` &ndash; Generuje kód *.cs* souborů z `RazorGenerate` položky elementy. Použití `RazorGenerateDependsOn` vlastnosti a určit další cílí, který můžete spustit před nebo po tento cíl.
* `RazorCompile` &ndash; Zkompiluje generované *.cs* soubory v sestavení Razor. Použití `RazorCompileDependsOn` zadat další cíle, které můžete spustit před nebo po tento cíl.

### <a name="runtime-compilation-of-razor-views"></a>Kompilace modulu runtime zobrazení Razor

* Ve výchozím nastavení nebude sada Razor SDK publikovat referenční sestavení, které jsou nutné k provedení kompilace modulu runtime. Výsledkem je selhání kompilace při aplikační model využívá kompilace modulu runtime&mdash;například aplikace používá vložený zobrazení nebo změny zobrazení po publikování aplikace. Nastavte `CopyRefAssembliesToPublishDirectory` k `true` můžete pokračovat v publikování referenční sestavení.

* Pro webovou aplikaci, ujistěte se vaše aplikace cílí `Microsoft.NET.Sdk.Web` SDK.

## <a name="razor-language-version"></a>Syntaxi Razor verze jazyka

Při cílení `Microsoft.NET.Sdk.Web` Razor jazykovou verzi sady SDK, je odvozen z cílovou verzi rozhraní framework aplikace. Pro projekty cílené na `Microsoft.NET.Sdk.Razor` SDK nebo ve výjimečných případech, že aplikace vyžaduje jinou verzi jazyka Razor než odvozené hodnotu, se dají konfigurovat na verzi tak, že nastavíte `<RazorLangVersion>` vlastnost v souboru projektu vaší aplikace:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a>Další zdroje

* [Dodatky k formátu csproj pro .NET Core](/dotnet/core/tools/csproj)
* [Společné položky projektu nástroje MSBuild](/visualstudio/msbuild/common-msbuild-project-items)
