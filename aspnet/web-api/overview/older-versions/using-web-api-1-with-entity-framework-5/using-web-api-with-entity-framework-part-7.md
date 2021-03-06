---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
title: "Część 7: Tworzenie głównym strony | Dokumentacja firmy Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: eb32a17b-626c-4373-9a7d-3387992f3c04
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-7
msc.type: authoredcontent
ms.openlocfilehash: 4d06e72bc664f707bbbe4603be41347158c58903
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="part-7-creating-the-main-page"></a>Część 7: Tworzenie głównym strony
====================
przez [Wasson Jan](https://github.com/MikeWasson)

[Pobieranie ukończone projektu](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="creating-the-main-page"></a>Tworzenie głównego strony

W tej sekcji utworzysz strony głównej aplikacji. Ta strona będzie bardziej skomplikowane niż strony administratora, więc firma Microsoft będzie podejścia w kilku etapach. Na bieżąco zobaczysz niektórych bardziej zaawansowanych technik Knockout.js. Poniżej przedstawiono podstawowe układ strony:

![](using-web-api-with-entity-framework-part-7/_static/image1.png)

- "Produkty" zawiera tablicę produktów.
- "Koszyk" zawiera tablicę produkty z ilości. Klikając przycisk "Dodaj do koszyka" aktualizuje koszyka.
- "Zamówienia" zawiera tablicę kolejności identyfikatorów.
- "Szczegóły" posiada szczegółami zamówienia, czyli tablicę elementów (produkty z ilości)

Zaczniemy, definiując niektóre podstawowe układu w języku HTML, bez powiązania danych lub skryptu. Otwórz plik Views/Home/Index.cshtml i zastąpienie całej zawartości z następujących czynności:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample1.html)]

Następnie dodaj sekcję skryptów i utworzyć pusty model widoku:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-7/samples/sample2.cshtml)]

Na podstawie projektu rysowane wcześniej, nasz model widoku musi dostrzegalne elementy dla produktów, koszyka zleceń i szczegóły. Dodaj następujące zmienne, które mają `AppViewModel` obiektu:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample3.js)]

Użytkownicy mogą dodać elementy z listy produktów do koszyka i usuwanie elementów z koszyka. Aby hermetyzować tych funkcji, utworzymy innej klasy modelu widoku, która reprezentuje produkt. Dodaj następujący kod do `AppViewModel`:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample4.js?highlight=4)]

`ProductViewModel` Klasa zawiera dwie funkcje, które są używane do przenoszenia produktu do i z koszyka: `addItemToCart` dodaje jedną jednostkę produktu do koszyka, i `removeAllFromCart` spowoduje usunięcie wszystkich ilości produktu.

Użytkownicy mogą wybrać istniejące i pobrać szczegółów zamówienia. Umieściliśmy tej funkcji do innego modelu widoku:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample5.js?highlight=4)]

`OrderDetailsViewModel` Jest inicjowany z zlecenia i jego pobiera szczegółów zamówienia, wysyłając żądanie AJAX do serwera.

Zauważ również, `total` właściwość `OrderDetailsViewModel`. Ta właściwość jest specjalnym rodzajem według o nazwie [obliczana według](http://knockoutjs.com/documentation/computedObservables.html). Jak wskazuje nazwę, obliczoną według umożliwia wiązanie danych z obliczoną wartością &#8212; w takim przypadku łączny koszt zlecenia.

Następnie dodaj tych funkcji `AppViewModel`:

- `resetCart`Usuwa wszystkie elementy z koszyka.
- `getDetails`pobiera szczegóły dla zamówienia (przez pusing nowy `OrderDetailsViewModel` na `details` listy).
- `createOrder`Tworzy nową kolejność i opróżnia koszyka.


[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample6.js?highlight=4)]

Na koniec zainicjować modelu widoku dokonując żądania AJAX dla produktów i zleceń:

[!code-javascript[Main](using-web-api-with-entity-framework-part-7/samples/sample7.js)]

OK dużo kodu, ale jego budujemy się krok po kroku, więc miejmy nadzieję, że projekt jest wyczyszczone. Teraz możemy dodać niektóre wiązania Knockout.js do kodu HTML.

**Produkty**

Poniżej przedstawiono powiązania dla listy produktów:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample8.html)]

Wykonuje iterację na tablicy produktów i wyświetla nazwę i cenę. Przycisk "Dodaj do kolejność" jest widoczny tylko wtedy, gdy użytkownik jest zalogowany.

Wywołania przycisk "Dodaj do kolejność" `addItemToCart` na `ProductViewModel` wystąpienia dla produktu. Oznacza to dobry funkcja Knockout.js: gdy model widoku zawiera inne modele widoku, można zastosować powiązania wewnętrzny modelu. W tym przykładzie powiązania w ramach `foreach` są stosowane do każdego z `ProductViewModel` wystąpień. Ta metoda jest znacznie czyszczący niż wprowadzanie wszystkich funkcji do pojedynczego model widoku.

**Koszyka**

Poniżej przedstawiono powiązania z koszyka:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample9.html)]

Wykonuje iterację na tablicy koszyka i wyświetla nazwę, ceny i ilości. Należy zwrócić uwagę, czy przycisk "Tworzenie zamówienia" i "Usuń" łącze jest powiązana z funkcji model widoku.

**Zlecenia**

Poniżej przedstawiono powiązania listę zleceń:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample10.html)]

To przechodzi przez zamówienia i zawiera identyfikator zamówienia. Zdarzenie click łącze jest powiązany z `getDetails` funkcji.

**Szczegóły zlecenia**

Poniżej przedstawiono powiązania szczegółów zamówienia:

[!code-html[Main](using-web-api-with-entity-framework-part-7/samples/sample11.html)]

Wykonuje iterację na elementy w kolejności i wyświetla produktu, ceny i ilość. Otaczające div jest widoczny tylko wtedy, gdy tablica szczegóły zawiera jeden lub więcej elementów.

## <a name="conclusion"></a>Wniosek

W tym samouczku utworzono aplikację, która korzysta z programu Entity Framework do komunikacji z bazy danych i interfejsu API sieci Web programu ASP.NET zapewnia interfejs publicznych na podstawie warstwy danych. Używamy ASP.NET MVC 4 do renderowania stron HTML i Knockout.js i jQuery aby zapewnić dynamiczne interakcje bez przeładowania strony.

Dodatkowe zasoby:

- [Mapa zawartości dostępu do danych w programie ASP.NET](https://msdn.microsoft.com/library/6759sth4.aspx)
- [Centrum deweloperów Entity Framework](https://msdn.microsoft.com/data/ef)

>[!div class="step-by-step"]
[Poprzednie](using-web-api-with-entity-framework-part-6.md)
