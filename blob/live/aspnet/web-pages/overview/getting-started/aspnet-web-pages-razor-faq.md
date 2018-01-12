---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: "ASP.NET Web Pages – nejčastější dotazy (Razor) | Microsoft Docs"
author: tfitzmac
description: "V tomto článku jsou uvedené některé nejčastější dotazy týkající se rozhraní ASP.NET Web Pages (Razor) a službě WebMatrix. Použité v kurzu ASP.NET webové stránky (R... verze softwaru"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 7f6dc3b56a33bcbe3e1e4086681ca1ba76d7d153
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-pages-razor-faq"></a><span data-ttu-id="f7f2e-104">ASP.NET Web Pages – nejčastější dotazy (Razor)</span><span class="sxs-lookup"><span data-stu-id="f7f2e-104">ASP.NET Web Pages (Razor) FAQ</span></span>
====================
<span data-ttu-id="f7f2e-105">podle [tní FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="f7f2e-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> > [!NOTE] 
> > <span data-ttu-id="f7f2e-106">Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-106">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="f7f2e-107">Použití [sady Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) nebo [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-107">Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>
>
> <span data-ttu-id="f7f2e-108">V tomto článku jsou uvedené některé nejčastější dotazy týkající se rozhraní ASP.NET Web Pages (Razor) a službě WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-108">This article lists some frequently asked questions about ASP.NET Web Pages (Razor) and WebMatrix.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="f7f2e-109">V tomto kurzu použili verze softwaru</span><span class="sxs-lookup"><span data-stu-id="f7f2e-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="f7f2e-110">Rozhraní ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="f7f2e-110">ASP.NET Web Pages (Razor) 3</span></span>
> - <span data-ttu-id="f7f2e-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="f7f2e-111">Visual Studio 2013</span></span>
> - <span data-ttu-id="f7f2e-112">Služba WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="f7f2e-112">WebMatrix 3</span></span>
>   
> 
> <span data-ttu-id="f7f2e-113">V tomto kurzu taky spolupracuje se službou ASP.NET Web Pages 2, služba WebMatrix 2 a Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-113">This tutorial also works with ASP.NET Web Pages 2, WebMatrix 2, and Visual Studio 2012.</span></span>


- [<span data-ttu-id="f7f2e-114">Jaký je rozdíl mezi webových stránek ASP.NET, webových formulářů ASP.NET a architektura ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-114">What's the difference between ASP.NET Web Pages, ASP.NET Web Forms, and ASP.NET MVC?</span></span>](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [<span data-ttu-id="f7f2e-115">Chcete-li pracovat s webovými stránkami potřeba WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-115">Do I need WebMatrix in order to work with Web Pages?</span></span>](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [<span data-ttu-id="f7f2e-116">Můžete použít ovládací prvky webových formulářů ASP.NET na stránce webové stránky?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-116">Can I use ASP.NET Web Forms controls on a Web Pages page?</span></span>](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [<span data-ttu-id="f7f2e-117">Můžete nasadit stránku ASP.NET Web Pages bez použití služby WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-117">Can I deploy an ASP.NET Web Pages site without using WebMatrix?</span></span>](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [<span data-ttu-id="f7f2e-118">Je nutné použít pomocnou metodou WebSecurity pro podporu přihlášení?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-118">Do I have to use the WebSecurity helper to support logins?</span></span>](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [<span data-ttu-id="f7f2e-119">Podporuje rozhraní ASP.NET Web Pages HTML5?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-119">Does ASP.NET Web Pages support HTML5?</span></span>](#Does_ASP.NET_Web_Pages_support_HTML5)
- [<span data-ttu-id="f7f2e-120">Můžete použít JavaScript a jQuery s webovými stránkami?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-120">Can I use JavaScript and jQuery with Web Pages?</span></span>](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [<span data-ttu-id="f7f2e-121">Další zdroje informací</span><span class="sxs-lookup"><span data-stu-id="f7f2e-121">Additional Resources</span></span>](#AdditionalResources)

<span data-ttu-id="f7f2e-122">Otázky týkající se chyby a další problémy, najdete v článku [Průvodce odstraňováním potíží technologie ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-122">For questions about errors and other issues, see the [ASP.NET Web Pages (Razor) Troubleshooting Guide](https://go.microsoft.com/fwlink/?LinkId=253001).</span></span>

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a><span data-ttu-id="f7f2e-123">Jaký je rozdíl mezi webových stránek ASP.NET, webových formulářů ASP.NET a architektura ASP.NET MVC?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-123">What's the difference between ASP.NET Web Pages, ASP.NET Web Forms, and ASP.NET MVC?</span></span>

<span data-ttu-id="f7f2e-124">Všechny tři jsou technologie ASP.NET pro vytváření dynamické webové aplikace:</span><span class="sxs-lookup"><span data-stu-id="f7f2e-124">All three are ASP.NET technologies for creating dynamic web applications:</span></span>

- <span data-ttu-id="f7f2e-125">ASP.NET – webové stránky se zaměřuje na přidání stránky HTML a funkce jednoduché a jednoduché syntaxe dynamický kód (na straně serveru) a přístup k databázi.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-125">ASP.NET Web Pages focuses on adding dynamic (server-side) code and database access to HTML pages, and features simple and lightweight syntax.</span></span>
- <span data-ttu-id="f7f2e-126">Webové formuláře ASP.NET je založen na modelu objektu stránky a ovládací prvky tradiční okno – typ (tlačítka, seznamy, atd.).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-126">ASP.NET Web Forms is based on a page object model and traditional window-type controls (buttons, lists, etc.).</span></span> <span data-ttu-id="f7f2e-127">Webové formuláře používá model na základě událostí, který je dobře známé pro osoby, které jste pracovali na základě klienta vývoj (Windows forms).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-127">Web Forms uses an event-based model that's familiar to those who've worked with client-based (Windows forms) development.</span></span>
- <span data-ttu-id="f7f2e-128">ASP.NET MVC implementuje vzor model-view-controller pro technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-128">ASP.NET MVC implements the model-view-controller pattern for ASP.NET.</span></span> <span data-ttu-id="f7f2e-129">Důraz je na "oddělené oblasti zájmu" (zpracování, dat a vrstvy uživatelského rozhraní).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-129">The emphasis is on "separation of concerns" (processing, data, and UI layers).</span></span>

<span data-ttu-id="f7f2e-130">Všechny tři architektury jsou plně podporované a pokračovat na tým ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-130">All three frameworks are fully supported and continue to be developed by the ASP.NET team.</span></span> <span data-ttu-id="f7f2e-131">Obecně platí, volba které framework použít závisí na pozadí a zkušenosti s technologií ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-131">In general, the choice of which framework to use depends on your background and experience with ASP.NET.</span></span>

<span data-ttu-id="f7f2e-132">ASP.NET – webové stránky na konkrétní byla navržena pro usnadnění pro osoby, které již víte, chcete-li přidat server zpracování na jejich stránky HTML.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-132">ASP.NET Web Pages in particular was designed to make it easy for people who already know HTML to add server processing to their pages.</span></span> <span data-ttu-id="f7f2e-133">Je vhodná pro studenty, hobbyists, obecně uživatelům, kteří jsou nové pro programování.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-133">It's a good choice for students, hobbyists, people in general who are new to programming.</span></span> <span data-ttu-id="f7f2e-134">Může být také dobrou volbou pro vývojáře, kteří mají zkušenosti s technologiemi web mimo technologii ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-134">It can also be a good choice for developers who have experience with non-ASP.NET web technologies.</span></span>

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a><span data-ttu-id="f7f2e-135">Chcete-li pracovat s webovými stránkami potřeba WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-135">Do I need WebMatrix in order to work with Web Pages?</span></span>

<span data-ttu-id="f7f2e-136">Ne.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-136">No.</span></span> <span data-ttu-id="f7f2e-137">Služba WebMatrix se už doporučuje jako integrované vývojové prostředí pro ASP.NET Web Pages.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-137">WebMatrix is no longer recommended as an integrated development environment for ASP.NET Web Pages.</span></span> <span data-ttu-id="f7f2e-138">Použití [sady Visual Studio](program-asp-net-web-pages-in-visual-studio.md) nebo [Visual Studio Code](https://code.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-138">Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) or [Visual Studio Code](https://code.visualstudio.com/).</span></span>

<span data-ttu-id="f7f2e-139">Pokud nechcete použít Visual Studio nebo Visual Studio Code, můžete nainstalovat součásti produkty jednotlivě pomocí [instalačního programu webové platformy Microsoft](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-139">If you don't want to use either Visual Studio or Visual Studio Code, you can install the component products individually using [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span> <span data-ttu-id="f7f2e-140">Budete potřebovat následující produkty:</span><span class="sxs-lookup"><span data-stu-id="f7f2e-140">You need the following products:</span></span>

- <span data-ttu-id="f7f2e-141">Rozhraní Microsoft .NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="f7f2e-141">Microsoft .NET Framework 4.5</span></span>
- <span data-ttu-id="f7f2e-142">ASP.NET MVC 5 (který se nainstaluje rozhraní ASP.NET Web Pages)</span><span class="sxs-lookup"><span data-stu-id="f7f2e-142">ASP.NET MVC 5 (which installs the ASP.NET Web Pages framework as well)</span></span>
- <span data-ttu-id="f7f2e-143">Služba IIS Express (webový server)</span><span class="sxs-lookup"><span data-stu-id="f7f2e-143">IIS Express (the web server)</span></span>
- <span data-ttu-id="f7f2e-144">Microsoft SQL Server Compact 4.0 (databáze)</span><span class="sxs-lookup"><span data-stu-id="f7f2e-144">Microsoft SQL Server Compact 4.0 (the database)</span></span>

<span data-ttu-id="f7f2e-145">Můžete pomocí textového editoru upravit *.cshtml* (nebo *.vbhtml*) stránky.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-145">You can use a text editor to edit *.cshtml* (or *.vbhtml*) pages.</span></span>

<span data-ttu-id="f7f2e-146">Správa databází systému SQL Server Compact (*SDF* soubory) bez nástroj je trochu složitější.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-146">Managing SQL Server Compact databases (*.sdf* files) without a tool is a bit harder.</span></span> <span data-ttu-id="f7f2e-147">Visual Studio containds nástroje pro správu *SDF* databáze.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-147">Visual Studio containds tools for managing *.sdf* databases.</span></span> <span data-ttu-id="f7f2e-148">Můžete také spustit příkazy SQL v kódu k provádění úloh správy systému SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-148">You can also run SQL commands in code to perform many SQL Server management tasks.</span></span>

<span data-ttu-id="f7f2e-149">K testování *.cshtml* stránky bez použití integrované vývojové prostředí (IDE), je můžete nasadit na aktivní server.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-149">To test *.cshtml* pages without using an integrated development environment (IDE), you can deploy them to a live server.</span></span> <span data-ttu-id="f7f2e-150">(Viz [můžete nasadit stránku ASP.NET Web Pages bez použití služby WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))</span><span class="sxs-lookup"><span data-stu-id="f7f2e-150">(See [Can I deploy an ASP.NET Web Pages site without using WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))</span></span>

### <a name="running-iis-express-without-using-an-ide"></a><span data-ttu-id="f7f2e-151">Spuštění služby IIS Express bez použití IDE</span><span class="sxs-lookup"><span data-stu-id="f7f2e-151">Running IIS Express without using an IDE</span></span>

<span data-ttu-id="f7f2e-152">Při instalaci služby IIS Express na počítače jako webový server, můžete použít k testování stránek.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-152">If you install IIS Express on your computer as a web server, you can use that to test the pages.</span></span> <span data-ttu-id="f7f2e-153">Můžete spustit službu IIS Express z příkazového řádku a přidružte ji k určité číslo portu.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-153">You can run IIS Express from the command line and associate it with a specific port number.</span></span> <span data-ttu-id="f7f2e-154">Pak zadejte tento port při žádosti o *.cshtml* soubory v prohlížeči.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-154">You then specify that port when you request *.cshtml* files in your browser.</span></span>

<span data-ttu-id="f7f2e-155">V systému Windows, otevřete příkazový řádek s oprávněními správce a změňte na *C:\Program Files\IIS Express.*</span><span class="sxs-lookup"><span data-stu-id="f7f2e-155">In Windows, open a command prompt with administrator privileges and change to *C:\Program Files\IIS Express.*</span></span> <span data-ttu-id="f7f2e-156">(Pro 64bitové systémy, použijte složku *Express \IIS C:\Program Files (x86).)* Potom zadejte následující příkaz, pomocí skutečné cesty na váš web:</span><span class="sxs-lookup"><span data-stu-id="f7f2e-156">(For 64-bit systems, use the folder *C:\Program Files (x86)\IIS Express.)* Then enter the following command, using the actual path to your site:</span></span>

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

<span data-ttu-id="f7f2e-157">Můžete použít libovolné číslo portu, který není rezervovaná jiný proces.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-157">You can use any port number that isn't already reserved by some other process.</span></span> <span data-ttu-id="f7f2e-158">(Čísla portů nad 1024 jsou obvykle volné). Pro `path` hodnoty, použijte cestu ke složce webu kde *.cshtml* jsou soubory.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-158">(Port numbers above 1024 are typically free.) For the `path` value, use the path of the website folder where the *.cshtml* files are.</span></span>

<span data-ttu-id="f7f2e-159">Po spuštění tohoto příkazu k nastavení služby IIS Express k obsluze svoje stránky, můžete otevřít prohlížeč a vyhledejte *.cshtml* souboru.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-159">After you run this command to set up IIS Express to serve your pages, you can open a browser and browse to a *.cshtml* file.</span></span> <span data-ttu-id="f7f2e-160">Použijte adresu URL podobnou následující:</span><span class="sxs-lookup"><span data-stu-id="f7f2e-160">Use a URL like the following:</span></span>

`http://localhost:35896/default.cshtml`

<span data-ttu-id="f7f2e-161">Nápovědu služby IIS Express možnosti příkazového řádku, zadejte `iisexpress.exe /?` na příkazovém řádku.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-161">For help with IIS Express command line options, enter `iisexpress.exe /?` at the command line.</span></span>

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a><span data-ttu-id="f7f2e-162">Můžete použít ovládací prvky webových formulářů ASP.NET na stránce webové stránky?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-162">Can I use ASP.NET Web Forms controls on a Web Pages page?</span></span>

<span data-ttu-id="f7f2e-163">Ne.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-163">No.</span></span> <span data-ttu-id="f7f2e-164">Ovládací prvky webových formulářů jako [políčko](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.checkbox) ovládací prvek, [ovládací prvky pro ověřování](https://msdn.microsoft.com/en-us/library/bwd43d0x)a [GridView](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview) řízení fungovat pouze v stránky webových formulářů (*.aspx* soubory).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-164">Web Forms controls like the [CheckBox](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.checkbox) control, the [validation controls](https://msdn.microsoft.com/en-us/library/bwd43d0x), and the [GridView](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.gridview) control only work in Web Forms pages (*.aspx* files).</span></span> <span data-ttu-id="f7f2e-165">Tyto ovládací prvky vyžadují rozhraní stránky webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-165">These controls require the Web Forms page framework.</span></span>

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a><span data-ttu-id="f7f2e-166">Můžete nasadit stránku ASP.NET Web Pages bez použití služby WebMatrix?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-166">Can I deploy an ASP.NET Web Pages site without using WebMatrix?</span></span>

<span data-ttu-id="f7f2e-167">Ano.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-167">Yes.</span></span> <span data-ttu-id="f7f2e-168">Soubory webové stránky můžete ručně zkopírovat na server (obvykle pomocí FTP).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-168">You can manually copy website files to a server (typically by using FTP).</span></span> <span data-ttu-id="f7f2e-169">Pokud provádíte ruční kopírování, máte také kopírovat soubory, které podporují systém SQL Server Compact (databáze).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-169">If you perform a manual copy, you also have to copy the files that support SQL Server Compact (the database).</span></span> <span data-ttu-id="f7f2e-170">Podrobnosti najdete v tématu v položce blogu [nasazení webové stránky aplikace bez nástroj](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-170">For details, see the blog entry [Deploying Web Pages applications without a tool](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).</span></span>

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a><span data-ttu-id="f7f2e-171">Je nutné použít pomocnou metodou WebSecurity pro podporu přihlášení?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-171">Do I have to use the WebSecurity helper to support logins?</span></span>

<span data-ttu-id="f7f2e-172">Ne.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-172">No.</span></span> <span data-ttu-id="f7f2e-173">`SimpleMembership` Zprostředkovatele, který je součástí z webové stránky ASP.NET je jednou z možností.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-173">The `SimpleMembership` provider that is part of ASP.NET Web Pages is one option.</span></span> <span data-ttu-id="f7f2e-174">Zprostředkovatele zabezpečení, které jsou součástí technologie ASP.NET, (která je lze použít pro práci s v webových formulářů) jsou k dispozici.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-174">The security providers that are part of ASP.NET (that you might be used to working with in Web Forms) are also available.</span></span> <span data-ttu-id="f7f2e-175">Například můžete ověřování pomocí formulářů na webových stránkách ASP.NET stejně jako v webových formulářů.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-175">For example, you can use forms authentication in ASP.NET Web Pages just as you would in Web Forms.</span></span> <span data-ttu-id="f7f2e-176">Jedním z příkladů použití ověřování pomocí formulářů, najdete v části článek Microsoft Support [jak implementovat ověřování ve vaší aplikaci technologie ASP.NET pomocí C# .NET](https://support.microsoft.com/kb/301240).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-176">For one example of how to use forms authentication, see the Microsoft Support article [How To Implement Forms-Based Authentication in Your ASP.NET Application by Using C#.NET](https://support.microsoft.com/kb/301240).</span></span> <span data-ttu-id="f7f2e-177">Chcete-li stáhnout jednoduchý příklad, přečtěte si téma [verzi ASP.NET "přihlášení &amp; heslo](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-177">To download a simple example, see [ASP.NET version of "Login &amp; Password](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).</span></span>

<span data-ttu-id="f7f2e-178">Informace o tom, jak použít ověřování systému Windows, naleznete v příspěvku blogu [ověřování pomocí systému Windows na webových stránkách ASP.NET](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-178">For information about how to use Windows authentication, see the blog post [Using Windows authentication in ASP.NET Web Pages](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).</span></span>

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a><span data-ttu-id="f7f2e-179">Podporuje rozhraní ASP.NET Web Pages HTML5?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-179">Does ASP.NET Web Pages support HTML5?</span></span>

<span data-ttu-id="f7f2e-180">Ano.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-180">Yes.</span></span> <span data-ttu-id="f7f2e-181">Na stránkách můžete vytvořit pomocí rozhraní ASP.NET Web Pages (*.cshtml* nebo *.vbhtml* stránky) jsou v podstatě stránky HTML, které také obsahovat kód, který běží na serveru před vykreslením stránky.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-181">The pages you create with ASP.NET Web Pages (*.cshtml* or *.vbhtml* pages) are essentially HTML pages that also contain code that runs on the server, before the page is rendered.</span></span> <span data-ttu-id="f7f2e-182">Tak dlouho, dokud prohlížeče uživatele podporuje HTML5, můžete použít prvků HTML5 v *.cshtml* nebo *.vbhtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-182">As long as the user's browser supports HTML5, you can use HTML5 elements in a *.cshtml* or *.vbhtml* page.</span></span>

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a><span data-ttu-id="f7f2e-183">Můžete použít JavaScript a jQuery s webovými stránkami?</span><span class="sxs-lookup"><span data-stu-id="f7f2e-183">Can I use JavaScript and jQuery with Web Pages?</span></span>

<span data-ttu-id="f7f2e-184">Absolutně.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-184">Absolutely.</span></span> <span data-ttu-id="f7f2e-185">Na stránkách můžete vytvořit pomocí rozhraní ASP.NET Web Pages (*.cshtml* nebo *.vbhtml* stránky) jsou právě HTML stránky s kódem serveru v nich.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-185">The pages you create with ASP.NET Web Pages (*.cshtml* or *.vbhtml* pages) are just HTML pages with server code in them.</span></span> <span data-ttu-id="f7f2e-186">Proto nic můžete provést na normální stránce HTML pomocí pomocí jazyka JavaScript nebo jQuery můžete provést také *.cshtml* nebo *.vbhtml* stránky.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-186">Therefore, anything you can do in a normal HTML page by using JavaScript or jQuery you can also do in a *.cshtml* or *.vbhtml* page.</span></span>

<span data-ttu-id="f7f2e-187">**Starter Site** šablony ve službě WebMatrix obsahuje řadu knihovny jQuery.</span><span class="sxs-lookup"><span data-stu-id="f7f2e-187">The **Starter Site** template in WebMatrix contains a number of jQuery libraries.</span></span> <span data-ttu-id="f7f2e-188">Pokud vytvoříte pomocí této šablony, web *skripty* složka obsahuje základní knihovna jQuery (*jquery 1.6.2.js)* a knihovny pro ověřování jQuery (*jquery.validate.js*atd.).</span><span class="sxs-lookup"><span data-stu-id="f7f2e-188">If you create a site by using that template, the *Scripts* folder contains a jQuery core library (*jquery-1.6.2.js)* and libraries for jQuery validation (*jquery.validate.js*, etc.).</span></span>

<span data-ttu-id="f7f2e-189">Zde jsou některé příspěvky blogu, které ilustrují způsoby použití jQuery s webových stránek ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="f7f2e-189">Here are some blog posts that illustrate ways to use jQuery with ASP.NET Web Pages:</span></span>

- <span data-ttu-id="f7f2e-190">[Přidání jQuery přesnosti pro webové stránky ASP.NET pomocí služby WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) podle Rachel Appel</span><span class="sxs-lookup"><span data-stu-id="f7f2e-190">[Adding jQuery Goodness to ASP.NET Web Pages using WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) by Rachel Appel</span></span>
- <span data-ttu-id="f7f2e-191">[5 minut: WebMatrix + jQuery UI + json + jQuery šablony](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) podle Jonas Eriksson</span><span class="sxs-lookup"><span data-stu-id="f7f2e-191">[5 min: WebMatrix + jQuery UI + json + jQuery templates](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) by Jonas Eriksson</span></span>
- <span data-ttu-id="f7f2e-192">[Služba WebMatrix a formulářů jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) podle Brind Jan</span><span class="sxs-lookup"><span data-stu-id="f7f2e-192">[WebMatrix And jQuery Forms](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) by Mike Brind</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="f7f2e-193">Další prostředky</span><span class="sxs-lookup"><span data-stu-id="f7f2e-193">Additional Resources</span></span>


[<span data-ttu-id="f7f2e-194">ASP.NET – webové stránky průvodce řešením potíží (Razor)</span><span class="sxs-lookup"><span data-stu-id="f7f2e-194">ASP.NET Web Pages (Razor) Troubleshooting Guide</span></span>](https://go.microsoft.com/fwlink/?LinkId=253001)

<span data-ttu-id="f7f2e-195">[Fórum pro službu WebMatrix a webové stránky ASP.NET](https://forums.asp.net/1224.aspx/1?WebMatrix) na webu technologie ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7f2e-195">[WebMatrix and ASP.NET Web Pages forum](https://forums.asp.net/1224.aspx/1?WebMatrix) on the ASP.NET website</span></span>