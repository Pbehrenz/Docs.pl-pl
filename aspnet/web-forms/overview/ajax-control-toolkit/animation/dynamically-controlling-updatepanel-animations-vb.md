---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
title: Dynamicznie kontrolowanie UpdatePanel animacji (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartości...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: bea66072-59b6-42b4-98fa-211812f5925f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-vb
msc.type: authoredcontent
ms.openlocfilehash: ff2853b4457a83a7367b4d1072d21929c40a3ed2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-controlling-updatepanel-animations-vb"></a>Dynamicznie kontrolowanie UpdatePanel animacji (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2VB.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartość elementu UpdatePanel, rozszerzający specjalne umożliwiającą odgrywa framework animacji: UpdatePanelAnimation. Można go także używać razem z wyzwalaczy elementu UpdatePanel.


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Zawartości `UpdatePanel`, rozszerzający specjalne umożliwiającą odgrywa framework animacji: `UpdatePanelAnimation`. Można go także używać razem z `UpdatePanel` wyzwalaczy.

## <a name="steps"></a>Kroki

Pierwszym krokiem jest normalnie obejmują `ScriptManager` na stronie, aby biblioteka ASP.NET AJAX została załadowana i można go używać zestawu narzędzi kontroli:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample1.aspx)]

Animacja w tym scenariuszu zostaną zastosowane do wyświetlenia bieżącego czasu. Te informacje mogą być zapisywane w etykiecie przy użyciu `Page_Load()` metody, lub (dla uproszczenia należy) jest używany następujący kod wbudowany:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample2.aspx)]

Ponadto jest tworzony przycisk, aby wyzwolić aktualizowanie czas:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample3.aspx)]

Ten kod jest następnie poddane `<ContentTemplate>` sekcji `UpdatePanel` elementu. Panelu `UpdateMode` atrybut musi być ustawiony na `"Conditional"`, ponieważ tylko wyzwalaczy może zaktualizować zawartość panelu. W `<Triggers>` sekcji `UpdatePanel`, wyzwalacz odświeżania strony asynchroniczne jest tworzony i powiązane `Click` zdarzeń przycisku. W związku z tym, gdy użytkownik kliknie przycisk, `UpdatePanel` zostanie odświeżony. Oto kod znaczników dla `UpdatePanel` sterowania:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample4.aspx)]

Na koniec `UpdatePanelAnimationExtender` musi być skonfigurowany: Ustaw `TargetControlID` atrybutu ID panelu, a także definiują animacji w rozszerzeń. Zanikania w ułatwia wykrywanie, który tworzy nieuprzywilejowany visual nacisk położono na czas zaktualizowane. Z rozszerzeń znaczników może następnie wyglądać następująco:


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-vb/samples/sample5.aspx)]

Uruchom plik w przeglądarce. Po kliknięciu przycisku bieżący czas jest wyświetlany w panelu zawsze zanikania czasie trwania jednej sekundzie.


[![Bieżący czas jest zanikania](dynamically-controlling-updatepanel-animations-vb/_static/image2.png)](dynamically-controlling-updatepanel-animations-vb/_static/image1.png)

Bieżący czas jest zanikania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-controlling-updatepanel-animations-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](animating-an-updatepanel-control-vb.md)
