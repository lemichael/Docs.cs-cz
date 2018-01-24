---
title: "Jádro ASP.NET na Nano Server"
author: shirhatti
description: "Zjistěte, jak chcete využít stávající aplikace ASP.NET Core a nasaďte ho do instance Nano Server služby IIS."
ms.author: riande
manager: wpickett
ms.date: 11/04/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/nano-server
ms.openlocfilehash: d9b55fb42088b447451326b7ee573d9bfa5f5941
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 01/19/2018
---
# <a name="aspnet-core-with-iis-on-nano-server"></a><span data-ttu-id="18a87-103">Jádro ASP.NET se službou IIS na Nano Server</span><span class="sxs-lookup"><span data-stu-id="18a87-103">ASP.NET Core with IIS on Nano Server</span></span>

<span data-ttu-id="18a87-104">Podle [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span><span class="sxs-lookup"><span data-stu-id="18a87-104">By [Sourabh Shirhatti](https://twitter.com/sshirhatti)</span></span>

<span data-ttu-id="18a87-105">V tomto kurzu budete trvat stávající aplikace ASP.NET Core a nasaďte ji do instance Nano Server služby IIS.</span><span class="sxs-lookup"><span data-stu-id="18a87-105">In this tutorial, you'll take an existing ASP.NET Core app and deploy it to a Nano Server instance running IIS.</span></span>

## <a name="introduction"></a><span data-ttu-id="18a87-106">Úvod</span><span class="sxs-lookup"><span data-stu-id="18a87-106">Introduction</span></span>

<span data-ttu-id="18a87-107">Nano Server je možnost instalace v systému Windows Server 2016, nabízí lepší zabezpečení, malými nároky na paměť a lepší obsluhy než jádra serveru nebo celý Server.</span><span class="sxs-lookup"><span data-stu-id="18a87-107">Nano Server is an installation option in Windows Server 2016, offering a tiny footprint, better security, and better servicing than Server Core or full Server.</span></span> <span data-ttu-id="18a87-108">Přečtěte si oficiální [Nano Server dokumentace](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) další podrobnosti a odkazy na stažení zkušební verze 180 dní.</span><span class="sxs-lookup"><span data-stu-id="18a87-108">Please consult the official [Nano Server documentation](https://docs.microsoft.com/windows-server/get-started/getting-started-with-nano-server) for more details and download links for 180 Days evaluation versions.</span></span> 

<span data-ttu-id="18a87-109">Existují tři způsoby snadno můžete vyzkoušet Nano Server.</span><span class="sxs-lookup"><span data-stu-id="18a87-109">There are three easy ways for you to try out Nano Server.</span></span> <span data-ttu-id="18a87-110">Pokud jste se přihlaste pomocí svého účtu MS:</span><span class="sxs-lookup"><span data-stu-id="18a87-110">When you sign in with your MS account:</span></span>

1. <span data-ttu-id="18a87-111">Můžete stáhnout soubor systému Windows Server 2016 ISO a sestavení Nano Server bitové kopie.</span><span class="sxs-lookup"><span data-stu-id="18a87-111">You can download the Windows Server 2016 ISO file and build a Nano Server image.</span></span>

2. <span data-ttu-id="18a87-112">Stáhněte si Nano Server virtuálního pevného disku.</span><span class="sxs-lookup"><span data-stu-id="18a87-112">Download the Nano Server VHD.</span></span>

3. <span data-ttu-id="18a87-113">Vytvoření virtuálního počítače v Azure pomocí bitové kopie Nano Server v Azure Gallery.</span><span class="sxs-lookup"><span data-stu-id="18a87-113">Create a VM in Azure using the Nano Server image in the Azure Gallery.</span></span> <span data-ttu-id="18a87-114">Pokud nemáte účet Azure, můžete získat bezplatnou 30denní zkušební verzi.</span><span class="sxs-lookup"><span data-stu-id="18a87-114">If you don’t have an Azure account, you can get a free 30-day trial.</span></span>

<span data-ttu-id="18a87-115">V tomto kurzu použijeme 2. možnost předdefinovaných VHD Nano Server ze systému Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="18a87-115">In this tutorial, we will be using the 2nd option, the pre-built Nano Server VHD from Windows Server 2016.</span></span>

<span data-ttu-id="18a87-116">Než budete pokračovat v tomto kurzu, budete potřebovat [publikovaná výstup](xref:host-and-deploy/directory-structure) stávající aplikace ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="18a87-116">Before proceeding with this tutorial, you will need the [published output](xref:host-and-deploy/directory-structure) of an existing ASP.NET Core application.</span></span> <span data-ttu-id="18a87-117">Ujistěte se, vaše aplikace je sestavena pro běh **64-bit** procesu.</span><span class="sxs-lookup"><span data-stu-id="18a87-117">Ensure your application is built to run in a **64-bit** process.</span></span>

## <a name="setting-up-the-nano-server-instance"></a><span data-ttu-id="18a87-118">Nastavení instance Nano Server</span><span class="sxs-lookup"><span data-stu-id="18a87-118">Setting up the Nano Server instance</span></span>

<span data-ttu-id="18a87-119">[Vytvoření nového virtuálního počítače pomocí technologie Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) na vývojovém počítači pomocí dříve stažené virtuální pevný disk.</span><span class="sxs-lookup"><span data-stu-id="18a87-119">[Create a new Virtual Machine using Hyper-V](https://technet.microsoft.com/library/hh846766.aspx) on your development machine using the previously downloaded VHD.</span></span> <span data-ttu-id="18a87-120">Tento počítač bude vyžadovat, abyste nastavili heslo správce před přihlášením.</span><span class="sxs-lookup"><span data-stu-id="18a87-120">The machine will require you to set an administrator password before logging on.</span></span> <span data-ttu-id="18a87-121">Na konzole virtuálního počítače stiskněte klávesu F11 k nastavení hesla před prvním přihlášení.</span><span class="sxs-lookup"><span data-stu-id="18a87-121">At the VM console, press F11 to set the password before the first log in.</span></span> <span data-ttu-id="18a87-122">Také je třeba zkontrolovat nového virtuálního počítače IP adresu buď Moje kontrola DHCP server vyřešeny IP zadaný při zřízení virtuálního počítače nebo v Nano Server konzoly pro zotavení nastavení sítě.</span><span class="sxs-lookup"><span data-stu-id="18a87-122">You also need to check your new VM's IP address either my checking your DHCP server's fixed IP supplied while provisioning your VM or in Nano Server recovery console's networking settings.</span></span>

> [!NOTE]
> <span data-ttu-id="18a87-123">Předpokládejme, že nový virtuální počítač spustí pomocí místní adresy V4 IP 192.168.1.10.</span><span class="sxs-lookup"><span data-stu-id="18a87-123">Let's assume your new VM runs with the local V4 IP address 192.168.1.10.</span></span>

<span data-ttu-id="18a87-124">Teď už brzo budete moct spravovat pomocí vzdálenou komunikaci prostředí PowerShell, který je jediným způsobem, jak plně spravovat Nano Server.</span><span class="sxs-lookup"><span data-stu-id="18a87-124">Now you're able to manage it using PowerShell remoting, which is the only way to fully administer your Nano Server.</span></span>

## <a name="connecting-to-your-nano-server-instance-using-powershell-remoting"></a><span data-ttu-id="18a87-125">Připojení k vaší instanci Nano Server pomocí vzdálenou komunikaci prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="18a87-125">Connecting to your Nano Server instance using PowerShell Remoting</span></span>

<span data-ttu-id="18a87-126">Otevřete okno prostředí PowerShell se zvýšenými oprávněními přidat vaše vzdálené instance systému Nano Server k vaší `TrustedHosts` seznamu.</span><span class="sxs-lookup"><span data-stu-id="18a87-126">Open an elevated PowerShell window to add your remote Nano Server instance to your `TrustedHosts` list.</span></span>

```PowerShell
$nanoServerIpAddress = "192.168.1.10"
Set-Item WSMan:\localhost\Client\TrustedHosts "$nanoServerIpAddress" -Concatenate -Force
```

> [!NOTE]
> <span data-ttu-id="18a87-127">Nahraďte proměnnou `$nanoServerIpAddress` se správnou adresou IP.</span><span class="sxs-lookup"><span data-stu-id="18a87-127">Replace the variable `$nanoServerIpAddress` with the correct IP address.</span></span>

<span data-ttu-id="18a87-128">Po přidání vaší instance Nano Server k vaší `TrustedHosts`, můžete připojit se pomocí vzdálenou komunikaci prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="18a87-128">Once you have added your Nano Server instance to your `TrustedHosts`, you can connect to it using PowerShell remoting.</span></span>

```PowerShell
$nanoServerSession = New-PSSession -ComputerName $nanoServerIpAddress -Credential ~\Administrator
Enter-PSSession $nanoServerSession
```

<span data-ttu-id="18a87-129">Úspěšné připojení výsledků v řádku s formátu vyhledávání jako:`[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span><span class="sxs-lookup"><span data-stu-id="18a87-129">A successful connection results in a prompt with a format looking like: `[192.168.1.10]: PS C:\Users\Administrator\Documents>`</span></span>

## <a name="creating-a-file-share"></a><span data-ttu-id="18a87-130">Vytvoření sdílené složky</span><span class="sxs-lookup"><span data-stu-id="18a87-130">Creating a file share</span></span>

<span data-ttu-id="18a87-131">Vytvoření sdílené složky na serveru Nano tak, aby k publikované aplikaci lze zkopírovat na ni.</span><span class="sxs-lookup"><span data-stu-id="18a87-131">Create a file share on the Nano server so that the published application can be copied to it.</span></span> <span data-ttu-id="18a87-132">Spusťte následující příkazy v relaci vzdálené plochy:</span><span class="sxs-lookup"><span data-stu-id="18a87-132">Run the following commands in the remote session:</span></span>

```PowerShell
New-Item C:\PublishedApps\AspNetCoreSampleForNano -type directory
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=yes
net share AspNetCoreSampleForNano=c:\PublishedApps\AspNetCoreSampleForNano /GRANT:EVERYONE`,FULL
```

<span data-ttu-id="18a87-133">Po spuštění výše uvedených příkazů, byste měli mít přístup k této sdílené složce, navštivte stránky `\\192.168.1.10\AspNetCoreSampleForNano` v Průzkumníku Windows hostitelský počítač.</span><span class="sxs-lookup"><span data-stu-id="18a87-133">After running the above commands, you should be able to access this share by visiting `\\192.168.1.10\AspNetCoreSampleForNano` in the host machine's Windows Explorer.</span></span>

## <a name="open-port-in-the-firewall"></a><span data-ttu-id="18a87-134">Otevřete port v bráně firewall</span><span class="sxs-lookup"><span data-stu-id="18a87-134">Open port in the firewall</span></span>

<span data-ttu-id="18a87-135">Spusťte následující příkazy v této relaci vzdálené otevření portu v bráně firewall chcete, aby služba IIS naslouchat komunikaci TCP na portu TCP nebo 8000.</span><span class="sxs-lookup"><span data-stu-id="18a87-135">Run the following commands in the remote session to open up a port in the firewall to let IIS listen for TCP traffic on port TCP/8000.</span></span>

```PowerShell
New-NetFirewallRule -Name "AspNet5 IIS" -DisplayName "Allow HTTP on TCP/8000" -Protocol TCP -LocalPort 8000 -Action Allow -Enabled True
```

## <a name="installing-iis"></a><span data-ttu-id="18a87-136">Instalace služby IIS</span><span class="sxs-lookup"><span data-stu-id="18a87-136">Installing IIS</span></span>

<span data-ttu-id="18a87-137">Přidat `NanoServerPackage` poskytovatele z Galerie prostředí PowerShell.</span><span class="sxs-lookup"><span data-stu-id="18a87-137">Add the `NanoServerPackage` provider from the PowerShell Gallery.</span></span> <span data-ttu-id="18a87-138">Jakmile zprostředkovatele je nainstalován a importovat, můžete nainstalovat balíčky systému Windows.</span><span class="sxs-lookup"><span data-stu-id="18a87-138">Once the provider is installed and imported, you can install Windows packages.</span></span>

<span data-ttu-id="18a87-139">Spusťte následující příkazy v relaci prostředí PowerShell, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="18a87-139">Run the following commands in the PowerShell session that was created earlier:</span></span>

```PowerShell
Install-PackageProvider NanoServerPackage
Import-PackageProvider NanoServerPackage
Install-NanoServerPackage -Name Microsoft-NanoServer-Storage-Package
Install-NanoServerPackage -Name Microsoft-NanoServer-IIS-Package
```

<span data-ttu-id="18a87-140">Chcete-li rychle ověřit, pokud služba IIS je nastavena správně, můžete navštívit adresu URL `http://192.168.1.10/` a měla by se zobrazit úvodní stránku.</span><span class="sxs-lookup"><span data-stu-id="18a87-140">To quickly verify if IIS is setup correctly, you can visit the URL `http://192.168.1.10/` and should see a welcome page.</span></span> <span data-ttu-id="18a87-141">Pokud je nainstalována služba IIS, volat web `Default Web Site` naslouchá na portu 80 se vytvoří ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="18a87-141">When IIS is installed, a website called `Default Web Site` listening on port 80 is created by default.</span></span>

## <a name="installing-the-aspnet-core-module-ancm"></a><span data-ttu-id="18a87-142">Instalace modulu ASP.NET Core (ANCM)</span><span class="sxs-lookup"><span data-stu-id="18a87-142">Installing the ASP.NET Core Module (ANCM)</span></span>

<span data-ttu-id="18a87-143">Základní modul ASP.NET je službu IIS 7.5 + modul, který je zodpovědný za správu proces naslouchacího procesu ASP.NET Core HTTP a na požadavky na proxy pro procesy, které spravuje.</span><span class="sxs-lookup"><span data-stu-id="18a87-143">The ASP.NET Core Module is an IIS 7.5+ module which is responsible for process management of ASP.NET Core HTTP listeners and to proxy requests to processes that it manages.</span></span> <span data-ttu-id="18a87-144">V tuto chvíli je ruční proces instalace technologie ASP.NET základní modul pro službu IIS.</span><span class="sxs-lookup"><span data-stu-id="18a87-144">At the moment, the process to install the ASP.NET Core Module for IIS is manual.</span></span> <span data-ttu-id="18a87-145">Budete muset nainstalovat [.NET jádra Windows serveru, který hostuje sady](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) na běžný (ne Nano) počítače.</span><span class="sxs-lookup"><span data-stu-id="18a87-145">You will need to install the [.NET Core Windows Server Hosting bundle](https://download.microsoft.com/download/B/1/D/B1D7D5BF-3920-47AA-94BD-7A6E48822F18/DotNetCore.2.0.0-WindowsHosting.exe) on a regular (not Nano) machine.</span></span> <span data-ttu-id="18a87-146">Po instalaci sady regulární počítače, musíte zkopírujte následující soubory do sdílené složky, kterou jsme vytvořili předtím.</span><span class="sxs-lookup"><span data-stu-id="18a87-146">After installing the bundle on a regular machine, you will need to copy the following files to the file share that we created earlier.</span></span>

<span data-ttu-id="18a87-147">Na server regular (ne Nano) se službou IIS spusťte následující příkazy pro kopírování:</span><span class="sxs-lookup"><span data-stu-id="18a87-147">On a regular (not Nano) server with IIS, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path  C:\windows\system32\inetsrv\aspnetcore.dll -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
Copy-Item -Path  C:\windows\system32\inetsrv\config\schema\aspnetcore_schema.xml -Destination `\\<nanoserver-ip-address>\AspNetCoreSampleForNano`
```

<span data-ttu-id="18a87-148">Nahraďte `C:\windows\system32\inetsrv` s `C:\Program Files\IIS Express` na počítače s Windows 10.</span><span class="sxs-lookup"><span data-stu-id="18a87-148">Replace `C:\windows\system32\inetsrv` with `C:\Program Files\IIS Express` on a Windows 10 machine.</span></span>

<span data-ttu-id="18a87-149">Na straně Nano musíte zkopírujte následující soubory ze sdílené složky, kterou jsme vytvořili výše na platné umístění.</span><span class="sxs-lookup"><span data-stu-id="18a87-149">On the Nano side, you will need to copy the following files from the file share that we created earlier to the valid locations.</span></span> <span data-ttu-id="18a87-150">Ano spusťte následující příkazy pro kopírování:</span><span class="sxs-lookup"><span data-stu-id="18a87-150">So, run the following copy commands:</span></span>

```PowerShell
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore.dll -Destination C:\windows\system32\inetsrv\
Copy-Item -Path C:\PublishedApps\AspNetCoreSampleForNano\aspnetcore_schema.xml -Destination C:\windows\system32\inetsrv\config\schema\
```

<span data-ttu-id="18a87-151">Spusťte následující skript v relaci vzdálené plochy:</span><span class="sxs-lookup"><span data-stu-id="18a87-151">Run the following script in the remote session:</span></span>

```PowerShell
# Backup existing applicationHost.config
Copy-Item -Path C:\Windows\System32\inetsrv\config\applicationHost.config -Destination  C:\Windows\System32\inetsrv\config\applicationHost_BeforeInstallingANCM.config

Import-Module IISAdministration

# Initialize variables
$aspNetCoreHandlerFilePath="C:\windows\system32\inetsrv\aspnetcore.dll"
Reset-IISServerManager -confirm:$false
$sm = Get-IISServerManager

# Add AppSettings section 
$sm.GetApplicationHostConfiguration().RootSectionGroup.Sections.Add("appSettings")

# Set Allow for handlers section
$appHostconfig = $sm.GetApplicationHostConfiguration()
$section = $appHostconfig.GetSection("system.webServer/handlers")
$section.OverrideMode="Allow"

# Add aspNetCore section to system.webServer
$sectionaspNetCore = $appHostConfig.RootSectionGroup.SectionGroups["system.webServer"].Sections.Add("aspNetCore")
$sectionaspNetCore.OverrideModeDefault = "Allow"
$sm.CommitChanges()

# Configure globalModule
Reset-IISServerManager -confirm:$false
$globalModules = Get-IISConfigSection "system.webServer/globalModules" | Get-IISConfigCollection
New-IISConfigCollectionElement $globalModules -ConfigAttribute @{"name"="AspNetCoreModule";"image"=$aspNetCoreHandlerFilePath}

# Configure module
$modules = Get-IISConfigSection "system.webServer/modules" | Get-IISConfigCollection
New-IISConfigCollectionElement $modules -ConfigAttribute @{"name"="AspNetCoreModule"}
```

> [!NOTE]
> <span data-ttu-id="18a87-152">Odstraňte soubory *aspnetcore.dll* a *aspnetcore_schema.xml* ze sdílené složky po předchozím kroku.</span><span class="sxs-lookup"><span data-stu-id="18a87-152">Delete the files *aspnetcore.dll* and *aspnetcore_schema.xml* from the share after the above step.</span></span>

## <a name="installing-net-core-framework"></a><span data-ttu-id="18a87-153">Instalace rozhraní .NET Framework Core</span><span class="sxs-lookup"><span data-stu-id="18a87-153">Installing .NET Core Framework</span></span>

<span data-ttu-id="18a87-154">Pokud vaše aplikace je publikován jako [nasazení závislé na framework (disketové jednotky)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core musí být nainstalován na serveru.</span><span class="sxs-lookup"><span data-stu-id="18a87-154">If your app is published as a [framework-dependent deployment (FDD)](/dotnet/core/deploying/#framework-dependent-deployments-fdd), .NET Core must be installed on the server.</span></span> <span data-ttu-id="18a87-155">Použití [skript prostředí PowerShell dotnet install.ps1](https://dot.net/v1/dotnet-install.ps1) v relaci vzdálené prostředí PowerShell na Nano Server nainstalovat .NET Core.</span><span class="sxs-lookup"><span data-stu-id="18a87-155">Use the [dotnet-install.ps1 PowerShell script](https://dot.net/v1/dotnet-install.ps1) in a remote PowerShell session to install .NET Core on your Nano Server.</span></span> <span data-ttu-id="18a87-156">Verze rozhraní příkazového řádku s předat `-Version` přepínače:</span><span class="sxs-lookup"><span data-stu-id="18a87-156">Pass the CLI version with the `-Version` switch:</span></span>

```console
dotnet-install.ps1 -Version 2.0.0
```

## <a name="publishing-the-application"></a><span data-ttu-id="18a87-157">Publikování aplikace</span><span class="sxs-lookup"><span data-stu-id="18a87-157">Publishing the application</span></span>

<span data-ttu-id="18a87-158">Zkopírujte výstup publikované aplikace do kořenové sdílené složky.</span><span class="sxs-lookup"><span data-stu-id="18a87-158">Copy the published output of your existing application to the file share's root.</span></span>

<span data-ttu-id="18a87-159">Budete muset provést změny vašeho *web.config* tak, aby odkazoval na které jste extrahovali *dotnet.exe*.</span><span class="sxs-lookup"><span data-stu-id="18a87-159">You may need to make changes to your *web.config* to point to where you extracted *dotnet.exe*.</span></span> <span data-ttu-id="18a87-160">Alternativně můžete přidat *dotnet.exe* pro vaši CESTU.</span><span class="sxs-lookup"><span data-stu-id="18a87-160">Alternatively, you can add *dotnet.exe* to your PATH.</span></span>

<span data-ttu-id="18a87-161">Příklad, jak *web.config* může vypadat, pokud *dotnet.exe* je **není** na CESTĚ:</span><span class="sxs-lookup"><span data-stu-id="18a87-161">Example of how a *web.config* might look if *dotnet.exe* is **not** on the PATH:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.webServer>
    <handlers>
      <add name="aspNetCore" path="*" verb="*" modules="AspNetCoreModule" resourceType="Unspecified" />
    </handlers>
    <aspNetCore processPath="C:\dotnet\dotnet.exe" arguments=".\AspNetCoreSampleForNano.dll" stdoutLogEnabled="false" stdoutLogFile=".\logs\aspnetcore-stdout" forwardWindowsAuthToken="true" />
  </system.webServer>
</configuration>
```

<span data-ttu-id="18a87-162">Spusťte následující příkazy v této relaci vzdálené k vytvoření nového webu ve službě IIS pro publikovanou aplikaci na jiný port než výchozí web.</span><span class="sxs-lookup"><span data-stu-id="18a87-162">Run the following commands in the remote session to create a new site in IIS for the published app on a different port than the default website.</span></span> <span data-ttu-id="18a87-163">Také budete muset otevřít tento port pro přístup k webu.</span><span class="sxs-lookup"><span data-stu-id="18a87-163">You also need to open that port to access the web.</span></span> <span data-ttu-id="18a87-164">Tento skript používá `DefaultAppPool` pro jednoduchost.</span><span class="sxs-lookup"><span data-stu-id="18a87-164">This script uses the `DefaultAppPool` for simplicity.</span></span> <span data-ttu-id="18a87-165">Další požadavky na spuštění pod fond aplikací, najdete v části [fondy aplikací](xref:host-and-deploy/iis/index#application-pools).</span><span class="sxs-lookup"><span data-stu-id="18a87-165">For more considerations on running under an application pool, see [Application Pools](xref:host-and-deploy/iis/index#application-pools).</span></span>

```PowerShell
Import-module IISAdministration
New-IISSite -Name "AspNetCore" -PhysicalPath c:\PublishedApps\AspNetCoreSampleForNano -BindingInformation "*:8000:"
```

## <a name="known-issue-running-net-core-cli-on-nano-server-and-workaround"></a><span data-ttu-id="18a87-166">Známé problémy s .NET Core rozhraní příkazového řádku v Nano Server a alternativní řešení</span><span class="sxs-lookup"><span data-stu-id="18a87-166">Known issue running .NET Core CLI on Nano Server and workaround</span></span>
```PowerShell
New-NetFirewallRule -Name "AspNetCore Port 81 IIS" -DisplayName "Allow HTTP on TCP/81" -Protocol TCP -LocalPort 81 -Action Allow -Enabled True
```

## <a name="running-the-application"></a><span data-ttu-id="18a87-167">Spuštění aplikace</span><span class="sxs-lookup"><span data-stu-id="18a87-167">Running the application</span></span>

<span data-ttu-id="18a87-168">Publikovaná webová aplikace je dostupný v prohlížeči na `http://192.168.1.10:8000`.</span><span class="sxs-lookup"><span data-stu-id="18a87-168">The published web app is accessible in a browser at `http://192.168.1.10:8000`.</span></span> <span data-ttu-id="18a87-169">Pokud jste si nastavili protokolování, jak je popsáno v [vytvoření a přesměrování protokolu](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), můžete zobrazit protokoly na *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span><span class="sxs-lookup"><span data-stu-id="18a87-169">If you've set up logging as described in [Log creation and redirection](xref:host-and-deploy/aspnet-core-module#log-creation-and-redirection), you can view your logs at *C:\PublishedApps\AspNetCoreSampleForNano\logs*.</span></span>