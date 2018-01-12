---
title: "Hostování v ASP.NET Core"
author: guardrex
description: "Další informace o webového hostitele v ASP.NET Core, která je zodpovědná za spuštění a životního cyklu správy aplikací."
keywords: Web ASP.NET Core hostitele, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 8adc58d67f103e8d1fc8fe197cf392752bdaf660
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/11/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="48d1d-104">Hostování v ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="48d1d-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="48d1d-105">Podle [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="48d1d-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="48d1d-106">Aplikace ASP.NET Core nakonfigurovat a spustit *hostitele*.</span><span class="sxs-lookup"><span data-stu-id="48d1d-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="48d1d-107">Hostitel je zodpovědná za spuštění a životního cyklu správy aplikací.</span><span class="sxs-lookup"><span data-stu-id="48d1d-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="48d1d-108">Minimálně hostitele nakonfiguruje server a kanálu zpracování požadavků.</span><span class="sxs-lookup"><span data-stu-id="48d1d-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="48d1d-109">Nastavení hostitele</span><span class="sxs-lookup"><span data-stu-id="48d1d-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-110">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="48d1d-111">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="48d1d-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="48d1d-112">To se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="48d1d-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="48d1d-113">V rámci šablon projektu `Main` se nachází v *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="48d1d-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="48d1d-114">Typické *Program.cs* volání [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) zahájíte nastavení hostitele:</span><span class="sxs-lookup"><span data-stu-id="48d1d-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="48d1d-115">`CreateDefaultBuilder`provádí následující úlohy:</span><span class="sxs-lookup"><span data-stu-id="48d1d-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="48d1d-116">Nakonfiguruje [Kestrel](servers/kestrel.md) jako webový server.</span><span class="sxs-lookup"><span data-stu-id="48d1d-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="48d1d-117">Výchozí možnosti Kestrel najdete v tématu [Kestrel možnosti části Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="48d1d-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="48d1d-118">Nastaví obsahu kořenovou cestu vrácený [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="48d1d-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="48d1d-119">Načítání z volitelné konfigurace:</span><span class="sxs-lookup"><span data-stu-id="48d1d-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="48d1d-120">*appSettings.JSON určený*.</span><span class="sxs-lookup"><span data-stu-id="48d1d-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="48d1d-121">*appSettings. {Prostředí} .json*.</span><span class="sxs-lookup"><span data-stu-id="48d1d-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="48d1d-122">[Tajné klíče uživatele](xref:security/app-secrets) při spuštění aplikace `Development` prostředí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="48d1d-123">Proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-123">Environment variables.</span></span>
  * <span data-ttu-id="48d1d-124">Argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="48d1d-124">Command-line arguments.</span></span>
* <span data-ttu-id="48d1d-125">Nakonfiguruje [protokolování](xref:fundamentals/logging/index) konzoly a ladění výstupu.</span><span class="sxs-lookup"><span data-stu-id="48d1d-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="48d1d-126">Zahrnuje protokolování [filtrování protokolu](xref:fundamentals/logging/index#log-filtering) pravidla stanovená v části Konfigurace protokolování *appSettings.JSON určený* nebo *appsettings. { Prostředí} .json* souboru.</span><span class="sxs-lookup"><span data-stu-id="48d1d-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="48d1d-127">Když spustíte za služby IIS, umožňuje [integrační služby IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="48d1d-127">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="48d1d-128">Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="48d1d-128">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="48d1d-129">Modul vytvoří reverzní proxy server mezi službou IIS a Kestrel.</span><span class="sxs-lookup"><span data-stu-id="48d1d-129">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="48d1d-130">Nakonfiguruje aplikaci taky [zaznamenat chyby při spuštění](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="48d1d-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="48d1d-131">Výchozí možnosti služby IIS najdete v tématu [IIS možnosti oddílu hostitele ASP.NET Core v systému Windows pomocí služby IIS](xref:host-and-deploy/iis/index#iis-options).</span><span class="sxs-lookup"><span data-stu-id="48d1d-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="48d1d-132">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="48d1d-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="48d1d-133">Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu.</span><span class="sxs-lookup"><span data-stu-id="48d1d-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="48d1d-134">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="48d1d-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="48d1d-135">Další informace o konfiguraci aplikace, najdete v části [konfigurace v ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="48d1d-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="48d1d-136">Jako alternativu k použití statických `CreateDefaultBuilder` metody vytvoření hostitele z [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) je podporované přístup pomocí ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="48d1d-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="48d1d-137">Další informace najdete v tématu kartě 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="48d1d-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-138">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-139">Vytvořit pomocí instance hostitele [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="48d1d-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="48d1d-140">Vytvoření hostitele se obvykle provádí v vstupní bod aplikace, `Main` metoda.</span><span class="sxs-lookup"><span data-stu-id="48d1d-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="48d1d-141">V rámci šablon projektu `Main` se nachází v *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="48d1d-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="48d1d-142">`WebHostBuilder`vyžaduje [serveru, který implementuje IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="48d1d-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="48d1d-143">Jsou předdefinované servery [Kestrel](servers/kestrel.md) a [HTTP.sys](servers/httpsys.md) (před verzí technologie ASP.NET 2.0 jádra, ovladač HTTP.sys volala [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="48d1d-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="48d1d-144">V tomto příkladu [UseKestrel rozšíření metoda](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) určuje Kestrel server.</span><span class="sxs-lookup"><span data-stu-id="48d1d-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="48d1d-145">*Obsahu kořenové* Určuje, kde hostitele hledá soubory obsahu, jako jsou například soubory zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="48d1d-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="48d1d-146">Výchozí kořen obsahu se získávají pro `UseContentRoot` podle [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="48d1d-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="48d1d-147">Když se aplikace spustí z kořenové složky projektu, projektu kořenové složky se používá jako kořenu obsahu.</span><span class="sxs-lookup"><span data-stu-id="48d1d-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="48d1d-148">Toto je výchozí hodnotu použitou v [Visual Studio](https://www.visualstudio.com/) a [nové šablony dotnet](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="48d1d-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="48d1d-149">Chcete-li použít jako reverzní proxy server služby IIS, volejte [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) jako součást sestavení hostitele.</span><span class="sxs-lookup"><span data-stu-id="48d1d-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="48d1d-150">`UseIISIntegration`neprovede konfiguraci *server*, například [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) nepodporuje.</span><span class="sxs-lookup"><span data-stu-id="48d1d-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="48d1d-151">`UseIISIntegration`Konfiguruje základní cesta a portu server naslouchá na při použití [ASP.NET Core modulu](xref:fundamentals/servers/aspnet-core-module) vytvořit reverzní proxy server mezi Kestrel a služby IIS.</span><span class="sxs-lookup"><span data-stu-id="48d1d-151">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="48d1d-152">Použití služby IIS s ASP.NET Core `UseKestrel` a `UseIISIntegration` musí být zadán.</span><span class="sxs-lookup"><span data-stu-id="48d1d-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="48d1d-153">`UseIISIntegration`aktivuje pouze při spuštění za služby IIS nebo IIS Express.</span><span class="sxs-lookup"><span data-stu-id="48d1d-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="48d1d-154">Další informace najdete v tématu [Úvod k modulu jádra ASP.NET](xref:fundamentals/servers/aspnet-core-module) a [odkazu na modul jádro ASP.NET konfigurace](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="48d1d-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="48d1d-155">Minimální implementace, které konfiguruje hostitele (a aplikace ASP.NET Core) zahrnuje určení serveru a konfiguraci kanálu žádostí aplikace:</span><span class="sxs-lookup"><span data-stu-id="48d1d-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

---

<span data-ttu-id="48d1d-156">Při nastavování hostitele, [konfigurace](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) a [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) metody lze zadat.</span><span class="sxs-lookup"><span data-stu-id="48d1d-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="48d1d-157">Pokud `Startup` je zadána třída, musíte definovat `Configure` metoda.</span><span class="sxs-lookup"><span data-stu-id="48d1d-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="48d1d-158">Další informace najdete v tématu [spuštění aplikace v ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="48d1d-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="48d1d-159">Více volá, aby se `ConfigureServices` připojit k sobě.</span><span class="sxs-lookup"><span data-stu-id="48d1d-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="48d1d-160">Více volá, aby se `Configure` nebo `UseStartup` na `WebHostBuilder` nahradit předchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="48d1d-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="48d1d-161">Hodnoty konfigurace hostitele</span><span class="sxs-lookup"><span data-stu-id="48d1d-161">Host configuration values</span></span>

<span data-ttu-id="48d1d-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) závisí na následujících dvou přístupů k nastavení hostitelské hodnoty konfigurace:</span><span class="sxs-lookup"><span data-stu-id="48d1d-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="48d1d-163">Konfigurace hostitele tvůrce, který zahrnuje proměnné prostředí ve formátu `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="48d1d-164">Například `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="48d1d-165">Explicitní metody, jako například `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="48d1d-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) a přidružené klíče.</span><span class="sxs-lookup"><span data-stu-id="48d1d-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="48d1d-167">Při nastavení hodnoty s `UseSetting`, je hodnota nastavena jako bez ohledu na typ řetězec.</span><span class="sxs-lookup"><span data-stu-id="48d1d-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="48d1d-168">Hostitel používá. obě tyto možnosti nastaví hodnotu poslední.</span><span class="sxs-lookup"><span data-stu-id="48d1d-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="48d1d-169">Další informace najdete v tématu [konfigurace přepíše](#overriding-configuration) v další části.</span><span class="sxs-lookup"><span data-stu-id="48d1d-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="48d1d-170">Zaznamenat chyby při spuštění</span><span class="sxs-lookup"><span data-stu-id="48d1d-170">Capture Startup Errors</span></span>

<span data-ttu-id="48d1d-171">Toto nastavení řídí zaznamenávání chyby při spuštění.</span><span class="sxs-lookup"><span data-stu-id="48d1d-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="48d1d-172">**Klíč**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="48d1d-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="48d1d-173">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="48d1d-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="48d1d-174">**Výchozí**: použije se výchozí hodnota `false` Pokud aplikace běží s Kestrel za služby IIS, kde výchozí hodnota je `true`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="48d1d-175">**Nastavit pomocí**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="48d1d-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="48d1d-176">**Proměnné prostředí**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="48d1d-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="48d1d-177">Když `false`, chyb během spuštění výsledek v hostiteli. operace bude ukončena.</span><span class="sxs-lookup"><span data-stu-id="48d1d-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="48d1d-178">Když `true`, hostitel zachytí výjimky během spouštění a pokusí o spuštění serveru.</span><span class="sxs-lookup"><span data-stu-id="48d1d-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-179">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-180">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="48d1d-181">Obsah kořenové</span><span class="sxs-lookup"><span data-stu-id="48d1d-181">Content Root</span></span>

<span data-ttu-id="48d1d-182">Toto nastavení určuje, kde začíná ASP.NET Core hledání obsahu souborů, jako je například zobrazení MVC.</span><span class="sxs-lookup"><span data-stu-id="48d1d-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="48d1d-183">**Klíč**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="48d1d-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="48d1d-184">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="48d1d-184">**Type**: *string*</span></span>  
<span data-ttu-id="48d1d-185">**Výchozí**: výchozí složce, kde se nachází sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="48d1d-186">**Nastavit pomocí**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="48d1d-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="48d1d-187">**Proměnné prostředí**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="48d1d-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="48d1d-188">Kořenu obsahu se používá jako základní cesta pro [kořenový Web nastavení](#web-root).</span><span class="sxs-lookup"><span data-stu-id="48d1d-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="48d1d-189">Pokud cesta neexistuje, hostitel se nepodaří spustit.</span><span class="sxs-lookup"><span data-stu-id="48d1d-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-190">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-191">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="48d1d-192">Podrobné chyby</span><span class="sxs-lookup"><span data-stu-id="48d1d-192">Detailed Errors</span></span>

<span data-ttu-id="48d1d-193">Určuje, zda podrobné chyby, které mají být zaznamenány.</span><span class="sxs-lookup"><span data-stu-id="48d1d-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="48d1d-194">**Klíč**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="48d1d-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="48d1d-195">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="48d1d-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="48d1d-196">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="48d1d-196">**Default**: false</span></span>  
<span data-ttu-id="48d1d-197">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="48d1d-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="48d1d-198">**Proměnné prostředí**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="48d1d-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="48d1d-199">Když povolené (nebo když <a href="#environment">prostředí</a> je nastaven na `Development`), aplikace zaznamená podrobné výjimky.</span><span class="sxs-lookup"><span data-stu-id="48d1d-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-200">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-201">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="48d1d-202">Prostředí</span><span class="sxs-lookup"><span data-stu-id="48d1d-202">Environment</span></span>

<span data-ttu-id="48d1d-203">Nastaví prostředí aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-203">Sets the app's environment.</span></span>

<span data-ttu-id="48d1d-204">**Klíč**: prostředí</span><span class="sxs-lookup"><span data-stu-id="48d1d-204">**Key**: environment</span></span>  
<span data-ttu-id="48d1d-205">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="48d1d-205">**Type**: *string*</span></span>  
<span data-ttu-id="48d1d-206">**Výchozí**: produkční</span><span class="sxs-lookup"><span data-stu-id="48d1d-206">**Default**: Production</span></span>  
<span data-ttu-id="48d1d-207">**Nastavit pomocí**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="48d1d-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="48d1d-208">**Proměnné prostředí**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="48d1d-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="48d1d-209">Prostředí můžete nastavit na jakoukoli hodnotu.</span><span class="sxs-lookup"><span data-stu-id="48d1d-209">The environment can be set to any value.</span></span> <span data-ttu-id="48d1d-210">Framework definované hodnoty zahrnují `Development`, `Staging`, a `Production`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="48d1d-211">Hodnoty nejsou velká a malá písmena.</span><span class="sxs-lookup"><span data-stu-id="48d1d-211">Values aren't case sensitive.</span></span> <span data-ttu-id="48d1d-212">Ve výchozím nastavení *prostředí* je pro čtení z `ASPNETCORE_ENVIRONMENT` proměnné prostředí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="48d1d-213">Při použití [Visual Studio](https://www.visualstudio.com/), proměnné prostředí může být nastavena v *launchSettings.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="48d1d-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="48d1d-214">Další informace najdete v tématu [práce s několika prostředí](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="48d1d-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-215">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-216">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="48d1d-217">Hostování spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="48d1d-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="48d1d-218">Nastaví hostování spuštění sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="48d1d-219">**Klíč**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="48d1d-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="48d1d-220">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="48d1d-220">**Type**: *string*</span></span>  
<span data-ttu-id="48d1d-221">**Výchozí**: prázdný řetězec</span><span class="sxs-lookup"><span data-stu-id="48d1d-221">**Default**: Empty string</span></span>  
<span data-ttu-id="48d1d-222">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="48d1d-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="48d1d-223">**Proměnné prostředí**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="48d1d-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="48d1d-224">Řetězec oddělený středníkem hostování spuštění sestavení načíst při spuštění.</span><span class="sxs-lookup"><span data-stu-id="48d1d-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="48d1d-225">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="48d1d-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="48d1d-226">I když výchozí hodnota konfigurace je řetězec prázdný, hostování spuštění sestavení, vždy zahrňte sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="48d1d-227">Při hostování spuštění sestavení jsou k dispozici, se přidají do sestavení aplikace pro načtení, když aplikace sestavení jeho společných služeb během spouštění.</span><span class="sxs-lookup"><span data-stu-id="48d1d-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-228">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-229">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-230">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="48d1d-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="48d1d-231">Dáváte přednost hostování adresy URL</span><span class="sxs-lookup"><span data-stu-id="48d1d-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="48d1d-232">Určuje, zda hostitel naslouchat požadavkům na adresy URL nakonfigurované `WebHostBuilder` místo nastavení nakonfigurovaného pomocí `IServer` implementace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="48d1d-233">**Klíč**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="48d1d-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="48d1d-234">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="48d1d-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="48d1d-235">**Výchozí**: true</span><span class="sxs-lookup"><span data-stu-id="48d1d-235">**Default**: true</span></span>  
<span data-ttu-id="48d1d-236">**Nastavit pomocí**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="48d1d-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="48d1d-237">**Proměnné prostředí**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="48d1d-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="48d1d-238">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="48d1d-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-239">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-240">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-241">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="48d1d-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="48d1d-242">Zabránit spuštění hostování</span><span class="sxs-lookup"><span data-stu-id="48d1d-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="48d1d-243">Brání automatické načítání hostování spuštění sestavení, a to včetně hostování spuštění sestavení nakonfiguroval sestavení aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="48d1d-244">V tématu [přidat funkce aplikací z externí sestavení pomocí IHostingStartup](xref:host-and-deploy/ihostingstartup) Další informace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-244">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="48d1d-245">**Klíč**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="48d1d-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="48d1d-246">**Typ**: *bool* (`true` nebo `1`)</span><span class="sxs-lookup"><span data-stu-id="48d1d-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="48d1d-247">**Výchozí**: false</span><span class="sxs-lookup"><span data-stu-id="48d1d-247">**Default**: false</span></span>  
<span data-ttu-id="48d1d-248">**Nastavit pomocí**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="48d1d-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="48d1d-249">**Proměnné prostředí**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="48d1d-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="48d1d-250">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="48d1d-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-251">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-252">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-253">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="48d1d-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="48d1d-254">Adresy URL serveru</span><span class="sxs-lookup"><span data-stu-id="48d1d-254">Server URLs</span></span>

<span data-ttu-id="48d1d-255">Určuje IP adres nebo adres hostitele s porty a protokoly, které server naslouchat požadavkům na požadavky.</span><span class="sxs-lookup"><span data-stu-id="48d1d-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="48d1d-256">**Klíč**: adresy URL</span><span class="sxs-lookup"><span data-stu-id="48d1d-256">**Key**: urls</span></span>  
<span data-ttu-id="48d1d-257">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="48d1d-257">**Type**: *string*</span></span>  
<span data-ttu-id="48d1d-258">**Výchozí**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="48d1d-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="48d1d-259">**Nastavit pomocí**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="48d1d-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="48d1d-260">**Proměnné prostředí**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="48d1d-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="48d1d-261">Nastavte na oddělených středníkem (;) seznam URL předpony serveru by měl odpovídat.</span><span class="sxs-lookup"><span data-stu-id="48d1d-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="48d1d-262">Například `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="48d1d-263">Použití "\*" k označení, že by měl server přijímat požadavky na všechny IP adresy nebo názvu hostitele pomocí zadaný port a protokol (například `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="48d1d-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="48d1d-264">Protokol (`http://` nebo `https://`) musí být součástí každou adresu URL.</span><span class="sxs-lookup"><span data-stu-id="48d1d-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="48d1d-265">Podporované formáty liší mezi servery.</span><span class="sxs-lookup"><span data-stu-id="48d1d-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-266">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="48d1d-267">Kestrel má svůj vlastní koncový bod rozhraní API konfigurace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="48d1d-268">Další informace najdete v tématu [Kestrel webového serveru implementace v ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="48d1d-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-269">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="48d1d-270">Časový limit vypnutí</span><span class="sxs-lookup"><span data-stu-id="48d1d-270">Shutdown Timeout</span></span>

<span data-ttu-id="48d1d-271">Určuje dobu čekání na webového hostitele vypnutí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="48d1d-272">**Klíč**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="48d1d-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="48d1d-273">**Typ**: *int*</span><span class="sxs-lookup"><span data-stu-id="48d1d-273">**Type**: *int*</span></span>  
<span data-ttu-id="48d1d-274">**Výchozí**: 5</span><span class="sxs-lookup"><span data-stu-id="48d1d-274">**Default**: 5</span></span>  
<span data-ttu-id="48d1d-275">**Nastavit pomocí**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="48d1d-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="48d1d-276">**Proměnné prostředí**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="48d1d-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="48d1d-277">I když přijme klíč *int* s `UseSetting` (například `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` rozšíření metoda přebírá `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="48d1d-278">Tato funkce je nového v technologii ASP.NET 2.0 jádra.</span><span class="sxs-lookup"><span data-stu-id="48d1d-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-279">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-280">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-281">Tato funkce není k dispozici v ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="48d1d-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="48d1d-282">Spuštění sestavení</span><span class="sxs-lookup"><span data-stu-id="48d1d-282">Startup Assembly</span></span>

<span data-ttu-id="48d1d-283">Určuje sestavení pro vyhledávání `Startup` třídy.</span><span class="sxs-lookup"><span data-stu-id="48d1d-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="48d1d-284">**Klíč**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="48d1d-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="48d1d-285">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="48d1d-285">**Type**: *string*</span></span>  
<span data-ttu-id="48d1d-286">**Výchozí**: sestavení aplikace</span><span class="sxs-lookup"><span data-stu-id="48d1d-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="48d1d-287">**Nastavit pomocí**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="48d1d-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="48d1d-288">**Proměnné prostředí**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="48d1d-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="48d1d-289">Sestavení podle názvu (`string`) nebo typ (`TStartup`) může být odkaz.</span><span class="sxs-lookup"><span data-stu-id="48d1d-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="48d1d-290">Pokud je to více `UseStartup` metody jsou volány, poslední změny mají přednost.</span><span class="sxs-lookup"><span data-stu-id="48d1d-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-291">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-292">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="48d1d-293">Kořenový web</span><span class="sxs-lookup"><span data-stu-id="48d1d-293">Web Root</span></span>

<span data-ttu-id="48d1d-294">Nastaví relativní cestu na statické prostředky aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="48d1d-295">**Klíč**: webroot</span><span class="sxs-lookup"><span data-stu-id="48d1d-295">**Key**: webroot</span></span>  
<span data-ttu-id="48d1d-296">**Typ**: *řetězec*</span><span class="sxs-lookup"><span data-stu-id="48d1d-296">**Type**: *string*</span></span>  
<span data-ttu-id="48d1d-297">**Výchozí**: Pokud není zadáno, výchozí hodnota je "(Content Root)/wwwroot", pokud cesta existuje.</span><span class="sxs-lookup"><span data-stu-id="48d1d-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="48d1d-298">Pokud cesta neexistuje, je použít soubor no-op zprostředkovatele.</span><span class="sxs-lookup"><span data-stu-id="48d1d-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="48d1d-299">**Nastavit pomocí**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="48d1d-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="48d1d-300">**Proměnné prostředí**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="48d1d-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-301">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-302">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="48d1d-303">Přepsání konfigurace</span><span class="sxs-lookup"><span data-stu-id="48d1d-303">Overriding configuration</span></span>

<span data-ttu-id="48d1d-304">Použití [konfigurace](xref:fundamentals/configuration/index) konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="48d1d-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="48d1d-305">V následujícím příkladu je konfigurace hostitele volitelně specifikován v *hosting.json* souboru.</span><span class="sxs-lookup"><span data-stu-id="48d1d-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="48d1d-306">Všechny konfigurace načtena z *hosting.json* soubor může být přepsána argumenty příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="48d1d-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="48d1d-307">Integrovaný konfigurace (v `config`) se používá ke konfiguraci hostitele s `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-308">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="48d1d-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="48d1d-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="48d1d-310">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="48d1d-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-311">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="48d1d-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="48d1d-313">Přepsání zadaná podle konfigurace `UseUrls` s *hosting.json* konfigurace příkazového řádku, první argument konfigurace druhý:</span><span class="sxs-lookup"><span data-stu-id="48d1d-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

---

> [!NOTE]
> <span data-ttu-id="48d1d-314">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) metoda rozšíření není aktuálně schopen analyzovat konfigurační oddíl vrácený `GetSection` (například `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="48d1d-315">`GetSection` Metoda filtry konfigurace klíče do části požadovaný, ale ponechá název oddílu na klíče (například `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="48d1d-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="48d1d-316">`UseConfiguration` Metoda očekává klíče tak, aby odpovídala `WebHostBuilder` klíče (například `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="48d1d-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="48d1d-317">Přítomnost názvu oddílu na klíče zabrání hodnoty v části Konfigurace hostitele.</span><span class="sxs-lookup"><span data-stu-id="48d1d-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="48d1d-318">Tento problém bude vyřešen v příští verzi.</span><span class="sxs-lookup"><span data-stu-id="48d1d-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="48d1d-319">Další informace a řešení, najdete v části [předávání konfigurační oddíl do WebHostBuilder.UseConfiguration používá úplné klíče](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="48d1d-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="48d1d-320">Pokud chcete zadat hostiteli spustit na konkrétní adresu URL, požadovanou hodnotu lze předat ve z příkazového řádku při provádění `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="48d1d-321">Přepíše argument příkazového řádku `urls` z hodnoty *hosting.json* souboru a server naslouchá na portu 8080:</span><span class="sxs-lookup"><span data-stu-id="48d1d-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="48d1d-322">Od hostitele</span><span class="sxs-lookup"><span data-stu-id="48d1d-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="48d1d-323">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="48d1d-324">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="48d1d-324">**Run**</span></span>

<span data-ttu-id="48d1d-325">`Run` Metoda spuštění webové aplikace a blokuje volající vlákno, dokud hostitel vypnutí:</span><span class="sxs-lookup"><span data-stu-id="48d1d-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="48d1d-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="48d1d-326">**Start**</span></span>

<span data-ttu-id="48d1d-327">Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="48d1d-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="48d1d-328">Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="48d1d-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="48d1d-329">Aplikace můžete inicializaci a spuštění nové hostitele pomocí předem nakonfigurovaných výchozích nastavení `CreateDefaultBuilder` pomocí jiné metody statické pohodlí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="48d1d-330">Tyto metody start pro server bez výstup konzoly a s [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) počkejte zalomení (Ctrl-C nebo sigint – nebo SIGTERM –):</span><span class="sxs-lookup"><span data-stu-id="48d1d-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="48d1d-331">**Spuštění (RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="48d1d-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="48d1d-332">Začněte `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="48d1d-333">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="48d1d-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="48d1d-334">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="48d1d-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="48d1d-335">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="48d1d-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="48d1d-336">**Spuštění (řetězec adresy url, RequestDelegate aplikace)**</span><span class="sxs-lookup"><span data-stu-id="48d1d-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="48d1d-337">Spustit s adresou URL a `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="48d1d-338">Stejný výsledek jako **spuštění (RequestDelegate aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="48d1d-339">**Spuštění (akce<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="48d1d-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="48d1d-340">Použít instanci `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) používat směrování middleware:</span><span class="sxs-lookup"><span data-stu-id="48d1d-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="48d1d-341">Použijte následující požadavky na prohlížeč s příkladu:</span><span class="sxs-lookup"><span data-stu-id="48d1d-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="48d1d-342">Požadavek</span><span class="sxs-lookup"><span data-stu-id="48d1d-342">Request</span></span>                                    | <span data-ttu-id="48d1d-343">Odpověď</span><span class="sxs-lookup"><span data-stu-id="48d1d-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="48d1d-344">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="48d1d-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="48d1d-345">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="48d1d-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="48d1d-346">Vyvolá výjimku řetězcem "ooops!"</span><span class="sxs-lookup"><span data-stu-id="48d1d-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="48d1d-347">Vyvolá výjimku řetězcem "Uh jejda!"</span><span class="sxs-lookup"><span data-stu-id="48d1d-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="48d1d-348">Santé, kevina!</span><span class="sxs-lookup"><span data-stu-id="48d1d-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="48d1d-349">Ahoj světe!</span><span class="sxs-lookup"><span data-stu-id="48d1d-349">Hello World!</span></span>                             |

<span data-ttu-id="48d1d-350">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="48d1d-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="48d1d-351">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="48d1d-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="48d1d-352">**Spuštění (řetězce adresy url, akce<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="48d1d-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="48d1d-353">Použijte adresu URL a instance `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="48d1d-354">Stejný výsledek jako **spuštění (akce<IRouteBuilder> routeBuilder)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="48d1d-355">**StartWith (akce<IApplicationBuilder> aplikace)**</span><span class="sxs-lookup"><span data-stu-id="48d1d-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="48d1d-356">Zadejte delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="48d1d-357">Vytvoření žádosti o v prohlížeči `http://localhost:5000` k obdrží odpověď "Hello, World!"</span><span class="sxs-lookup"><span data-stu-id="48d1d-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="48d1d-358">`WaitForShutdown`bloky, dokud se objeví zalomení (Ctrl-C nebo sigint – nebo SIGTERM –).</span><span class="sxs-lookup"><span data-stu-id="48d1d-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="48d1d-359">Zobrazí aplikace `Console.WriteLine` zprávu a čeká keypress ukončíte.</span><span class="sxs-lookup"><span data-stu-id="48d1d-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="48d1d-360">**StartWith (řetězce adresy url, akce<IApplicationBuilder> aplikace)**</span><span class="sxs-lookup"><span data-stu-id="48d1d-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="48d1d-361">Zadejte adresu URL a delegát pro konfiguraci `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="48d1d-362">Stejný výsledek jako **StartWith (akce<IApplicationBuilder> aplikace)**, s výjimkou aplikace reaguje na `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="48d1d-363">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="48d1d-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="48d1d-364">**Spustit**</span><span class="sxs-lookup"><span data-stu-id="48d1d-364">**Run**</span></span>

<span data-ttu-id="48d1d-365">`Run` Metoda spustí webovou aplikaci a blokuje volající vlákno, dokud se vypne hostitele:</span><span class="sxs-lookup"><span data-stu-id="48d1d-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="48d1d-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="48d1d-366">**Start**</span></span>

<span data-ttu-id="48d1d-367">Spuštění hostitele způsobem neblokující voláním jeho `Start` metoda:</span><span class="sxs-lookup"><span data-stu-id="48d1d-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="48d1d-368">Pokud je předán seznam adres URL, `Start` metoda, naslouchá na zadané adresy URL:</span><span class="sxs-lookup"><span data-stu-id="48d1d-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="48d1d-369">IHostingEnvironment rozhraní</span><span class="sxs-lookup"><span data-stu-id="48d1d-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="48d1d-370">[IHostingEnvironment rozhraní](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) poskytuje informace o hostování prostředí webové aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="48d1d-371">Použít [konstruktor vkládání](xref:fundamentals/dependency-injection) získat `IHostingEnvironment` Chcete-li použít její vlastnosti a metody rozšíření:</span><span class="sxs-lookup"><span data-stu-id="48d1d-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="48d1d-372">A [založené na konvenci přístup](xref:fundamentals/environments#startup-conventions) můžete použít ke konfiguraci aplikace při spuštění založených na prostředí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="48d1d-373">Můžete také vložit `IHostingEnvironment` do `Startup` konstruktor pro použití v `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="48d1d-374">Kromě `IsDevelopment` metoda rozšíření `IHostingEnvironment` nabízí `IsStaging`, `IsProduction`, a `IsEnvironment(string environmentName)` metody.</span><span class="sxs-lookup"><span data-stu-id="48d1d-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="48d1d-375">V tématu [práce s několika prostředí](xref:fundamentals/environments) podrobnosti.</span><span class="sxs-lookup"><span data-stu-id="48d1d-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="48d1d-376">`IHostingEnvironment` Služby může také vložit přímo do `Configure` metoda pro nastavení zpracování kanálu:</span><span class="sxs-lookup"><span data-stu-id="48d1d-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="48d1d-377">`IHostingEnvironment`můžete vložit do `Invoke` metoda při vytváření vlastní [middleware](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="48d1d-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="48d1d-378">IApplicationLifetime rozhraní</span><span class="sxs-lookup"><span data-stu-id="48d1d-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="48d1d-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) umožňuje po spuštění a vypnutí aktivity.</span><span class="sxs-lookup"><span data-stu-id="48d1d-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="48d1d-380">Zrušení tokenů použitá pro zaregistrování jsou tři vlastnosti na rozhraní `Action` metody, které definují události spuštění a vypnutí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="48d1d-381">K dispozici je také `StopApplication` metoda.</span><span class="sxs-lookup"><span data-stu-id="48d1d-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="48d1d-382">Token zrušení</span><span class="sxs-lookup"><span data-stu-id="48d1d-382">Cancellation Token</span></span>    | <span data-ttu-id="48d1d-383">Aktivuje, když &#8230;</span><span class="sxs-lookup"><span data-stu-id="48d1d-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="48d1d-384">Hostitele plně byla spuštěna.</span><span class="sxs-lookup"><span data-stu-id="48d1d-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="48d1d-385">Hostitel provádí řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="48d1d-386">Může být stále aktivní žádosti.</span><span class="sxs-lookup"><span data-stu-id="48d1d-386">Requests may still be processing.</span></span> <span data-ttu-id="48d1d-387">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="48d1d-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="48d1d-388">Hostitel je dokončení řádné vypnutí.</span><span class="sxs-lookup"><span data-stu-id="48d1d-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="48d1d-389">Všechny požadavky, měla by být zpracována.</span><span class="sxs-lookup"><span data-stu-id="48d1d-389">All requests should be processed.</span></span> <span data-ttu-id="48d1d-390">Vypnutí bloky až po dokončení této události.</span><span class="sxs-lookup"><span data-stu-id="48d1d-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="48d1d-391">Metoda</span><span class="sxs-lookup"><span data-stu-id="48d1d-391">Method</span></span>            | <span data-ttu-id="48d1d-392">Akce</span><span class="sxs-lookup"><span data-stu-id="48d1d-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="48d1d-393">Žádosti o ukončení aktuální aplikace.</span><span class="sxs-lookup"><span data-stu-id="48d1d-393">Requests termination of the current application.</span></span> |

```csharp
public class Startup 
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime) 
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="48d1d-394">Řešení potíží s System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="48d1d-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="48d1d-395">**Platí pro pouze ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="48d1d-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="48d1d-396">Hostitel může být sestaven vložením `IStartup` přímo do kontejneru pro vkládání závislosti místo volání `UseStartup` nebo `Configure`:</span><span class="sxs-lookup"><span data-stu-id="48d1d-396">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="48d1d-397">Pokud hostitel je integrovaný tímto způsobem, může dojít k následující chybě:</span><span class="sxs-lookup"><span data-stu-id="48d1d-397">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="48d1d-398">K tomu dochází, protože [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (aktuální sestavení) je nutná ke kontrole pro `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-398">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="48d1d-399">Pokud aplikace ručně vloží `IStartup` do kontejneru pro vkládání závislosti, přidejte následující volání `WebHostBuilder` se zadaným názvem sestavení:</span><span class="sxs-lookup"><span data-stu-id="48d1d-399">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="48d1d-400">Můžete taky přidat fiktivní `Configure` k `WebHostBuilder`, která nastaví `applicationName`(`ApplicationKey`) automaticky:</span><span class="sxs-lookup"><span data-stu-id="48d1d-400">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="48d1d-401">**Poznámka:**: Toto je pouze požadované verze technologie ASP.NET 2.0 jádra a pouze pokud aplikace nemá volání `UseStartup` nebo `Configure`.</span><span class="sxs-lookup"><span data-stu-id="48d1d-401">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="48d1d-402">Další informace najdete v tématu [oznámení: Microsoft.Extensions.PlatformAbstractions byl odebrán (komentář)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) a [StartupInjection ukázka](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="48d1d-402">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48d1d-403">Další zdroje</span><span class="sxs-lookup"><span data-stu-id="48d1d-403">Additional resources</span></span>

* [<span data-ttu-id="48d1d-404">Hostování ve Windows se službou IIS</span><span class="sxs-lookup"><span data-stu-id="48d1d-404">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="48d1d-405">Hostování v Linuxu na serveru Nginx</span><span class="sxs-lookup"><span data-stu-id="48d1d-405">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="48d1d-406">Hostování v Linuxu na serveru Apache</span><span class="sxs-lookup"><span data-stu-id="48d1d-406">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="48d1d-407">Hostitele ve službě Windows</span><span class="sxs-lookup"><span data-stu-id="48d1d-407">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)