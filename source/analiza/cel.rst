Cel pracy
=========

Celem pracy jest dostarczenie aplikacji do zarządzania artykułami dla wydawnictwa, a także projekt systemu który będzie wysoko wydajny i skalowalny. 

Większość dostępnego oprogramowania do zarządzania treścią oparte jest o zestaw aplikacji napisanej w PHP (WordPress) działającą o bazę lub klony baz typu MySQL. O ile w przypadkach serwisów działających w niedużych skalach to takie połączenie nie stanowi większego problemu jednak w przypadku dużych rozwiązań implikuje to różnego rodzaju problemy takie jak brak możliwości skalowania, duża złożoność wdrażania zmian, a także wyzwiana związane z relacyjnością. W przypadku budowy CMS opertego o aplikacje zbudowane w klastrach kubernates i użyciu baz dokumentowych (w tym przypadku MongoDB) możliwe jest budowanie bardzo skalowalnych środowisk gdzie właściwie nie ma ograniczeń. Dodatkowo prezentowane rozwiązanie umożliwia wdrażanie zmian bez impaktu na uzytkowników. Poniżej opisane rozwiązanie bazując na bazie DocumentDB tak naprawdę nie posiada własnego schematu więc wszelkie zmiany dokumentów mogą być wprowadzane dla nowych dokumentów a zmiany są wprowadzane dla nowych dokumentów. W przypadku serwerów aplikacyjnych, tworzenie nowych instancji aplikacji może być wdrażane "z boku" i testowanie aplikacji i zmian tylko dl określonych użytkowników. 

Na rynku jest mało podobnych rozwiązań, a istniejące oprogramowanie niestety to platformy do budowy środowisk. 


Oprogramowanie:
---------------

[https://www.keystonejs.com/] -> oprogramowanie działające na NodeJS i jest to platforma do budowy środowisk. Posiada współpracę z GraphQL.


[https://strapi.io/] -> Właściwie jest to narzędzie dla programistów pozwalające zbudować rozwiązanie CMS.


[https://www.endurojs.com/] -> Owszem jest na Node.Js ale jest oparty na plikach płaskich bez bazy danych.


[https://www.totaljs.com/] -> Oparty jest na wbudowanym silniku NoSQL
