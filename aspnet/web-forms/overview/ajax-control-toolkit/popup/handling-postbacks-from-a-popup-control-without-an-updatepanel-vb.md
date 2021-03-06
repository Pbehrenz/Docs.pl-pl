---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony występuje w su...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Obsługa ogłaszania zwrotnego przez formant okna podręcznego bez elementu UpdatePanel (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.


## <a name="overview"></a>Omówienie

Rozszerzenie PopupControl w zestawie narzędzi kontroli AJAX zapewnia prosty sposób do wyzwolenia menu podręcznego, gdy inny formant jest aktywny. Podczas odświeżania strony odbywa się w takich panelu i na stronie istnieje kilka paneli trudno jest określić, który panel został kliknięty.

## <a name="steps"></a>Kroki

Korzystając z `PopupControl` odświeżenie strony, ale bez konieczności `UpdatePanel` na stronie Toolkit formantu nie oferuje możliwość określenia, który element klienta wyzwolił menu podręczne, które z kolei spowodowało ogłaszania zwrotnego. Jednak małych lewy dostarcza obejście dla tego scenariusza.

Po pierwsze, w tym miejscu jest podstawowa konfiguracja: dwa pola tekstowe podlegających zarówno samego menu podręczne kalendarza. Dwa `PopupControlExtenders` zebranie okna podręczne oraz pól tekstowych.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

Podstawowe rozwiązaniem jest dodanie ukryte pole formularza w &lt; `form` &gt; element, który zawiera pole tekstowe, która została uruchomiona menu podręcznego:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Podczas ładowania strony kodu JavaScript dodaje program obsługi zdarzeń do obu polach tekstowych: gdy użytkownik kliknie w polu tekstowym, jego nazwa jest zapisywany w ukryte pole formularza:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

W kodzie po stronie serwera można odczytać wartości pola ukrytego. Ponieważ ukryte pola są prosta do manipulowania, podejście listy dozwolonych adresów IP można sprawdzić poprawności wartości hidden jest wymagana. Po zidentyfikowaniu pola tekstowego poprawną datę z kalendarza są zapisywane w nim.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Kalendarza jest wyświetlany, gdy użytkownik kliknie w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Kliknij datę umieszcza je w polu tekstowym](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Kliknij datę umieszcza je w polu tekstowym ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Poprzednie](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
