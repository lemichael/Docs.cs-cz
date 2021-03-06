---
title: ASP.NET Core Blazor podporované platformy
author: guardrex
description: Další informace o podporovaných platforem pro ASP.NET Core Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2019
ms.locfileid: "67855797"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core Blazor podporované platformy

Podle [Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>Požadavky na prohlížeč

### <a name="blazor-client-side"></a>Blazor na straně klienta

| Browser                          | Version               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | aktuální               |
| Mozilla Firefox                  | aktuální               |
| Google Chrome, včetně Androidu | aktuální               |
| Safari, včetně systému iOS            | aktuální               |
| Microsoft Internet Explorer      | Nepodporuje se&dagger; |

&dagger;Microsoft Internet Explorer nepodporuje [WebAssembly](https://webassembly.org).

### <a name="blazor-server-side"></a>Blazor na straně serveru

| Browser                          | Version    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | aktuální    |
| Mozilla Firefox                  | aktuální    |
| Google Chrome, včetně Androidu | aktuální    |
| Safari, včetně systému iOS            | aktuální    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;Jsou vyžadovány další polyfills (například příslibů jde přidat prostřednictvím [Polyfill.io](https://polyfill.io/v3/) sady).

## <a name="additional-resources"></a>Další zdroje

* <xref:blazor/hosting-models>
