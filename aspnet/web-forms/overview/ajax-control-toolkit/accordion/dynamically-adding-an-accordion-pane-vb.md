---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamiczne dodawanie Harmonijka okienka (VB) | Dokumentacja firmy Microsoft
author: wenz
description: Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-vb"></a>Dynamiczne dodawanie Harmonijka okienka (VB)
====================
przez [Wenz Chrześcijańskie](https://github.com/wenz)

[Pobierz kod](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)

> Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w samej strony, ale kod po stronie serwera może być używany do osiągnąć ten sam rezultat.


## <a name="overview"></a>Omówienie

Accordion formantu w zestawie narzędzi kontroli AJAX zapewnia wiele okienek i umożliwia użytkownikowi wyświetlanie jeden z nich w czasie. Panele są zwykle deklarowanych w samej strony, ale kod po stronie serwera może być używany do osiągnąć ten sam rezultat.

## <a name="steps"></a>Kroki

Formant Accordion udostępnia wszystkie ważne właściwości do kodu po stronie serwera. Między innymi `Panes` właściwość udziela dostępu do kolekcji okienka wchodzące w skład Accordion. Okienko, co jest typu `AccordionPane`. W związku z tym jest prosta, aby utworzyć takie okienka:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

`HeaderContainer` Właściwość `AccordionPane` zapewnia dostęp do formantów ASP.NET w sekcji nagłówka okienka; `ContentContainer` właściwość `AccordionPane` jest taki sam dla zawartości sekcji okienka. Dzięki temu kodu platformy ASP.NET można dodać zawartości do okienka:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

Na koniec pane(s) musi zostać dodany do `Panes` kolekcji Accordion:

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

W tym miejscu jest kompletny kod po stronie serwera, który dodaje dwa okienka do kontrolki Accordion:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

Accordion, która zależy od obecności ASP.NET jest jedynym elementem Brak `ScriptManager` sterowania:

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

Aby zakończyć w przykładzie, dwie klasy CSS, do którego odwołuje się formant Accordion dostarcza informacji o stylu przeglądarki:

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


[![Dane w accordion dynamicznie została dodana przez kod po stronie serwera](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)

Dane w accordion dynamicznie została dodana przez kod po stronie serwera ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](dynamically-adding-an-accordion-pane-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Poprzednie](databinding-to-an-accordion-vb.md)
