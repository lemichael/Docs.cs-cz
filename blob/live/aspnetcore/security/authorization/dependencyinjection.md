---
title: "Vkládání závislostí v obslužné rutiny požadavku"
author: rick-anderson
description: "Tento dokument popisuje postup vložit obslužné rutiny požadavek ověřování do aplikace ASP.NET Core pomocí vkládání závislostí."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/dependencyinjection
ms.openlocfilehash: 9aaa356fe67a7e2c8177ffa1b886ec6b3dc13ef0
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="dependency-injection-in-requirement-handlers"></a><span data-ttu-id="419f2-103">Vkládání závislostí v obslužné rutiny požadavku</span><span class="sxs-lookup"><span data-stu-id="419f2-103">Dependency Injection in requirement handlers</span></span>

<a name="security-authorization-di"></a>

<span data-ttu-id="419f2-104">[Autorizace obslužné rutiny musí být zaregistrovaný](policies.md#handler-registration) v kolekci služby během konfigurace (pomocí [vkládání závislostí](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span><span class="sxs-lookup"><span data-stu-id="419f2-104">[Authorization handlers must be registered](policies.md#handler-registration) in the service collection during configuration (using [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)).</span></span>

<span data-ttu-id="419f2-105">Předpokládejme, že jste měli úložiště pravidel, kterou chcete vyhodnotit uvnitř obslužné rutiny autorizace a že úložiště se zaregistroval v kolekci služby.</span><span class="sxs-lookup"><span data-stu-id="419f2-105">Suppose you had a repository of rules you wanted to evaluate inside an authorization handler and that repository was registered in the service collection.</span></span> <span data-ttu-id="419f2-106">Autorizace se vyřešit a který vložit do vaší konstruktor.</span><span class="sxs-lookup"><span data-stu-id="419f2-106">Authorization will resolve and inject that into your constructor.</span></span>

<span data-ttu-id="419f2-107">Například pokud jste chtěli použít ASP. NET je protokolování infrastrukturu, kterou chcete vložit `ILoggerFactory` do vaší obslužné rutiny.</span><span class="sxs-lookup"><span data-stu-id="419f2-107">For example, if you wanted to use ASP.NET's logging infrastructure you would want to inject `ILoggerFactory` into your handler.</span></span> <span data-ttu-id="419f2-108">Tato obslužná rutina může vypadat například:</span><span class="sxs-lookup"><span data-stu-id="419f2-108">Such a handler might look like:</span></span>

```csharp
public class LoggingAuthorizationHandler : AuthorizationHandler<MyRequirement>
   {
       ILogger _logger;

       public LoggingAuthorizationHandler(ILoggerFactory loggerFactory)
       {
           _logger = loggerFactory.CreateLogger(this.GetType().FullName);
       }

       protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MyRequirement requirement)
       {
           _logger.LogInformation("Inside my handler");
           // Check if the requirement is fulfilled.
           return Task.CompletedTask;
       }
   }
   ```

<span data-ttu-id="419f2-109">Zaregistrujete by obslužná rutina s `services.AddSingleton()`:</span><span class="sxs-lookup"><span data-stu-id="419f2-109">You would register the handler with `services.AddSingleton()`:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, LoggingAuthorizationHandler>();
```

<span data-ttu-id="419f2-110">Instance obslužné rutiny bude vytvořen při spuštění aplikace a bude DI vložit zaregistrovanou `ILoggerFactory` do vaší konstruktor.</span><span class="sxs-lookup"><span data-stu-id="419f2-110">An instance of the handler will be created when your application starts, and DI will inject the registered `ILoggerFactory` into your constructor.</span></span>

> [!NOTE]
> <span data-ttu-id="419f2-111">Obslužné rutiny, které používají rozhraní Entity Framework by neměl být registrován jako jednotlivé prvky.</span><span class="sxs-lookup"><span data-stu-id="419f2-111">Handlers that use Entity Framework shouldn't be registered as singletons.</span></span>