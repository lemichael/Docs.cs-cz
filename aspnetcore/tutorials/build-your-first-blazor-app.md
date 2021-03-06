---
title: Vytvořte svoji první aplikaci Blazor
author: guardrex
description: Vytvoření podrobné Blazor aplikace.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: tutorials/first-blazor-app
ms.openlocfilehash: d592c5bac1eb9822843a1ad1513a15fdfd6b1032
ms.sourcegitcommit: eb3e51d58dd713eefc242148f45bd9486be3a78a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/02/2019
ms.locfileid: "67500315"
---
# <a name="build-your-first-blazor-app"></a>Vytvořte svoji první aplikaci Blazor

Podle [Daniel Roth](https://github.com/danroth27) a [Luke Latham](https://github.com/guardrex)

V tomto kurzu se dozvíte, jak vytvářet a upravovat aplikace Blazor.

Postupujte podle pokynů v <xref:blazor/get-started> článku vytvořte projekt Blazor pro účely tohoto kurzu. Pojmenujte projekt *ToDoList*.

## <a name="build-components"></a>Sestavení komponent

1. Přejděte do všech tří stránek vaší aplikace v *stránky* složky: Domů čítač a načíst data. Tyto stránky jsou implementované soubory součástí Razor *Index.razor*, *Counter.razor*, a *FetchData.razor*.

1. Na stránce čítače, vyberte **klikněte na mě** tlačítka se zvýší čítač bez aktualizace stránky. Zvyšování hodnoty čítače na webové stránce obvykle vyžaduje zadání jazyka JavaScript, ale Blazor poskytuje lepší přístup pomocí C#.

1. Prozkoumat provádění `Counter` v komponentu *Counter.razor* souboru.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter1.razor)]

   Uživatelské rozhraní `Counter` komponenta je definován v jazyce HTML. Dynamické vykreslování logiku (například smyčky, podmíněné příkazy, výrazy) přidána pomocí vložený C# syntaxe volá [Razor](xref:mvc/views/razor). Značka jazyka HTML a C# logiku vykreslení se převedou na třídu komponenty v okamžiku sestavení. Název generované třídy .NET odpovídá názvu souboru.

   Členy třídy komponenty jsou definovány v `@code` bloku. V `@code` blokovat, stav komponent (vlastnosti, pole) a metody jsou určené pro zpracování událostí nebo definování dalších součástí logiky. Tyto členy, se použije jako součást logiky komponenty vykreslování a pro zpracování událostí.

   Když **klikněte na mě** výběru tlačítka:

   * `Counter` Zaregistrované komponenty `onclick` obslužná rutina volána ( `IncrementCount` metoda).
   * `Counter` Obnoví komponenty jeho vykreslení stromu.
   * Nový vykreslovací stromu je ve srovnání s předchozím histogramem.
   * Se použijí pouze změny do modelu Document Object Model (DOM). Počet zobrazených se aktualizuje.

1. Upravit C# logiku `Counter` součást přírůstek počtu ve dvou místo jednoho.

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter2.razor?highlight=14)]

1. Znovu sestavte a spusťte aplikaci, aby se změny projevily. Vyberte **klikněte na mě** tlačítko. Zvýší čítač ve dvou.

## <a name="use-components"></a>Použití komponent

Zahrňte součásti v jiné součásti pomocí syntaxe pro HTML.

1. Přidat `Counter` komponentu aplikace `Index` tak, že přidáte komponentu `<Counter />` elementu `Index` součásti (*Index.razor*).

   Pokud používáte Blazor na straně klienta pro toto prostředí `SurveyPrompt` používá komponenty `Index` komponenty. Nahradit `<SurveyPrompt>` element s `<Counter />` elementu. Pokud používáte aplikaci na straně serveru Blazor pro toto prostředí, přidejte `<Counter />` elementu `Index` komponenty:

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index1.razor?highlight=7)]

1. Znovu sestavte a spusťte aplikaci. `Index` Komponenta má svůj vlastní čítače.

## <a name="component-parameters"></a>Parametry komponenty

Součástí mohou mít také parametry. Parametry komponenty jsou definovány pomocí vlastnosti neveřejné na komponentní třída dekorován `[Parameter]`. Atributy můžete zadat argumenty pro komponentu v kódu.

1. Aktualizace komponenty `@code` C# kódu:

   * Přidat `IncrementAmount` vlastnost upravené pomocí `[Parameter]` atribut.
   * Změnit `IncrementCount` metoda se má použít `IncrementAmount` při zvýšit hodnotu `currentCount`.

   *Pages/Counter.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Counter.razor?highlight=13,17)]

<!-- Add back when supported.
   > [!NOTE]
   > From Visual Studio, you can quickly add a component parameter by using the `para` snippet. Type `para` and press the `Tab` key twice.
-->

1. Zadejte `IncrementAmount` parametr `Index` komponenty `<Counter>` pomocí atributu element. Nastavte hodnotu čítače přírůstku deset.

   *Pages/Index.razor*:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Index2.razor?highlight=7)]

1. Znovu načíst `Index` komponenty. Hodnota čítače se zvýší o hodnotu deset pokaždé, když **klikněte na mě** výběru tlačítka. Čítače v `Counter` součást pokračuje se zvýší o jedna.

## <a name="route-to-components"></a>Směrovat do komponenty

`@page` Direktiv v horní části *Counter.razor* souboru Určuje, že `Counter` komponenta je koncový bod směrování. `Counter` Komponenta zpracovává požadavky odeslané na `/counter`. Bez `@page` direktiv, komponenta nebude zpracovávat směrování žádostí, ale komponenta je stále možné ostatními komponentami.

## <a name="dependency-injection"></a>Injektáž závislostí

Služby zaregistrované v kontejneru aplikace služby jsou k dispozici na komponenty prostřednictvím [injektáž závislostí (DI)](xref:fundamentals/dependency-injection). Vložit do součástí s použitím služby `@inject` směrnice.

Prozkoumejte direktivy z `FetchData` komponenty.

Je-li pracovat s aplikací Blazor na straně serveru, `WeatherForecastService` není registrován jako [singleton](xref:fundamentals/dependency-injection#service-lifetimes), takže jedna instance služby je k dispozici v rámci aplikace. `@inject` Směrnice slouží k vložení instance `WeatherForecastService` služby do komponenty.

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1.razor?highlight=3)]

`FetchData` Komponenta používá vložený služby jako `ForecastService`, k načtení pole `WeatherForecast` objekty:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData2.razor?highlight=6)]

Je-li pracovat s aplikací na straně klienta Blazor, `HttpClient` se vloží získávat data předpovědi počasí z *weather.json* ve *wwwroot/ukázková data* složky:

*Pages/FetchData.razor*:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData1_client.razor?highlight=7-8)]

A [ \@foreach](/dotnet/csharp/language-reference/keywords/foreach-in) smyčky se použije k vykreslení každou prognózy instanci jako řadu data o počasí v tabulce:

[!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/FetchData3.razor?highlight=11-19)]


## <a name="build-a-todo-list"></a>Vytvoření seznamu úkolů

Přidáte novou součást aplikaci, která implementuje seznam úkolů.

1. Přidat prázdný soubor nazvaný *Todo.razor* k aplikaci v *stránky* složky:

1. Zadejte počáteční značka pro komponentu:

   ```cshtml
   @page "/todo"

   <h1>Todo</h1>
   ```

1. Přidat `Todo` komponentu do navigačního panelu.

   `NavMenu` Součásti (*Shared/NavMenu.razor*) se používá v rozložení aplikace. Rozložení jsou komponenty, které umožňují, aby se zabránilo duplicitě obsahu v aplikaci.

   Přidat `<NavLink>` – element pro `Todo` součásti tak, že přidáte následující značky položky seznamu níže existující seznam položek v *Shared/NavMenu.razor* souboru:

   ```cshtml
   <li class="nav-item px-3">
       <NavLink class="nav-link" href="todo">
           <span class="oi oi-list-rich" aria-hidden="true"></span> Todo
       </NavLink>
   </li>
   ```

1. Znovu sestavte a spusťte aplikaci. Na stránce Nový Todo potvrdit, že odkaz `Todo` komponenty funguje.

1. Přidat *TodoItem.cs* souboru do kořenového adresáře projektu k uložení třídu, která představuje položku seznamu úkolů. Pomocí následujících C# kód `TodoItem` třídy:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/TodoItem.cs)]

1. Vraťte se `Todo` součásti (*Pages/Todo.razor*):

   * Přidání polí pro položky seznamu úkolů `@code` bloku. `Todo` Součásti používá toto pole pro uchování stavu pro seznam úkolů.
   * Přidat neuspořádaný seznam značek a `foreach` smyčky k vykreslení každé položky todo jako položku seznamu (`<li>`).

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo4.razor?highlight=5-10,12-14)]

1. Aplikace vyžaduje prvky uživatelského rozhraní pro přidávání položek todo do seznamu. Přidat textový vstup (`<input>`) a tlačítko (`<button>`) pod neuspořádaný seznam (`<ul>...</ul>`):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo5.razor?highlight=12-13)]

1. Znovu sestavte a spusťte aplikaci. Když **přidat todo** se vybere tlačítko, nic se nestane, protože obslužná rutina události není svázanou tlačítka.

1. Přidat `AddTodo` metodu `Todo` komponenty a zaregistrujte ho pro výběr tlačítka pomocí `@onclick` atribut. `AddTodo` C# Metoda je volána při výběru tlačítka:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo6.razor?highlight=2,7-10)]

1. Chcete-li získat název nové položky seznamu úkolů, přidejte `newTodo` pole řetězce v horní části `@code` blokovat a navázat jej na hodnotu text input pomocí `bind` atribut v `<input>` element:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo7.razor?highlight=2)]

   ```cshtml
   <input placeholder="Something todo" @bind="@newTodo" />
   ```

1. Aktualizace `AddTodo` způsob, jak přidat `TodoItem` se zadaným názvem do seznamu. Smazat hodnotu textového zadání tak, že nastavíte `newTodo` na prázdný řetězec:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo8.razor?highlight=19-26)]

1. Znovu sestavte a spusťte aplikaci. Přidejte několik položek todo do seznamu todo k testování nového kódu.

1. Text názvu pro každou položku seznamu úkolů lze upravovat, a zaškrtávací políčko může pomoci udržovat přehled o dokončené položky uživatele. Přidat vstup zaškrtněte políčko u každé položky todo a její hodnotu na vytvoření vazby `IsDone` vlastnost. Změna `@todo.Title` do `<input>` prvek vázán na `@todo.Title`:

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/ToDo9.razor?highlight=5-6)]

1. Chcete-li ověřit, že tyto hodnoty jsou vázány, aktualizujte `<h1>` záhlaví zobrazíte počet nesplněné položky seznamu úkolů nejsou kompletní (`IsDone` je `false`).

   ```cshtml
   <h1>Todo (@todos.Count(todo => !todo.IsDone))</h1>
   ```

1. Dokončené `Todo` součásti (*Pages/Todo.razor*):

   [!code-cshtml[](build-your-first-blazor-app/samples_snapshot/3.x/Todo.razor)]

1. Znovu sestavte a spusťte aplikaci. Přidání položky seznamu úkolů k testování nového kódu.

> [!div class="nextstepaction"]
> <xref:blazor/components>
