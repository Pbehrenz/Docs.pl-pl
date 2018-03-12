---
uid: signalr/overview/releases/upgrading-signalr-1x-projects-to-20
title: "Uaktualnianie projektów 1.x SignalR do wersji 2 | Dokumentacja firmy Microsoft"
author: pfletcher
description: "W tym temacie opisano sposób uaktualniania istniejącego projektu 1.x SignalR do SignalR 2.x i jak rozwiązywać problemy, które mogą wystąpić podczas procesu uaktualniania..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: adcfef99-9bc5-489d-a91b-9b7c2bc35e04
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/releases/upgrading-signalr-1x-projects-to-20
msc.type: authoredcontent
ms.openlocfilehash: e372275ae5dd4bbf354db2d02e4407f8c513b7a3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-signalr-1x-projects-to-version-2"></a><span data-ttu-id="7641e-103">Uaktualnianie projektów 1.x SignalR do wersji 2</span><span class="sxs-lookup"><span data-stu-id="7641e-103">Upgrading SignalR 1.x Projects to version 2</span></span>
====================
<span data-ttu-id="7641e-104">przez [Patrick Fletcher](https://github.com/pfletcher)</span><span class="sxs-lookup"><span data-stu-id="7641e-104">by [Patrick Fletcher](https://github.com/pfletcher)</span></span>

> <span data-ttu-id="7641e-105">W tym temacie opisano sposób uaktualniania istniejącego projektu 1.x SignalR do SignalR 2.x i jak rozwiązywać problemy, które mogą wystąpić podczas procesu uaktualniania.</span><span class="sxs-lookup"><span data-stu-id="7641e-105">This topic describes how to upgrade an existing SignalR 1.x project to SignalR 2.x, and how to troubleshoot issues that may arise during the upgrade process.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="7641e-106">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="7641e-106">Software versions used in the tutorial</span></span>
> 
> 
> - [<span data-ttu-id="7641e-107">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7641e-107">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - <span data-ttu-id="7641e-108">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="7641e-108">.NET 4.5</span></span>
> - <span data-ttu-id="7641e-109">SignalR w wersji 1 i 2</span><span class="sxs-lookup"><span data-stu-id="7641e-109">SignalR versions 1 and 2</span></span>
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a><span data-ttu-id="7641e-110">Z tego samouczka przy użyciu programu Visual Studio 2012</span><span class="sxs-lookup"><span data-stu-id="7641e-110">Using Visual Studio 2012 with this tutorial</span></span>
> 
> 
> <span data-ttu-id="7641e-111">Aby używać programu Visual Studio 2012 z tego samouczka, wykonaj następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="7641e-111">To use Visual Studio 2012 with this tutorial, do the following:</span></span>
> 
> - <span data-ttu-id="7641e-112">Aktualizacja Twojego [Menedżera pakietów](http://docs.nuget.org/docs/start-here/installing-nuget) do najnowszej wersji.</span><span class="sxs-lookup"><span data-stu-id="7641e-112">Update your [Package Manager](http://docs.nuget.org/docs/start-here/installing-nuget) to the latest version.</span></span>
> - <span data-ttu-id="7641e-113">Zainstaluj [sieci Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span><span class="sxs-lookup"><span data-stu-id="7641e-113">Install the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx).</span></span>
> - <span data-ttu-id="7641e-114">Instalator platformy sieci Web, wyszukiwanie i instalowanie **ASP.NET i 2013.1 narzędzia sieci Web dla programu Visual Studio 2012**.</span><span class="sxs-lookup"><span data-stu-id="7641e-114">In the Web Platform Installer, search for and install **ASP.NET and Web Tools 2013.1 for Visual Studio 2012**.</span></span> <span data-ttu-id="7641e-115">Spowoduje to zainstalowanie szablony programu Visual Studio dla biblioteki SignalR klas takich jak **Centrum**.</span><span class="sxs-lookup"><span data-stu-id="7641e-115">This will install Visual Studio templates for SignalR classes such as **Hub**.</span></span>
> - <span data-ttu-id="7641e-116">Niektóre szablony (takich jak **klasy początkowej OWIN**) nie są dostępne; w tym przypadku powinien użyć pliku klasy.</span><span class="sxs-lookup"><span data-stu-id="7641e-116">Some templates (such as **OWIN Startup Class**) will not be available; for these, use a Class file instead.</span></span>
> 
> 
> ## <a name="questions-and-comments"></a><span data-ttu-id="7641e-117">Pytania i komentarze</span><span class="sxs-lookup"><span data-stu-id="7641e-117">Questions and comments</span></span>
> 
> <span data-ttu-id="7641e-118">Wystaw opinię na jak zbędne tego samouczka i jakie firma Microsoft może poprawić w komentarze u dołu strony.</span><span class="sxs-lookup"><span data-stu-id="7641e-118">Please leave feedback on how you liked this tutorial and what we could improve in the comments at the bottom of the page.</span></span> <span data-ttu-id="7641e-119">Jeśli masz pytania, które nie są bezpośrednio związane z tego samouczka możesz zamieścić je do [forum ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) lub [StackOverflow.com](http://stackoverflow.com/).</span><span class="sxs-lookup"><span data-stu-id="7641e-119">If you have questions that are not directly related to the tutorial, you can post them to the [ASP.NET SignalR forum](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) or [StackOverflow.com](http://stackoverflow.com/).</span></span>


<span data-ttu-id="7641e-120">SignalR 2 oferuje środowisko spójne opracowywanie platformach serwera przy użyciu [OWIN](http://owin.org).</span><span class="sxs-lookup"><span data-stu-id="7641e-120">SignalR 2 offers a consistent development experience across server platforms using [OWIN](http://owin.org).</span></span> <span data-ttu-id="7641e-121">W tym artykule opisano kilka kroków, które są potrzebne, aby zaktualizować aplikację 1.x SignalR do wersji 2.</span><span class="sxs-lookup"><span data-stu-id="7641e-121">This article describes the few steps that are needed to update a SignalR 1.x application to version 2.</span></span>

<span data-ttu-id="7641e-122">Zaleca się uaktualnienie aplikacji SignalR 2, SignalR 1.x, będzie nadal być obsługiwane.</span><span class="sxs-lookup"><span data-stu-id="7641e-122">While it is encouraged to upgrade applications to SignalR 2, SignalR 1.x will still be supported.</span></span>

<span data-ttu-id="7641e-123">Ten przewodnik opisuje sposób uaktualnić aplikację sieci web hostowanych na SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="7641e-123">This tutorial describes how to upgrade a web-hosted application to SignalR 2.</span></span> <span data-ttu-id="7641e-124">Hostowanie Samoobsługowe aplikacji, (te, które serwerze w aplikacji konsoli, usługa systemu Windows lub inny proces hosta) są teraz obsługiwane 2 SignalR.</span><span class="sxs-lookup"><span data-stu-id="7641e-124">Self-hosted applications (those that host a server in a console application, Windows service, or other process) are now supported under SignalR 2.</span></span> <span data-ttu-id="7641e-125">Informacje na temat sposobu rozpocząć tworzenie aplikacji siebie z SignalR 2, zobacz [samouczek: SignalR hosta samodzielnego](../deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="7641e-125">For information on how to get started creating a self-hosted application with SignalR 2, see [Tutorial: SignalR Self-Host](../deployment/tutorial-signalr-self-host.md).</span></span>

## <a name="contents"></a><span data-ttu-id="7641e-126">Spis treści</span><span class="sxs-lookup"><span data-stu-id="7641e-126">Contents</span></span>

<span data-ttu-id="7641e-127">W poniższych sekcjach opisano zadania związane z Uaktualnianie projektów SignalR i jak rozwiązywać problemy, które mogą wystąpić.</span><span class="sxs-lookup"><span data-stu-id="7641e-127">The following sections describe tasks involved with upgrading SignalR projects, and how to troubleshoot issues that may arise.</span></span>

- [<span data-ttu-id="7641e-128">Przykład: Uaktualnienie samouczka Wprowadzenie do SignalR 2</span><span class="sxs-lookup"><span data-stu-id="7641e-128">Example: Upgrading the Getting Started tutorial to SignalR 2</span></span>](#example)
- [<span data-ttu-id="7641e-129">Rozwiązywanie problemów z błędami podczas uaktualniania</span><span class="sxs-lookup"><span data-stu-id="7641e-129">Troubleshooting errors encountered during upgrading</span></span>](#troubleshooting)

<a id="example"></a>

## <a name="example-upgrading-the-getting-started-tutorial-application-to-signalr-2"></a><span data-ttu-id="7641e-130">Przykład: Uaktualnianie Wprowadzenie samouczek aplikacji SignalR 2</span><span class="sxs-lookup"><span data-stu-id="7641e-130">Example: Upgrading the Getting Started tutorial application to SignalR 2</span></span>

<span data-ttu-id="7641e-131">W tej sekcji będzie aktualizowana aplikacji utworzonych w [SignalR wersja 1.x Samouczek wprowadzający](../older-versions/index.md) do użycia SignalR 2.</span><span class="sxs-lookup"><span data-stu-id="7641e-131">In this section, you'll update the application created in the [SignalR 1.x version of the Getting Started Tutorial](../older-versions/index.md) to use SignalR 2.</span></span>

1. <span data-ttu-id="7641e-132">Po zakończeniu samouczka Wprowadzenie, kliknij prawym przyciskiem myszy projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="7641e-132">Once you've finished the Getting Started tutorial, right-click on the project, and select **Properties**.</span></span> <span data-ttu-id="7641e-133">Sprawdź, czy **platformy docelowej** ustawiono **.NET Framework 4.5.**</span><span class="sxs-lookup"><span data-stu-id="7641e-133">Verify that the **Target framework** is set to **.NET Framework 4.5.**</span></span>
2. <span data-ttu-id="7641e-134">Otwórz konsolę Menedżera pakietów.</span><span class="sxs-lookup"><span data-stu-id="7641e-134">Open the Package Manager Console.</span></span> <span data-ttu-id="7641e-135">Usuń SignalR 1.x z projektu za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="7641e-135">Remove SignalR 1.x from the project using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample1.ps1)]
3. <span data-ttu-id="7641e-136">Zainstaluj 2 SignalR za pomocą następującego polecenia:</span><span class="sxs-lookup"><span data-stu-id="7641e-136">Install SignalR 2 using the following command:</span></span>

    [!code-powershell[Main](upgrading-signalr-1x-projects-to-20/samples/sample2.ps1)]
4. <span data-ttu-id="7641e-137">Na stronie HTML zaktualizować odwołanie do skryptu dla biblioteki SignalR do zgodna z wersją skryptu teraz zawarta w projekcie.</span><span class="sxs-lookup"><span data-stu-id="7641e-137">In the HTML page, update the script reference for SignalR to match the version of the script now included in the project.</span></span>

    [!code-html[Main](upgrading-signalr-1x-projects-to-20/samples/sample3.html)]
5. <span data-ttu-id="7641e-138">Klasy globalne aplikacji Usuń wywołanie MapHubs.</span><span class="sxs-lookup"><span data-stu-id="7641e-138">In the global application class, remove the call to MapHubs.</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample4.cs)]
6. <span data-ttu-id="7641e-139">Kliknij prawym przyciskiem myszy rozwiązanie, a następnie wybierz **Dodaj**, **nowy element...** . W oknie dialogowym wybierz **klasy początkowej Owin**.</span><span class="sxs-lookup"><span data-stu-id="7641e-139">Right-click the solution, and select **Add**, **New Item...**. In the dialog, select **Owin Startup Class**.</span></span> <span data-ttu-id="7641e-140">Nazwa nowej klasy **Startup.cs**.</span><span class="sxs-lookup"><span data-stu-id="7641e-140">Name the new class **Startup.cs**.</span></span>

    ![](upgrading-signalr-1x-projects-to-20/_static/image1.png)
7. <span data-ttu-id="7641e-141">Zastąp zawartość pliku Startup.cs następujący kod:</span><span class="sxs-lookup"><span data-stu-id="7641e-141">Replace the contents of Startup.cs with the following code:</span></span>

    [!code-csharp[Main](upgrading-signalr-1x-projects-to-20/samples/sample5.cs)]

    <span data-ttu-id="7641e-142">Atrybut zestawu Dodaje klasę do jego Owin uruchomienia procesu, który wykonuje `Configuration` metody podczas uruchamiania Owin.</span><span class="sxs-lookup"><span data-stu-id="7641e-142">The assembly attribute adds the class to Owin's startup process, which executes the `Configuration` method when Owin starts up.</span></span> <span data-ttu-id="7641e-143">To z kolei wywołuje `MapSignalR` metodę, która tworzy trasy dla wszystkich koncentratorów SignalR w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="7641e-143">This in turn calls the `MapSignalR` method, which creates routes for all SignalR hubs in the application.</span></span>
8. <span data-ttu-id="7641e-144">Uruchom projekt i skopiuj adres URL strony głównej do innej przeglądarki lub okienko przeglądarki, jak wcześniej.</span><span class="sxs-lookup"><span data-stu-id="7641e-144">Run the project, and copy the URL of the main page into another browser or browser pane, as before.</span></span> <span data-ttu-id="7641e-145">Każdej stronie poprosi o nazwę użytkownika i komunikatów wysyłanych z każdej strony powinny być widoczne w obu okienka w przeglądarce.</span><span class="sxs-lookup"><span data-stu-id="7641e-145">Each page will ask for a username, and messages sent from each page should be visible in both browser panes.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting-errors-encountered-during-upgrading"></a><span data-ttu-id="7641e-146">Rozwiązywanie problemów z błędami podczas uaktualniania</span><span class="sxs-lookup"><span data-stu-id="7641e-146">Troubleshooting errors encountered during upgrading</span></span>

<span data-ttu-id="7641e-147">W tej sekcji opisano problemy, które mogą wystąpić podczas uaktualniania.</span><span class="sxs-lookup"><span data-stu-id="7641e-147">This section describes issues that may arise during upgrading.</span></span> <span data-ttu-id="7641e-148">Na pełniejsze listy błędów i problemów, które mogą wystąpić przy użyciu aplikacji SignalR, zobacz [SignalR Rozwiązywanie problemów z](../testing-and-debugging/troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="7641e-148">For a more comprehensive list of errors and issues that may occur with a SignalR application, see [SignalR Troubleshooting](../testing-and-debugging/troubleshooting.md).</span></span>

### <a name="the-call-is-ambiguous-between-the-following-methods-or-properties"></a><span data-ttu-id="7641e-149">"Wywołanie jest niejednoznaczne między następujące metody lub właściwości"</span><span class="sxs-lookup"><span data-stu-id="7641e-149">'The call is ambiguous between the following methods or properties'</span></span>

<span data-ttu-id="7641e-150">Ten błąd wystąpi, jeśli odniesienie do `Microsoft.AspNet.SignalR.Owin` nie zostanie usunięta.</span><span class="sxs-lookup"><span data-stu-id="7641e-150">This error will occur if a reference to `Microsoft.AspNet.SignalR.Owin` is not removed.</span></span> <span data-ttu-id="7641e-151">Ten pakiet jest przestarzały; należy usunąć odwołanie i wersja 1.x pakietu SelfHost musi zostać odinstalowane.</span><span class="sxs-lookup"><span data-stu-id="7641e-151">This package is deprecated; the reference must be removed and the 1.x version of the SelfHost package must be uninstalled.</span></span>

### <a name="hub-methods-fail-silently"></a><span data-ttu-id="7641e-152">Metody koncentratora nie w trybie dyskretnym</span><span class="sxs-lookup"><span data-stu-id="7641e-152">Hub methods fail silently</span></span>

<span data-ttu-id="7641e-153">Sprawdź, czy aktualny i że odwołań do skryptów w kliencie `OwinStartup` atrybutu dla klasy uruchomienia ma poprawne klas i nazwy zestawu dla projektu.</span><span class="sxs-lookup"><span data-stu-id="7641e-153">Verify that the script references in your client are up to date, and that the `OwinStartup` attribute for your Startup class has the correct class and assembly names for your project.</span></span> <span data-ttu-id="7641e-154">Ponadto spróbuj otwierania centra adresu (/ signalr/hubs) w przeglądarce; jakiegokolwiek błędu, który pojawia się będą oferować więcej informacji na temat co się dzieje niewłaściwy.</span><span class="sxs-lookup"><span data-stu-id="7641e-154">Also, try opening up the hubs address (/signalr/hubs) in your browser; any error that appears will offer more information about what's going wrong.</span></span>