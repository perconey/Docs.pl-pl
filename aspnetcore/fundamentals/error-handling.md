---
title: "Obsługa błędów w ASP.NET Core"
author: ardalis
description: "Wykryj sposób obsługi błędów w aplikacji platformy ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 1504e49d97c4e72a34597360ad05bd9116188b33
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/11/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Wprowadzenie do obsługi błędów w platformy ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Dykstra niestandardowy](https://github.com/tdykstra/)

W tym artykule omówiono typowe appoaches do obsługi błędów w aplikacji platformy ASP.NET Core.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Strona wyjątek dewelopera

Aby skonfigurować aplikację do wyświetlenia strony, który zawiera szczegółowe informacje dotyczące wyjątków, zainstaluj `Microsoft.AspNetCore.Diagnostics` NuGet pakiet, a następnie dodaj wiersz do [skonfigurować metodę w klasie uruchamiania](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Umieść `UseDeveloperExceptionPage` przed wszystkich programów pośredniczących chcesz przechwytywać wyjątki, takich jak `app.UseMvc`.

>[!WARNING]
> Włącz stronę wyjątek developer **tylko, gdy aplikacja jest uruchomiona w środowisku programistycznym**. Nie chcesz udostępniać informacje szczegółowe wyjątek publicznie, po uruchomieniu aplikacji w środowisku produkcyjnym. [Dowiedz się więcej o konfigurowaniu środowisk](environments.md).

Aby wyświetlić stronę wyjątek developer, uruchom przykładową aplikację ze środowiskiem ustawioną `Development`i Dodaj `?throw=true` do podstawowego adresu URL aplikacji. Strona zawiera kilka kart, informacje o wyjątku i żądania. Karta pierwszy zawiera ślad stosu. 

![Ślad stosu](error-handling/_static/developer-exception-page.png)

Następna karta zawiera zapytanie parametrów ciągu ewentualne.

![Parametry ciągu zapytania](error-handling/_static/developer-exception-page-query.png)

To żądanie nie ma żadnych plików cookie, ale jeśli jak, będą widoczne w **plików cookie** kartę. Nagłówki, które zostały przekazane na karcie ostatniego jest widoczny.

![Nagłówki](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>Konfigurowanie niestandardowych wyjątków, Obsługa strony

Należy dobrze, aby skonfigurować stronę programu obsługi wyjątków do użycia, gdy aplikacja nie jest uruchomiona `Development` środowiska.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

W aplikacji MVC nie jawnie dekoracji metody akcji programu obsługi błędu z atrybutami metody HTTP, takie jak `HttpGet`. Za pomocą jawnego zlecenia może uniemożliwić osiągnięcia metody niektórych żądań.

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>Konfigurowanie stanu strony kodowe

Domyślnie aplikacji nie będzie zapewniać strona kodowa sformatowanego stan kodów stanu HTTP, takich jak 500 (wewnętrzny błąd serwera) lub 404 (nie znaleziono). Można skonfigurować `StatusCodePagesMiddleware` przez dodanie wiersza do `Configure` metody:

```csharp
app.UseStatusCodePages();
```

Domyślnie to oprogramowanie pośredniczące dodaje prosty, tekstowy obsługi wspólnej kodów stanu, na przykład 404:

![strona 404](error-handling/_static/default-404-status-code.png)

Oprogramowanie pośredniczące obsługuje kilka metod inne rozszerzenie. Wyższy wyrażenia lambda, ma inny ciąg zawartości typu i formatu.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

Brak metody rozszerzenia przekierowania. Kod stanu 302 jedną wysyła do klienta, a jeden zwraca oryginalnego kodu stanu do klienta, ale również wykonuje program obsługi dla adresu URL przekierowania.

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Jeśli trzeba wyłączyć stan strony kodowe dla niektórych żądań, możesz to zrobić:

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Kod obsługi wyjątków

Kod w stronach obsługi wyjątków można zgłaszają wyjątki. Często jest dobrym rozwiązaniem dla stron błędów produkcji ma zawierać wyłącznie statyczne.

Ponadto należy pamiętać, że po wysłaniu nagłówków odpowiedzi kod stanu odpowiedzi nie można zmienić ani żadnych stron wyjątek lub obsługi uruchomić. Odpowiedzi muszą być wypełnione lub połączenie zostało przerwane.

## <a name="server-exception-handling"></a>Obsługa wyjątków serwera

Oprócz obsługi logikę w aplikacji, wyjątków [serwera](servers/index.md) hosting aplikacji wykonuje niektóre obsługi wyjątków. Jeśli serwer przechwytuje wyjątek przed wysłaniem nagłówki, serwer wysyła 500 Wewnętrzny błąd serwera odpowiedzi z nie treści. Jeśli serwer przechwytuje wyjątek po wysłaniu nagłówków, serwer zamyka połączenie. Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer. Wszystkie wyjątki, która występuje jest obsługiwany przez wyjątek serwera obsługi. Wszelkie skonfigurowane niestandardowe strony błędów lub oprogramowanie pośredniczące obsługi wyjątków lub filtrów nie mają wpływu na tego zachowania.

## <a name="startup-exception-handling"></a>Obsługa wyjątków uruchamiania

Tylko warstwę hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji. Możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](hosting.md#detailed-errors) przy użyciu `captureStartupErrors` i `detailedErrors` klucza.

Jeśli błąd pojawia się po adres/port hosta powiązanie hosting można wyświetlić tylko stronę błędu dla błędu uruchomienia przechwycony. Jeśli żadnego powiązania nie powiedzie się z jakiegokolwiek powodu, hostingu warstwy loguje wyjątek krytyczny dotnet awarie procesów, a żadna strona błędu jest wyświetlane, gdy aplikacja jest uruchomiona [Kestrel](xref:fundamentals/servers/kestrel) serwera.

Podczas uruchamiania [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 awarii procesu* zwróconego przez [moduł platformy ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) Jeśli proces nie może być Rozpoczęto. Wykonaj porady dotyczące rozwiązywania problemów w [Rozwiązywanie problemów z platformy ASP.NET Core w usługach IIS](xref:host-and-deploy/iis/troubleshoot) tematu.

## <a name="aspnet-mvc-error-handling"></a>Obsługa błędów platformy ASP.NET MVC

[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.

### <a name="exception-filters"></a>Filtry wyjątków

Filtry wyjątków można skonfigurować globalnie lub na podstawie-controller lub -action w aplikacji MVC. Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr, a nie są nazywane inaczej. Dowiedz się więcej na temat filtrów wyjątków na [filtry](../mvc/controllers/filters.md).

>[!TIP]
> Filtry wyjątków są dobrym zalewania wyjątków, które występują w ramach działań MVC, ale nie są one tak elastyczne jako błąd obsługi oprogramowania pośredniczącego. Preferowane jest oprogramowanie pośredniczące w przypadku ogólnych i za pomocą filtrów, tylko gdy należy wykonywać obsługi błędów *inaczej* oparte na Akcja kontrolera MVC, który został wybrany.

### <a name="handling-model-state-errors"></a>Stan modelu obsługi błędów

[Sprawdzanie poprawności modelu](../mvc/models/validation.md) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio zareagować.

Niektóre aplikacje wybierze wykonać standardowej konwencji zajmujących się błędy sprawdzania poprawności modelu, w którym to przypadku [filtru](../mvc/controllers/filters.md) może być odpowiednie miejsce do wdrożenia tych zasad. Należy przetestować zachowanie akcji stanów nieprawidłowy model. Dowiedz się więcej w [testowania logiką kontrolera](../mvc/controllers/testing.md).



