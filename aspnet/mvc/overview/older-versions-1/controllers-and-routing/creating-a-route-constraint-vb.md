---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
title: Tworzenie ograniczenia trasy (VB) | Dokumentacja firmy Microsoft
author: StephenWalther
description: W tym samouczku Stephen Walther pokazano, jak można kontrolować, jak przeglądarka żąda trasy do dopasowania, tworząc ograniczenia trasy za pomocą wyrażeń regularnych.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/16/2009
ms.topic: article
ms.assetid: b7cce113-c82c-45bf-b97b-357e5d9f7f56
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-route-constraint-vb
msc.type: authoredcontent
ms.openlocfilehash: 2f50b371ac679218b06c4848e6d33516d29d3a82
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-route-constraint-vb"></a>Tworzenie ograniczenia trasy (VB)
====================
przez [Stephen Walther](https://github.com/StephenWalther)

> W tym samouczku Stephen Walther pokazano, jak można kontrolować, jak przeglądarka żąda trasy do dopasowania, tworząc ograniczenia trasy za pomocą wyrażeń regularnych.


Ograniczenia trasy umożliwia ograniczenie żądań przeglądarki, które pasuje do określonej trasy. Wyrażenie regularne służy do określania ograniczenia trasy.

Na przykład załóżmy, że lista 1 zdefiniowano trasy w pliku Global.asax.

**Wyświetlanie listy 1 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample1.vb)]

Wyświetlanie listy 1 zawiera trasy o nazwie produktu. Trasy produktu służy do mapowania żądania przeglądarki ProductController zawarte w wyświetlania 2.

**Wyświetlanie listy 2 - Controllers\ProductController.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample2.vb)]

Zwróć uwagę, że akcja Details() udostępnianych przez kontroler produktu przyjmuje jeden parametr o nazwie productId. Ten parametr jest parametru liczby całkowitej.

Trasy zdefiniowane w 1 Lista będzie pasuje do żadnego z następujących adresów URL:

- / Produktu/23
- / Produktu/7

Niestety trasy również zgodna następujące adresy URL:

- / Produktu/blah
- / Produktu/firmy apple

Ponieważ akcji Details() oczekuje parametru liczby całkowitej, wnioskiem zawiera inną niż wartość całkowitą spowoduje błąd. Na przykład jeśli wpiszesz /Product/apple adres URL w przeglądarce następnie otrzymasz strony błędu na rysunku 1.


[![Okno dialogowe nowego projektu](creating-a-route-constraint-vb/_static/image1.jpg)](creating-a-route-constraint-vb/_static/image1.png)

**Rysunek 01**: strona, rozwiń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-route-constraint-vb/_static/image2.png))


Co naprawdę chcesz zrobić to dopasowanie tylko adresów URL zawierających productId odpowiednie liczby całkowitej. Umożliwia ograniczenie podczas definiowania trasy ograniczyć adresy URL zgodne trasy. Zmodyfikowane trasy produktu w wersji 3 lista zawiera ograniczenie wyrażenie regularne, które jest zgodny tylko liczby całkowite.

**Wyświetlanie listy 3 - Global.asax.vb**

[!code-vb[Main](creating-a-route-constraint-vb/samples/sample3.vb)]

\D+ wyrażenia regularnego odpowiada jednej lub więcej liczb całkowitych. To ograniczenie powoduje, że produkt trasy do dopasowania następujące adresy URL:

- / Produkt/3
- / Produktu/8999

Ale nie następujących adresów URL:

- / Produktu/firmy apple
- / Produktu

Te żądania przeglądarki będzie obsługiwany przez inny trasy lub, nie ma zgodnych tras *nie można odnaleźć zasobu* zostanie zwrócony błąd.

> [!div class="step-by-step"]
> [Poprzednie](creating-custom-routes-vb.md)
> [dalej](creating-a-custom-route-constraint-vb.md)
