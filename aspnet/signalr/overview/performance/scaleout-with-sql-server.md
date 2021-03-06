---
uid: signalr/overview/performance/scaleout-with-sql-server
title: Skalowania SignalR z programem SQL Server | Dokumentacja firmy Microsoft
author: MikeWasson
description: Wersje oprogramowania używane w tym temacie Visual Studio 2013 .NET 4.5 SignalR w wersji 2 poprzednie wersje tego tematu informacji o wcześniejszych wersji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 98358b6e-9139-4239-ba3a-2d7dd74dd664
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/scaleout-with-sql-server
msc.type: authoredcontent
ms.openlocfilehash: b3189c36fc076333c0c6007bd039b12e03d63bc8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="signalr-scaleout-with-sql-server"></a>Skalowania SignalR z programem SQL Server
====================
przez [Wasson Jan](https://github.com/MikeWasson), [Patrick Fletcher](https://github.com/pfletcher)

> ## <a name="software-versions-used-in-this-topic"></a>Wersje oprogramowania używane w tym temacie
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR w wersji 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Poprzednie wersje tego tematu
> 
> Aby uzyskać informacje dotyczące starszych wersji biblioteki signalr, zobacz [starsze wersje biblioteki SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Pytania i komentarze
> 
> Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony. Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).


W tym samouczku użyjesz programu SQL Server do dystrybucji wiadomości w aplikacji SignalR, które zostało wdrożone w dwóch osobnych wystąpień usług IIS. W tym samouczku można również uruchomić na maszynie pojedynczy test, ale uzyskanie pełnego efektu, należy wdrożyć co najmniej dwa serwery aplikacji SignalR. SQL Server należy również zainstalować na jednym serwerze lub na osobnym serwerze dedykowanym. Innym rozwiązaniem jest Uruchamianie samouczka przy użyciu maszyn wirtualnych na platformie Azure.

![](scaleout-with-sql-server/_static/image1.png)

## <a name="prerequisites"></a>Wymagania wstępne

Microsoft SQL Server 2005 lub nowszego. Systemu backplane obsługuje zarówno stacjonarnych i serwerów wersje programu SQL Server. Nie obsługuje programu SQL Server Compact Edition lub baza danych SQL Azure. (Jeśli aplikacja jest hostowana na platformie Azure, należy wziąć pod uwagę płyty montażowej usługi Service Bus zamiast.)

## <a name="overview"></a>Omówienie

Przed uzyskujemy do szczegółowy samouczek, w tym miejscu jest to szybki przegląd będzie wykonywać.

1. Tworzenie nowej pustej bazy danych. Systemu backplane utworzy potrzebne tabele w tej bazie danych.
2. Dodaj te pakiety NuGet do aplikacji: 

    - [Microsoft.AspNet.SignalR](http://nuget.org/packages/Microsoft.AspNet.SignalR)
    - [Microsoft.AspNet.SignalR.SqlServer](http://nuget.org/packages/Microsoft.AspNet.SignalR.SqlServer)
3. Tworzenie aplikacji SignalR.
4. Dodaj następujący kod do pliku Startup.cs do skonfigurowania systemu backplane: 

    [!code-csharp[Main](scaleout-with-sql-server/samples/sample1.cs)]

   Ten kod konfiguruje systemu backplane z wartościami domyślnymi dla [TableCount](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.sqlscaleoutconfiguration.tablecount(v=vs.118).aspx) i [MaxQueueLength](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.scaleoutconfiguration.maxqueuelength(v=vs.118).aspx). Aby uzyskać informacje na temat zmiany tych wartości, zobacz [wydajności SignalR: metryki skalowania](signalr-performance.md#scaleout_metrics). 

## <a name="configure-the-database"></a>Skonfiguruj bazę danych

Zdecyduj, czy aplikacja będzie używać uwierzytelniania systemu Windows lub uwierzytelniania programu SQL Server dostęp do bazy danych. Niezależnie od tego upewnij się, że użytkownik bazy danych ma uprawnienia do logowania, Utwórz schematy i tworzenie tabel.

Utwórz nową bazę danych do płyty montażowej do użycia. Bazy danych można nadać dowolną nazwę. Nie trzeba tworzyć żadnych tabel w bazie danych. systemu backplane utworzy potrzebne tabele.

![](scaleout-with-sql-server/_static/image2.png)

## <a name="enable-service-broker"></a>Włącz brokera usług

Zalecane jest, aby włączyć brokera usługi dla bazy danych systemu backplane. Usługa Service Broker zapewnia macierzystą obsługę dla obsługi wiadomości i kolejkowania w programie SQL Server, co pozwala montażowa wydajniej otrzymywać aktualizacje. (Jednak systemu backplane również działa bez programu Service Broker.)

Aby sprawdzić, czy Usługa Service Broker jest włączona, zapytanie **jest\_brokera\_włączone** kolumny w **sys.databases** widoku katalogu.

[!code-sql[Main](scaleout-with-sql-server/samples/sample2.sql)]

![](scaleout-with-sql-server/_static/image3.png)

Aby włączyć brokera usługi, użyj następującej kwerendy SQL:

[!code-sql[Main](scaleout-with-sql-server/samples/sample3.sql)]

> [!NOTE]
> Jeśli to zapytanie wydaje się do zakleszczenia, upewnij się, że nie ma żadnych aplikacji, połączony z bazą danych.


Jeśli śledzenie jest włączone, dane śledzenia zostaną wyświetlone również czy Service Broker jest włączona.

## <a name="create-a-signalr-application"></a>Tworzenie aplikacji SignalR

Utwórz aplikację SignalR, wykonując jedną z tych samouczki:

- [Wprowadzenie do korzystania z SignalR 2.0](../getting-started/tutorial-getting-started-with-signalr.md)
- [Wprowadzenie do korzystania z SignalR 2.0 i MVC 5](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)

Firma Microsoft będzie następnie zmodyfikować rozmów aplikację do obsługi skalowania w poziomie z programem SQL Server. Najpierw Dodaj pakiet SignalR.SqlServer NuGet do projektu. W programie Visual Studio z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**. W oknie Konsola Menedżera pakietów wprowadź następujące polecenie:

[!code-powershell[Main](scaleout-with-sql-server/samples/sample4.ps1)]

Następnie otwórz folder pliku Startup.cs. Dodaj następujący kod do **Konfiguruj** metody:

[!code-csharp[Main](scaleout-with-sql-server/samples/sample5.cs)]

## <a name="deploy-and-run-the-application"></a>Wdrażanie i uruchamianie aplikacji

Przygotuj swoich wystąpień systemu Windows Server, aby wdrożyć aplikację SignalR.

Dodaj rolę usług IIS. Obejmują funkcje "Programowanie aplikacji", takie jak protokół WebSocket.

![](scaleout-with-sql-server/_static/image4.png)

Obejmuje to też usługi zarządzania (wymienione w obszarze "Narzędzia do zarządzania").

![](scaleout-with-sql-server/_static/image5.png)

**Zainstaluj narzędzie Web Deploy 3.0.** Uruchom Menedżera usług IIS, zostanie wyświetlony monit do zainstalowania platformy Microsoft Web lub można [Pobierz intstaller](https://go.microsoft.com/fwlink/?LinkId=255386). W Instalatorze platformy wyszukiwanie narzędzia Web Deploy i instalowanie usługi sieci Web Deploy 3.0

![](scaleout-with-sql-server/_static/image6.png)

Sprawdź, czy jest uruchomiona usługa zarządzania siecią Web. Jeśli nie, należy uruchomić usługę. (Usługa zarządzania siecią Web nie jest widoczny na liście usług systemu Windows, aby się, że po dodaniu roli usług IIS są zainstalowane usługi zarządzania.)

Na koniec Otwórz port 8172 dla protokołu TCP. Jest to port, który używa narzędzia Web Deploy.

Teraz można przystąpić do wdrażania projektu programu Visual Studio z komputerze deweloperskim z serwerem. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy rozwiązanie, a następnie kliknij przycisk **publikowania**.

Aby uzyskać bardziej szczegółowe dokumentacji dotyczące wdrożenia sieci web, zobacz [Mapa zawartości wdrożenia sieci Web dla platformy ASP.NET i Visual Studio](../../../whitepapers/aspnet-web-deployment-content-map.md).

W przypadku wdrożenia aplikacji na dwóch serwerach, można otworzyć każde wystąpienie w osobnym oknie przeglądarki i zobacz, każdy z nich odbierać wiadomości SignalR z innych. (Oczywiście w środowisku produkcyjnym, dwa serwery będą sit za modułem równoważenia obciążenia.)

![](scaleout-with-sql-server/_static/image7.png)

Po uruchomieniu aplikacji widać, że SignalR automatycznie utworzył tabele w bazie danych:

![](scaleout-with-sql-server/_static/image8.png)

SignalR zarządza tabel. Tak długo, jak aplikacja jest wdrażana, nie usuwać wiersze, zmodyfikuj tabelę i tak dalej.
