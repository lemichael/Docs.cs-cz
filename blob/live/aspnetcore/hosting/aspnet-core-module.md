---
title: "Odkaz na konfiguraci základní modul ASP.NET"
author: guardrex
description: "Postup konfigurace modulu jádra ASP.NET pro hostování aplikací ASP.NET Core."
keywords: "ASP.NET Core, ancm, základní modul, iis, stdout protokolování, proměnné prostředí, env var, subapplication, subapp, appoffline, app_offline, 502, schématu"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 5de0c8f7-50ce-4e2c-b3d4-a1bd9fdfcff5
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/aspnet-core-module
ms.openlocfilehash: ac52b791e02ce52da35fe8d599465076d251b4da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-core-module-configuration-reference"></a><span data-ttu-id="3cb84-104">Odkaz na konfiguraci základní modul ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3cb84-104">ASP.NET Core Module configuration reference</span></span>

<span data-ttu-id="3cb84-105">Podle [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), a [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="3cb84-105">By [Luke Latham](https://github.com/guardrex), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="3cb84-106">Tento dokument obsahuje podrobnosti o tom, jak nakonfigurovat modul jádro ASP.NET pro hostování aplikací ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3cb84-106">This document provides details on how to configure the ASP.NET Core Module for hosting ASP.NET Core applications.</span></span> <span data-ttu-id="3cb84-107">Úvod do modulu jádra ASP.NET a pokyny k instalaci, najdete v článku [ASP.NET Core modulu přehled](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3cb84-107">For an introduction to the ASP.NET Core Module and installation instructions, see the [ASP.NET Core Module overview](xref:fundamentals/servers/aspnet-core-module).</span></span>

## <a name="configuration-via-webconfig"></a><span data-ttu-id="3cb84-108">Konfigurace pomocí souboru web.config</span><span class="sxs-lookup"><span data-stu-id="3cb84-108">Configuration via web.config</span></span>

<span data-ttu-id="3cb84-109">Základní modul ASP.NET je nakonfigurovat přes web nebo aplikaci *web.config* souborů a má svou vlastní `aspNetCore` konfigurační oddíl v rámci `system.webServer`.</span><span class="sxs-lookup"><span data-stu-id="3cb84-109">The ASP.NET Core Module is configured via a site or application *web.config* file and has its own `aspNetCore` configuration section within `system.webServer`.</span></span> <span data-ttu-id="3cb84-110">Tady je příklad *web.config* souboru, který `Microsoft.NET.Sdk.Web` SDK bude poskytovat, když na projekt je publikována pro [nasazení závislé na framework](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) se zástupnými symboly pro `processPath` a `arguments`:</span><span class="sxs-lookup"><span data-stu-id="3cb84-110">Here's an example *web.config* file that the `Microsoft.NET.Sdk.Web` SDK will provide when the project is published for a [framework-dependent deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#framework-dependent-deployments-fdd) with placeholders for the `processPath` and `arguments`:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="%LAUNCHER_PATH%" 
        arguments="%LAUNCHER_ARGS%" 
        stdoutLogEnabled="false" 
        stdoutLogFile=".\logs\stdout" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="3cb84-111">*Web.config* následující příklad je pro [samostatná nasazení](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) k [Azure App Service](https://azure.microsoft.com/services/app-service/).</span><span class="sxs-lookup"><span data-stu-id="3cb84-111">The *web.config* example below is for a [self-contained deployment](https://docs.microsoft.com/dotnet/articles/core/deploying/#self-contained-deployments-scd) to the [Azure App Service](https://azure.microsoft.com/services/app-service/).</span></span> <span data-ttu-id="3cb84-112">Další informace najdete v tématu [publikování do služby IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="3cb84-112">For more information, see [Publishing to IIS](xref:publishing/iis).</span></span> <span data-ttu-id="3cb84-113">V tématu [konfiguraci dílčí aplikací](xref:publishing/iis#configuration-of-sub-applications) pro důležitá poznámka týkající se konfigurace *web.config* soubory v dílčí aplikace.</span><span class="sxs-lookup"><span data-stu-id="3cb84-113">See [Configuration of sub-applications](xref:publishing/iis#configuration-of-sub-applications) for an important note pertaining to the configuration of *web.config* files in sub-applications.</span></span>

```xml
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath=".\MyApp.exe" 
        stdoutLogEnabled="false" 
        stdoutLogFile="\\?\%home%\LogFiles\stdout" />
  </system.webServer>
</configuration>
```

### <a name="attributes-of-the-aspnetcore-element"></a><span data-ttu-id="3cb84-114">Atributy elementu aspNetCore</span><span class="sxs-lookup"><span data-stu-id="3cb84-114">Attributes of the aspNetCore element</span></span>

| <span data-ttu-id="3cb84-115">Atribut</span><span class="sxs-lookup"><span data-stu-id="3cb84-115">Attribute</span></span> | <span data-ttu-id="3cb84-116">Popis</span><span class="sxs-lookup"><span data-stu-id="3cb84-116">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3cb84-117">processPath</span><span class="sxs-lookup"><span data-stu-id="3cb84-117">processPath</span></span> | <p><span data-ttu-id="3cb84-118">Požadovaný atribut typu string.</span><span class="sxs-lookup"><span data-stu-id="3cb84-118">Required string attribute.</span></span></p><p><span data-ttu-id="3cb84-119">Cesta ke spustitelnému souboru, který se spustí proces naslouchání požadavkům HTTP.</span><span class="sxs-lookup"><span data-stu-id="3cb84-119">Path to the executable that will launch a process listening for HTTP requests.</span></span> <span data-ttu-id="3cb84-120">Jsou podporovány relativní cesty.</span><span class="sxs-lookup"><span data-stu-id="3cb84-120">Relative paths are supported.</span></span> <span data-ttu-id="3cb84-121">Pokud cesta začíná '.', cesta se považuje za relativní vůči kořenovému adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-121">If the path begins with '.', the path is considered to be relative to the site root.</span></span></p><p><span data-ttu-id="3cb84-122">Není k dispozici žádná výchozí hodnota.</span><span class="sxs-lookup"><span data-stu-id="3cb84-122">There is no default value.</span></span></p> |
| <span data-ttu-id="3cb84-123">argumenty</span><span class="sxs-lookup"><span data-stu-id="3cb84-123">arguments</span></span> | <p><span data-ttu-id="3cb84-124">Volitelný řetězec atributu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-124">Optional string attribute.</span></span></p><p><span data-ttu-id="3cb84-125">Argumenty pro spustitelný soubor určený v **processPath**.</span><span class="sxs-lookup"><span data-stu-id="3cb84-125">Arguments to the executable specified in **processPath**.</span></span></p><p><span data-ttu-id="3cb84-126">Výchozí hodnota je prázdný řetězec.</span><span class="sxs-lookup"><span data-stu-id="3cb84-126">The default value is an empty string.</span></span></p> |
| <span data-ttu-id="3cb84-127">startupTimeLimit</span><span class="sxs-lookup"><span data-stu-id="3cb84-127">startupTimeLimit</span></span> | <p><span data-ttu-id="3cb84-128">Atribut volitelné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="3cb84-128">Optional integer attribute.</span></span></p><p><span data-ttu-id="3cb84-129">Doba v sekundách, které vyčká, modul pro spuštění procesu naslouchání na portu spustitelný soubor.</span><span class="sxs-lookup"><span data-stu-id="3cb84-129">Duration in seconds that the module will wait for the executable to start a process listening on the port.</span></span> <span data-ttu-id="3cb84-130">Pokud je tento časový limit překročen, modul se ukončit proces.</span><span class="sxs-lookup"><span data-stu-id="3cb84-130">If this time limit is exceeded, the module will kill the process.</span></span> <span data-ttu-id="3cb84-131">Modul se pokusí znovu spusťte proces, při přijetí nového požadavku a bude dále pokoušet pro restartování procesu na následné příchozí žádosti, pokud aplikace se nepodaří spustit **rapidFailsPerMinute** číslo kolikrát za poslední minutu postupného.</span><span class="sxs-lookup"><span data-stu-id="3cb84-131">The module will attempt to launch the process again when it receives a new request and will continue to attempt to restart the process on subsequent incoming requests unless the application fails to start **rapidFailsPerMinute** number of times in the last rolling minute.</span></span></p><p><span data-ttu-id="3cb84-132">Výchozí hodnota je 120.</span><span class="sxs-lookup"><span data-stu-id="3cb84-132">The default value is 120.</span></span></p> |
| <span data-ttu-id="3cb84-133">shutdownTimeLimit</span><span class="sxs-lookup"><span data-stu-id="3cb84-133">shutdownTimeLimit</span></span> | <p><span data-ttu-id="3cb84-134">Atribut volitelné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="3cb84-134">Optional integer attribute.</span></span></p><p><span data-ttu-id="3cb84-135">Doba v sekundách, pro které modul vyčká pro spustitelný soubor řádně vypnutí při *app_offline.htm* je detekován soubor.</span><span class="sxs-lookup"><span data-stu-id="3cb84-135">Duration in seconds for which the module will wait for the executable to gracefully shutdown when the *app_offline.htm* file is detected.</span></span></p><p><span data-ttu-id="3cb84-136">Výchozí hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="3cb84-136">The default value is 10.</span></span></p> |
| <span data-ttu-id="3cb84-137">rapidFailsPerMinute</span><span class="sxs-lookup"><span data-stu-id="3cb84-137">rapidFailsPerMinute</span></span> | <p><span data-ttu-id="3cb84-138">Atribut volitelné celé číslo.</span><span class="sxs-lookup"><span data-stu-id="3cb84-138">Optional integer attribute.</span></span></p><p><span data-ttu-id="3cb84-139">Určuje, kolikrát proces zadaný v **processPath** je dovoleno havárií za minutu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-139">Specifies the number of times the process specified in **processPath** is allowed to crash per minute.</span></span> <span data-ttu-id="3cb84-140">Pokud je tento limit překročen, modul se zastaví spuštění procesu pro zbytek minutu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-140">If this limit is exceeded, the module will stop launching the process for the remainder of the minute.</span></span></p><p><span data-ttu-id="3cb84-141">Výchozí hodnota je 10.</span><span class="sxs-lookup"><span data-stu-id="3cb84-141">The default value is 10.</span></span></p> |
| <span data-ttu-id="3cb84-142">RequestTimeout</span><span class="sxs-lookup"><span data-stu-id="3cb84-142">requestTimeout</span></span> | <p><span data-ttu-id="3cb84-143">Atribut volitelné časový interval.</span><span class="sxs-lookup"><span data-stu-id="3cb84-143">Optional timespan attribute.</span></span></p><p><span data-ttu-id="3cb84-144">Určuje dobu, pro který modul ASP.NET Core bude čekat na odpověď z procesu naslouchání na ASPNETCORE_PORT %.</span><span class="sxs-lookup"><span data-stu-id="3cb84-144">Specifies the duration for which the ASP.NET Core Module will wait for a response from the process listening on %ASPNETCORE_PORT%.</span></span></p><p><span data-ttu-id="3cb84-145">Výchozí hodnota je "00: 02:00".</span><span class="sxs-lookup"><span data-stu-id="3cb84-145">The default value is "00:02:00".</span></span></p> |
| <span data-ttu-id="3cb84-146">stdoutLogEnabled</span><span class="sxs-lookup"><span data-stu-id="3cb84-146">stdoutLogEnabled</span></span> | <p><span data-ttu-id="3cb84-147">Volitelný logický atribut.</span><span class="sxs-lookup"><span data-stu-id="3cb84-147">Optional Boolean attribute.</span></span></p><p><span data-ttu-id="3cb84-148">V případě hodnoty true **stdout** a **stderr** pro proces zadaný v **processPath** bude přesměrován na soubor zadaný v **stdoutLogFile**.</span><span class="sxs-lookup"><span data-stu-id="3cb84-148">If true, **stdout** and **stderr** for the process specified in **processPath** will be redirected to the file specified in **stdoutLogFile**.</span></span></p><p><span data-ttu-id="3cb84-149">Výchozí hodnota je False.</span><span class="sxs-lookup"><span data-stu-id="3cb84-149">The default value is false.</span></span></p> |
| <span data-ttu-id="3cb84-150">stdoutLogFile</span><span class="sxs-lookup"><span data-stu-id="3cb84-150">stdoutLogFile</span></span> | <p><span data-ttu-id="3cb84-151">Volitelný řetězec atributu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-151">Optional string attribute.</span></span></p><p><span data-ttu-id="3cb84-152">Určuje cestu k souboru relativní nebo absolutní, pro kterou **stdout** a **stderr** z procesu zadaný v **processPath** bude do protokolu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-152">Specifies the relative or absolute file path for which **stdout** and **stderr** from the process specified in **processPath** will be logged.</span></span> <span data-ttu-id="3cb84-153">Relativní cesty jsou relativní vůči kořenovému adresáři webu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-153">Relative paths are relative to the root of the site.</span></span> <span data-ttu-id="3cb84-154">Jakoukoli cestu, počínaje '. ", budou relativní vůči kořenovému adresáři webu a všechny ostatní cesty bude považována za absolutní cesty.</span><span class="sxs-lookup"><span data-stu-id="3cb84-154">Any path starting with '.' will be relative to the site root and all other paths will be treated as absolute paths.</span></span> <span data-ttu-id="3cb84-155">Všechny složky zadaná v cestě musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-155">Any folders provided in the path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="3cb84-156">ID procesu je časové razítko (*yyyyMdhms*) a příponu souboru (*.log*) s podtržítka oddělovače přidají na poslední segment **stdoutLogFile** zadat.</span><span class="sxs-lookup"><span data-stu-id="3cb84-156">The process ID, timestamp (*yyyyMdhms*), and file extension (*.log*) with underscore delimiters are added to the last segment of the **stdoutLogFile** provided.</span></span></p><p><span data-ttu-id="3cb84-157">Výchozí hodnota je `aspnetcore-stdout`.</span><span class="sxs-lookup"><span data-stu-id="3cb84-157">The default value is `aspnetcore-stdout`.</span></span></p> |
| <span data-ttu-id="3cb84-158">forwardWindowsAuthToken</span><span class="sxs-lookup"><span data-stu-id="3cb84-158">forwardWindowsAuthToken</span></span> | <span data-ttu-id="3cb84-159">Hodnota true nebo false</span><span class="sxs-lookup"><span data-stu-id="3cb84-159">true or false.</span></span></p><p><span data-ttu-id="3cb84-160">V případě hodnoty true token se předají do podřízeného procesu naslouchání na ASPNETCORE_PORT % jako hlavičku, MS-ASPNETCORE-WINAUTHTOKEN' každý požadavek.</span><span class="sxs-lookup"><span data-stu-id="3cb84-160">If true, the token will be forwarded to the child process listening on %ASPNETCORE_PORT% as a header 'MS-ASPNETCORE-WINAUTHTOKEN' per request.</span></span> <span data-ttu-id="3cb84-161">Je zodpovědností procesu pro volání funkce CloseHandle na tento token na základě požadavku.</span><span class="sxs-lookup"><span data-stu-id="3cb84-161">It is the responsibility of that process to call CloseHandle on this token per request.</span></span></p><p><span data-ttu-id="3cb84-162">Výchozí hodnota je true.</span><span class="sxs-lookup"><span data-stu-id="3cb84-162">The default value is true.</span></span></p> |
| <span data-ttu-id="3cb84-163">disableStartUpErrorPage</span><span class="sxs-lookup"><span data-stu-id="3cb84-163">disableStartUpErrorPage</span></span> | <span data-ttu-id="3cb84-164">Hodnota true nebo false</span><span class="sxs-lookup"><span data-stu-id="3cb84-164">true or false.</span></span></p><p><span data-ttu-id="3cb84-165">V případě hodnoty true **502.5 - selhání procesu** potlačeno stránky a znaková stránka 502 stav konfigurované v vaše *web.config* bude mít přednost.</span><span class="sxs-lookup"><span data-stu-id="3cb84-165">If true, the **502.5 - Process Failure** page will be suppressed, and the 502 status code page configured in your *web.config* will take precedence.</span></span></p><p><span data-ttu-id="3cb84-166">Výchozí hodnota je False.</span><span class="sxs-lookup"><span data-stu-id="3cb84-166">The default value is false.</span></span></p> |

### <a name="setting-environment-variables"></a><span data-ttu-id="3cb84-167">Nastavení proměnných prostředí</span><span class="sxs-lookup"><span data-stu-id="3cb84-167">Setting environment variables</span></span>

<span data-ttu-id="3cb84-168">Základní modul ASP.NET umožňuje zadat proměnné prostředí pro proces zadaný v `processPath` atribut zadáním v jednom nebo více `environmentVariable` podřízených elementů `environmentVariables` kolekce element v části `aspNetCore` elementu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-168">The ASP.NET Core Module allows you specify environment variables for the process specified in the `processPath` attribute by specifying them in one or more `environmentVariable` child elements of an `environmentVariables` collection element under the `aspNetCore` element.</span></span> <span data-ttu-id="3cb84-169">Nastavení v této části proměnné prostředí mají přednost před systému proměnných prostředí pro proces.</span><span class="sxs-lookup"><span data-stu-id="3cb84-169">Environment variables set in this section take precedence over system environment variables for the process.</span></span>

<span data-ttu-id="3cb84-170">Následující příklad nastaví dvou proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="3cb84-170">The example below sets two environment variables.</span></span> <span data-ttu-id="3cb84-171">`ASPNETCORE_ENVIRONMENT`nakonfiguruje prostředí aplikace `Development`.</span><span class="sxs-lookup"><span data-stu-id="3cb84-171">`ASPNETCORE_ENVIRONMENT` will configure the application's environment to `Development`.</span></span> <span data-ttu-id="3cb84-172">Vývojář může dočasně nastavit, že tato hodnota *web.config* souboru, aby bylo možné vynutit [vývojáře výjimka stránky](xref:fundamentals/error-handling) načíst při ladění výjimku aplikace.</span><span class="sxs-lookup"><span data-stu-id="3cb84-172">A developer may temporarily set this value in the *web.config* file in order to force the [developer exception page](xref:fundamentals/error-handling) to load when debugging an app exception.</span></span> <span data-ttu-id="3cb84-173">`CONFIG_DIR`je příkladem proměnnou uživatelské prostředí, kde byl vývojář zápis kód, který bude číst hodnotu na spuštění a aby bylo možné načíst konfigurační soubor aplikace vytvořit cestu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-173">`CONFIG_DIR` is an example of a user-defined environment variable, where the developer has written code that will read the value on startup to form a path in order to load the app's configuration file.</span></span>

```xml
<aspNetCore processPath="dotnet"
      arguments=".\MyApp.dll"
      stdoutLogEnabled="false"
      stdoutLogFile="\\?\%home%\LogFiles\stdout">
  <environmentVariables>
    <environmentVariable name="ASPNETCORE_ENVIRONMENT" value="Development" />
    <environmentVariable name="CONFIG_DIR" value="f:\application_config" />
  </environmentVariables>
</aspNetCore>
```

## <a name="appofflinehtm"></a><span data-ttu-id="3cb84-174">App_offline.htm</span><span class="sxs-lookup"><span data-stu-id="3cb84-174">app_offline.htm</span></span>

<span data-ttu-id="3cb84-175">Pokud umístit soubor s názvem *app_offline.htm* v kořenovém adresáři adresář webové aplikace, bude modul základní technologie ASP.NET pokusí řádně vypnutí aplikace a zastavit zpracování příchozích požadavků.</span><span class="sxs-lookup"><span data-stu-id="3cb84-175">If you place a file with the name *app_offline.htm* at the root of a web application directory, the ASP.NET Core Module will attempt to gracefully shutdown the app and stop processing incoming requests.</span></span> <span data-ttu-id="3cb84-176">Pokud aplikace stále běží `shutdownTimeLimit` počet sekund, bude modul ASP.NET Core kill běžící proces.</span><span class="sxs-lookup"><span data-stu-id="3cb84-176">If the app is still running after `shutdownTimeLimit` number of seconds, the ASP.NET Core Module will kill the running process.</span></span>

<span data-ttu-id="3cb84-177">Při *app_offline.htm* je soubor k dispozici, modul ASP.NET Core bude odpovídat na požadavky odesláním zpět obsah *app_offline.htm* souboru.</span><span class="sxs-lookup"><span data-stu-id="3cb84-177">While the *app_offline.htm* file is present, the ASP.NET Core Module will respond to requests by sending back the contents of the *app_offline.htm* file.</span></span> <span data-ttu-id="3cb84-178">Jednou *app_offline.htm* soubor bude odstraněn, další požadavek načte aplikaci, které pak reaguje na požadavky.</span><span class="sxs-lookup"><span data-stu-id="3cb84-178">Once the *app_offline.htm* file is removed, the next request loads the application, which then responds to requests.</span></span>

## <a name="start-up-error-page"></a><span data-ttu-id="3cb84-179">Spuštění chybová stránka</span><span class="sxs-lookup"><span data-stu-id="3cb84-179">Start-up error page</span></span>

<span data-ttu-id="3cb84-180">Pokud modul jádro ASP.NET se nepodaří spustit proces back-end nebo spustí proces back-end ale selže tak, aby naslouchala na konfigurovaném portu, zobrazí se na stránku kód stavu HTTP 502.5.</span><span class="sxs-lookup"><span data-stu-id="3cb84-180">If the ASP.NET Core Module fails to launch the backend process or the backend process starts but fails to listen on the configured port, you will see an HTTP 502.5 status code page.</span></span> <span data-ttu-id="3cb84-181">Chcete-li potlačit tuto stránku a vrátit na stránku výchozího stavu služby IIS 502 kódu, použijte `disableStartUpErrorPage` atribut.</span><span class="sxs-lookup"><span data-stu-id="3cb84-181">To suppress this page and revert to the default IIS 502 status code page, use the `disableStartUpErrorPage` attribute.</span></span> <span data-ttu-id="3cb84-182">Další informace o konfiguraci vlastních chybových zpráv najdete v tématu [chyby protokolu HTTP `<httpErrors>` ](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).</span><span class="sxs-lookup"><span data-stu-id="3cb84-182">For more information on configuring custom error messages, see [HTTP Errors `<httpErrors>`](https://docs.microsoft.com/iis/configuration/system.webServer/httpErrors/).</span></span>

![502 stavové stránce](aspnet-core-module/_static/ANCM-502_5.png)

## <a name="log-creation-and-redirection"></a><span data-ttu-id="3cb84-184">Vytvoření protokolu a přesměrování</span><span class="sxs-lookup"><span data-stu-id="3cb84-184">Log creation and redirection</span></span>

<span data-ttu-id="3cb84-185">Základní modul ASP.NET přesměruje `stdout` a `stderr` protokoly na disk, pokud jste nastavili `stdoutLogEnabled` a `stdoutLogFile` atributy `aspNetCore` element.</span><span class="sxs-lookup"><span data-stu-id="3cb84-185">The ASP.NET Core Module redirects `stdout` and `stderr` logs to disk if you set the `stdoutLogEnabled` and `stdoutLogFile` attributes of the `aspNetCore` element.</span></span> <span data-ttu-id="3cb84-186">Všechny složky aplikace `stdoutLogFile` cesta, musí existovat v pořadí pro modul pro vytvoření souboru protokolu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-186">Any folders in the `stdoutLogFile` path must exist in order for the module to create the log file.</span></span> <span data-ttu-id="3cb84-187">Časové razítko a soubor rozšíření se automaticky přidá, když se vytvoří soubor protokolu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-187">A timestamp and file extension will be added automatically when the log file is created.</span></span> <span data-ttu-id="3cb84-188">Protokoly nejsou otáčet, pokud dojde k recyklování procesů nebo restartování.</span><span class="sxs-lookup"><span data-stu-id="3cb84-188">Logs are not rotated, unless process recycling/restart occurs.</span></span> <span data-ttu-id="3cb84-189">Je zodpovědností hostitel k omezení místa na disku, které využívají protokol.</span><span class="sxs-lookup"><span data-stu-id="3cb84-189">It is the responsibility of the hoster to limit the disk space the logs consume.</span></span> <span data-ttu-id="3cb84-190">Pomocí `stdout` protokolu se doporučuje jenom pro řešení potíží při spuštění aplikace a ne pro účely protokolování obecné aplikace.</span><span class="sxs-lookup"><span data-stu-id="3cb84-190">Using the `stdout` log is only recommended for troubleshooting application startup issues and not for general application logging purposes.</span></span>

<span data-ttu-id="3cb84-191">Název souboru protokolu se skládá připojením ID procesu (PID), časové razítko (*yyyyMdhms*) a příponu souboru (*.log*) na poslední segment `stdoutLogFile` cesta (obvykle *stdout* ) oddělená podtržítka.</span><span class="sxs-lookup"><span data-stu-id="3cb84-191">The log file name is composed by appending the process ID (PID), timestamp (*yyyyMdhms*), and file extension (*.log*) to the last segment of the `stdoutLogFile` path (typically *stdout*) delimited by underscores.</span></span> <span data-ttu-id="3cb84-192">Například pokud `stdoutLogFile` cesta končí *stdout*, má název souboru protokolu pro aplikace pomocí PID 10652 vytvořen v 8/10/2017 v 12:05:02 *stdout_10652_20178101252.log*.</span><span class="sxs-lookup"><span data-stu-id="3cb84-192">For example if the `stdoutLogFile` path ends with *stdout*, a log for an app with a PID of 10652 created on 8/10/2017 at 12:05:02 has the file name *stdout_10652_20178101252.log*.</span></span>

<span data-ttu-id="3cb84-193">Zde je ukázka `aspNetCore` element, který konfiguruje `stdout` protokolování.</span><span class="sxs-lookup"><span data-stu-id="3cb84-193">Here's a sample `aspNetCore` element that configures `stdout` logging.</span></span> <span data-ttu-id="3cb84-194">`stdoutLogFile` Cesta ukazuje příklad je vhodný pro Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="3cb84-194">The `stdoutLogFile` path shown in the example is appropriate for the Azure App Service.</span></span> <span data-ttu-id="3cb84-195">Místní cestu nebo cestu sdílené síťové složky je přijatelné pro místní protokolování.</span><span class="sxs-lookup"><span data-stu-id="3cb84-195">A local path or network share path is acceptable for local logging.</span></span> <span data-ttu-id="3cb84-196">Zkontrolujte, jestli uživatel identita fondu aplikací má oprávnění k zápisu do zadané cestě.</span><span class="sxs-lookup"><span data-stu-id="3cb84-196">Confirm that the AppPool user identity has permission to write to the path provided.</span></span>

```xml
<aspNetCore processPath="dotnet"
    arguments=".\MyApp.dll"
    stdoutLogEnabled="true"
    stdoutLogFile="\\?\%home%\LogFiles\stdout">
</aspNetCore>
```

## <a name="aspnet-core-module-with-an-iis-shared-configuration"></a><span data-ttu-id="3cb84-197">Modul ASP.NET Core s službu IIS sdílenou konfiguraci</span><span class="sxs-lookup"><span data-stu-id="3cb84-197">ASP.NET Core Module with an IIS Shared Configuration</span></span>

<span data-ttu-id="3cb84-198">Základní modul ASP.NET instalační program spustí s oprávněními **systému** účtu.</span><span class="sxs-lookup"><span data-stu-id="3cb84-198">The ASP.NET Core Module installer runs with the privileges of the **SYSTEM** account.</span></span> <span data-ttu-id="3cb84-199">Vzhledem k tomu, že místní systémový účet neobsahuje oprávnění k provádění oprávnění pro cestu ke sdílené složce, kterou používá sdílené konfiguraci IIS, instalační program, se setkají chybu při pokusu o konfiguraci nastavení modulu v odepření přístupu  *applicationHost.config* ve sdílené složce.</span><span class="sxs-lookup"><span data-stu-id="3cb84-199">Because the local system account does not have modify permission for the share path which is used by the IIS Shared Configuration, the installer will hit an access denied error when attempting to configure the module settings in *applicationHost.config* on the share.</span></span>

<span data-ttu-id="3cb84-200">Nepodporované řešením je zakázat sdílené konfiguraci IIS, spusťte instalační program, exportovat aktualizovaný *applicationHost.config* souborů do sdílené složky a znovu povolit sdílenou konfiguraci IIS.</span><span class="sxs-lookup"><span data-stu-id="3cb84-200">The unsupported workaround is to disable the IIS Shared Configuration, run the installer, export the updated *applicationHost.config* file to the share, and re-enable the IIS Shared Configuration.</span></span>

## <a name="module-schema-and-configuration-file-locations"></a><span data-ttu-id="3cb84-201">Umístění souborů modulu, schémat a konfigurace</span><span class="sxs-lookup"><span data-stu-id="3cb84-201">Module, schema, and configuration file locations</span></span>

### <a name="module"></a><span data-ttu-id="3cb84-202">Modul</span><span class="sxs-lookup"><span data-stu-id="3cb84-202">Module</span></span>

<span data-ttu-id="3cb84-203">**Služba IIS (x86 nebo amd64):**</span><span class="sxs-lookup"><span data-stu-id="3cb84-203">**IIS (x86/amd64):**</span></span>

   * <span data-ttu-id="3cb84-204">%windir%\System32\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3cb84-204">%windir%\System32\inetsrv\aspnetcore.dll</span></span>

   * <span data-ttu-id="3cb84-205">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3cb84-205">%windir%\SysWOW64\inetsrv\aspnetcore.dll</span></span>

<span data-ttu-id="3cb84-206">**Služba IIS Express (x86 nebo amd64):**</span><span class="sxs-lookup"><span data-stu-id="3cb84-206">**IIS Express (x86/amd64):**</span></span>

   * <span data-ttu-id="3cb84-207">%ProgramFiles%\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3cb84-207">%ProgramFiles%\IIS Express\aspnetcore.dll</span></span>

   * <span data-ttu-id="3cb84-208">% ProgramFiles (x86) %\IIS Express\aspnetcore.dll</span><span class="sxs-lookup"><span data-stu-id="3cb84-208">%ProgramFiles(x86)%\IIS Express\aspnetcore.dll</span></span>

### <a name="schema"></a><span data-ttu-id="3cb84-209">Schéma</span><span class="sxs-lookup"><span data-stu-id="3cb84-209">Schema</span></span>

<span data-ttu-id="3cb84-210">**SLUŽBY IIS**</span><span class="sxs-lookup"><span data-stu-id="3cb84-210">**IIS**</span></span>

   * <span data-ttu-id="3cb84-211">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.XML</span><span class="sxs-lookup"><span data-stu-id="3cb84-211">%windir%\System32\inetsrv\config\schema\aspnetcore_schema.xml</span></span>

<span data-ttu-id="3cb84-212">**Služby IIS Express**</span><span class="sxs-lookup"><span data-stu-id="3cb84-212">**IIS Express**</span></span>

   * <span data-ttu-id="3cb84-213">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span><span class="sxs-lookup"><span data-stu-id="3cb84-213">%ProgramFiles%\IIS Express\config\schema\aspnetcore_schema.xml</span></span>

### <a name="configuration"></a><span data-ttu-id="3cb84-214">Konfigurace</span><span class="sxs-lookup"><span data-stu-id="3cb84-214">Configuration</span></span>

<span data-ttu-id="3cb84-215">**SLUŽBY IIS**</span><span class="sxs-lookup"><span data-stu-id="3cb84-215">**IIS**</span></span>

   * <span data-ttu-id="3cb84-216">%windir%\System32\inetsrv\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="3cb84-216">%windir%\System32\inetsrv\config\applicationHost.config</span></span>

<span data-ttu-id="3cb84-217">**Služby IIS Express**</span><span class="sxs-lookup"><span data-stu-id="3cb84-217">**IIS Express**</span></span>

   * <span data-ttu-id="3cb84-218">.vs\config\applicationHost.config</span><span class="sxs-lookup"><span data-stu-id="3cb84-218">.vs\config\applicationHost.config</span></span>

<span data-ttu-id="3cb84-219">Můžete vyhledat *aspnetcore.dll* v *applicationHost.config* souboru.</span><span class="sxs-lookup"><span data-stu-id="3cb84-219">You can search for *aspnetcore.dll* in the *applicationHost.config* file.</span></span> <span data-ttu-id="3cb84-220">Pro službu IIS Express *applicationHost.config* soubor nebude existovat ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="3cb84-220">For IIS Express, the *applicationHost.config* file won't exist by default.</span></span> <span data-ttu-id="3cb84-221">Soubor je vytvořen v *{kořenový adresář aplikace}\.vs\config* při spuštění jakékoli projekt webové aplikace v řešení sady Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3cb84-221">The file is created at *{application root}\.vs\config* when you start any web application project in the Visual Studio solution.</span></span>