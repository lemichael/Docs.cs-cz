---
title: Opakovaně použitelné Razor uživatelského rozhraní v knihovnách tříd pomocí ASP.NET Core
author: Rick-Anderson
description: Vysvětluje, jak vytvářet opakovaně použitelné uživatelské rozhraní Razor pomocí částečných zobrazení v knihovně tříd v ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 06/28/2019
ms.custom: mvc, seodec18
uid: razor-pages/ui-class
ms.openlocfilehash: d59f643a23b48ccbddf498ef534ee8432b010f40
ms.sourcegitcommit: 6d9cf728465cdb0de1037633a8b7df9a8989cccb
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/28/2019
ms.locfileid: "67463259"
---
# <a name="create-reusable-ui-using-the-razor-class-library-project-in-aspnet-core"></a>Vytvoření opakovaně použitelné uživatelské rozhraní v ASP.NET Core pomocí projektu knihovny tříd Razor

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

Zobrazení syntaxe Razor, stránky, řadiče, stránka modely [Razor komponenty](xref:blazor/class-libraries), [zobrazení součástí](xref:mvc/views/view-components), a datových modelů může být sestaven do knihovny tříd Razor (RCL). RCL můžete zabalit a znovu použít. Aplikace můžete zahrnout RCL a přepsání, zobrazení a stránky, které obsahuje. Při zobrazení, částečná zobrazení nebo stránky Razor se nachází v webové aplikace a RCL kód Razor ( *.cshtml* soubor) na webu aplikace má přednost.

Tato funkce vyžaduje [!INCLUDE[](~/includes/2.1-SDK.md)]

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) ([stažení](xref:index#how-to-download-a-sample))

## <a name="create-a-class-library-containing-razor-ui"></a>Vytvoření knihovny tříd obsahující Razor uživatelského rozhraní

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Ze sady Visual Studio **souboru** nabídce vyberte možnost **nový** > **projektu**.
* Vyberte **webová aplikace ASP.NET Core**.
* Název knihovny (například "RazorClassLib") > **OK**. Aby se zabránilo kolize názvů souborů s knihovnou generované zobrazení, ujistěte se ale nekončí název knihovny v `.Views`.
* Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.
* Vyberte **knihovny tříd Razor** > **OK**.

RCL má následující soubor projektu:

[!code-xml[Main](ui-class/samples/cli/RazorUIClassLib/RazorUIClassLib.csproj)]

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Z příkazového řádku, spusťte `dotnet new razorclasslib`. Příklad:

```console
dotnet new razorclasslib -o RazorUIClassLib
```

Další informace najdete v tématu [dotnet nové](/dotnet/core/tools/dotnet-new). Aby se zabránilo kolize názvů souborů s knihovnou generované zobrazení, ujistěte se ale nekončí název knihovny v `.Views`.

---

Přidáte soubory Razor RCL.

Šablony ASP.NET Core předpokládat RCL obsah je *oblasti* složky. V tématu [rozložení stránek RCL](#afs) k vytvoření obsahu v RCL, který zpřístupňuje `~/Pages` spíše než `~/Areas/Pages`.

## <a name="referencing-rcl-content"></a>Odkazování na obsah RCL

RCL lze odkazovat pomocí:

* Balíček NuGet. V tématu [balíčky NuGet vytváření](/nuget/create-packages/creating-a-package) a [se příkaz dotnet add package](/dotnet/core/tools/dotnet-add-package) a [vytvoření a publikování balíčku NuGet](/nuget/quickstart/create-and-publish-a-package-using-visual-studio).
* *{ProjectName} .csproj*. Zobrazit [dotnet-přidat odkaz na](/dotnet/core/tools/dotnet-add-reference).

## <a name="walkthrough-create-an-rcl-project-and-use-from-a-razor-pages-project"></a>Návod: Vytvoření projektu aplikace RCL a použití z projektu pro stránky Razor

Můžete stáhnout [dokončený projekt](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/ui-class/samples) a otestovat ho namísto jeho vytvoření. Vzorku ke stažení obsahuje další kódu a odkazy, které usnadňuje testování projektu. Zanecháte zpětnou vazbu v [tento problém Githubu](https://github.com/aspnet/AspNetCore.Docs/issues/6098) s komentáře na stažení ukázky a podrobné pokyny.

### <a name="test-the-download-app"></a>Testování aplikace ke stažení

Pokud jste nestáhli dokončené aplikace a by místo toho vytvořte projekt návodu, pokračujte [další části](#create-an-rcl).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Otevřít *.sln* souboru v sadě Visual Studio. Spusťte aplikaci.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Z příkazového řádku v *rozhraní příkazového řádku* adresáře, vytvářet RCL a webové aplikace.

```console
dotnet build
```

Přesunout *WebApp1* adresáře a spusťte aplikaci:

```console
dotnet run
```

---

Postupujte podle pokynů v [WebApp1 testu](#test)

## <a name="create-an-rcl"></a>Vytvoření RCL

V této části se vytvoří RCL. Soubory Razor jsou přidány do RCL.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Vytvoření projektu RCL:

* Ze sady Visual Studio **souboru** nabídce vyberte možnost **nový** > **projektu**.
* Vyberte **webová aplikace ASP.NET Core**.
* Pojmenujte aplikaci **RazorUIClassLib** > **OK**.
* Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.
* Vyberte **knihovny tříd Razor** > **OK**.
* Přidejte do ní soubor částečného zobrazení Razor *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml*.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Z příkazového řádku spusťte následující příkaz:

```console
dotnet new razorclasslib -o RazorUIClassLib
dotnet new page -n _Message -np -o RazorUIClassLib/Areas/MyFeature/Pages/Shared
dotnet new viewstart -o RazorUIClassLib/Areas/MyFeature/Pages
```

Předchozí příkazy:

* Vytvoří `RazorUIClassLib` RCL.
* Vytvoří stránku Razor _TEXT a přidá jej RCL. `-np` Parametr vytvoří, aniž by `PageModel`.
* Vytvoří [soubor _ViewStart.cshtml](xref:mvc/views/layout#running-code-before-each-view) soubor a přidá jej RCL.

*Soubor _ViewStart.cshtml* soubor je vyžadován pro rozložení stránky Razor projektu (která se přidá v další části).

---

### <a name="add-razor-files-and-folders-to-the-project"></a>Přidání Razor souborů a složek do projektu

* Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* následujícím kódem:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml)]

* Nahraďte kód v *RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml* následujícím kódem:

[!code-cshtml[Main](ui-class/samples/cli/RazorUIClassLib/Areas/MyFeature/Pages/Page1.cshtml)]

`@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` je potřeba používat částečné zobrazení (`<partial name="_Message" />`). Místo včetně `@addTagHelper` direktiv, můžete přidat *_ViewImports.cshtml* souboru. Příklad:

```console
dotnet new viewimports -o RazorUIClassLib/Areas/MyFeature/Pages
```

Další informace o *_ViewImports.cshtml*, naleznete v tématu [import sdílených direktivy](xref:mvc/views/layout#importing-shared-directives)

* Sestavení knihovny tříd pro ověření, že zde nejsou žádné chyby kompilátoru:

```console
dotnet build RazorUIClassLib
```

Výstup sestavení obsahuje *RazorUIClassLib.dll* a *RazorUIClassLib.Views.dll*. *RazorUIClassLib.Views.dll* obsahuje kompilovaný obsah Razor.

### <a name="use-the-razor-ui-library-from-a-razor-pages-project"></a>Použití knihovny uživatelského rozhraní Razor z projektu pro stránky Razor

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Vytvoření webové aplikace stránky Razor:

* Z **Průzkumníka řešení**, klikněte pravým tlačítkem na řešení > **přidat** >  **nový projekt**.
* Vyberte **webová aplikace ASP.NET Core**.
* Pojmenujte aplikaci **WebApp1**.
* Ověřte **ASP.NET Core 2.1** nebo novější je vybrána.
* Vyberte **webovou aplikaci** > **OK**.

* Z **Průzkumníka řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **nastavit jako spouštěný projekt**.
* Z **Průzkumníka řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **závislosti sestavení** > **závislosti projektu**.
* Zkontrolujte **RazorUIClassLib** jako závislost **WebApp1**.
* Z **Průzkumníka řešení**, klikněte pravým tlačítkem na **WebApp1** a vyberte **přidat** > **odkaz**.
* V **správce odkazů** dialogové okno Kontrola **RazorUIClassLib** > **OK**.

Spusťte aplikaci.

# <a name="net-core-clitabnetcore-cli"></a>[Rozhraní příkazového řádku .NET Core](#tab/netcore-cli)

Vytvoření webová aplikace Razor Pages a soubor řešení obsahující aplikace Razor Pages a RCL:

```console
dotnet new webapp -o WebApp1
dotnet new sln
dotnet sln add WebApp1
dotnet sln add RazorUIClassLib
dotnet add WebApp1 reference RazorUIClassLib
```

Sestavení a spuštění webové aplikace:

```console
cd WebApp1
dotnet run
```

---

<a name="test"></a>

### <a name="test-webapp1"></a>WebApp1 testu

Ověřte, zda že knihovny tříd Razor uživatelského rozhraní se používá:

* Přejděte do `/MyFeature/Page1`.

## <a name="override-views-partial-views-and-pages"></a>Přepsání, zobrazení, částečná zobrazení a stránky

Při zobrazení, částečná zobrazení nebo stránky Razor se nachází v webové aplikace a RCL kód Razor ( *.cshtml* soubor) na webu aplikace má přednost. Například přidejte *WebApp1/Areas/MyFeature/Pages/Page1.cshtml* k WebApp1, a Page1 v WebApp1 přednost Page1 v RCL.

Ve vzorku ke stažení, přejmenujte *WebApp1/oblasti/MyFeature2* k *WebApp1/oblasti/MyFeature* otestovat prioritu.

Kopírovat *RazorUIClassLib/Areas/MyFeature/Pages/Shared/_Message.cshtml* částečné zobrazení k *WebApp1/Areas/MyFeature/Pages/Shared/_Message.cshtml*. Aktualizace značky k označení nového umístění. Sestavení a spuštění aplikace a zkontrolujte, že verze aplikace s částečným se používá.

<a name="afs"></a>

### <a name="rcl-pages-layout"></a>Rozložení stránek RCL

K odkazu RCL obsah, jako by šlo součást webové aplikace *stránky* složku, vytvořte projekt RCL s následující strukturou souboru:

* *RazorUIClassLib/stránky*
* *RazorUIClassLib/stránek/Shared*

Předpokládejme, že *RazorUIClassLib/stránek/Shared* obsahuje dva soubory částečné: *_Header.cshtml* a *_Footer.cshtml*. `<partial>` Značky může být přidán do *_Layout.cshtml* souboru:

```cshtml
<body>
  <partial name="_Header">
  @RenderBody()
  <partial name="_Footer">
</body>
```

::: moniker range=">= aspnetcore-3.0"

## <a name="create-an-rcl-with-static-assets"></a>Vytvoření RCL pomocí statické prostředky

RCL může vyžadovat doprovodná statické prostředky, které lze odkazovat pomocí náročné aplikace RCL. ASP.NET Core je umožněno vytvoření RCLs, které zahrnují statické prostředky, které jsou k dispozici pro využívání aplikaci.

Pokud chcete zahrnout jako součást RCL doprovodná prostředky, vytvořte *wwwroot* složku v knihovně tříd a zahrnout všechny požadované soubory v této složce.

Při balení RCL, všechny doprovodná assety *wwwroot* složky jsou součástí balíčku automaticky a jsou k dispozici pro odkazování na balíček aplikace.

### <a name="consume-content-from-a-referenced-rcl"></a>Využívat obsah od odkazované RCL

Soubory obsažené v *wwwroot* spotřebitelskou aplikaci v rámci předpona, která jsou vystaveny složky RCL `_content/{LIBRARY NAME}/`. `{LIBRARY NAME}` název projektu knihovny je převeden na malá písmena s dobami (`.`) odebrat. Například knihovnu s názvem *Razor.Class.Lib* výsledků v cestě na statický obsah na `_content/razorclasslib/`.

Využívání aplikace odkazuje na statické prostředky poskytovaných knihovnou s `<script>`, `<style>`, `<img>`a další značky HTML. Využívání aplikace musí mít [statického souboru podpory](xref:fundamentals/static-files) povolena.

### <a name="multi-project-development-flow"></a>Vývoj pro více projektů toku

Při spuštění aplikaci:

* Prostředky v RCL zůstat v jejich původním složek. Prostředky nejsou přesunuta do používání aplikace.
* Všechny změny v rámci RCL *wwwroot* složky se projeví v spotřebitelskou aplikaci po znovu sestaví RCL a bez nutnosti opětovného sestavení spotřebitelskou aplikací.

Při vytváření RCL, je vytvořen manifestu, který popisuje umístění statický webový prostředek. Používání aplikací přečte manifest za běhu využívat prostředky z odkazovaných projektů a balíčky. Při přidání nového prostředku do RCL, RCL musí znovu vytvořit k aktualizaci manifestu náročné aplikace mohli získat přístup ke nový prostředek.

### <a name="publish"></a>Publikování

Při publikování aplikace companion assety z všechny odkazované projekty a balíčky jsou zkopírovány do *wwwroot* složku publikované aplikace v rámci `_content/{LIBRARY NAME}/`.

::: moniker-end
