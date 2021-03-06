---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Przy użyciu TextBoxWatermark z formantami sprawdzania poprawności (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formant TextBoxWatermark w zestawie narzędzi kontroli AJAX rozszerza pole tekstowe tak, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu go i...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>Przy użyciu TextBoxWatermark z formantami sprawdzania poprawności (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> Formant TextBoxWatermark w zestawie narzędzi kontroli AJAX rozszerza pole tekstowe tak, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, jest opróżniany. Jeśli użytkownik opuści pole bez wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. To może dojść do kolizji z formantami weryfikacji platformy ASP.NET na tej samej stronie, ale może można rozwiązać te problemy.


## <a name="overview"></a>Omówienie

`TextBoxWatermark` Formantu w zestawie narzędzi kontroli AJAX rozszerza pola tekstowego, aby tekst jest wyświetlany w polu. Gdy użytkownik kliknie w polu, jest opróżniany. Jeśli użytkownik opuści pole bez wprowadzania tekstu, wstępnie wypełnionych tekstu pojawi się ponownie. To może dojść do kolizji z formantami weryfikacji platformy ASP.NET na tej samej stronie, ale może można rozwiązać te problemy.

## <a name="steps"></a>Kroki

Podstawowa konfiguracja próbki jest następująca: `TextBox` kontroli jest oznaczone znakiem wodnym przy użyciu `TextBoxWatermarkExtender` formantu. Przycisk wyzwala odświeżania strony i później będzie służyć do wyzwolenia formanty walidacji na tej stronie. Ponadto `ScriptManager` formant jest wymagany do zainicjowania ASP.NET AJAX:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Teraz Dodaj `RequiredFieldValidator` formant, który sprawdza, czy jest tekst w polu po przesłaniu formularza. `InitialValue` Właściwości modułu sprawdzania poprawności musi mieć ustawioną taką samą wartość, która jest używana w `TextBoxWatermarkExtender` kontroli: po przesłaniu formularza, wartości bez zmian pole tekstowe jest wartość znaku wodnego w niej:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Jednak jest jednym z problemów z tej metody: Jeśli klient wyłącza JavaScript, pola tekstowego jest nie wstępnie z tekstu znaku wodnego, w związku z tym `RequiredFieldValidator` nie spowoduje wyzwolenia komunikat o błędzie. W związku z tym drugiej `RequiredFieldValidator` kontroli jest wymagana, która sprawdza, czy pole puste (pominięcie `InitialValue` atrybutu).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Ponieważ korzystać z obu modułów sprawdzania poprawności `Display` = `"Dynamic"`, użytkownik końcowy nie może odróżnić od wyglądu, którego dwa moduły weryfikacji zostało zainicjowane; należy prawdopodobnie wystąpił tylko jeden z nich.

Na koniec należy dodać kodu po stronie serwera do wyjściowego tekst w polu, jeśli żadnego modułu weryfikacji wystawiony komunikat o błędzie:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Moduł weryfikacji narzeka, że nie ma żadnego tekstu w polu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](using-textboxwatermark-in-a-formview-vb.md)
