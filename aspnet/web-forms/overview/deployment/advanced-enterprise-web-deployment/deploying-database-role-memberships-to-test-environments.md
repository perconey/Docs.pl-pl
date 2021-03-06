---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "Wdrażanie członkostwo roli bazy danych do środowiska testowego | Dokumentacja firmy Microsoft"
author: jrjlee
description: "W tym temacie opisano sposób dodawania kont użytkowników do ról bazy danych jako część wdrożenia rozwiązania do środowiska testowego. Podczas wdrażania zawierający rozwiązania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 226c28622f76e866fba1fc33cf9b9b7a01e5295b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Wdrażanie członkostwo roli bazy danych do środowisk testowych
====================
przez [Lewandowski Jason](https://github.com/jrjlee)

[Pobierz plik PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> W tym temacie opisano sposób dodawania kont użytkowników do ról bazy danych jako część wdrożenia rozwiązania do środowiska testowego.
> 
> Wdrażając rozwiązanie zawierające projekt bazy danych w środowisku tymczasowym czy produkcyjnym, zwykle nie mają deweloperom automatyzację dodawania kont użytkowników do ról bazy danych. W większości przypadków projektanta nie będzie wiadomo, które konta użytkowników, należy dodać do ról bazy danych, a tych wymagań można zmienić w dowolnym momencie. Jednak wdrożyć rozwiązanie zawierające projekt bazy danych na komputerze projektowym lub testowym, sytuacja jest zazwyczaj zamiast różny:
> 
> - Deweloper zwykle ponownie wdraża rozwiązanie na bieżąco, często kilka razy dziennie.
> - Bazy danych ponownie jest zwykle tworzony przy każdym wdrożeniu, co oznacza, że użytkownicy bazy danych musi być utworzony i dodać do ról po każdym wdrożeniu.
> - Deweloper zwykle ma pełną kontrolę nad środowiska docelowego deweloperskich lub testowania.
> 
> W tym scenariuszu często jest przydatne do automatycznego tworzenia bazy danych użytkowników i przypisać członkostwo roli bazy danych jako część procesu wdrażania.
> 
> Kluczowym czynnikiem jest, że ta operacja musi być warunkowego oparte na środowisku docelowym. Jeśli wdrażasz przemieszczania lub w środowisku produkcyjnym, chcesz pominąć operację. Jeśli jest wdrażany do dewelopera lub środowiska testowego, którą chcesz wdrożyć członkostwo w rolach bez dalszej interwencji. W tym temacie opisano jeden podejście, które można użyć, aby rozwiązać ten problem.


Ten temat jest częścią serii samouczków na podstawie tych wymagań związanych z przedsiębiorstwa wdrażaniem fikcyjnej firmy o nazwie firmy Fabrikam, Inc. Ten samouczek serii używa przykładowe rozwiązanie & #x 2014; [rozwiązania z menedżerem skontaktuj się z](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; do reprezentowania aplikacji sieci web z realistyczne poziom złożoności, w tym aplikacji ASP.NET MVC 3, systemu Windows Usługi Communication Foundation (WCF), a projekt bazy danych.

Istotą te samouczki metody wdrażania opiera się na podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), w którym jest kontrolowany przez proces kompilacji projektu dwa pliki & #x 2014; jeden zawierający Tworzenie instrukcji, które mają zastosowanie do każdego środowiska docelowego i dysk zawierający ustawienia kompilacji i wdrożenia określonego środowiska. W czasie kompilacji pliku projektu określonego środowiska jest scalany pliku projektu niezależny od środowiska pełny zestaw instrukcji kompilacji.

## <a name="task-overview"></a>Omówienie zadań

W tym temacie założono, że:

- Użyj podziału podejścia pliku projektu wdrażania rozwiązania, zgodnie z opisem w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Wywołanie VSDBCMD z pliku projektu, aby wdrożyć projekt bazy danych, zgodnie z opisem w [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Aby utworzyć bazy danych użytkowników i przypisać członkostwo w rolach podczas wdrażania projektu bazy danych do środowiska testowego, musisz:

- Utwórz skrypt Transact Structured Query Language (Transact-SQL), który zmienia niezbędne bazy danych.
- Utwórz docelowy Microsoft kompilacji Engine (MSBuild), który używa narzędzia sqlcmd.exe do uruchomienia skryptu SQL.
- Skonfiguruj plików projektu, aby wywołać element docelowy podczas wdrażania rozwiązania do środowiska testowego.

W tym temacie opisano sposób wykonywania każdego z tych procedur.

## <a name="scripting-the-database-role-memberships"></a>Wykonywanie skryptów członkostwo roli bazy danych

Można utworzyć skrypt języka Transact-SQL w wiele różnych sposobów, a w dowolnym miejscu możesz wybrać. Najprostszym podejście jest tworzenie skryptu w ramach rozwiązania w programie Visual Studio 2010.

**Aby utworzyć skrypt SQL**

1. W **Eksploratora rozwiązań** okna, rozwiń węzeł projektu bazy danych.
2. Kliknij prawym przyciskiem myszy **skryptów** folderu, wskaż **Dodaj**, a następnie kliknij przycisk **nowy Folder**.
3. Typ **testu** jako nazwa folderu, a następnie naciśnij klawisz Enter.
4. Kliknij prawym przyciskiem myszy **testu** folderu, wskaż **Dodaj**, a następnie kliknij przycisk **skryptu**.
5. W **Dodaj nowy element** okna dialogowego należy podać nazwę opisową skryptu (na przykład **AddRoleMemberships.sql**), a następnie kliknij przycisk **Dodaj**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. W *AddRoleMemberships.sql* plików, Dodawanie instrukcji języka Transact-SQL który:

    1. Utwórz użytkownika bazy danych dla nazwy logowania programu SQL Server, które będą uzyskiwać dostęp do bazy danych.
    2. Dodaj użytkownika bazy danych do żadnych ról wymaganej bazy danych.
7. Plik powinien wyglądać następująco:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Zapisz plik.

## <a name="executing-the-script-on-the-target-database"></a>Wykonanie skryptu na docelowej bazy danych

W idealnym przypadku należy uruchomić wszystkie wymagane skrypty języka Transact-SQL w ramach skrypt po wdrożeniu podczas wdrażania projektu bazy danych. Jednak skrypty powdrożeniowe nie pozwalają na wykonywanie logiki warunkowo na podstawie konfiguracji rozwiązania lub właściwości kompilacji. Alternatywą jest uruchomienie skrypty SQL bezpośrednio z pliku projektu MSBuild, tworząc **docelowej** element, który wykonuje polecenia sqlcmd.exe. To polecenie służy do uruchamiania skryptu na docelowa baza danych:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Aby uzyskać więcej informacji na temat opcji wiersza polecenia sqlcmd, zobacz [narzędzia sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Zanim to polecenie jest osadzić w celu MSBuild, należy wziąć pod uwagę pod jakimi warunkami ma uruchomienie skryptu:

- Docelowa baza danych musi istnieć przed zmianą jego przynależności do ról. Tak, należy uruchomić ten skrypt *po* wdrażania bazy danych.
- Należy uwzględnić warunek, dzięki czemu skrypt jest wykonywać tylko dla środowisk testowych.
- Jeśli korzystasz z wdrożenia "co w przypadku" & #x 2014; innymi słowy, jeśli jest generowanie skryptów wdrażania, ale nie są na nich uruchomione ich & #x 2014; nie należy uruchomieniem skryptu SQL.

Jeśli używasz podejście pliku projektu podziału opisane w [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md), jak dowodzą kontaktów Menedżerze przykładowe rozwiązanie, można podzielić instrukcje kompilacji skryptu SQL następująco:

- Wszystkie wymagane właściwości specyficzne dla środowiska, razem z właściwością, która określa, czy do wdrażania uprawnienia, powinien znajdować się w pliku projektu określonego środowiska (na przykład *Env Dev.proj*).
- Element docelowy programu MSBuild, oraz wszystkie właściwości, które nie zmieni się między środowiskami docelowy powinien znajdować się w pliku projektu uniwersalnej (na przykład *Publish.proj*).

W pliku projektu określonego środowiska należy zdefiniować nazwę serwera bazy danych, nazwa docelowej bazy danych i właściwości typu Boolean, który umożliwia użytkownikowi określenie, czy wdrożyć przynależności do ról.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


W pliku projektu uniwersalnej należy podać lokalizację pliku wykonywalnego sqlcmd i lokalizację skryptu SQL, który chcesz uruchomić. Te właściwości jest taka sama niezależnie od środowiska docelowego. Należy również utworzyć obiekt docelowy programu MSBuild, można wykonać polecenia sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Zwróć uwagę, Dodaj lokalizację pliku wykonywalnego sqlcmd właściwość statyczna, ponieważ może to być przydatne do innych celów. Z kolei zdefiniujesz lokalizacji skryptu SQL i składnia polecenia sqlcmd jako właściwości dynamicznych w docelowym, jak nie będą wymagane przed wykonaniem obiektu docelowego. W takim przypadku **DeployTestDBPermissions** docelowej będą wykonywane tylko, jeśli te warunki są spełnione:

- **DeployTestDBRoleMemberships** właściwość jest ustawiona na **true**.
- Użytkownik nie określił **WhatIf = true** flagi.

Na koniec nie zapomnij o Wywołaj element docelowy. W *Publish.proj* plików, można to zrobić przez dodanie docelowego na liście zależności domyślnie **FullPublish** docelowej. Należy upewnić się, że **DeployTestDBPermissions** docelowy nie jest wykonywany do momentu **PublishDbPackages** docelowej zostało wykonane.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Wniosek

W tym temacie opisano jeden sposób, w którym można dodać bazy danych użytkowników i ról jako akcję po wdrożeniu podczas wdrażania projektu bazy danych. Jest to zazwyczaj przydatne, gdy regularnie ponownie utworzyć bazę danych w środowisku testowym, ale jego zwykle należy unikać podczas wdrażania bazy danych dla środowisk przemieszczania i produkcji. Tak należy upewnić się, użyj niezbędne logikę warunkową, tak, aby użytkownicy bazy danych i członkostwo w rolach są tworzone tylko gdy jest to zrobić.

## <a name="further-reading"></a>Dalsze informacje

Aby uzyskać więcej informacji na temat używania VSDBCMD wdrażania projektów bazy danych, zobacz [wdrażania projektów bazy danych](../web-deployment-in-the-enterprise/deploying-database-projects.md). Aby uzyskać wskazówki dotyczące dostosowywania wdrożenia bazy danych dla środowisk inny element docelowy, zobacz [Dostosowywanie wdrożenia bazy danych w wielu środowiskach](customizing-database-deployments-for-multiple-environments.md). Aby uzyskać więcej informacji na temat używania niestandardowe pliki projektu MSBuild kontrolować proces wdrażania, zobacz [opis pliku projektu](../web-deployment-in-the-enterprise/understanding-the-project-file.md) i [opis procesu kompilacji](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Aby uzyskać więcej informacji na temat opcji wiersza polecenia sqlcmd, zobacz [narzędzia sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

>[!div class="step-by-step"]
[Poprzednie](customizing-database-deployments-for-multiple-environments.md)
[dalej](deploying-membership-databases-to-enterprise-environments.md)
