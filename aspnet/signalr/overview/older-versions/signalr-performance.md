---
uid: signalr/overview/older-versions/signalr-performance
title: "SignalR wydajności (SignalR 1.x) | Dokumentacja firmy Microsoft"
author: pfletcher
description: "SignalR wydajności"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2013
ms.topic: article
ms.assetid: 9594d644-66b6-4223-acdd-23e29a6e4c46
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/signalr-performance
msc.type: authoredcontent
ms.openlocfilehash: bad742af28d6c36bb1b66207c2ba09d140332449
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="signalr-performance-signalr-1x"></a>SignalR wydajności (SignalR 1.x)
====================
przez [Patrick Fletcher](https://github.com/pfletcher)

> W tym temacie opisano, jak projektować pod kątem pomiaru i zwiększyć wydajność w aplikacji SignalR.


Ostatnie prezentacji na SignalR wydajności i skalowania, zobacz [skalowanie sieci Web w czasie rzeczywistym za pomocą programu ASP.NET SignalR](https://channel9.msdn.com/Events/Build/2013/3-502).

Ten temat zawiera następujące sekcje:

- [Zagadnienia dotyczące projektowania](#design)
- [Dostrajanie wydajności serwera SignalR](#tuning)
- [Rozwiązywanie problemów z wydajnością](#troubleshooting)
- [Za pomocą liczników wydajności SignalR](#perfcounters)
- [Korzystanie z innych liczników wydajności](#othercounters)
- [Inne zasoby](#otherresources)

<a id="design"></a>

## <a name="design-considerations"></a>Zagadnienia dotyczące projektowania

W tej sekcji opisano wzorców, które można zastosować podczas projektowania aplikacji SignalR, aby upewnić się, że wydajność jest nie utrudniona, generując ruchu sieciowego niepotrzebne.

### <a name="throttling-message-frequency"></a>Ograniczanie częstotliwość komunikatów

Nawet w przypadku aplikacji, która wysyła komunikaty wysoka częstotliwość (na przykład w czasie rzeczywistym gry aplikacji) większość aplikacji, nie trzeba więcej niż kilka wiadomości na sekundę. Aby zmniejszyć ilość ruchu sieciowego, który generuje każdego klienta, można zaimplementować pętli komunikatów wiadomości nie częściej niż stały szybkość kolejek i wysyła się (to znaczy do określonej liczby wiadomości zostaną wysłane co sekundę w przypadku komunikatów w tym czasie w terval do wysłania). Przykładową aplikację, która ogranicza wiadomości do niektórych wartości (od klienta i serwera), zobacz [czasu rzeczywistego wysokiej częstotliwości z SignalR](../getting-started/tutorial-high-frequency-realtime-with-signalr.md).

### <a name="reducing-message-size"></a>Zmniejszenie rozmiaru wiadomości

Rozmiar obiektów serializowanych, można zmniejszyć rozmiar komunikatu SignalR. W kodzie serwera wysyłasz obiekt, który zawiera właściwości, które nie muszą być przekazywane, uniemożliwi te właściwości poddany serializacji przy użyciu `JsonIgnore` atrybutu. Nazwy właściwości są także przechowywane w komunikacie; nazwy właściwości może zostać skrócony przy użyciu `JsonProperty` atrybutu. W poniższym przykładzie kodu pokazano wykluczać właściwość zostały wysłane do klienta oraz skrócić nazwy właściwości:

**Kod serwera .NET, który demonstruje atrybutu JsonIgnore wykluczanie danych zostały wysłane do klienta, a atrybut JsonProperty, aby zmniejszyć rozmiar wiadomości**

[!code-csharp[Main](signalr-performance/samples/sample1.cs?highlight=5,7,10)]

Aby zachować czytelność / utrzymanie kodu klienta, nazwy skróconej właściwości mogą być ponownie zmapowany do personelu przyjaznych nazw po odebraniu komunikatu. W poniższym przykładzie kodu pokazano możliwe sposób ponownego mapowania skrócone nazwy do tych dłużej, definiując kontraktu komunikatu (mapowanie) i przy użyciu `reMap` funkcja dotyczyć kontrakt klasy zoptymalizowane wiadomości:

**Kod JavaScript po stronie klienta, który ponownie mapuje skrócony nazw właściwości do nazwy zrozumiałą dla użytkownika**

[!code-javascript[Main](signalr-performance/samples/sample2.js)]

Nazwy mogą skrócony w wiadomościach z klienta do serwera, jak również za pomocą tej samej metody.

Zmniejszenie zużycia pamięci (to znaczy ilość pamięci użytej wiadomości) komunikatu obiektu mogą także podnieść wydajność. Na przykład jeśli pełny zakres `int` nie jest potrzebny, `short` lub `byte` może być użyty.

Ponieważ komunikaty są przechowywane w magistrali komunikatów w pamięci serwera, zmniejszenie jego rozmiar wiadomości, można także rozwiązać problemy pamięci serwera.

<a id="tuning"></a>

### <a name="tuning-your-signalr-server-for-performance"></a>Dostrajanie wydajności serwera SignalR

Następujące ustawienia konfiguracji może służyć do dostrojenia serwera w celu poprawy wydajności w aplikacji SignalR. Aby uzyskać ogólne informacje na temat sposobu zwiększenia wydajności w aplikacji ASP.NET, zobacz [poprawę wydajności ASP.NET](https://msdn.microsoft.com/library/ff647787.aspx).

**Ustawienia konfiguracji SignalR**

- **DefaultMessageBufferSize**: domyślnie SignalR zachowuje 1000 komunikatów w pamięci dla koncentratora dla każdego połączenia. Jeśli są używane duże wiadomości, może to spowodować problemy z pamięcią, które mogą być złagodzone przez zmniejszenie tej wartości. To ustawienie można ustawić w `Application_Start` obsługi zdarzeń w aplikacji ASP.NET lub `Configuration` metody klasy początkowej OWIN w siebie aplikacji. W poniższym przykładzie pokazano sposób Zmniejsz tę wartość, aby zmniejszyć ilość pamięci zajmowaną przez aplikację, aby zmniejszyć ilość używanej pamięci serwera:

    **.NET server kod w pliku Global.asax zmniejszenie domyślny rozmiar buforu komunikatu**

    [!code-csharp[Main](signalr-performance/samples/sample3.cs)]

**Ustawienia konfiguracji usług IIS**

- **Maksymalna liczba jednoczesnych żądań na aplikację**: zwiększenie liczby równoczesnych usługi IIS żądań spowoduje zwiększenie zasobów serwera dostępne dla obsługi żądań. Wartość domyślna to 5000; Aby zwiększyć to ustawienie, uruchom następujące polecenia w wierszu polecenia z pełnymi uprawnieniami:

    [!code-console[Main](signalr-performance/samples/sample4.cmd)]

**Ustawienia konfiguracji programu ASP.NET**

Ta sekcja zawiera ustawienia konfiguracji, które można ustawić w `aspnet.config` pliku. Ten plik znajduje się w dwóch miejscach, w zależności od platformy:

- `%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet.config`
- `%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet.config`

Ustawienia programu ASP.NET, które może poprawić wydajność SignalR są następujące:

- **Maksymalna liczba równoczesnych żądań dla każdego procesora CPU**: zwiększenie tego ustawienia może zlikwidować wąskie gardła wydajności. Aby zwiększyć to ustawienie, Dodaj następujące ustawienie konfiguracji do `aspnet.config` pliku:

    [!code-xml[Main](signalr-performance/samples/sample5.xml?highlight=4)]
- **Ograniczenie kolejki żądań**: gdy przekracza całkowitą liczbę połączeń `maxConcurrentRequestsPerCPU` ustawienie ASP.NET rozpocznie ograniczania przy użyciu kolejki żądań. Aby zwiększyć rozmiar kolejki, można zwiększyć `requestQueueLimit` ustawienie. Aby to zrobić, Dodaj następujące ustawienie konfiguracji, aby `processModel` w węźle `config/machine.config` (zamiast `aspnet.config`):

    [!code-xml[Main](signalr-performance/samples/sample6.xml)]

<a id="troubleshooting"></a>

## <a name="troubleshooting-performance-issues"></a>Rozwiązywanie problemów z wydajnością

W tej sekcji opisano sposoby znajdowania wąskich gardeł zmniejszających wydajność aplikacji.

### <a name="verifying-that-websocket-is-being-used"></a>Weryfikowanie, że używany jest protokół WebSocket

Podczas SignalR można używać różnych transportu do komunikacji między klientem a serwerem, WebSocket zapewnia korzyści znaczących wydajności i powinien być używany, jeśli klient i serwer obsługuje. Aby określić, jeśli Twoje klienta i serwera spełniają wymagania protokołu WebSocket, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports). Aby ustalić, jakie transportu jest używany w aplikacji, można użyć narzędzi deweloperskich przeglądarki i sprawdź dzienniki, aby zobaczyć, jakie transportu jest używany przez połączenie. Aby uzyskać informacje na temat używania narzędzia programistyczne przeglądarki w programie Internet Explorer i Chrome, zobacz [transportu i przejścia](../getting-started/introduction-to-signalr.md#transports).

<a id="perfcounters"></a>

## <a name="using-signalr-performance-counters"></a>Za pomocą liczników wydajności SignalR

W tej sekcji opisano, jak włączanie i używanie liczniki wydajności SignalR, w `Microsoft.AspNet.SignalR.Utils` pakietu.

### <a name="installing-signalrexe"></a>Instalowanie signalr.exe

Na serwerze przy użyciu narzędzia, nazywany SignalR.exe można dodać liczniki analizy. Aby zainstalować to narzędzie, wykonaj następujące kroki:

1. W aplikacji Visual Studio, wybierz **narzędzia**, **Menedżer pakietów biblioteki**, **Zarządzaj pakietami NuGet rozwiązania...**
2. Wyszukaj **signalr.utils**i wybierz opcję instalacji.

    ![](signalr-performance/_static/image1.png)
3. Zaakceptuj Umowę licencyjną, aby zainstalować pakiet.
4. Zostanie zainstalowana SignalR.exe `<project folder>/packages/Microsoft.AspNet.SignalR.Utils.<version>/tools`.

### <a name="installing-performance-counters-with-signalrexe"></a>Instalowanie liczników wydajności z SignalR.exe

Aby zainstalować liczniki wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, z następującym parametrem:

[!code-console[Main](signalr-performance/samples/sample7.cmd)]

Aby usunąć liczniki wydajności SignalR, uruchom SignalR.exe w wierszu polecenia z podwyższonym poziomem uprawnień, z następującym parametrem:

[!code-console[Main](signalr-performance/samples/sample8.cmd)]

### <a name="signalr-performance-counters"></a>Liczniki wydajności SignalR

Pakiet utilites instaluje następujące liczniki wydajności. "Całkowita" liczniki mierzenia liczby zdarzeń od czasu ostatniego puli aplikacji lub serwera ponownego uruchomienia.

**Metryki połączenia**

Następujące metryki pomiaru połączenia okres istnienia zdarzeń. Aby uzyskać więcej informacji, zobacz [zrozumienia i obsługi zdarzeń okres istnienia połączenia](../guide-to-the-api/handling-connection-lifetime-events.md).

- **Połączeń**
- **Ponowne łączenie zakończone połączeń**
- **Disonnected połączenia**
- **Bieżące połączenia**

**Metryki wiadomości**

Następujące metryki pomiaru ruch wiadomości generowany przez SignalR.

- **Odebrane komunikaty połączenia danych całkowitych**
- **Całkowita liczba wysyłane komunikaty połączenia**
- **Komunikaty połączeń odebrane/s**
- **Połączenie komunikaty wysłane/s**

**Metryki magistrali komunikatów**

Następujące metryki pomiaru ruchu za pośrednictwem wewnętrznego magistrali komunikatu SignalR, kolejki, w którym znajdują się wszystkie komunikaty przychodzące i wychodzące SignalR. Komunikat jest **opublikowano** po jest wysyłana lub emisji. A **subskrybenta** w tym kontekście jest subskrypcji na magistrali komunikatu; ta powinna być równa liczbę klientów oraz sam serwer. **Przydzielone procesu roboczego** jest składnikiem, który wysyła dane do aktywnych połączeń; **zajęty procesu roboczego** to taki, który jest aktywnie wysłanie wiadomości.

- **Całkowita liczba odebranych komunikatów magistrali komunikatów**
- **Magistrala komunikatów wiadomości odebrane/s**
- **Komunikatów magistrali komunikatów publikowanych danych całkowitych**
- **Magistrala komunikatu komunikaty opublikowane na sekundę**
- **Bieżąca subskrybentów magistrali komunikatów**
- **Łączna liczba subskrybentów magistrali komunikatów**
- **Subskrybentów magistrali komunikatów na sekundę**
- **Magistrala komunikatów przydzielonych pracowników**
- **Zajętych pracowników magistrali komunikatów**
- **Bieżąca tematy magistrali komunikatów**

**Błąd metryk**

Następujące metryki pomiaru błędy generowane przez ruch komunikatów SignalR. **Rozpoznawania koncentratora** błędy występują, gdy nie można ustalić koncentratora lub metody koncentratora. **Wywołania koncentratora** błędy są wyjątki zgłaszane podczas wywoływania metody koncentratora. **Transport** wyjątek podczas żądania lub odpowiedzi HTTP błędów połączeń są błędy.

- **Błędów: Suma wszystkich**
- **Błędy: All/s**
- **Błędy: Całkowita liczba rozpoznawania koncentratora**
- **Błędy: Rozpoznawania koncentratora na sekundę**
- **Błędy: Całkowita liczba wywołania koncentratora**
- **Błędy: Wywołania koncentratora na sekundę**
- **Błędy: Całkowita liczba transportu**
- **Błędy: Transportu na sekundę**

**Metryki skalowania**

Następujące metryki pomiaru ruchu i błędów wygenerowanych przez dostawcę skalowania w poziomie. A **strumienia** w tym kontekście jest jednostką skalowania jest używany przez dostawcę skalowania, co jest tabeli, jeśli jest używany program SQL Server, tematu, jeśli jest używana usługa Service Bus i subskrypcji, użycie pamięci podręcznej Redis. Domyślnie jest używane tylko jednego strumienia, ale to można zwiększyć za pomocą konfiguracji na serwerze SQL i usługi Service Bus. A **buforowanie** strumienia, który przeszedł w stan; gdy strumień jest w stanie błędnej, wszystkie komunikaty wysyłane na płytę montażową zakończy się niepowodzeniem aż do strumienia nie spowodował błąd. **Długość kolejki wysyłania** jest liczba komunikatów, które zostały przesłane, ale nie zostały jeszcze wysłane.

- **Magistrali komunikatów skalowania wiadomości odebrane/s**
- **Całkowita liczba strumieni skalowania w poziomie**
- **Otwórz strumieni skalowania w poziomie**
- **Strumienie skalowania buforowania**
- **Całkowita liczba błędów skalowania w poziomie**
- **Błędów skalowania na sekundę**
- **Długość kolejki wysyłania skalowania w poziomie**

Aby uzyskać więcej informacji na jakie te liczniki są pomiaru, zobacz [skalowania SignalR z usługi Azure Service Bus](scaleout-with-windows-azure-service-bus.md).

<a id="othercounters"></a>

## <a name="using-other-performance-counters"></a>Korzystanie z innych liczników wydajności

Następujące liczniki wydajności mogą być również przydatne w ramach monitorowania wydajności aplikacji.

**Pamięci**

- .NET CLR pamięci bajtów we wszystkich sterty (dla w3wp)

**ASP.NET**

- Bieżąca ASP.NET\Requests
- ASP.NET\Queued
- ASP.NET\Rejected

**CPU**

- Czas procesora Information\Processor

**TCP/IP**

- TCPv6/połączeń
- TCPv4/połączeń

**Usługi sieci Web**

- Połączenia Service\Current sieci Web
- Połączenia Service\Maximum sieci Web

**Wątkowość**

- .NET CLR LocksAndThreads\# aktualna liczba wątków logicznych
- .NET CLR LocksAnd wątków\# aktualna liczba wątków fizycznych

<a id="otherresources"></a>

## <a name="other-resources"></a>Inne zasoby

Aby uzyskać więcej informacji na temat wydajności programu ASP.NET, monitorowania i dostrajania, zobacz następujące tematy:

- [Omówienie wydajności programu ASP.NET](https://msdn.microsoft.com/library/cc668225(v=vs.100).aspx)
- [Użycie wątku ASP.NET usług IIS 7.5, usługi IIS 7.0 i IIS 6.0](https://blogs.msdn.com/b/tmarq/archive/2007/07/21/asp-net-thread-usage-on-iis-7-0-and-6-0.aspx)
- [&lt;applicationPool&gt; elementu (ustawienia sieci Web)](https://msdn.microsoft.com/library/dd560842.aspx)
