---
title: Konwencje autoryzacji stron razor w ASP.NET Core
author: guardrex
description: "Informacje o sposobie kontrolowania dostępu do stron z konwencjami przy uruchamianiu autoryzacji użytkowników, które umożliwiają użytkownikom anonimowy dostęp do poszczególnych stron lub folderów stron."
manager: wpickett
ms.author: riande
ms.date: 10/27/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 2bad6e1cc654b972206af03f99160628f81e026f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Konwencje autoryzacji stron razor w ASP.NET Core

Przez [Luke Latham](https://github.com/guardrex)

Jednym ze sposobów kontroli dostępu w aplikacji stron Razor ma używać konwencji autoryzacji podczas uruchamiania. Konwencje te pozwalają na autoryzowanie użytkowników i anonimowe użytkownicy mogą uzyskiwać dostęp do poszczególnych stron lub folderów stron. Konwencje opisanych w tym temacie automatycznie zastosować [filtry autoryzacji](xref:mvc/controllers/filters#authorization-filters) do kontroli dostępu.

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([sposobu pobierania](xref:tutorials/index#how-to-download-a-sample))

## <a name="require-authorization-to-access-a-page"></a>Wymaga autoryzacji do dostępu do strony

Użyj [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do strony w określonej ścieżce:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko kreskami ukośnymi.

[Przeciążenia AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, gdy konieczne jest określenie zasad autoryzacji.

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Wymaga zezwolenia na dostęp do folderu stron

Użyj [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor.

[Przeciążenia AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) jest dostępna, gdy konieczne jest określenie zasad autoryzacji.

## <a name="allow-anonymous-access-to-a-page"></a>Zezwalaj na dostęp anonimowy do strony

Użyj [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do strony w określonej ścieżce:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor bez rozszerzenia i zawierający tylko kreskami ukośnymi.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Zezwalaj na dostęp anonimowy do folderu stron

Użyj [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) Konwencji za pośrednictwem [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) można dodać [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) do wszystkich stron w folderze w określonej ścieżce:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

Określona ścieżka jest ścieżki aparat widoku, która jest ścieżką względną głównego stron Razor.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Uwaga na łączenie uprawnień i dostępu anonimowego

Jest on idealnie prawidłowy, aby określić, że folder stron wymaga autoryzacji i określić, że strony w tym folderze zezwala na dostęp anonimowy:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Odwrotnie, jednak nie jest spełniony. Nie można zadeklarować folderu stron dla dostępu anonimowego i określ strony na potrzeby autoryzacji:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Wymaganie autoryzacji na stronie prywatnych nie będą działać, ponieważ podczas zarówno `AllowAnonymousFilter` i `AuthorizeFilter` filtry są stosowane do strony, `AllowAnonymousFilter` wins i kontrolujących dostęp.

## <a name="see-also"></a>Zobacz także

* [Razor strony trasy i strony modelu dostawców niestandardowych](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) — klasa
