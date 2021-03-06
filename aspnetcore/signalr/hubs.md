---
title: Użyj koncentratory w ASP.NET Core SignalR
author: rachelappel
description: Dowiedz się, jak używać koncentratory w ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: d9e06c75692b68c4147b775e5eb77ef000578b2e
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/08/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a>Użyj koncentratory w SignalR platformy ASP.NET Core

Przez [Rachel Appel](https://twitter.com/rachelappel) i [Griffin Kevina](https://twitter.com/1kevgriff)

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

[Wyświetlić lub pobrać przykładowy kod](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(jak pobrać)](xref:tutorials/index#how-to-download-a-sample)

## <a name="what-is-a-signalr-hub"></a>Co to jest Centrum SignalR

API koncentratorów SignalR można wywoływać metod w połączonych klientów z serwera. W kodzie serwera należy zdefiniować metody, które są wywoływane przez klienta. W kodzie klienta można zdefiniować metody, które są wywoływane z serwera. SignalR odpowiada on za wszystko w tle, które umożliwia komunikacji klient serwer i klient serwera w czasie rzeczywistym.

## <a name="configure-signalr-hubs"></a>Konfigurowanie koncentratorów SignalR

Oprogramowanie pośredniczące SignalR wymaga niektórych usług, które są konfigurowane przez wywołanie metody `services.AddSignalR`.

[!code-csharp[Configure service](hubs/sample/startup.cs?range=37)]

Podczas dodawania funkcji SignalR dla aplikacji platformy ASP.NET Core, Konfiguracja trasy SignalR przez wywołanie metody `app.UseSignalR` w `Startup.Configure` metody.

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=56-59)]

## <a name="create-and-use-hubs"></a>Tworzenie i używanie koncentratory

Utwórz koncentratora od zadeklarowania klasy, która dziedziczy `Hub`i Dodaj do niej metody publiczne. Klientów można wywołać metody, które są zdefiniowane jako `public`.

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

Można określić zwracany typ i parametry, łącznie z typami złożonymi i tablic, tak jak w dowolnej metody C#. SignalR obsługi serializacji i deserializacji złożone obiekty i tablice parametrów i zwracanych wartości.

## <a name="the-clients-object"></a>Obiekt klientów

Każde wystąpienie `Hub` klasa ma właściwości o nazwie `Clients` zawiera następujące elementy członkowskie do komunikacji między serwerem a klientem:

| Właściwość | Opis |
| ------ | ----------- |
| `All` | Wywołuje metodę dla wszystkich połączonych klientów |
| `Caller` | Wywołuje metodę dla klienta, który wywołał metodę koncentratora |
| `Others` | Wywołuje metodę dla wszyscy połączeni klienci oprócz klienta, który wywołał metodę |

Ponadto `Hub` klasa zawiera następujące metody:

| Metoda | Opis |
| ------ | ----------- |
| `AllExcept` | Wywołuje metodę dla wszystkich połączonych klientów z wyjątkiem wskazanych połączeń |
| `Client` | Wywołuje metodę dla określonego połączony klient |
| `Clients` | Wywołuje metodę dla określonego połączonych klientów |
| `Group` | Wysyła komunikat do wszystkich połączeń w określonej grupie  |
| `GroupExcept` | Wysyła komunikat do wszystkich połączeń w określonej grupie, z wyjątkiem wskazanych połączeń |
| `Groups` | Wysyła komunikat do wielu grup połączeń  |
| `OthersInGroup` | Wysyła komunikat do grupy połączeń z wykluczeniem klienta, który wywołał metodę koncentratora  |
| `User` | Wysyła komunikat do wszystkich połączeń skojarzone z określonym użytkownikiem |
| `Users` | Wysyła komunikat do wszystkich połączeń skojarzone z określonych użytkowników |

Każda właściwość lub metoda w poprzednich tabelach zwraca obiekt z `SendAsync` metody. `SendAsync` Metoda umożliwia podanie nazwy i parametry metody klienta do wywołania.

## <a name="send-messages-to-clients"></a>Wysyłanie komunikatów do klientów

W celu wykonywania wywołań do określonych klientów, należy użyć właściwości `Clients` obiektu. W poniższym przykładzie `SendMessageToCaller` przedstawiono metody wysyłania komunikatu do połączenia, który wywołał metodę koncentratora. `SendMessageToGroups` Metoda wysyła komunikat do grupy przechowywane w `List` o nazwie `groups`.

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a>Obsługa zdarzeń dla połączenia

Interfejs API koncentratorów SignalR zawiera `OnConnectedAsync` i `OnDisconnectedAsync` metody wirtualne do zarządzania i śledzenia połączeń. Zastąpienie `OnConnectedAsync` metody wirtualnej do wykonania akcji, gdy klient nawiąże połączenie z koncentratorem, takie jak dodanie go do grupy.

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a>Obsługa błędów

Wyjątki zgłaszane w metodach koncentratora, z są wysyłane do klienta, który wywołał metodę. Na komputerze klienckim JavaScript `invoke` metoda zwraca [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises). Gdy klient odbierze błąd obsługi dołączony do promise przy użyciu `catch`, ma ona wywoływana i przekazywane jako JavaScript `Error` obiektu.

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a>Zasoby pokrewne

[Wprowadzenie do usługi SignalR platformy ASP.NET Core](xref:signalr/introduction)
