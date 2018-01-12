---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: "Spuštění modální automaticky otevíraném okně z kódu serveru (VB) | Microsoft Docs"
author: wenz
description: "ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale některé scénáře vyžadují tento t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: c4bcf3e32b3aa91bb73e01296bc1fc1a2e064711
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Spuštění modální automaticky otevíraném okně z kódu serveru (VB)
====================
podle [Christian Wenz](https://github.com/wenz)

[Stáhněte si kód](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale některé scénáře vyžadují, aby otevření Modální překryvné okno se aktivuje na straně serveru.


## <a name="overview"></a>Přehled

ModalPopup ovládacího prvku Toolkitu AJAX nabízí jednoduchý způsob, jak vytvořit modální místní prostředky klienta. Ale některé scénáře vyžadují, aby otevření Modální překryvné okno se aktivuje na straně serveru.

## <a name="steps"></a>Kroky

První řadě prvku tlačítko ASP.NET je potřeba ukazují, jak funguje řízení ModalPopup. Přidání tlačítka v rámci &lt;formuláře&gt; element na novou stránku:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Pak musíte kód pro místní, kterou chcete vytvořit. Definovat jako `<asp:Panel>` řízení a ujistěte se, že zahrnuje ovládací prvek tlačítko. Ovládací prvek ModalPopup nabízí funkci, aby toto tlačítko Zavřít místní; v opačném případě neexistuje žádný snadný způsob, jak jej zmizí.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Přidáte další ovládací prvek ModalPopup z ASP.NET AJAX Toolkit na stránku. Nastavit vlastnosti pro tlačítka, který načte ovládacího prvku tlačítko, takže je zmizí a ID skutečného místní nabídky.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Stejně jako u všech webových stránek, které jsou založené na ASP.NET AJAX; Správce skriptu je potřeba načíst potřebné knihovny JavaScript pro jiný cíl prohlížeče:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Spouštění v příkladu v prohlížeči. Po kliknutí na tlačítko se zobrazí Modální místní okno. K dosažení stejného efektu pomocí kódu na straně serveru, je nutné zadat nové tlačítko:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Jak můžete vidět, a klikněte na tlačítko generuje zpětné volání a provede `ServerButton_Click()` metoda na serveru. Tato metoda volá funkci jazyka JavaScript `launchModal()` je provést, aby byl přesný, bude proveden funkce JavaScript, která po načtení stránky:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Úkolem `launchModal()` je zobrazit ModalPopup. `launchModal()` Funkce se spustí po dokončení stránky HTML se načetl. V tuto chvíli však rozhraní ASP.NET AJAX nebyl úplným načtením ještě. Proto `launchModal()` funkce právě nastaví proměnné, která ovládacího prvku ModalPopup musí zobrazit později na:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

`pageLoad()` Funkce JavaScript, která je speciální funkce, která provede se jednou prvku ASP.NET AJAX byla plně načtena. Proto přidáme kód k této funkci můžete zobrazit ModalPopup řízení, ale jenom v případě `launchModal()` bylo voláno před:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

`$find()` Hledá pojmenovaného elementu na stránce funkce a očekává ID na straně serveru jako parametr. Proto `$find("mpe")` vrátí reprezentaci klienta ovládacího prvku ModalPopup; jeho `show()` metoda umožňuje automaticky otevřeném okně se zobrazí.


[![Modální automaticky otevřeném okně se zobrazí, když po kliknutí na buď tlačítek](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Modální automaticky otevřeném okně se zobrazí, když po kliknutí na buď tlačítek ([Kliknutím zobrazit obrázek v plné velikosti](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

>[!div class="step-by-step"]
[Předchozí](positioning-a-modalpopup-cs.md)
[další](using-modalpopup-with-a-repeater-control-vb.md)