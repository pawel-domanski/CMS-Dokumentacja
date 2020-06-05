Instrukcja instalacji 
=====================

Infrastruktura
--------------

Oprogramowanie działa na serwerach wirtualnych działających w chmurze obliczeniowej. Celowo korzystam z kilku niezależnych dostawców aby zaproponować wszechstronność i zwinne podejscie o tematu jakim jest wysoka dostępność systemu zarządzania treścią. Pomiędzy sieciami jest utworzona reguła firewall i możliwość logowania się do poszczególnych części jest zabezpieczony.

Podzielona jest na dwie części. Pierwsza część działająca chmurze obliczeniowej Digital Ocean i są to:

ubuntu-s-1vcpu-1gb-fra1-01
++++++++++++++++++++++++++

Serwer (droplet) z systemem operacyjnym Debian. 
1 GB Memory / 25 GB Disk / FRA1 - Ubuntu 18.04.3 (LTS) x64

Na serwerze jest zainstalowany Docker.

Działające usługi w ramach Docker:

**portainer** - oprogramowanie do zarządzania kontenerami w chmurze publicznej dla projektu. 

Adres do zarządzania https://cloud.dexterlab.pl/
Do zalogowania jest potrzebne konto zakładane przez administratora systemu.

Dostęp do zasobów znajdujących się na serwerze realizowany jest przez serwer Ngnix.

TODO: Dopisać jak jest realizowany dostęp i jak zakłada się Endpoint na serwerze.

Informacja dotycząca endpointów.

[Lista wszystkich endpointów](/doc_tech/env/images/Portainer_01.png)

Informacja o aplikacjach (obrazów Docker) na serwerze. 

[Lista wszystkich endpointów](/doc_tech/env/images/Portainer_02.png)


**registry** - rejestr DOckera na potrzeby projektu. Rejestr *Docker* jest zabezpieczony hasłem żeby osoby niepowołane nie mogły się zalobować.

**dexterlab_pl** - strona internetowa projektu oparta na systemie CMS Ghost. 


ubuntu-s-1vcpu-1gb-fra1-02
++++++++++++++++++++++++++

Serwer (droplet) z systemem operacyjnym Debian. 

1 GB Memory / 25 GB Disk / FRA1 - Ubuntu 18.04.3 (LTS) x64

Na serwerze jest zainstalowany Docker.

Maszyna jest odpoweidzialna za zarządzanie kodem żródłowym poprzez oprogramowanie GitLab. Zainstalowana jest wersja Community Edition dostępna pod adresem https://git.dexterlab.pl/ Dostęp realizowany jest przez konta wewnętrzne systemu i zakładane są przez administratora systemu.

GitHub
------
Dokumentacja do projektu jest zamieszczona na GitHub Pages i można przeczytać pod adresem https://doc.dexterlab.pl/

Środowisko testowe
------------------

Środowisko testowe ze względu na to że nie wymaga takiej wysokiej dostępności co środowisko preodukcyjne nie ma także konieczności żeby był zastosowany klaster Kubernetesa i MongoDB ustawione w shardeningu. Wszystkie serwery do pracy dla projektu znajdują się w Google Cloud i analogicznie jak w przypadku infrastruktury całość jest oparta o serwery wirtualne z systemem Linux.

Baza danych
+++++++++++

MongoDB 

Baza danych w tym przypadku użyta wersja Community i uruchomiona jako serwer standalone. Aby utworzyć instancję nalezy użyć wcześniej przygotowanego pliku dla Docker compose. Plik zawiera poniższe informacje:

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

** konfiguracja GitLab
