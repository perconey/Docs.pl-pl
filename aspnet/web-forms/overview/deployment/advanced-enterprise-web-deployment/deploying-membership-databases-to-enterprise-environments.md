---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: "Wdrożenie bazy danych członkostwa w środowiskach przedsiębiorstw | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano najważniejsze kwestie związane z a wyzwania, które należy rozwiązać podczas obsługi administracyjnej bazy danych usług aplikacji platformy ASP.NET (więcej wspólne..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 27fade9fc5cae917579d4963da7bca12f6a5cda1
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Wdrożenie bazy danych członkostwa w środowiskach przedsiębiorstw
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano najważniejsze kwestie związane z a wyzwania napotykane przez należy rozwiązać, gdy świadczenia aplikacji ASP.NET usług baz danych (określone częściej członkostwa bazy danych) w środowisku testowym, tymczasowym czy produkcyjnym. Omówiono także podejścia, których można użyć, aby spełnić te problemy.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>Jakie problemy podczas wdrażania bazy danych członkostwa?

W większości przypadków gdy opracować strategię wdrażania bazy danych, w pierwszej kolejności należy wziąć pod uwagę jest jakie dane mają zostać wdrożone. W środowisku programowania lub testowym można wdrożyć danych konta użytkownika, aby ułatwić szybkie i łatwe testowanie. W środowisku tymczasowym czy produkcyjnym jest bardzo prawdopodobne, czy chcesz wdrożyć dane konta użytkownika.

Niestety bazy danych członkostwa ASP.NET wprowadzić niektóre określonych wyzwania, które podjęcie tej decyzji znacznie bardziej złożonych:

- Wdrożenie tylko schematu pozostawi bazie danych członkostwa w stanie działającym stanie. Jest to spowodowane bazie danych członkostwa zawiera niektóre dane konfiguracji (w **aspnet\_SchemaVersions** tabeli), która baza danych wymaga prawidłowego działania. Tak można wykonać wdrożenie tylko schematu bazy danych członkostwa w celu wykluczenia danych konta użytkownika, należy uruchomić skrypt po wdrożeniu, aby dodać dane konfiguracji niezbędne.
- W zależności od konfiguracji bazy danych członkostwa Dostawca członkostwa może używać klucza komputera do szyfrowania haseł i przechowywania ich w bazie danych. W takim przypadku danych z konta użytkownika, które wdrażasz z bazą danych stanie się bezużyteczne na serwerze docelowym. Z tego powodu wdrażanie dane konto użytkownika nie jest obsługiwanym scenariuszem.

## <a name="choosing-a-membership-database-strategy"></a>Wybieranie strategii bazy danych członkostwa

Po wybraniu udostępnianie bazy danych członkostwa w środowisku przedsiębiorstwa serwera, użyj poniższych wskazówek:

- Gdy jest to możliwe, nie należy wdrażać bazy danych członkostwa. Zamiast tego należy utworzyć bazy danych członkostwa ręcznie na docelowym serwerze bazy danych. Jeśli nie zostały dostosowane schemat bazy danych członkostwa, możesz po prostu utworzyć nowy na miejscu docelowym za pomocą [ASP.NET SQL Server Registration Tool (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Jeśli nie jest dostępna opcja, ale aby wdrożyć bazy danych członkostwa & #x 2014; na przykład, jeśli dokonano rozległych modyfikacji schematu bazy danych & #x 2014; należy wykonać wdrożenie tylko do schematu bazy danych członkostwa, aby wykluczyć danych konta użytkownika, a następnie należy uruchomić skrypt po wdrożeniu można dodać żadnych danych konfiguracji. Szerokie wskazówki można znaleźć w tych metod w [porady: Wdrażanie ASP.NET członkostwa bazy danych bez tym kont użytkowników](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Należy pamiętać, że *schemat bazy danych członkostwa jest może być dość statycznych*. Nawet wtedy, gdy został dostosowany bazie danych członkostwa, jest mało prawdopodobne, że konieczne będzie zaktualizowanie schematu w regularnych odstępach czasu & #x 2014; nie będzie można zmienić z taką samą częstotliwością jako kod w aplikacji sieci web lub projektu bazy danych. Tak nie należy dołączyć wszystkie procesy wdrażania automatycznego lub pojedynczy krok bazie danych członkostwa.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Aby zaktualizować schemat bazy danych członkostwa przy użyciu VSDBCMD

Jeśli zmodyfikujesz struktury bazy danych członkostwa po wdrożeniu pierwszej, możesz nie Internet Information Services (IIS) Narzędzie wdrażania Web (Web Deploy) umożliwia wdrożenie bazy danych. Funkcja wdrażania bazy danych w sieci Web wdrażanie nie zawiera możliwości nawiązać aktualizacje różnicowe do docelowej bazy danych & #x 2014; zamiast tego narzędzia Web Deploy porzucić i ponownie utworzyć bazę danych. Oznacza to, że utracić istniejących danych konta użytkownika, który jest zwykle niepożądanych w środowisku tymczasowym czy produkcyjnym.

Alternatywą jest użycie narzędzia VSDBCMD do aktualizacji schematu docelowej bazy danych. VSDBCMD zawiera dwie ważne funkcje. Najpierw należy go zaimportować schemat z istniejącej bazy danych do pliku .dbschema. Po drugie go wdrożyć pliku .dbschema istniejącą bazę danych jako aktualizacji różnicowych, co oznacza, że ułatwia tylko zmiany potrzebne do docelowej bazy danych na bieżąco, a nie utracić żadnych danych.

Następujące ogólne kroki służy do aktualizacji schematu bazy danych członkostwa:

1. Użyj VSDBCMD **importu** akcji, aby wygenerować plik .dbschema dla źródłowej bazy danych członkostwa. Ta procedura jest opisana w [porady: Importowanie schematu z wiersza polecenia](https://msdn.microsoft.com/library/dd172135.aspx).
2. Użyj VSDBCMD **Wdróż** akcji można wdrożyć pliku .dbschema do docelowej bazy danych członkostwa. Ta procedura jest opisana w [dotyczące wiersza polecenia dla VSDBCMD. EXE (wdrożenia i importowania schematu)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Wniosek

W tym temacie opisano niektóre wyzwania, które mogą się spodziewać po aprowizacji bazy danych członkostwa ASP.NET w różnych środowiskach docelowych. W szczególności wyjaśnić, dlaczego wdrożeń tylko schematu spowoduje pozostawienie bazy danych członkostwa w stanie niedziałającą i dlaczego wdrażanie dane konto użytkownika nie jest obsługiwany. Temat także przedstawione wskazówki na temat sposobu udostępniania, wdrażanie i zaktualizować bazy danych członkostwa w różnych scenariuszach.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej wskazówki i przykłady dotyczące używania VSDBCMD, zobacz [dotyczące wiersza polecenia dla VSDBCMD. EXE (wdrożenia i importowania schematu)](https://msdn.microsoft.com/library/dd193283.aspx) i [porady: Importowanie schematu z wiersza polecenia](https://msdn.microsoft.com/library/dd172135.aspx). Aby uzyskać więcej informacji na temat używania aspnet\_regsql.exe do tworzenia baz danych członkostwa, zobacz [ASP.NET SQL Server Registration Tool (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Zawiera ogólne wskazówki dotyczące wdrażania bazy danych członkostwa, zobacz [porady: Wdrażanie ASP.NET członkostwa bazy danych bez tym kont użytkowników](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

>[!div class="step-by-step"]
[Poprzednie](deploying-database-role-memberships-to-test-environments.md)
[dalej](excluding-files-and-folders-from-deployment.md)
