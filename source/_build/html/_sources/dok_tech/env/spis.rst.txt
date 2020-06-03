Instrukcja instalacji 
=====================

Środowisko testowe
------------------

Środowisko testowe ze względu na to że nie wymaga takiej wysokiej dostępności co środowisko preodukcyjne nie ma także konieczności żeby był zastosowany klaster Kubernetesa i MongoDB ustawione w shardeningu.

### Baza danych

MongoDB 

Baza danych w tym przypadku użyta wersja COmmunity i uruchomiona jako serwer standalone. Aby utworzyć instancję nalezy użyć wcześniej przygotowanego pliku dla Docker compose. Plik zawiera poniższe informacje:

...

Po utworzeniu instancji należy zalogować się do kontenera przejść do katalogu script i pokolei uruchomić skrypty 
aaaa.sh - skrypt założy dla nas odpowiednich użytkowników, bazy danych wraz z wymaganymi uprawnieniami.
bbbb.sh - (do utworzenia) skrypt tworzący przykłądowe wpisy do bazy dla aplikacji.

CMS Api

...

CMS Frontend

....

Środowisko produkcyjne
----------------------