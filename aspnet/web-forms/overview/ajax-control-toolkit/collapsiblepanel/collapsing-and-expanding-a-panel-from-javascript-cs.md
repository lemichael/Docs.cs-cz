---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
title: "Sbalení a rozšiřování panely z jazyka JavaScript (C#) | Microsoft Docs"
author: wenz
description: "CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: de5500be-75e5-461a-8064-b70ae52ea6a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-cs
msc.type: authoredcontent
ms.openlocfilehash: 666f3e212ccdd5b26b466f4672134ce751dc5dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-c"></a>Sbalení a rozšiřování panely z jazyka JavaScript (C#)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1CS.pdf)

> CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho znovu. Tyto dvě akce se dá taky spustit z vlastního kódu jazyka JavaScript.


## <a name="overview"></a>Přehled

CollapsiblePanel ovládacího prvku ASP.NET AJAX Control Toolkit rozšiřuje panelu a poskytuje schopnost Sbalit obsah a rozbalte ho znovu. Tyto dvě akce se dá taky spustit z vlastního kódu jazyka JavaScript.

## <a name="steps"></a>Kroky

Nejdřív všech, vytvořte novou stránku ASP.NET a zahrnout `ScriptManager` v rámci ten `<form>` elementu. Tento kód načte knihovny ASP.NET AJAX, který je požadován Toolkitu:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample1.aspx)]

Pak vytvořte panelu s nějaký text tak, aby si můžete prohlédnout účinek rozbalit nebo sbalit:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample2.aspx)]

Jak je vidět na panelu odkazuje na třídu CSS, která se zobrazí zde (a v podstatě definuje barvu pozadí a šířka panelu):

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample3.css)]

`CollapsiblePanelExtender` Vyžaduje ovládací prvek `TargetControlID` atributů tak, aby sada nástrojů zná panelu sbalit nebo rozšířit na žádost:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample4.aspx)]

Bohužel rozšiřujícího objektu aktuálně nevystavuje konkrétní rozhraní API pro sbalení nebo rozbalení panelu, ale některé nedokumentovanými metody provede. Nejprve přidejte tři tlačítka HTML na stránku, který se potom aktivuje JavaScript na straně klienta sbalit nebo rozbalte obsah panelu:

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample5.aspx)]

V kódu jazyka JavaScript na straně klienta (práce s `<script type="text/javascript">`), `$find()` metoda se musí použít pro přístup k `CollapsiblePanelExtender`. `$find("cpe")`Vrátí odkaz na něj. Odtud na konkrétní metody vyřeší na prováděné úloze.

Metoda pro otevření (rozšíření) se nazývá panelu `_doOpen()`; následující kód implementuje `doOpen()` funkce volána při kliknutí na první tlačítko:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample6.js)]

Zavření nebo sbalení panelu, `_doClose()` metoda je třeba provést. Proto když uživatel klikne na tlačítko druhé, se nazývá následující kód v JavaScriptu:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample7.js)]

Tlačítko třetí přepne stav panelu: z sbaleny do rozšiřovat a naopak. `CollapsiblePanelExtender` Zpřístupní `toggle()` metoda, která zajišťuje přesně který: obrátí stav panelu. Je však také jiné přístup (která vnitřně používá `toggle()` metoda): `get_Collapsed()` metodu `CollapsiblePanelExtender()` nám oznamuje, zda je panel sbalený nebo ne. V závislosti na návratovou hodnotu této funkce, panelu je pak buď rozšířit (`_doOpen()` metoda) nebo sbalené (`_doClose()`) metoda:

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-cs/samples/sample8.js)]


[![Tlačítko třetí změní stav panelu: z sbalené rozšířené a zpět](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image1.png)

Tlačítko třetí změní stav panelu: z sbalené rozšířené a pozadí ([Kliknutím zobrazit obrázek v plné velikosti](collapsing-and-expanding-a-panel-from-javascript-cs/_static/image3.png))

>[!div class="step-by-step"]
[Další](collapsing-and-expanding-a-panel-from-javascript-vb.md)