---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: "Pomocí postback ReorderList (C#) | Microsoft Docs"
author: wenz
description: "ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení. Vždy, když je pořadí v seznamu změníte, objednávky a..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 5f5c5e253f6d85203a488152c5ad908157e6ee0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="b5e3a-104">Pomocí postback ReorderList (C#)</span><span class="sxs-lookup"><span data-stu-id="b5e3a-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="b5e3a-105">podle [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b5e3a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b5e3a-106">[Stáhněte si kód](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) nebo [stáhnout PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b5e3a-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="b5e3a-107">ReorderList ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení.</span><span class="sxs-lookup"><span data-stu-id="b5e3a-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="b5e3a-108">Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.</span><span class="sxs-lookup"><span data-stu-id="b5e3a-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="b5e3a-109">Přehled</span><span class="sxs-lookup"><span data-stu-id="b5e3a-109">Overview</span></span>

<span data-ttu-id="b5e3a-110">`ReorderList` Ovládacího prvku AJAX Control Toolkit poskytuje seznam, který může být přeuspořádány uživatelem pomocí operace přetažení.</span><span class="sxs-lookup"><span data-stu-id="b5e3a-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="b5e3a-111">Vždy, když je pořadí v seznamu změníte, zpětné volání informuje server změny.</span><span class="sxs-lookup"><span data-stu-id="b5e3a-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="b5e3a-112">Kroky</span><span class="sxs-lookup"><span data-stu-id="b5e3a-112">Steps</span></span>

<span data-ttu-id="b5e3a-113">Existuje několik možných datových zdrojů pro `ReorderList` ovládacího prvku.</span><span class="sxs-lookup"><span data-stu-id="b5e3a-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="b5e3a-114">Jedna je používat `XmlDataSource` ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="b5e3a-115">Chcete-li vytvořit vazbu tento XML tak, aby `ReorderList` musí být nastavena řízení a povolit postback, následující atributy:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="b5e3a-116">`DataSourceID`: ID zdroje dat</span><span class="sxs-lookup"><span data-stu-id="b5e3a-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="b5e3a-117">`SortOrderField`: Vlastnost řadit podle</span><span class="sxs-lookup"><span data-stu-id="b5e3a-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="b5e3a-118">`AllowReorder`: Zda chcete povolit uživatelům změnit pořadí prvky seznamu</span><span class="sxs-lookup"><span data-stu-id="b5e3a-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="b5e3a-119">`PostBackOnReorder`: Zda dojde k automatickému seznamu je přeskupení zpětné volání</span><span class="sxs-lookup"><span data-stu-id="b5e3a-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="b5e3a-120">Tady je vhodné značky ovládacího prvku:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="b5e3a-121">V rámci `ReorderList` , konkrétní data ze zdroje dat může být vázán pomocí `Eval()` metoda:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="b5e3a-122">Na libovolné pozici na stránce štítek bude obsahovat informace při poslední změna došlo k chybě:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="b5e3a-123">Tento popisek, naplní se text v kódu na straně serveru, zpracování zpětného volání:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="b5e3a-124">Nakonec, aby bylo možné aktivovat funkce ASP.NET AJAX a sady nástrojů ovládacího prvku `ScriptManager` řízení musíte umístit na stránce:</span><span class="sxs-lookup"><span data-stu-id="b5e3a-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="b5e3a-125">[![Každý změna aktivuje zpětné volání](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b5e3a-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="b5e3a-126">Každý změna aktivuje zpětné volání ([Kliknutím zobrazit obrázek v plné velikosti](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b5e3a-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="b5e3a-127">Další</span><span class="sxs-lookup"><span data-stu-id="b5e3a-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)