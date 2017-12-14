---
uid: web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
title: "Prostřednictvím kódu programu nastavování hodnot parametrů ObjectDataSource (C#) | Microsoft Docs"
author: rick-anderson
description: "V tomto kurzu budeme zabývat přidání metody do našich DAL a BLL, který přijímá jeden vstupní parametr a vrací data. V příkladu bude nastavte tento parametr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2010
ms.topic: article
ms.assetid: 1c4588bb-255d-4088-b319-5208da756f4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/basic-reporting/programmatically-setting-the-objectdatasource-s-parameter-values-cs
msc.type: authoredcontent
ms.openlocfilehash: 7a009d57f97838feb5b4a3253c6de9a872a9e9ee
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 11/10/2017
---
<a name="programmatically-setting-the-objectdatasources-parameter-values-c"></a>Prostřednictvím kódu programu nastavování hodnot parametrů ObjectDataSource (C#)
====================
podle [Scott Meisnerová](https://twitter.com/ScottOnWriting)

[Stáhněte si ukázkovou aplikaci](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_6_CS.exe) nebo [stáhnout PDF](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/datatutorial06cs1.pdf)

> V tomto kurzu budeme zabývat přidání metody do našich DAL a BLL, který přijímá jeden vstupní parametr a vrací data. V příkladu bude nastavte tento parametr prostřednictvím kódu programu.


## <a name="introduction"></a>Úvod

Jak jsme viděli v [předchozí kurzu](declarative-parameters-cs.md), jsou k dispozici pro deklarativně předání hodnoty parametru metody ObjectDataSource několik možností. Pokud hodnota parametru je pevně zakódovaná, pochází z ovládacího prvku na webové stránce, nebo je v jakýkoli jiný zdroj, který je zdrojem dat čitelná `Parameter` objektu například, že hodnota může být vázaný na vstupní parametr bez nutnosti psaní řádek kódu.

Může se stát, ale pokud hodnota parametru pocházejí z některé zdroje ještě není pozornost pomocí jedné z předdefinované datové zdroje `Parameter` objekty. Pokud náš web podporován uživatelské účty jsme chtít nastavit parametr podle aktuálně přihlášeného v návštěvníka ID uživatele. Nebo může potřebujeme upravit hodnotu parametru před odesláním podél metodě ObjectDataSource podkladového objektu.

Vždy, když ObjectDataSource `Select` metoda je volána ObjectDataSource nejprve vyvolá jeho [zvolíte událostí](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selecting%28VS.80%29.aspx). Potom je volána metoda ObjectDataSource podkladového objektu. Poté, která se dokončí ObjectDataSource [vybrané události](https://msdn.microsoft.com/en-US/library/system.web.ui.webcontrols.objectdatasource.selected%28VS.80%29.aspx) aktivuje (Tato posloupnost událostí, které znázorňuje obrázek 1). Hodnoty parametru předána do metody ObjectDataSource podkladového objektu můžete nastavit nebo upravit v obslužné rutiny události pro `Selecting` událostí.


[![Je volána vybrané a metodu výběru ještě efektivněji události před a po její podkladového objektu ObjectDataSource](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image2.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image1.png)

**Obrázek 1**: The ObjectDataSource `Selected` a `Selecting` události ještě efektivněji před a po její podkladového objektu metoda je volána ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image3.png))


V tomto kurzu budeme zabývat přidání metody do našich DAL a BLL, který přijímá jeden vstupní parametr `Month`, typu `int` a vrátí `EmployeesDataTable` naplněný tyto zaměstnanci, kteří mají jejich náborové výročí v zadaném objektu `Month`. Náš příklad bude nastavte tento parametr prostřednictvím kódu programu na základě aktuálního měsíce zobrazující seznam "Výročí tohoto měsíce."

Můžeme začít!

## <a name="step-1-adding-a-method-toemployeestableadapter"></a>Krok 1: Přidání metody do`EmployeesTableAdapter`

Pro našeho prvního příkladu budeme muset přidat prostředek k načtení těchto zaměstnanci jejichž `HireDate` došlo k chybě v zadaném měsíci. Tato funkce v souladu s Naše architektura musíme nejprve vytvořit metoda v `EmployeesTableAdapter` která se mapuje na správný příkaz jazyka SQL. K tomu, začněte otevřením datové Northwind zadané sady. Klikněte pravým tlačítkem na `EmployeesTableAdapter` popisku a zvolte dotazu přidat.


[![Přidat nový dotaz do EmployeesTableAdapter](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image5.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image4.png)

**Obrázek 2**: přidejte nový dotaz, který `EmployeesTableAdapter` ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image6.png))


Vyberte Přidat příkazu SQL, která vrací řádky. Když se dostanete určení `SELECT` příkaz obrazovky výchozí `SELECT` příkaz pro `EmployeesTableAdapter` již bude načten. Jednoduše přidejte do `WHERE` klauzule: `WHERE DATEPART(m, HireDate) = @Month`. [DATEPART](https://msdn.microsoft.com/en-us/library/ms174420.aspx) je funkce T-SQL, která vrátí část konkrétní datum `datetime` typ; v tomto případě používáme `DATEPART` vrátit měsíc `HireDate` sloupce.


[![Návratový pouze ty řádky kde HireDate sloupec je menší než nebo rovno @HiredBeforeDate parametr](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image8.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image7.png)

**Obrázek 3**: vrátí pouze ty řádky kde `HireDate` sloupec je menší než nebo rovno `@HiredBeforeDate` parametr ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image9.png))


Nakonec změnit `FillBy` a `GetDataBy` metoda názvy na `FillByHiredDateMonth` a `GetEmployeesByHiredDateMonth`, v uvedeném pořadí.


[![Vyberte vhodnější názvy metod než FillBy a GetDataBy](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image11.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image10.png)

**Obrázek 4**: Zvolte více odpovídající metoda názvy než `FillBy` a `GetDataBy` ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image12.png))


Kliknutím na tlačítko Dokončit dokončete průvodce a vraťte se na návrhovou plochu datovou sadu. `EmployeesTableAdapter` By měl nyní zahrnují několik metod přístupu k zaměstnance zařazené v zadaném měsíci.


[![Nové metody se zobrazí v návrhové ploše datové sady](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image14.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image13.png)

**Obrázek 5**: nové metody se zobrazí v návrhové ploše datovou sadu ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image15.png))


## <a name="step-2-adding-thegetemployeesbyhireddatemonthmonthmethod-to-the-business-logic-layer"></a>Krok 2: Přidání`GetEmployeesByHiredDateMonth(month)`metodu pro vrstvu obchodní logiky

Vzhledem k tomu, že naše aplikace architektura používá samostatné vrstvy obchodní logiku a data přístup logiku, musíme přidání metody do našich BLL, který volání dolů vrstvy DAL k načtení zaměstnanci pronajaty formou před určitým datem. Otevřete `EmployeesBLL.cs` souboru a přidejte následující metodu:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample1.cs)]

Stejně jako u našich jiné metody v této třídě `GetEmployeesByHiredDateMonth(month)` jednoduše volá do vrstvy DAL a vrátí výsledky.

## <a name="step-3-displaying-employees-whose-hiring-anniversary-is-this-month"></a>Krok 3: Zobrazení zaměstnanců, jejichž náborové výročí je tohoto měsíce

Posledním krokem v tomto příkladu je zobrazení těchto zaměstnanci, jejichž náborové výročí je tohoto měsíce. Začněte přidáním GridView k `ProgrammaticParams.aspx` stránku `BasicReporting` složky a přidat nové ObjectDataSource jako svůj zdroj dat. Konfigurace ObjectDataSource používat `EmployeesBLL` třídy s `SelectMethod` nastavena na `GetEmployeesByHiredDateMonth(month)`.


[![Použití třídy EmployeesBLL](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image17.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image16.png)

**Obrázek 6**: použití `EmployeesBLL` – třída ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image18.png))


[![Vyberte z GetEmployeesByHiredDateMonth(month) – metoda](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image20.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image19.png)

**Obrázek 7**: vyberte z `GetEmployeesByHiredDateMonth(month)` – metoda ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image21.png))


Poslední obrazovka zobrazí dotaz, abychom mohli poskytovat `month` hodnota parametru zdroje. Vzhledem k tomu, že tato hodnota vytvoříme prostřednictvím kódu programu, ponechejte zdroji parametr nastaven na výchozí žádné možnost a kliknutím na tlačítko Dokončit.


[![Nechte Zadaná sada parametrů zdroje na hodnotu None](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image23.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image22.png)

**Obrázek 8**: ponechte zdrojový parametr nastaven na hodnotu None ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image24.png))


Tím se vytvoří `Parameter` objekt v prvku ObjectDataSource `SelectParameters` kolekce, který nemá hodnotu zadat.


[!code-aspx[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample2.aspx)]

Pokud chcete nastavit tuto hodnotu prostřednictvím kódu programu, je potřeba vytvořit obslužnou rutinu události pro ObjectDataSource `Selecting` událostí. K tomu, přejděte do zobrazení návrhu a dvakrát klikněte ObjectDataSource. Nebo vyberte ObjectDataSource, přejděte do okna vlastností a klikněte na ikonu funkcí bolt. V dalším kroku buď dvakrát klikněte do textového pole vedle `Selecting` události nebo zadejte název obslužné rutiny události, kterou chcete použít.


![Klikněte na na ikonu Bolt v okně vlastností události ovládacího prvku seznam](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image25.png)

**Obrázek 9**: klikněte na na ikonu Bolt v okně vlastností události ovládacího prvku seznam


Oba přístupy přidat novou obslužnou rutinu události pro ObjectDataSource `Selecting` události třídy kódu stránky. V této obslužné rutiny události jsme číst a zapisovat do hodnot parametru pomocí `e.InputParameters[parameterName]`, kde  *`parameterName`*  je hodnota `Name` atribut `<asp:Parameter>` značky ( `InputParameters` může být také kolekce indexované ordinally, jako v `e.InputParameters[index]`). Chcete-li nastavit `month` parametr pro aktuální měsíc, přidejte následující `Selecting` obslužné rutiny události:


[!code-csharp[Main](programmatically-setting-the-objectdatasource-s-parameter-values-cs/samples/sample3.cs)]

Při návštěvě této stránky prostřednictvím prohlížeče uvidíme přijetí této pouze jeden zaměstnance tento měsíc (březen) Laura Novák, který byl od 1994 ve společnosti.


[![Tyto zaměstnanci, jejichž výročích tohoto měsíce jsou zobrazeny.](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image27.png)](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image26.png)

**Obrázek 10**: těch, které zaměstnanci jejichž výročích tento měsíc se zobrazují ([Kliknutím zobrazit obrázek v plné velikosti](programmatically-setting-the-objectdatasource-s-parameter-values-cs/_static/image28.png))


## <a name="summary"></a>Souhrn

Při ObjectDataSource parametry hodnoty lze obvykle nastavit deklarativně, bez nutnosti řádek kódu, je snadné programové nastavení hodnoty parametru. Všechny budeme muset udělat je vytvoření obslužné rutiny události pro ObjectDataSource `Selecting` událost, která aktivuje před podkladového objektu metoda je volána a ručně nastavit hodnoty pro jeden nebo více parametrů prostřednictvím `InputParameters` kolekce.

V tomto kurzu se ukončí části základní vytváření sestav. [Další kurz](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md) se spustí v hlavní tabulka a podrobnosti scénáře a filtrování části, ve kterém jsme budete podívejte se na techniky pro povolení návštěvníka pro filtrování dat a podrobnostem podrobnosti sestavy z hlavní sestavy.

Radostí programování!

## <a name="about-the-author"></a>O autorovi

[Scott Meisnerová](http://www.4guysfromrolla.com/ScottMitchell.shtml), Autor sedm ASP/ASP.NET knih a zakladatele z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje s technologií Microsoft Web od 1998. Scott funguje jako nezávislé poradce, trainer a zapisovače. Jeho nejnovější seznam k [ *Edice nakladatelství Sams naučit sami technologii ASP.NET 2.0 za 24 hodin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Dosažitelný v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) nebo prostřednictvím svého blogu, který najdete na [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Zvláštní poděkování

Tento kurz řady byla zkontrolovány uživatelem mnoho užitečné kontrolorů. Vést kontrolorem pro tento kurz byl Hilton Giesenow. Kontrola Moje nadcházející články MSDN máte zájem? Pokud ano, vyřaďte mi řádek v [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Předchozí](declarative-parameters-cs.md)
[další](displaying-data-with-the-objectdatasource-vb.md)