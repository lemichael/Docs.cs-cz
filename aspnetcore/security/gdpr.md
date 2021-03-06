---
title: General Data Protection Regulation (GDPR) podporují v ASP.NET Core
author: rick-anderson
description: Zjistěte, jak získat přístup k GDPR Rozšiřovací body ve webové aplikaci ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: security/gdpr
ms.openlocfilehash: 01d2f8943c0995c1400122b89c4ca7c459a85279
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724566"
---
# <a name="eu-general-data-protection-regulation-gdpr-support-in-aspnet-core"></a>Podpora EU obecného Regulation (GDPR) v ASP.NET Core

Podle [Rick Anderson](https://twitter.com/RickAndMSFT)

ASP.NET Core nabízí rozhraní API a šablony, které vám pomohou splnit některé [EU obecného Regulation (GDPR)](https://www.eugdpr.org/) požadavky:

::: moniker range=">= aspnetcore-3.0"

* Šablony projektů zahrnout Rozšiřovací body a provizorní kód, který můžete nahradit ochrany osobních údajů a zásady použití souborů cookie.
* *Pages/Privacy.cshtml* stránky nebo *Views/Home/Privacy.cshtml* zobrazení obsahuje stránku, kterou chcete podrobnosti o zásadách ochrany osobních údajů vašeho webu.

Součástí šablony ASP.NET Core 2.2 v aplikaci ASP.NET Core 3.0 vygenerovanou šablonu k povolení této funkce výchozí soubor cookie souhlas s tímto způsobem:

* Přidat [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) příliš `Startup.ConfigureServices` a [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) k `Startup.Configure`:

  [!code-csharp[Main](gdpr/sample/RP3.0/Startup.cs?name=snippet1&highlight=12-19,38)]

* Přidejte částečnou souhlasu soubor cookie k *_Layout.cshtml* souboru:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_Layout.cshtml?name=snippet&highlight=4)]

* Přidat  *\_CookieConsentPartial.cshtml* soubor do projektu:

  [!code-cshtml[Main](gdpr/sample/RP3.0/Pages/Shared/_CookieConsentPartial.cshtml)]

* Vyberte verzi 2.2 technologie ASP.NET Core tohoto článku, přečtěte si informace o funkci soubor cookie souhlas.

::: moniker-end

::: moniker range="= aspnetcore-2.2"

* Šablony projektů zahrnout Rozšiřovací body a provizorní kód, který můžete nahradit ochrany osobních údajů a zásady použití souborů cookie.
* Souhlas funkci soubor cookie umožňuje požádat o (a sledovat) souhlas uživatele pro ukládání osobních údajů. Pokud uživatel nevyjádřil souhlas shromažďování dat a k němu má aplikace [CheckConsentNeeded](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.checkconsentneeded) nastavena na `true`, které není nezbytné soubory cookie se neodesílají do prohlížeče.
* Soubory cookie, může být označený jako důležité. Základní cookie se odesílají do prohlížeče, i v případě, že uživatel nevyjádřil a sledování je vypnuté.
* [Soubory cookie TempData a relace](#tempdata) nejsou funkční při sledování je vypnuté.
* [Spravovat Identity](#pd) stránka obsahuje odkaz ke stažení a odstranění dat uživatele.

[Ukázkovou aplikaci](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) umožňuje testování většinu Rozšiřovací body podle nařízení GDPR a přidali rozhraní API pro ASP.NET Core 2.1 šablony. Zobrazit [ReadMe](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) souboru pokynů.

[Zobrazení nebo stažení ukázkového kódu](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) ([stažení](xref:index#how-to-download-a-sample))

## <a name="aspnet-core-gdpr-support-in-template-generated-code"></a>ASP.NET Core GDPR podporují v šabloně generovaný kód

Stránky Razor a MVC projekty vytvořené pomocí šablony projektu zahrnují následující podpory nařízení GDPR:

* [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) a [UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) jsou nastaveny `Startup` třídy.
* *\_CookieConsentPartial.cshtml* [částečné zobrazení](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper). **Přijmout** tlačítko je zahrnuta v tomto souboru. Pokud uživatel klikne **přijmout** tlačítko, vyjadřoval souhlas k ukládání souborů cookie je k dispozici.
* *Pages/Privacy.cshtml* stránky nebo *Views/Home/Privacy.cshtml* zobrazení obsahuje stránku, kterou chcete podrobnosti o zásadách ochrany osobních údajů vašeho webu. *\_CookieConsentPartial.cshtml* souboru vytvoří odkaz na stránku o ochraně osobních údajů.
* U aplikací vytvořených pomocí jednotlivých uživatelských účtů, Správa stránka obsahuje odkazy na stažení a odstranění [osobní údaje](#pd).

### <a name="cookiepolicyoptions-and-usecookiepolicy"></a>CookiePolicyOptions a UseCookiePolicy

[CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) jsou inicializovány v `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/Startup.cs?name=snippet1&highlight=14-20)]

[UseCookiePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyappbuilderextensions.usecookiepolicy) je volána `Startup.Configure`:

[!code-csharp[](gdpr/sample/Startup.cs?name=snippet1&highlight=51)]

### <a name="cookieconsentpartialcshtml-partial-view"></a>\_CookieConsentPartial.cshtml částečného zobrazení

*\_CookieConsentPartial.cshtml* částečné zobrazení:

[!code-html[](gdpr/sample/RP2.2/Pages/Shared/_CookieConsentPartial.cshtml)]

Tato části:

* Získá stav sledování pro uživatele. Pokud aplikace je nakonfigurovaná tak, aby vyžadovala souhlasu, musí uživatel souhlasit předtím, než lze sledovat soubory cookie. Pokud se vyžaduje souhlas, panelu souhlasu souboru cookie vyřešen v horní navigační panel vytvořené  *\_Layout.cshtml* souboru.
* Poskytuje HTML `<p>` element slouží ke shrnutí vašich osobních údajů a soubory cookie použít zásady.
* Obsahuje odkaz na stránku o ochraně osobních údajů nebo zobrazení, ve kterém můžete podrobně popisují zásady ochrany osobních údajů vašeho webu.

## <a name="essential-cookies"></a>Základní soubory cookie

Pokud souhlas k ukládání souborů cookie nebyl zadán, jsou jenom soubory cookie označené základní odesláno prohlížeči. Následující kód provede základní cookie:

[!code-csharp[Main](gdpr/sample/RP2.2/Pages/Cookie.cshtml.cs?name=snippet1&highlight=5)]

<a name="tempdata"></a>

### <a name="tempdata-provider-and-session-state-cookies-arent-essential"></a>Soubory cookie stavu TempData zprostředkovatele a relace nejsou essential

[TempData poskytovatele](xref:fundamentals/app-state#tempdata) souboru cookie není podstatné. Pokud sledování je vypnuté, zprostředkovatel TempData není funkční. K povolení zprostředkovatele TempData při sledování je vypnuté, označte TempData soubor cookie jako důležité pro `Startup.ConfigureServices`:

[!code-csharp[Main](gdpr/sample/RP2.2/Startup.cs?name=snippet1)]

[Stav relace](xref:fundamentals/app-state) soubory cookie nejsou nezbytně nutné. Stav relace není funkční, při sledování je vypnuté. Následující kód provede nezbytné soubory cookie relace:

[!code-csharp[](gdpr/sample/RP2.2/Startup.cs?name=snippet2)]

<a name="pd"></a>

## <a name="personal-data"></a>Osobní údaje

Aplikace ASP.NET Core s jednotlivými uživatelskými účty vytvořené zahrnout kód ke stažení a odstranění osobních údajů.

Vyberte uživatelské jméno a pak vyberte **osobní údaje**:

![Spravovat stránky osobních údajů](gdpr/_static/pd.png)

Poznámky:

* Ke generování `Account/Manage` kódu naleznete v tématu [vygenerované uživatelské rozhraní Identity](xref:security/authentication/scaffold-identity).
* **Odstranit** a **Stáhnout** odkazy reagovat jen na výchozí identifikační údaje. Aplikace, které vytvořit vlastní uživatelská data musí být rozšířené na odstranění a stažení vlastní uživatelská data. Další informace najdete v tématu [přidat, stahování a odstranit vlastní uživatelská data na identitu](xref:security/authentication/add-user-data).
* Uložit tokeny pro daného uživatele, které jsou uložené v tabulce databáze Identity `AspNetUserTokens` se odstraní při odstranění uživatele prostřednictvím kaskádové odstranění chování kvůli [cizí klíč](https://github.com/aspnet/Identity/blob/release/2.1/src/EF/IdentityUserContext.cs#L152).
* [Externí zprostředkovatel ověřování](xref:security/authentication/social/index), jako je Facebook nebo Google, není k dispozici, před přijetím zásady souboru cookie.

::: moniker-end

## <a name="encryption-at-rest"></a>Šifrování v klidovém stavu

Některé databáze a úložiště mechanismy povolit šifrování v klidovém stavu. Šifrování v klidovém stavu:

* Uložená data automaticky šifruje.
* Šifruje bez konfigurace, programování a další práci pro software, který přistupuje k datům.
* Nejsnadnější a nejbezpečnější způsob je.
* Umožňuje databáze, kterou chcete spravovat klíče a šifrování.

Příklad:

* Microsoft SQL a Azure SQL poskytují [transparentního šifrování dat](/sql/relational-databases/security/encryption/transparent-data-encryption) (TDE).
* [SQL Azure ve výchozím nastavení šifruje databázi](https://azure.microsoft.com/updates/newly-created-azure-sql-databases-encrypted-by-default/)
* [Ve výchozím nastavení jsou zašifrovány Azure objekty BLOB, soubory, tabulky a fronty úložiště](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).

Pro databáze, které neposkytují integrované šifrování v klidovém stavu budete moci použít šifrování disku k poskytnutí stejnou úroveň ochrany. Příklad:

* [Nástroj BitLocker pro systém Windows Server](/windows/security/information-protection/bitlocker/bitlocker-how-to-deploy-on-windows-server)
* Linux:
  * [eCryptfs](https://launchpad.net/ecryptfs)
  * [EncFS](https://github.com/vgough/encfs).

## <a name="additional-resources"></a>Další zdroje

* [Microsoft.com/GDPR](https://www.microsoft.com/trustcenter/Privacy/GDPR)
