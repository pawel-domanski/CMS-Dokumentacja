# Wysoka dostępność – realizacja poprzez utworzenie klastra kubernates i MongoDB

Co pod tym będziemy rozumieć? 
* Zaprojektowana i wdrożona w miarę możliwości redundadtność baz danych opartych na usłudze shchardeningu i mirroringu. Zaprojektowane pełne środowisko do pracy z bazą danych i całkowity przebieg wdrożenia. 
* Aplikacje oparte na Kubernetes lub Docker. Środowisko 

# Bezpieczeństwo – część dla użytkowników będzie oddzielona od części dla Redaktora i administratora

Warstwa wyświetlania musi być oddzielona od warstwy danych.Będzie przygotowane API, do którego będzie podłączone wyłącznie środowisko z frontend. Nie będzie się można podłączyć do API spoza serwerów FrontEnd. Dodatkowo API ma być zabiezpieczone.

# Skalowalność – system ma być skalowalny

W łatwyy sposób a nawet w sposób automatyczny środowisko może się powiększać.


## Wnioski:

Buduję środowisko schardeningowane i testuje je



# Czynności do wykonania w tej sprawie

* Przygotowanie środowiska development do pisania pracy
* Przygotowanie środowiska do pracy i celów prezentacyjnych. Tutaj zamierzam przygotować dwa środowiska 1) środowisko oparte na mirroringu opisując wady i zalety tego środowiska 2) środowisko schardeningowe opisanie środowiska
* Zastanowić się nad tym czy przypadkiem nie użyć jakiegoś fajnego CI żeby się go chociaż pobieżnie nauczyć
* Nauczyć się Kubernates i jak zbudować środowisko wysokodostępne
* Nauczyć się Vue.js jest to chyba najprostszy z dostępnych frameworków
