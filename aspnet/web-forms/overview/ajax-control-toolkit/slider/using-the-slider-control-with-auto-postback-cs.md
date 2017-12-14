---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: "Použití ovládacího prvku posuvník s automatického provedení operace (C#) | Microsoft Docs"
author: wenz
description: "Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné, aby automaticky zaúčtovat posuvníku..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: f6291f4162b3069a6316a60b4b29f82f55121aac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-the-slider-control-with-auto-postback-c"></a>Použití ovládacího prvku posuvník s automatického provedení operace (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)

> Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.


## <a name="overview"></a>Přehled

Ovládacího prvku posuvník Toolkitu AJAX poskytuje grafické jezdce, která se dá řídit pomocí myši. Je možné změnit automatické odeslání posuvníku jednou jeho hodnotu.

## <a name="steps"></a>Kroky

Aby bylo možné posuvník automaticky zpětného volání při změně, třeba obou polí atribut `AutoPostBack="true"`: textové pole, které bude posuvník sám a textové pole, které obsahuje pozici posuvníku. Zde je kód požadované pro tento:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

`SliderExtender` Řízení z ovládacího prvku ASP.NET AJAX Toolkit přiřadí funkci posuvník do dvou textových polí:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

Element label další se později použije informovat uživatele zpětné volání:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

Nakonec `ScriptManager` ovládacího prvku ASP.NET AJAX načte požadované JavaScript pro Toolkitu postup:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

Nyní je jezdec publikování zpět; Tato událost může na straně serveru, místo zachycení a reagovali na ni:

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


[![Posunutím jezdce aktivuje zpětné volání](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)

Posunutím jezdce aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-cs/_static/image3.png))


[![Později datum této změny je napsána v popisku](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)

Později, datum této změny je napsána v popisku ([Kliknutím zobrazit obrázek v plné velikosti](using-the-slider-control-with-auto-postback-cs/_static/image6.png))

>[!div class="step-by-step"]
[Další](databinding-the-slider-control-cs.md)