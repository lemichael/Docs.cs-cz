---
title: Hostování ASP.NET Core v kontejnerech Docker
author: rick-anderson
description: Seznamte se s odkazy na zdroje a Naučte se, jak hostovat aplikace ASP.NET Core v kontejnerech Docker.
ms.author: riande
ms.custom: mvc
ms.date: 01/08/2018
uid: host-and-deploy/docker/index
ms.openlocfilehash: cb5f774db5fab46a57f8ca4bbbca148f20f371ba
ms.sourcegitcommit: b40613c603d6f0cc71f3232c16df61550907f550
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/18/2019
ms.locfileid: "68308051"
---
# <a name="host-aspnet-core-in-docker-containers"></a>Hostování ASP.NET Core v kontejnerech Docker

Následující články jsou k dispozici pro získání informací o hostování ASP.NET Corech aplikací v Docker:

[Úvod ke kontejnerům a Dockeru](/dotnet/standard/microservices-architecture/container-docker-introduction/index)  
Podívejte se, jak vytváření kontejnerů představuje přístup k vývoji softwaru, ve kterém je aplikace nebo služba, její závislosti a její konfigurace zabaleny společně jako image kontejneru. Bitovou kopii lze otestovat a následně nasadit na hostitele.

[Co je Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-defined)  
Seznamte se s tím, jak Docker je open source projekt pro automatizaci nasazení aplikací jako přenosných a vlastních kontejnerů, které můžou běžet v cloudu nebo místně.

[Terminologie Docker](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-terminology)  
Seznamte se s podmínkami a definicemi pro technologii Docker.

[Kontejnery, obrázky a registry Dockeru](/dotnet/standard/microservices-architecture/container-docker-introduction/docker-containers-images-registries)  
Zjistěte, jak jsou image kontejnerů Docker uložené v registru imagí pro konzistentní nasazení napříč prostředími.

<xref:host-and-deploy/docker/building-net-docker-images>Naučte se sestavovat a ukotvovat aplikaci ASP.NET Core. Prozkoumejte image Docker udržované Microsoftem a prozkoumejte případy použití.

[Nástroje kontejneru sady Visual Studio](xref:host-and-deploy/docker/visual-studio-tools-for-docker)  
Zjistěte, jak Visual Studio podporuje sestavování, ladění a spouštění aplikací ASP.NET Core cílících na .NET Framework nebo .NET Core na Docker for Windows. Podporují se kontejnery Windows i Linux.

[Publikovat do Azure Container Registry](/azure/vs-azure-tools-docker-hosting-web-apps-in-docker)  
Zjistěte, jak pomocí rozšíření sady Visual Studio Container Tools nasadit aplikaci ASP.NET Core do hostitele Docker v Azure pomocí PowerShellu.

[Konfigurace ASP.NET Core pro práci se servery proxy a nástroji pro vyrovnávání zatížení](xref:host-and-deploy/proxy-load-balancer)  
Pro aplikace hostované za proxy servery a nástroji pro vyrovnávání zatížení může být vyžadována další konfigurace. Předávání požadavků prostřednictvím proxy serveru často překrývá informace o původní žádosti, jako je například schéma a IP adresa klienta. Může být nutné přeslat některé informace o žádosti do aplikace ručně.
