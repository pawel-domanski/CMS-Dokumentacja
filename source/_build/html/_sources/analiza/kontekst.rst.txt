.. System Zarządzania Treścią documentation master file, created by
   sphinx-quickstart on Wed May 27 11:02:55 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Kontekst
==========

Jeśli zaczynamy tworzenie kontent nie zawsze potrzebujemy środowiska które jest docelowe czyli mocno przeskalowane. Na samym początku potrzebujemy bardzo małego środowiska zainstalowanego na naszym komputerze lub usytuowane w chmurze które będzie mogło nam służyć zarówno w dla środowisk developerskich jak i środowisk produkcyjnych dla pierwszej fazy wdrożenia produktu (firmy z reguły do czasu uruchomienia nie mają już funduszy na rozruch i przycinają każdy koszt). 

Zastosowanie bazy MongoDB i oprogramowania napisanego na klastrach kubernates pozwala nam jak wcześniej napisałem szybko i przy małych nakładach pieniężnych. W późniejszych etapach można rozbudować ilość uruchamianych instancji i skalować aby dostarczyć usługi jak najbardziej optymalnie.

Kolejnym elementem proponowanego środowiska to szybkie testowanie i wdrażanie nowych funkcjonalności na środowiskach produkcyjnych. Aplikacja może być wdrożona tylko na niewielkiej ilości instancji oprogramowania a następnie aplikować dla reszty starego oprogramowania. Rozwiązanie takie daje możliwość wdrażania bez niedostępności.