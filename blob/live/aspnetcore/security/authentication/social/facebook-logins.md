---
title: "Instalační program externí přihlášení sítě Facebook v ASP.NET Core"
author: rick-anderson
description: "Tento kurz představuje integraci ověřování uživatele účet Facebook do existující aplikace ASP.NET Core."
keywords: "ASP.NET Core, Facebook, přihlášení, ověřování"
ms.author: riande
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.assetid: 8c65179b-688c-4af1-8f5e-1862920cda95
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/facebook-logins
ms.openlocfilehash: 058670b4f699288e1acbe76bae08dcebf69346b8
ms.sourcegitcommit: 198fb0488e961048bfa376cf58cb853ef1d1cb91
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/14/2017
---
# <a name="configuring-facebook-authentication"></a><span data-ttu-id="6a8d3-104">Konfigurace ověřování Facebook</span><span class="sxs-lookup"><span data-stu-id="6a8d3-104">Configuring Facebook authentication</span></span>

<span data-ttu-id="6a8d3-105">Podle [Valeriy Novytskyy](https://github.com/01binary) a [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6a8d3-105">By [Valeriy Novytskyy](https://github.com/01binary) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="6a8d3-106">V tomto kurzu se dozvíte, jak povolit uživatelům se přihlásit pomocí účtu sítě Facebook pomocí projekt ASP.NET 2.0 základní ukázka vytvořený na [předchozí stránce](index.md).</span><span class="sxs-lookup"><span data-stu-id="6a8d3-106">This tutorial shows you how to enable your users to sign in with their Facebook account using a sample ASP.NET Core 2.0 project created on the [previous page](index.md).</span></span> <span data-ttu-id="6a8d3-107">Začneme vytvořením ID aplikace pro Facebook podle [oficiální kroky](https://developers.facebook.com).</span><span class="sxs-lookup"><span data-stu-id="6a8d3-107">We start by creating a Facebook App ID by following the [official steps](https://developers.facebook.com).</span></span>

## <a name="create-the-app-in-facebook"></a><span data-ttu-id="6a8d3-108">Vytvoření aplikace ve službě Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-108">Create the app in Facebook</span></span>

*  <span data-ttu-id="6a8d3-109">Přejděte na [Facebook vývojáři aplikace](https://developers.facebook.com/apps/) stránce a přihlaste se.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-109">Navigate to the [Facebook Developers app](https://developers.facebook.com/apps/) page and sign in.</span></span> <span data-ttu-id="6a8d3-110">Pokud nemáte účet Facebook, použijte **zaregistrovat pro Facebook** odkaz na přihlašovací stránku k jeho vytvoření.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-110">If you don't already have a Facebook account, use the **Sign up for Facebook** link on the login page to create one.</span></span>

* <span data-ttu-id="6a8d3-111">Klepněte **přidejte novou aplikaci** tlačítko v pravém horním rohu k vytvoření nového ID aplikace.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-111">Tap the **Add a New App** button in the upper right corner to create a new App ID.</span></span>

   ![Facebook pro portál pro vývojáře otevřít v Microsoft Edge](index/_static/FBMyApps.png)

* <span data-ttu-id="6a8d3-113">Vyplňte formulář a klepněte **vytvoření ID aplikace** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-113">Fill out the form and tap the **Create App ID** button.</span></span>

   ![Vytvoření nové aplikace ID formuláře](index/_static/FBNewAppId.png)

* <span data-ttu-id="6a8d3-115">Na **vybrat produkt** klikněte na tlačítko **nastavit až** na **Facebook přihlášení** karty.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-115">On the **Select a product** page, click **Set Up** on the **Facebook Login** card.</span></span>

   ![Stránka instalační program produktu](index/_static/FBProductSetup.png)
  
* <span data-ttu-id="6a8d3-117">**Rychlý Start** průvodce se spustí s **vybrat platformu** jako první stránka.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-117">The **Quickstart** wizard will launch with **Choose a Platform** as the first page.</span></span> <span data-ttu-id="6a8d3-118">Vynechat Průvodce prozatím kliknutím **nastavení** odkaz v nabídce na levé straně:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-118">Bypass the wizard for now by clicking the **Settings** link in the menu on the left:</span></span>

   ![Přeskočit rychlý Start](index/_static/FBSkipQuickStart.png)

* <span data-ttu-id="6a8d3-120">Zobrazí se **nastavení klienta OAuth** stránky:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-120">You are presented with the **Client OAuth Settings** page:</span></span>

![Stránka OAuth nastavení klienta](index/_static/FBOAuthSetup.png)

* <span data-ttu-id="6a8d3-122">Zadejte váš vývojový identifikátor URI s */signin-facebook* připojí do **identifikátory URI pro přesměrování platný OAuth** pole (například: `https://localhost:44320/signin-facebook`).</span><span class="sxs-lookup"><span data-stu-id="6a8d3-122">Enter your development URI with */signin-facebook* appended into the **Valid OAuth Redirect URIs** field (for example: `https://localhost:44320/signin-facebook`).</span></span> <span data-ttu-id="6a8d3-123">Ověřování Facebook nakonfigurované později v tomto kurzu bude automaticky zpracovávat požadavky na */signin-facebook* trasy k implementaci toku OAuth.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-123">The Facebook authentication configured later in this tutorial will automatically handle requests at */signin-facebook* route to implement the OAuth flow.</span></span>

* <span data-ttu-id="6a8d3-124">Klikněte na tlačítko **uložit změny**.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-124">Click **Save Changes**.</span></span>

* <span data-ttu-id="6a8d3-125">Klikněte **řídicí panel** odkaz v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-125">Click the **Dashboard** link in the left navigation.</span></span> 

    <span data-ttu-id="6a8d3-126">Na této stránce, poznamenejte si vaše `App ID` a `App Secret`.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-126">On this page, make a note of your `App ID` and your `App Secret`.</span></span> <span data-ttu-id="6a8d3-127">Přidá do vaší aplikace ASP.NET Core v další části:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-127">You will add both into your ASP.NET Core application in the next section:</span></span>

   ![Řídicí panel vývojáře Facebook](index/_static/FBDashboard.png)

* <span data-ttu-id="6a8d3-129">Při nasazování webu potřebujete k pokroku **Facebook přihlášení** stránce instalace a registrace nový veřejný identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-129">When deploying the site you need to revisit the **Facebook Login** setup page and register a new public URI.</span></span>

## <a name="store-facebook-app-id-and-app-secret"></a><span data-ttu-id="6a8d3-130">Ukládání ID aplikace pro Facebook a tajný klíč aplikace</span><span class="sxs-lookup"><span data-stu-id="6a8d3-130">Store Facebook App ID and App Secret</span></span>

<span data-ttu-id="6a8d3-131">Odkaz citlivá nastavení, jako je Facebook `App ID` a `App Secret` do konfigurace vaší aplikace pomocí [tajný klíč správce](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="6a8d3-131">Link sensitive settings like Facebook `App ID` and `App Secret` to your application configuration using the [Secret Manager](xref:security/app-secrets).</span></span> <span data-ttu-id="6a8d3-132">Pro účely tohoto kurzu, název tokeny `Authentication:Facebook:AppId` a `Authentication:Facebook:AppSecret`.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-132">For the purposes of this tutorial, name the tokens `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret`.</span></span>

<span data-ttu-id="6a8d3-133">Spuštěním následujících příkazů bezpečně uložit `App ID` a `App Secret` pomocí tajný klíč správce:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-133">Execute the following commands to securely store `App ID` and `App Secret` using Secret Manager:</span></span>

```console
dotnet user-secrets set Authentication:Facebook:AppId <app-id>
dotnet user-secrets set Authentication:Facebook:AppSecret <app-secret>
```

## <a name="configure-facebook-authentication"></a><span data-ttu-id="6a8d3-134">Konfigurace ověřování Facebook</span><span class="sxs-lookup"><span data-stu-id="6a8d3-134">Configure Facebook Authentication</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6a8d3-135">ASP.NET základní 2.x</span><span class="sxs-lookup"><span data-stu-id="6a8d3-135">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6a8d3-136">Přidání služby Facebook `ConfigureServices` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-136">Add the Facebook service in the `ConfigureServices` method in the *Startup.cs* file:</span></span>

```csharp
services.AddIdentity<ApplicationUser, IdentityRole>()
        .AddEntityFrameworkStores<ApplicationDbContext>()
        .AddDefaultTokenProviders();

services.AddAuthentication().AddFacebook(facebookOptions =>
{
    facebookOptions.AppId = Configuration["Authentication:Facebook:AppId"];
    facebookOptions.AppSecret = Configuration["Authentication:Facebook:AppSecret"];
});
```

[!INCLUDE[default settings configuration](includes/default-settings.md)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6a8d3-137">ASP.NET základní 1.x</span><span class="sxs-lookup"><span data-stu-id="6a8d3-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6a8d3-138">Nainstalujte [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) balíčku.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-138">Install the [Microsoft.AspNetCore.Authentication.Facebook](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Facebook) package.</span></span>

* <span data-ttu-id="6a8d3-139">Chcete-li nainstalovat tento balíček s Visual Studio 2017, klikněte pravým tlačítkem na projekt a vyberte **spravovat balíčky NuGet**.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-139">To install this package with Visual Studio 2017, right-click on the project and select **Manage NuGet Packages**.</span></span>
* <span data-ttu-id="6a8d3-140">Chcete-li nainstalovat s .NET Core rozhraní příkazového řádku, spusťte následující v adresáři projektu:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-140">To install with .NET Core CLI, execute the following in your project directory:</span></span>

   `dotnet add package Microsoft.AspNetCore.Authentication.Facebook`

<span data-ttu-id="6a8d3-141">Přidat middlewaru Facebook v `Configure` metoda v *Startup.cs* souboru:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-141">Add the Facebook middleware in the `Configure` method in *Startup.cs* file:</span></span>

```csharp
app.UseFacebookAuthentication(new FacebookOptions()
{
    AppId = Configuration["Authentication:Facebook:AppId"],
    AppSecret = Configuration["Authentication:Facebook:AppSecret"]
});
```

---

<span data-ttu-id="6a8d3-142">Najdete v článku [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) referenční dokumentace rozhraní API pro další informace o možnostech konfigurace nepodporuje ověřování Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-142">See the [FacebookOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.facebookoptions) API reference for more information on configuration options supported by Facebook authentication.</span></span> <span data-ttu-id="6a8d3-143">Možnosti konfigurace umožňuje:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-143">Configuration options can be used to:</span></span>

* <span data-ttu-id="6a8d3-144">Žádost různé informace o uživateli.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-144">Request different information about the user.</span></span>
* <span data-ttu-id="6a8d3-145">Přidejte argumenty řetězce dotazu přizpůsobit přihlašovací prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-145">Add query string arguments to customize the login experience.</span></span>

## <a name="sign-in-with-facebook"></a><span data-ttu-id="6a8d3-146">Přihlaste se pomocí sítě Facebook</span><span class="sxs-lookup"><span data-stu-id="6a8d3-146">Sign in with Facebook</span></span>

<span data-ttu-id="6a8d3-147">Spusťte aplikaci a klikněte na tlačítko **přihlásit**.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-147">Run your application and click **Log in**.</span></span> <span data-ttu-id="6a8d3-148">Zobrazí se možnost přihlásit se přes Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-148">You see an option to sign in with Facebook.</span></span>

![Webové aplikace: uživatel není ověřen.](index/_static/DoneFacebook.png)

<span data-ttu-id="6a8d3-150">Po kliknutí na **Facebook**, budete přesměrováni na Facebook pro ověření:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-150">When you click on **Facebook**, you are redirected to Facebook for authentication:</span></span>

![Stránka ověřování Facebook](index/_static/FBLogin.png)

<span data-ttu-id="6a8d3-152">Ověřování Facebook požadavky veřejného profilu a e-mailovou adresu ve výchozím nastavení:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-152">Facebook authentication requests public profile and email address by default:</span></span>

![Stránka ověřování Facebook](index/_static/FBLoginDone.png)

<span data-ttu-id="6a8d3-154">Po zadání přihlašovacích údajů Facebook budete přesměrováni zpět na váš web, kde můžete nastavit e-mailu.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-154">Once you enter your Facebook credentials you are redirected back to your site where you can set your email.</span></span>

<span data-ttu-id="6a8d3-155">Nyní jste se přihlásili pomocí přihlašovacích údajů Facebook:</span><span class="sxs-lookup"><span data-stu-id="6a8d3-155">You are now logged in using your Facebook credentials:</span></span>

![Webové aplikace: uživatel ověřený](index/_static/Done.png)

## <a name="troubleshooting"></a><span data-ttu-id="6a8d3-157">Poradce při potížích</span><span class="sxs-lookup"><span data-stu-id="6a8d3-157">Troubleshooting</span></span>

* <span data-ttu-id="6a8d3-158">**ASP.NET Core 2.x pouze:** pokud identita není nastavena voláním `services.AddIdentity` v `ConfigureServices`, pokusu o ověření bude mít za následek *ArgumentException –: musí být použita volba 'SignInScheme'*.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-158">**ASP.NET Core 2.x only:** If Identity is not configured by calling `services.AddIdentity` in `ConfigureServices`, attempting to authenticate will result in *ArgumentException: The 'SignInScheme' option must be provided*.</span></span> <span data-ttu-id="6a8d3-159">Šablona projektu použili v tomto kurzu zajistí, že to probíhá.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-159">The project template used in this tutorial ensures that this is done.</span></span>
* <span data-ttu-id="6a8d3-160">Jestliže databáze lokality nebyl vytvořen použitím počáteční migrace, můžete získat *databázová operace se nezdařila při zpracování požadavku* chyby.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-160">If the site database has not been created by applying the initial migration, you get *A database operation failed while processing the request* error.</span></span> <span data-ttu-id="6a8d3-161">Klepněte na **použít migrace** vytvořit databázi a aktualizujte pokračujte dále chyba.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-161">Tap **Apply Migrations** to create the database and refresh to continue past the error.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6a8d3-162">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6a8d3-162">Next steps</span></span>

* <span data-ttu-id="6a8d3-163">Tento článek vám ukázal, jak můžete ověřit pomocí služby Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-163">This article showed how you can authenticate with Facebook.</span></span> <span data-ttu-id="6a8d3-164">Můžete postupovat podle podobný postup k ověření pomocí jiných poskytovatelů uvedené na [předchozí stránce](index.md).</span><span class="sxs-lookup"><span data-stu-id="6a8d3-164">You can follow a similar approach to authenticate with other providers listed on the [previous page](index.md).</span></span>

* <span data-ttu-id="6a8d3-165">Jakmile budete publikovat web vaší webové aplikace Azure, byste měli obnovit `AppSecret` v portálu pro vývojáře Facebook.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-165">Once you publish your web site to Azure web app, you should reset the `AppSecret` in the Facebook developer portal.</span></span>

* <span data-ttu-id="6a8d3-166">Nastavte `Authentication:Facebook:AppId` a `Authentication:Facebook:AppSecret` jako nastavení aplikace v portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-166">Set the `Authentication:Facebook:AppId` and `Authentication:Facebook:AppSecret` as application settings in the Azure portal.</span></span> <span data-ttu-id="6a8d3-167">Konfigurace systému je nastavený čtení klíčů z proměnných prostředí.</span><span class="sxs-lookup"><span data-stu-id="6a8d3-167">The configuration system is set up to read keys from environment variables.</span></span>