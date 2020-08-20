---
title: Diagram związków encji
keywords: analiza, encje, baza danych
last_updated: august 10, 2020
tags: [encje, analiza, analiza związków encji]
summary: "W tej części przedstawiona jest analiza związków encji/"
sidebar: mydoc_sidebar
permalink: analiza_encje.html
folder: analiza
---
### Diagram związków encji

Baza danych podzielona jest na dwie części. W pierwszej części przedstawiony jest diagram związków encji dla bazy dokumentów, a drugi przedstawia bazę plików z obrazami dla aplikacji.
Ze względu na to, że jest inna struktura obu baz i inne zastosowania baza danych została rozdzielona i odtatecznie będzie znajdować się na dwóch oddzielnych miejscach.


![Diagram związków encji - baza artukułów](/images/analiza/Diagram_Zwiazkow_Encji-Baza_Artykulow_01.png)

![Diagram związków encji - baza obrazów](/images/analiza/Diagram_Zwiazkow_Encji-Baza_Obrazow_01.png)

Zastosowanie bazy MongoDB spowodowało znaczne uproszczenie relacji pomiędzy poszczególnymi encjami.

W przypadku znormalizowanego modelu moglibyśmy mieć taki przypadek, że dla zapisu keywords w artykule należało by dodać dodatkową tabelę jak zostało pokazano na rysunku poniżej. Zastosowana tabela keywords w moim projekcie służy jako tabela słownikowa. To samo się tyczy takich obiektów jak comment, dla których można nie wydzielać danych do dodatkowej tabeli. 
Prszykład związku encji dla standardowej relacyjnej bazy danych.

![Diagram związków encji - baza obrazów](/images/analiza/Diagram_Zwiazkow_Encji-Baza_Obrazow_02.png)