---
layout: default
title: "Jekyll Docs Template"
---

### Opis pomysłu pracy dyplomowej

Początkowo miałem przygotować wyłącznie system bazy danych, która by była wysokodostępna i odporna na wszelkie możliwe awarie i utraty danych. Niestety nie jest to do końca dobry pomysł, ponieważ praca dyplomowa powinna zawierać także wkład programistyczny. Zastaniawiałem się jak mogę zaradzić na ten problem i wpadłem na pomysł napisania własnego autorskiego systemu zarządzania treścią operte na dokumentowej bazie danych i środowisku wysokiej dostępności. Jednocześnie przedstawię to w jaki sposób przygotować infrastrukturę.

Drugim powodem takiego poysłu jest to że na rynku nie ma takiego programu, który można zaimplementować. Większość tego typu systemów oparta jest na relacyjnych bazach danych, które przy dużej ilości danych może nastręczać wiele problemów wydajnościowych i zabezpieczenia danych przed awarią. W systemie zarządzania treścią możemy pozwilić żeby część danych może być utraconych więc spokojnie można zastosować bazę taką jak MongoDB czy też CouchDB.


Trzecim powodem jest to że obecnie najbardziej popularny system zarządzania trością jest już tak rozbudowany że triudno jest go dostosować do swoich wymogów mimo swojej elastycznej budowie i dużej ilości specjalistów do rozbudowy.

#### Praca będzie podzielona na kilka części opisanych poniżej

- Cześć backend, który będzie odpowiedzialny za wszelkie zapisy i odczyty do bazy dancyh. Dodatkowo będzie podzielony na dwie części, gdzie pierwsza będzie odpowiedzialna za to żeby można było odczytywać informacje z bazy danych i będzie to publiczny dostęp tylko do odczytu. Druga czść będzie częścią gdzie będą wykonywane zapisy do bazy danych. Część administracyjna będzie miała okrojony zasięg. Dostęp do części administracyjnej będzie tylko z określonych sieci lub wewnątrz firmy. Zwykły użytkownik nie dostanie się do tych danych.
- Frontend - napisany w Vue, który jest prosty i elastyczny. Nie będzie wymagać to dużego nakładu pracy doi jego nauczenia. Tutaj tak samo będzie podzielone to na dwie części. Część publiczną czyli nasz ostateczny produkt, gdzie użytkownicy będą mogli przeczytać artykluły, Części administracyjnej, czyli tego wszystkiego co jest porrzebne do zarządzania CMSem. Wszystkie opisane wyżej opcje mają działać na kubernates
- Część składowania danych. Będzie to baza danych, która będzie przechowywać dokumenty. Początkowo miał być tylko punkt 4 ale w związku z wymaganiami dodane ziostały inne elementy.
