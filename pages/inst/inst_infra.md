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




Oba systemy działają pod kontrolą systemu operacyjnego Ubuntu 18.04.3 (LTS) x64. Na każdej maszynie zainstalowany jest Docker..


>>>>>>> refs/remotes/origin/master
* Środowisko bazodanowe

* Środowisko aplikacyjne

### Środowisko testowe

### Środowisko pre-produkcyjne

### Środowisko produkcyjne


#### GitHub

Dokumentacja do projektu jest zamieszczona na GitHub Pages i można przeczytać pod adresem https://doc.dexterlab.pl/

Maszyna będzie odpowiedzialna za hostowanie środowisk testowych bez użycia wysokiej dostępności i skalowalności.

TODO: Instrukcja instalacji środowiska testowego. Dla Mongo. Sprawdzić czy nie można podłączyć gitlaba do GCP żeby mógł klonować maszyny. Mongo może być przecież spreparowane na nasze potrzeby i zapisana w naszym rejestrze :) Nie musimy za kaym razem korzystać z sieciowej wersji bazy danych. Ba możemy nawet załadować specjalne dane, żeby już były załadowane. 

