---
title: Visual Studio publikačních profilů pro nasazení aplikace ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit profily publikování v sadě Visual Studio a jejich použití pro správu nasazení aplikací ASP.NET Core do různých cílů.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/21/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 50be5a20f6d927270ef2d9dbc6c1cbf24196978f
ms.sourcegitcommit: 28646e8ca62fb094db1557b5c0c02d5b45531824
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/23/2019
ms.locfileid: "67333419"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a>Visual Studio publikačních profilů pro nasazení aplikace ASP.NET Core

Podle [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) a [Rick Anderson](https://twitter.com/RickAndMSFT)

Tento dokument se zaměřuje na pomocí Visual Studio 2019 nebo později k vytvoření a použití publikačních profilů. Profily publikování vytvořené pomocí sady Visual Studio je možné pomocí nástroje MSBuild a sadě Visual Studio. Pokyny k publikování do Azure najdete v tématu <xref:tutorials/publish-to-azure-webapp-using-vs>.

`dotnet new mvc` Příkaz vytvoří soubor projektu obsahující následující úrovni kořenového adresáře [ \<Projekt > element](/visualstudio/msbuild/project-element-msbuild):

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

Předchozí `<Project>` elementu `Sdk` atribut importuje MSBuild [vlastnosti](/visualstudio/msbuild/msbuild-properties) a [cíle](/visualstudio/msbuild/msbuild-targets) z *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* a *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*v uvedeném pořadí. Výchozím umístěním pro `$(MSBuildSDKsPath)` (s Visual Studio Enterprise. 2019) je *% programfiles (x86) %\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* složky.

`Microsoft.NET.Sdk.Web` (Sada web SDK) závisí na dalších sad SDK, včetně `Microsoft.NET.Sdk` (.NET Core SDK) a `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)). Vlastnosti nástroje MSBuild a cíle, které jsou spojené s každou závislé sady SDK jsou importovány. Publikujte cíle importu příslušné sady cíle založené na metodě publikování použít.

Při načtení projektu nástroje MSBuild nebo Visual Studio provedou tyto akce vysoké úrovně:

* Sestavit projekt
* COMPUTE souborů k publikování
* Publikování souborů do cíle

## <a name="compute-project-items"></a>COMPUTE položky projektu

Při načtení projektu [položky projektu nástroje MSBuild](/visualstudio/msbuild/common-msbuild-project-items) jsou vypočítány (soubory). Typ položky určuje způsob zpracování souboru. Ve výchozím nastavení *.cs* soubory jsou zahrnuty v `Compile` seznam položek. Soubory `Compile` jsou zkompilovány seznam položek.

`Content` Seznamu položek obsahuje soubory, které jsou publikovány kromě výstupy sestavení. Ve výchozím nastavení, soubory odpovídající vzorům `wwwroot\**`, `**\*.config`, a `**\*.json` jsou součástí `Content` seznam položek. Například `wwwroot\**` [model podpory zástupných znaků](https://gruntjs.com/configuring-tasks#globbing-patterns) vyhledá všechny soubory v *wwwroot* složce a jejích podsložkách.

::: moniker range=">= aspnetcore-3.0"

Importuje Web SDK [Razor SDK](xref:razor-pages/sdk). V důsledku toho soubory odpovídající vzorům `**\*.cshtml` a `**\*.razor` jsou taky součástí `Content` seznam položek.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Importuje Web SDK [Razor SDK](xref:razor-pages/sdk). V důsledku toho soubory odpovídající `**\*.cshtml` vzoru jsou taky součástí `Content` seznam položek.

::: moniker-end

Pokud chcete explicitně přidat soubor do seznamu publikovat, přidejte přímo v souboru *.csproj* sdílené, jak je znázorněno [vložených souborů](#include-files) části.

Při výběru **publikovat** tlačítko v sadě Visual Studio nebo při publikování z příkazového řádku:

* Jsou vypočítané vlastnosti/položky (soubory, které jsou potřebné k sestavení).
* **Visual Studio pouze**: Obnovení balíčků NuGet. (Obnovit musí být explicitní uživatelem na rozhraní příkazového řádku).
* Sestavení projektu.
* Publikovat položky se zpracovávají (soubory, které jsou potřebné k publikování).
* Publikování projektu (vypočítané soubory se zkopírují do cílového umístění publikování).

Když se odkazuje projekt ASP.NET Core `Microsoft.NET.Sdk.Web` v souboru projektu *app_offline.htm* soubor umístěn v kořenovém adresáři webové aplikace. Pokud soubor existuje, modul ASP.NET Core řádně ukončí aplikaci, slouží *app_offline.htm* souboru během nasazení. Další informace najdete v tématu [odkaz Konfigurace modul ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).

## <a name="basic-command-line-publishing"></a>Publikování základních příkazového řádku

Příkazový řádek publikování funguje na všech platformách .NET Core podporovány a nevyžaduje, aby Visual Studio. V následujících příkladech, rozhraní příkazového řádku .NET Core pro [dotnet publikovat](/dotnet/core/tools/dotnet-publish) příkaz spustí z adresáře projektu (který obsahuje *.csproj* souboru). Pokud složka projektu není aktuální pracovní adresář, explicitně předejte v cestě k souboru projektu. Příklad:

```console
dotnet publish C:\Webs\Web1
```

Spusťte následující příkazy k vytvoření a publikování webové aplikace:

```console
dotnet new mvc
dotnet publish
```

`dotnet publish` Příkaz vytvoří varianta následující výstup:

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {VERSION} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\{TARGET FRAMEWORK MONIKER}\publish\
```

Složky pro publikování výchozí formát je *bin\Debug\\{MONIKER CÍLOVÉHO rozhraní} \publish\\* . Například *bin\Debug\netcoreapp2.2\publish\\* .

Následující příkaz určuje `Release` sestavení a publikování adresáře:

```console
dotnet publish -c Release -o C:\MyWebs\test
```

`dotnet publish` Volání MSBuild, který vyvolá příkaz `Publish` cíl. Žádné parametry předány `dotnet publish` jsou předávaným do MSBuild. `-c` a `-o` parametry mapování v nástroji MSBuild `Configuration` a `OutputPath` vlastnosti, v uvedeném pořadí.

Vlastnosti nástroje MSBuild mohou být předány některou z následujících formátů:

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

Například následující příkaz publikuje `Release` sestavení do sdílené síťové složky. Sdílené síťové složky se zadaným lomítka ( *//r8/* ) a funguje na všech platformách .NET Core podporovány.

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

Potvrďte, že publikované aplikace pro nasazení neběží. Soubory *publikovat* složky jsou zamčené, když je aplikace spuštěna. Nasazení nebyla vytvořena, protože uzamčena, nelze zkopírovat soubory.

## <a name="publish-profiles"></a>Profily publikování

Tato část používá Visual Studio 2019 nebo novější k vytvoření profilu publikování. Po vytvoření profilu publikování ze sady Visual Studio nebo příkazového řádku je k dispozici. Publikování profilů může zjednodušit proces publikování, a může existovat libovolný počet profilů.

Vytvořte profil publikování v sadě Visual Studio výběrem jedné z následujících cest:

* Klikněte pravým tlačítkem na projekt v **Průzkumníka řešení** a vyberte **publikovat**.
* Vyberte **publikovat {název projektu}** z **sestavení** nabídky.

**Publikovat** kartu Možnosti stránky aplikace se zobrazí. Pokud projekt nemá profil publikování, **vyberte cíl publikování** zobrazí se stránka. Zobrazí se dotaz, vyberte jednu z těchto cílů publikovat:

* Azure App Service
* Azure App Service v Linuxu
* Azure Virtual Machines
* Folder
* Služba IIS, FTP, nasazení webu (pro všechny webové servery)
* Importovat profil

Pokud chcete určit nejvhodnější publikovat cíl, naleznete v tématu [jaké možnosti publikování jsou pro mě nejlepší](/visualstudio/ide/not-in-toc/web-publish-options).

Když **složky** publikovat cíl se vybírá, zadejte cestu ke složce pro ukládání publikovaných prostředků. Výchozí cesta je *bin\\{konfigurace projektu}\\{MONIKER CÍLOVÉHO rozhraní} \publish\\* . Například *bin\Release\netcoreapp2.2\publish\\* . Vyberte **vytvořit profil** tlačítko Dokončit.

Po vytvoření profilu publikování **publikovat** změny obsahu karty. V rozevíracím seznamu se zobrazí nově vytvořený profil. V rozevíracím seznamu níže vyberte **vytvořit nový profil** vytvořte další nový profil.

Nástroj Publikovat sady Visual Studio vytvoří *vlastnosti/PublishProfiles / {název profilu} .pubxml* MSBuild soubor s popisem profilu publikování. *.Pubxml* souboru:

* Obsahuje konfigurační nastavení publikování a je využívána proces publikování.
* Můžete upravit tak, aby vlastní nastavení sestavení a publikování procesu.

Při publikování do Azure cíl, *.pubxml* soubor obsahuje identifikátor vašeho předplatného Azure. S cílovým typem přidává se tento soubor do správy zdrojového kódu se nedoporučuje. Při publikování na cíl mimo Azure, je k vrácení se změnami *.pubxml* souboru.

Se šifrují citlivé údaje (třeba publikovat heslo) na úrovni uživatele nebo počítače. Je uložený v *vlastnosti/PublishProfiles / {název profilu}.pubxml.user* souboru. Protože tento soubor můžete uložit citlivé informace, by neměla být zaškrtnuta do správy zdrojového kódu.

Přehled o tom, jak publikovat webovou aplikaci ASP.NET Core, najdete v části <xref:host-and-deploy/index>. Úlohy MSBuild a cíle plánování pro publikování webové aplikace ASP.NET Core je open source na [aspnet/websdk úložiště](https://github.com/aspnet/websdk).

`dotnet publish` Složku, MSDeploy, můžete použít příkaz a [Kudu](https://github.com/projectkudu/kudu/wiki) publikační profily. Protože MSDeploy nemá podporu pro různé platformy, následující možnosti MSDeploy jsou podporovány pouze na Windows.

**Složka (funguje napříč platformami):**

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

**MSDeploy:**

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

**MSDeploy balíčku:**

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

V předchozích ukázkách, nepředávejte `deployonbuild` k `dotnet publish`.

Další informace najdete v tématu [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).

`dotnet publish` podporuje rozhraní API Kudu k publikování do Azure z jakékoliv platformy. Visual Studio publikování podporuje Kudu rozhraní API, ale je podporovaný modulem WebSDK pro různé platformy publikovat do Azure.

Přidat profil publikování projektu *vlastnosti/PublishProfiles* složka s následujícím obsahem:

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

Spusťte následující příkaz, který zip obsah publikovat a publikujete ji do Azure pomocí rozhraní API Kudu:

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

Při použití profil publikování, nastavte následující vlastnosti MSBuild:

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

Při publikování tak, že profil s názvem *FolderProfile*, lze provést jednu z následujících příkazů:

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

.NET Core CLI [dotnet sestavení](/dotnet/core/tools/dotnet-build) příkaz volání `msbuild` ke spuštění sestavení a publikování procesu. `dotnet build` a `msbuild` příkazy jsou ekvivalentní při předávání ve složce profilu. Při volání metody `msbuild` přímo na Windows, se používá rozhraní .NET Framework verze nástroje MSBuild. Volání `dotnet build` na mimo složku profilu:

* Vyvolá `msbuild`, který používá MSDeploy.
* Důsledkem chyby (i když a systémem Windows). Chcete-li publikovat pomocí profilu bez složky, zavolejte `msbuild` přímo.

Profil publikování následující složka byla vytvořena pomocí sady Visual Studio a publikuje do sdílené síťové složky:

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

V předchozím příkladu:

* `<ExcludeApp_Data>` Vlastnost je k dispozici pouze pro uspokojení požadavku na XML schématu. `<ExcludeApp_Data>` Vlastnost nemá žádný vliv na proces publikování, i v případě *App_Data* složky v kořenové složce projektu. *App_Data* složky neobdrží zvláštní zacházení, stejně jako v projektech ASP.NET 4.x.

* `<LastUsedBuildConfiguration>` Je nastavena na `Release`. Při publikování ze sady Visual Studio, hodnota `<LastUsedBuildConfiguration>` nastavená pomocí hodnoty, když se spustí proces publikování. `<LastUsedBuildConfiguration>` je speciální a nesmí být přepsána nastaveními v importovaný soubor MSBuild. Tato vlastnost může, ale být přepsáno z příkazového řádku pomocí jedné z následujících přístupů.
  * Použití .NET Core CLI:

    ```console
    dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  * Použití nástroje MSBuild:

    ```console
    msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
    ```

  Další informace najdete v tématu [MSBuild: jak nastavit vlastnost konfigurace](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx).

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a>Publikování do koncového bodu MSDeploy z příkazového řádku

Následující příklad používá webovou aplikaci ASP.NET Core, vytvořené pomocí sady Visual Studio s názvem *AzureWebApp*. Profil publikování aplikací Azure se přidá s Visual Studio. Další informace o tom, jak vytvořit profil, najdete v článku [publikační profily](#publish-profiles) oddílu.

Chcete-li nasadit aplikaci pomocí profilu publikování, spusťte `msbuild` příkaz ze sady Visual Studio **Developer Command Prompt**. Je k dispozici v příkazovém řádku *sady Visual Studio* složky **Start** nabídky na hlavním panelu Windows. Pro snadnější přístup, můžete přidat do příkazového řádku **nástroje** nabídky v sadě Visual Studio. Další informace najdete v tématu [Developer Command Prompt pro sadu Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).

Nástroj MSBuild používá následující syntaxe příkazu:

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* POLOŽKY {PATH} &ndash; Cestu k souboru projektu vaší aplikace.
* {PROFIL} &ndash; Název profilu publikování.
* {USERNAME} &ndash; MSDeploy uživatelské jméno. {USERNAME} najdete v profilu publikování.
* {HESLO} &ndash; MSDeploy heslo. Získat {heslo} z *{profil}. PublishSettings* souboru. Stáhněte si *. PublishSettings* soubor z aplikace:
  * **Průzkumník řešení**: Vyberte **zobrazení** > **Cloud Explorer**. Připojení k vašemu předplatnému Azure. Otevřít **App Services**. Klikněte pravým tlačítkem na aplikaci. Vyberte **stáhnout profil publikování**.
  * Azure portal: Vyberte **získat profil publikování** ve webové aplikaci **přehled** panelu.

Následující příklad používá profil publikování s názvem *AzureWebApp – nasazení webu*:

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

Profil publikování můžete také použít s .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) z příkazové okno Windows:

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!IMPORTANT]
> `dotnet msbuild` Příkaz příkaz pro různé platformy a můžete zkompilovat aplikace ASP.NET Core v macOS a Linuxu. Však není schopné nasadit aplikaci do Azure nebo jiné koncové body MSDeploy MSBuild v systému macOS a Linux.

## <a name="set-the-environment"></a>Nastavení prostředí

Zahrnout `<EnvironmentName>` vlastnost v profilu publikování ( *.pubxml*) nebo soubor projektu a nastavení aplikace [prostředí](xref:fundamentals/environments):

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

Pokud budete potřebovat *web.config* transformace (například nastavení proměnné prostředí na základě konfigurace, profil nebo prostředí), najdete v článku <xref:host-and-deploy/iis/transform-webconfig>.

## <a name="exclude-files"></a>Vyloučení souborů

Při publikování webové aplikace ASP.NET Core, jsou zahrnuty následující prostředky:

* Artefakty sestavení
* Složky a soubory odpovídající následujících vzorů podpory zástupných znaků:
  * `**\*.config` (například *web.config*)
  * `**\*.json` (například *appsettings.json*)
  * `wwwroot\**`

Podporuje MSBuild [vzorů podpory zástupných znaků](https://gruntjs.com/configuring-tasks#globbing-patterns). Například následující `<Content>` element potlačí kopírování textu ( *.txt*) soubory *wwwroot\content* složce a jejích podsložkách:

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Předchozí kód lze přidat do profilu publikování nebo *.csproj* souboru. Když se přidá *.csproj* , pravidlo se přidá soubor všechny profily publikování v projektu.

Následující `<MsDeploySkipRules>` element vyloučí všechny soubory *wwwroot\content* složky:

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

`<MsDeploySkipRules>` nedojde k odstranění *přeskočit* cílů nasazení webu. `<Content>` cílové soubory a složky, jsou odstraněny z nasazení webu. Předpokládejme například, že nasazené webové aplikace má následující soubory:

* *Views/Home/About1.cshtml*
* *Views/Home/About2.cshtml*
* *Views/Home/About3.cshtml*

Pokud následující `<MsDeploySkipRules>` prvky jsou přidány, by dojít k odstranění těchto souborů na web pro nasazení.

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

Předchozí `<MsDeploySkipRules>` zabránit prvky *přeskočeno* soubory z nasazení. Tyto soubory neodstraní po nasazení.

Následující `<Content>` element odstraní cílové soubory na web pro nasazení:

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

Pomocí příkazového řádku nasazení s předchozím `<Content>` element vrací varianta následující výstup:

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\{TARGET FRAMEWORK MONIKER}\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a>Soubory k zahrnutí

Následující části osnovy různé přístupy k zahrnutí souboru na čas publikování. [Zahrnutí souboru Obecné](#general-file-inclusion) části používá `DotNetPublishFiles` položky, které zajišťuje publikování souboru cílů v sadě SDK webové. [Selektivní soubor zahrnutí](#selective-file-inclusion) části používá `ResolvedFileToPublish` položky, které zajišťuje publikování souboru cílů v .NET Core SDK. Vzhledem k tomu, že Web SDK závisí na .NET Core SDK, buď položka se dá použít v projektu aplikace ASP.NET Core.

### <a name="general-file-inclusion"></a>Obecné sdílené zahrnutí

Následující příklad `<ItemGroup>` element ukazuje kopírování složky nachází mimo adresář projektu do složky publikovaného webu. Všechny soubory přidané do následující značky `<ItemGroup>` jsou zahrnuté ve výchozím nastavení.

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotNetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotNetPublishFiles>
</ItemGroup>
```

Předchozí kód:

* Lze přidat *.csproj* souboru nebo profil publikování. Pokud je přidána do *.csproj* souboru, je zahrnutý v každé profilu publikování v projektu.
* Deklaruje `_CustomFiles` položku, kterou chcete ukládat soubory odpovídající `Include` model podpory zástupných znaků atributu. *Imagí* složky odkazuje ve vzoru se nachází mimo adresář projektu. A [rezervované vlastnosti](/visualstudio/msbuild/msbuild-reserved-and-well-known-properties)s názvem `$(MSBuildProjectDirectory)`, se přeloží na absolutní cestu k souboru projektu.
* Obsahuje seznam souborů, které mají `DotNetPublishFiles` položky. Ve výchozím nastavení, položka společnosti `<DestinationRelativePath>` element je prázdný. Výchozí hodnota je přepsána v kódu a používá [známá metadata položky](/visualstudio/msbuild/msbuild-well-known-item-metadata) například `%(RecursiveDir)`. Představuje vnitřní text *wwwroot/imagí* složky publikovaného webu.

### <a name="selective-file-inclusion"></a>Zahrnutí selektivní souboru

Ukazuje zvýrazněný kód v následujícím příkladu:

* Kopírování souboru do publikovaného webu se sídlem mimo projekt *wwwroot* složky. Název souboru *ReadMe2.md* zachovaný.
* S výjimkou *wwwroot\Content* složky.
* S výjimkou *Views\Home\About2.cshtml*.

[!code-xml[](visual-studio-publish-profiles/samples/Web1.pubxml?highlight=18-23)]

V předchozím příkladu `ResolvedFileToPublish` položku, jejíž výchozí chování je vždy kopírovat soubory součástí `Include` atribut publikovaného webu. Výchozí chování potlačíte včetně `<CopyToPublishDirectory>` podřízený element s vnitřní text buď `Never` nebo `PreserveNewest`. Příklad:

```xml
<ResolvedFileToPublish Include="..\ReadMe2.md">
  <RelativePath>wwwroot\ReadMe2.md</RelativePath>
  <CopyToPublishDirectory>PreserveNewest</CopyToPublishDirectory>
</ResolvedFileToPublish>
```

Další ukázky nasazení, najdete v článku [Web SDK úložiště Readme](https://github.com/aspnet/websdk).

## <a name="run-a-target-before-or-after-publishing"></a>Spustit cíl před nebo po publikování

Předdefinované `BeforePublish` a `AfterPublish` cíle spustit cíl před nebo za cíl publikování. Přidáte do profilu publikování k protokolování zpráv konzoly před i po publikování následující prvky:

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a>Publikovat na server pomocí nedůvěryhodného certifikátu

Přidat `<AllowUntrustedCertificate>` vlastnost s hodnotou `True` do profilu publikování:

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a>Kudu service

Chcete-li zobrazit soubory v nasazení služby Azure App Service webovou aplikaci, použijte [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service). Připojit `scm` tokenu název webové aplikace. Příklad:

| Adresa URL                                    | Výsledek       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | Webové aplikace      |
| `http://mysite.scm.azurewebsites.net/` | Kudu service |

Vyberte [ladění konzoly](https://github.com/projectkudu/kudu/wiki/Kudu-console) položka nabídky zobrazit, upravit, odstranit nebo přidat soubory.

## <a name="additional-resources"></a>Další zdroje

* [Webu nasadit](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) zjednodušuje nasazení webové aplikace a weby pro servery služby IIS.
* [Úložiště GitHub sady SDK webové](https://github.com/aspnet/websdk/issues): Soubor problémů a požádat o funkce pro nasazení.
* [Publikování webové aplikace v ASP.NET do virtuálního počítače Azure ze sady Visual Studio](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
