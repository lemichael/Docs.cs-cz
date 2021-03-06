---
title: Autorizace na základě zásad v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak vytvořit a použít rutiny zásad autorizace pro vynucují požadavky pro autorizaci v aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837345"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Autorizace na základě zásad v ASP.NET Core

Na pozadí [autorizace na základě rolí](xref:security/authorization/roles) a [autorizace na základě rolí](xref:security/authorization/claims) pomocí požadavku, obslužné rutiny požadavku a předem nakonfigurované zásady. Tyto stavební bloky podporují výraz vyhodnocení autorizace v kódu. Výsledkem je struktura bohatší, opakovaně použitelný, testovatelného autorizace.

Zásady autorizace se skládá z jednoho nebo více požadavků. Je registrován jako součást konfigurace služby ověřování v `Startup.ConfigureServices` metody:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

V předchozím příkladu se vytvoří zásadu "AtLeast21". Má jeden požadavek&mdash;s minimálním stářím, který je poskytnut jako parametr s požadavkem.

## <a name="iauthorizationservice"></a>IAuthorizationService 

Primární služba, která určuje, zda je ověření úspěšné <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

Předchozí kód zvýrazní dvou metod [služby IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> je služba značky se žádné metody a mechanismus pro sledování, jestli je ověřování úspěšné.

Každý <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> zodpovídá za probíhá kontrola, zda jsou splněny požadavky:
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> Třída je obslužná rutina používá k označení, zda byly splněny požadavky:

```csharp
 context.Succeed(requirement)
```

Následující kód ukazuje zjednodušený (i s poznámkami s komentáři) výchozí implementace autorizační službu:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

Následující kód ukazuje typické `ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

Použití <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> nebo `[Authorize(Policy = "Something"]` pro autorizaci.

## <a name="applying-policies-to-mvc-controllers"></a>Přiřazení zásad pro kontrolery MVC

Pokud používáte pro stránky Razor, přečtěte si téma [přiřazení zásad pro stránky Razor](#applying-policies-to-razor-pages) v tomto dokumentu.

Zásady se použijí k řadičům pomocí `[Authorize]` atribut s názvem zásady. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Přiřazení zásad pro stránky Razor

Zásady se použijí pro stránky Razor pomocí `[Authorize]` atribut s názvem zásady. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Zásady můžete použít také pro stránky Razor pomocí [autorizace konvence](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Požadavky

Požadavek na autorizaci je kolekce dat parametry, které zásady můžete použít k vyhodnocení aktuální hlavní název uživatele. V zásadách "AtLeast21" požadavek je jediný parametr&mdash;minimálním stářím. Implementuje požadavek [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), což je rozhraní prázdnou značku. Parametrizované minimální stáří požadavek může být implementován takto:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Zásad autorizace obsahuje více požadavků na autorizaci, všechny požadavky musí předat v pořadí pro hodnocení zásad proběhla úspěšně. Jinými slovy, více autorizace požadavky do jednoho autorizační zásady jsou považovány na **a** základ.

> [!NOTE]
> Požadavek není nutné mít data nebo vlastnosti.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Povolení obslužné rutiny

Je zodpovědná za vyhodnocení vlastností požadavek na obslužnou rutinu ověřování. Obslužná rutina autorizace vyhodnotí požadavky proti zadaný [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) k určení, zda je povolen přístup.

Může mít požadavek [více obslužných rutin](#security-authorization-policies-based-multiple-handlers). Obslužná rutina může dědit [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), kde `TRequirement` je potřeba zpracovat. Alternativně může implementovat obslužnou rutinu [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) pro zpracování více než jeden typ požadavku.

### <a name="use-a-handler-for-one-requirement"></a>Použijte obslužnou rutinu pro jeden požadavek

<a name="security-authorization-handler-example"></a>

Následuje příklad vztah 1: 1, ve kterém obslužnou rutinu minimální stáří využívá jeden požadavek:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Předchozí kód určuje, zda má aktuální uživatel instančního objektu datum narození deklarace identity, který byl vydán známého a důvěryhodného vystavitele. Ověření nelze provést, pokud chybí deklarace identity, v takovém případě se vrátí dokončeného úkolu. Při deklaraci identity je k dispozici, se vypočítá věk uživatele. Pokud uživatel splňuje minimální stáří určené požadavku, autorizace se bude považovat za úspěšné. Když je ověření úspěšné, `context.Succeed` vyvolání splnění požadavku jako svůj jediný parametr.

### <a name="use-a-handler-for-multiple-requirements"></a>Použijte obslužnou rutinu pro několik požadavků

Následuje příklad vztah jeden mnoho, ve kterém obslužnou rutinu oprávnění dokáže zpracovat tři různé typy požadavků:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Předchozí kód projde [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;vlastnost obsahující požadavky není označena jako úspěšně dokončený. Pro `ReadPermission` požadavek, uživatel musí být vlastníkem nebo sponzor přístup k požadovanému prostředku. V případě třídy `EditPermission` nebo `DeletePermission` požadavek, nezíská musí být vlastníkem pro přístup k požadovanému prostředku.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Zápis obslužné rutiny

Obslužné rutiny jsou registrovány v kolekci služeb během konfigurace. Příklad:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Předchozí kód registruje `MinimumAgeHandler` jako singleton vyvoláním `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Obslužné rutiny lze registrovat pomocí některé z integrovaného [služby životnosti](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Co by měl vrátit obslužnou rutinu?

Všimněte si, že `Handle` metodu [příklad obslužná rutina](#security-authorization-handler-example) nevrací žádnou hodnotu. Jak se stavem úspěch nebo selhání uvedené?

* Obslužná rutina indikuje úspěch voláním `context.Succeed(IAuthorizationRequirement requirement)`, předá tento požadavek, který se úspěšně ověřilo.

* Obslužná rutina nemusí řešit selhání obecně platí, jako ostatní obslužné rutiny pro stejný požadavek může být úspěšné.

* Chcete-li zaručit selhání, i v případě ostatních obslužných rutin požadavek úspěšný, zavolejte `context.Fail`.

Pokud obslužná rutina volá `context.Succeed` nebo `context.Fail`, stále se nazývají všechny ostatní obslužné rutiny. To umožňuje požadavek na vytvoření vedlejší účinky, jako je například protokolování, které dojde i v případě, že má jinou obslužnou rutinu úspěšně ověřen nebo požadavku se nezdařilo. Pokud je nastavena na `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) vlastnosti (k dispozici v ASP.NET Core 1.1 nebo novější) zkratům provádění obslužné rutiny při `context.Fail` je volána. `InvokeHandlersAfterFailure` Výchozí hodnota je `true`, v takovém případě se nazývají všechny obslužné rutiny.

> [!NOTE]
> I když se ověřování nezdaří, jsou volány obslužné rutiny autorizace.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Proč byste měli více obslužných rutin pro požadavek?

V případech, kde chcete vyhodnocení na **nebo** základ, implementovat více obslužných rutin pro jeden požadavek. Microsoft má například dveří, které otevřít pouze pomocí klíčů karty. Ponecháte-li karta s klíčem v domácnostech, recepční vytiskne dočasné štítku a otevře dveře za vás. V tomto scénáři bude mít jeden požadavek *BuildingEntry*, ale více obslužných rutin, každý z nich zkoumání jeden požadavek.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Ujistěte se, že jsou oba obslužné rutiny [zaregistrovaný](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Pokud je buď obslužná rutina úspěšné při zásady vyhodnotí `BuildingEntryRequirement`, hodnocení zásad úspěšné.

## <a name="using-a-func-to-fulfill-a-policy"></a>Používat funkci func pro splnění zásad

Může nastat situace, ve které splňující zásady je jednoduchý vyjádřit v kódu. Je možné zadat `Func<AuthorizationHandlerContext, bool>` při konfiguraci zásad s `RequireAssertion` tvůrce zásad.

Například předchozí `BadgeEntryHandler` měl by být přepsán následujícím způsobem:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Přístup k kontext požadavku MVC v obslužné rutině

`HandleRequirementAsync` Implementovat v obslužné rutině autorizační metoda má dva parametry: `AuthorizationHandlerContext` a `TRequirement` obsluhujete. Architektur, jako je MVC nebo Jabbr jsou můžete přidat libovolný objekt `Resource` vlastnost `AuthorizationHandlerContext` k předávání dalších informací.

Například MVC předá instance [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) v `Resource` vlastnost. Tato vlastnost poskytuje přístup k `HttpContext`, `RouteData`a všechno, co je jinak poskytované MVC a stránky Razor.

Použití `Resource` vlastnost je konkrétní rozhraní framework. Pomocí informací `Resource` vlastnost omezuje vaše zásady autorizace pro konkrétní platformy. Měli přetypovat `Resource` pomocí vlastnosti `is` – klíčové slovo a potom potvrdit přetypování proběhla úspěšně, aby se váš kód nebude k chybě s `InvalidCastException` při spuštění na ostatní platformy:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
