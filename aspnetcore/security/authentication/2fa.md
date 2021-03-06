---
title: "Uwierzytelnianie dwuskładnikowe z programem SMS"
author: rick-anderson
description: "Pokazuje, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) z platformy ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 08/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/2fa
ms.openlocfilehash: 7bca1c6249bebe84b532b652ab736186f35c50ee
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="two-factor-authentication-with-sms"></a>Uwierzytelnianie dwuskładnikowe z programem SMS

Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Devs Szwajcaria](https://github.com/Swiss-Devs)

Ten samouczek dotyczy platformy ASP.NET Core 1.x tylko. Zobacz [generowania kodu QR włączania uwierzytelniania aplikacji w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.

Ten samouczek pokazuje, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS. Instrukcje są podane dla [usługi twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS. Firma Microsoft zaleca, należy wykonać [potwierdzenie konta i hasła odzyskiwania](accconfirm.md) przed rozpoczęciem tego samouczka.

Widok [ukończone próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA). [Jak pobrać](xref:tutorials/index#how-to-download-a-sample).

## <a name="create-a-new-aspnet-core-project"></a>Utwórz nowy projekt platformy ASP.NET Core

Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` z indywidualnych kont użytkowników. Postępuj zgodnie z instrukcjami [Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core](xref:security/enforcing-ssl) do ustawiania i wymagać protokołu SSL.

### <a name="create-an-sms-account"></a>Tworzenie konta usługi programu SMS

Tworzenie konta usługi programu SMS, na przykład z [usługi twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/). Zapisz poświadczenia uwierzytelniania (dla usługi twilio: accountSid i parametr authToken dla ASPSMS: Userkey i hasło).

#### <a name="figuring-out-sms-provider-credentials"></a>Ustaleniem poświadczenia dostawcy programu SMS

**Usługi Twilio:**  
Z konta usługi Twilio, na karcie pulpitu nawigacyjnego, skopiuj **identyfikator SID konta** i **token uwierzytelniania**.

**ASPSMS:**  
W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.

Przechowujemy później te wartości przy użyciu narzędzia menedżera klucza tajnego w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.

#### <a name="specifying-senderid--originator"></a>Określanie SenderID / inicjator

**Usługi Twilio:**  
Na karcie liczb, skopiować z usługi Twilio **numer telefonu**. 

**ASPSMS:**  
W Menu nadawcy odblokować Odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci). 

Przechowujemy później tę wartość za pomocą narzędzia menedżera klucza tajnego w kluczu `SMSAccountFrom`.


### <a name="provide-credentials-for-the-sms-service"></a>Podaj poświadczenia dla usługi programu SMS

Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostęp do konta i klucz ustawień użytkownika. 

   * Utwórz klasę, aby pobrać klucz zabezpieczeń programu SMS. Dla tego przykładu `SMSoptions` klasy jest tworzony w *Services/SMSoptions.cs* pliku.

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets). Na przykład:

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* Dodaj pakiet NuGet dostawcy programu SMS. Z pakietu Menedżera konsoli (PMC) uruchom:

**Usługi Twilio:**  
`Install-Package Twilio`

**ASPSMS:**  
`Install-Package ASPSMS`


* Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS. Przy użyciu usługi Twilio lub sekcji ASPSMS:


**Usługi Twilio:**  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

**ASPSMS:**  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a>Konfigurowanie uruchamiania do użycia`SMSoptions`

Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` metody w *Startup.cs*:

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a>Włącz uwierzytelnianie dwuskładnikowe

Otwórz *Views/Manage/Index.cshtml* pliku widoku Razor i Usuń komentarz znaków (taki sposób, nie ujęty w komentarzu).

## <a name="log-in-with-two-factor-authentication"></a>Zaloguj się za pomocą uwierzytelniania dwuskładnikowego

* Uruchom aplikację i zarejestrować nowego użytkownika

![Otwórz w programie Microsoft Edge widok zarejestrować aplikacji sieci Web](2fa/_static/login2fa1.png)

* Wybierz nazwę użytkownika, który uaktywnia `Index` metody akcji w kontrolerze zarządzanie. Następnie wybierz numer telefonu **Dodaj** łącza.

![Zarządzanie widoku](2fa/_static/login2fa2.png)

* Numer telefonu, który zostanie wyświetlony kod weryfikacyjny i naciśnij pozycję Dodaj **wysłać kod weryfikacyjny**.

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* Otrzymasz wiadomość SMS z kodem weryfikacyjnym. Wprowadź go i wybierz **przesyłania**

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.

* Zarządzaj ilustracja pokazuje numer telefonu został pomyślnie dodany.

![Zarządzanie widoku](2fa/_static/login2fa5.png)

* Wybierz **włączyć** do włączenia uwierzytelniania dwuskładnikowego.

![Zarządzanie widoku](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a>Uwierzytelnianie dwuskładnikowe testu

* Wyloguj.

* Zaloguj się.

* Konto użytkownika ma włączony uwierzytelniania dwuskładnikowego, dlatego należy podać drugiego etapu uwierzytelniania. W tym samouczku włączono weryfikację telefoniczną. Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego etapu. Można skonfigurować dodatkowe czynniki drugi do uwierzytelniania, takich jak kody QR. Wybierz **przesłać**.

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* Wprowadź kod w wiadomości SMS.

* Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z 2FA się zalogować, korzystając z tego samego urządzenia i przeglądarki. Włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia 2FA silnej ochrony przed złośliwymi użytkownikami próby dostęp do tego konta, dopóki nie mają dostępu do urządzenia. Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane. Przez ustawienie **Pamiętaj przeglądarka**, Pobierz zwiększa bezpieczeństwo 2FA z urządzeń, które nie są regularnie używane i pobrać wygody na ominięcie za pośrednictwem 2FA na własnych urządzeniach.

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a>Blokada konta do ochrony przed atakami siłowymi

Firma Microsoft zaleca się, że używasz blokady konta z 2FA. Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowych), każdy nieudane próby 2FA są przechowywane i po osiągnięciu maksymalnej liczby prób (wartość domyślna to 5), użytkownik jest zablokowany przez pięć minut (można ustawić blokady czas z `DefaultAccountLockoutTimeSpan`). Następujące konfiguruje konto zostało zablokowane na 10 minut po 10 nieudanych prób.

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
