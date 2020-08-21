---
title: Instalacja infrastruktury
keywords: instalacja, infrastruktura
last_updated: June 3, 2020
tags: [zakres_pracy]
summary: "Opis infrastruktury."
sidebar: mydoc_sidebar
permalink: inst_infra.html
folder: insta
---
# Instrukcja instalacji 


## Infrastruktura


Oprogramowanie działa na serwerach wirtualnych działających w chmurze obliczeniowej. Celowo korzystam z kilku niezależnych dostawców aby zaproponować wszechstronność i zwinne podejscie o tematu jakim jest wysoka dostępność systemu zarządzania treścią. Pomiędzy sieciami jest utworzona reguła firewall i możliwość logowania się do poszczególnych części jest zabezpieczony. Dodakowo użycie różnych dostawców sprawia, że nie jesteśmy uzależnieni wyłącznie z jednego tylko miejsca.

### Infrastruktura


#### Środowisko oparte na dropletach na Digital Ocean

**ubuntu-s-1vcpu-1gb-fra1-02** - środowisko do testów, na którym można trenować wszystkoe możliwe kombinacje do projektu.
![ubuntu-s-1vcpu-1gb-fra1-02](/images/insta/inst_infra_01.png)
    

**ubuntu-s-1vcpu-1gb-fra1-01** - Maszyna na której znajduje się
    
Działające usługi w ramach Docker:
    
**portainer** - oprogramowanie do zarządzania kontenerami w chmurze publicznej dla projektu.
Adres do zarządzania https://cloud.dexterlab.pl/
Do zalogowania jest potrzebne konto zakładane przez administratora systemu.

![Portainer 01](/images/insta/Portainer_01.png)

Informacja o aplikacjach (obrazów Docker) na serwerze. 

![Portainer 02](/images/insta/Portainer_02.png)

**registry** - rejestr Dockera na potrzeby projektu. Rejestr *Docker* jest zabezpieczony hasłem żeby osoby niepowołane nie mogły się zalobować. Wykorzystujemy wewnęrzny rejestr gdzie prztrzymujemy nasze wszystkie dostępne gotowe aplikacje.

**dexterlab_pl** - strona internetowa projektu oparta na systemie CMS Ghost. 

**cms.dexterlab.pl** - adres strony gotowego produktu dla użytkowników końcowych.

**backend.dezterlab.pl** - adres aplikacji dla pracowników

Dostęp do zasobów znajdujących się na serwerze realizowany jest przez serwer Ngnix.

Oba systemy działają pod kontrolą systemu operacyjnego Ubuntu 18.04.3 (LTS) x64. Na każdej maszynie zainstalowany jest Docker.

#### Środowisko Google Cloud Platform

**instance-1** - naszyna, na któej zainstalowane jest oprogramowanie CMS do testów. Środowisko gdzie przeprowadzana jest instalacja wszystkich komponentów. 

**instance-2** - zawiera kontenery dla instrancji configuracyjnych i dla routera MongoDB 

**instance-3** - zawiera kontentenery dla pierwszego węzeła dla replik bazy MongoDB (rs1)

**instance-4** - zawiera kontentenery dla drugiego węzeła dla replik bazy MongoDB (rs2)

**instance-5** - zawiera kontentenery dla APi naszej aplikacji i aplikacji dla pracowników.


Wszystkie systemy działają pod kontrolą systemu operacyjnego Debian. Na każdej maszynie zainstalowany jest Docker..

#### Przygotowanie maszyny do pracy

Po przygotowaniu systemy operacyjnego do pracy (w przypadku środowisk chmurowych jest to wybór odpowiedniego obrazy) musimy przygotować instancje Docker do poprawnego działania. W tym celu należy zalgować się na każdą z maszyn i wykonać poolecenia poniżej. 

Na początek musimy zaktualizować wszystkie pakiety, żebyśmy działali na odpowiednich wersjach.

`sudo apt update`

Następnie musimy zainstalować niezbędne pakiety do instalacji przez HTTPS

`sudo apt install apt-transport-https ca-certificates curl gnupg2 software-properties-common`

Dodajemy aktyalny klucz GPG dla Docker

`curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -`

Dodanie repozytoriów Dockera do lokalnych APT

`udo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"`

Uaktualniamy bazę pakietów

`sudo apt update` 

Instalujemy pakiet Docker

`sudo apt install docker-ce`

Uruchamiamy edytor i otwieramy plik 

`sudo vi /lib/systemd/system/docker.service`

w linii 

`ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock` 

na koniec dopisujmey 

`-H tcp://0.0.0.0:4243`

Zapisujemy plik, wychodzimy i restartujemy usługę. Następnie przechodzimy do **Portainer** i w zakładce ***Endpoints*** dodajemy nasz serwis docker. Należy wybrać połączenie używająć API.

Na koniec jeszcze musimy zainstalować **docker-compose**

`sudo apt install docker-compose` 


## Opis architektury technicznej rozwiązania

Poniżej opis środowis, tóre były użyte do wytworzenia gotowego produktu. W projekcie zalżało mi żeby można było zarówno skalować rozwiązanie jak i budować nowe środowiska.



### Środowisko testowe

Środowisko testowe jest środowiskiem które zainstalowane jest na stacji roboczej programisty. Aby działać na takim środowisku należy zainstalować bazę MongoDB tak jak jest opisane pod tym linkiem https://docs.mongodb.com/manual/installation/

API i aplikacje backendowe i frontendowe wymagają zainstalowania Node.JS w wersji 12 i klienta Git w celu pobrania źródeł programu. 

Po wykonaniu powyższych czynności przygotowawczych  uruchamiamy konsole, przechodzimy do miejsca gdzie mamy zainstalować nasze oprogramowanie i wykonać kolejno 

`git clone https://github.com/pawel-domanski/CMS-api.git`

`git clone https://github.com/pawel-domanski/CMS-Frontend.git`

`git clone https://github.com/pawel-domanski/CMS-backend.git`

Następnie musimy przygotować plik .env w katalogu CMS-api/server/config (przykładowy plik jest zawarty w tym katalogu) należy uzupełnić zmienne swoimi parametrami.

W katalogu CMS-Frontend/public/static i CMS-backend/public/static jest plik config.json w którym wpisujemy adres naszego api. W naszym przypadku wpisujemy adres http://localhost:3000 . 

Mamy już oprogramowanie. Ze względu na to że są to 3 odzielne aplikacje to należy w uruchamiać w odzielnych konsolach.

CMS-api uruchamiamy poprszez uruchomienie 

`npm run dev`

Aplikację CMS-Frontend jak i CMS-Backend uruchamiamy poprzez 

`npm run serve`


### Środowisko pre-produkcyjne

Środowisko pre-produkcyjne jest zainstalowane na pojedyńczej maszynie (może to być wirtualna) a przybliżony schemat przedstawiony jest na wysunku poniżej.

![Środowisko pre-prod](/images/insta/insta_env_uat.png)

Szczegółowy opis instalacji przedstawiony jest w dalszej części tego dokumentu. Środowisko UAT znajduje się na Instance-1

### Środowisko produkcyjne

Środowisko produkcyjne składa się z jednego klastrta z dwoma replikami opartych na schardach dla systemu baz danych MongoDB. Jednego hosta z aplikacją backendową i API. Trzynodowego klastra Kubernates, na którym działa aplikacja dla klientów.

Baza danych znajduje się na 3 wirtualnych hostach znajdujących się w różnych lokalizacjach, aby zdywersyfikować dostępnosć w przypadku awarii jednego ośrodka danych. 

![Środowisko produckyjne](/images/insta/insta_env_prd.png)

Poniżej budowa klastra Kubernates opartego o 3 nody.

![cluster-1](/images/insta/insta_arch_02.png)

![Cluster-1 nodes](/images/insta/insta_arch_01.png)

![Aplikacja](/images/insta/insta_arch_03.png)


