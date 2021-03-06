---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
title: Modyfikowanie animacji po stronie serwera (C#) | Dokumentacja firmy Microsoft
author: wenz
description: Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Może to również animacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b0abec39-a1c9-422d-ba9a-ef16f6185af8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-cs
msc.type: authoredcontent
ms.openlocfilehash: 6946875552c885ffb1f2a2eb7e728b85d7dd3973
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="modifying-animations-from-the-server-side-c"></a>Modyfikowanie animacji po stronie serwera (C#)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.cs.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9CS.pdf)

> Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji mogą również zostać zmienione po stronie serwera


## <a name="overview"></a>Omówienie

Formantu animacji w zestawie narzędzi programu ASP.NET AJAX formantu nie jest po prostu formantu, ale całego framework do Dodawanie animacji do formantu. Animacji mogą również zostać zmienione po stronie serwera

## <a name="steps"></a>Kroki

Po pierwsze, obejmują `ScriptManager` na stronie; następnie biblioteki ASP.NET AJAX został załadowany, dzięki czemu można użyć kontroli zestawu narzędzi:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample1.aspx)]

Animacja zostanie zastosowana do panelu tekstu, która wygląda następująco:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample2.aspx)]

Skojarzone klasy CSS panelu Zdefiniuj kolor tła nieuprzywilejowany i również ustawić stałą szerokość panelu:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample3.css)]

Pozostała część kodu jest uruchamiana po stronie serwera i nie używa znaczników; Zamiast tego kod używane w celu utworzenia `AnimationExtender` sterowania:

[!code-aspx[Main](modifying-animations-from-the-server-side-cs/samples/sample4.aspx)]

Jednak zestaw narzędzi do sterowania aktualnie nie zapewnia dostęp API do tworzenia indywidualnych animacji. Jest jednak można ustawić `AnimationExtender`właściwości animacji na ciąg zawierający znaczników XML używane podczas przypisywania animacji deklaratywnie. Aby utworzyć plik XML, który nie może zawierać `<Animations>` elementu można użyć programu .NET Framework XML obsługuje lub zgodnie z poniższym kodem, wystarczy podać ciąg:

[!code-css[Main](modifying-animations-from-the-server-side-cs/samples/sample5.css)]

Na koniec należy dodać `AnimationExtender` sterowania do bieżącej strony w `<form runat="server">` elementu, upewniając się, czy animacja jest dołączony i uruchamia:

[!code-html[Main](modifying-animations-from-the-server-side-cs/samples/sample6.html)]


[![Animacja jest tworzony przy użyciu po stronie serwera C# /VB kodu](modifying-animations-from-the-server-side-cs/_static/image2.png)](modifying-animations-from-the-server-side-cs/_static/image1.png)

Animacja jest tworzony przy użyciu po stronie serwera C# / VB. kodu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](modifying-animations-from-the-server-side-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](triggering-an-animation-in-another-control-cs.md)
> [dalej](executing-animations-using-client-side-code-cs.md)
