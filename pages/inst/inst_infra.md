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
### Instrukcja instalacji 


## Infrastruktura


Oprogramowanie działa na serwerach wirtualnych działających w chmurze obliczeniowej. Celowo korzystam z kilku niezależnych dostawców aby zaproponować wszechstronność i zwinne podejscie o tematu jakim jest wysoka dostępność systemu zarządzania treścią. Pomiędzy sieciami jest utworzona reguła firewall i możliwość logowania się do poszczególnych części jest zabezpieczony.

Podzielona jest na dwie części. 

Środowisko Digital Ocean (Frannkfurt)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

ubuntu-s-1vcpu-1gb-fra1-01
""""""""""""""""""""""""""

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


![Portainer 01](/images/insta/Portainer_01.png)

Informacja o aplikacjach (obrazów Docker) na serwerze. 

![Portainer 02](/images/insta/Portainer_02.png)

**registry** - rejestr Dockera na potrzeby projektu. Rejestr *Docker* jest zabezpieczony hasłem żeby osoby niepowołane nie mogły się zalobować.

**dexterlab_pl** - strona internetowa projektu oparta na systemie CMS Ghost. 


ubuntu-s-1vcpu-1gb-fra1-02
""""""""""""""""""""""""""

Serwer (droplet) z systemem operacyjnym Debian. 

1 GB Memory / 25 GB Disk / FRA1 - Ubuntu 18.04.3 (LTS) x64

Na serwerze jest zainstalowany Docker.

Maszyna jest odpoweidzialna za zarządzanie kodem żródłowym poprzez oprogramowanie GitLab. Zainstalowana jest wersja Community Edition dostępna pod adresem https://git.dexterlab.pl/ Dostęp realizowany jest przez konta wewnętrzne systemu i zakładane są przez administratora systemu.

GitHub
""""""
Dokumentacja do projektu jest zamieszczona na GitHub Pages i można przeczytać pod adresem https://doc.dexterlab.pl/
TODO: Dopisać instrukcję instalacji i kjonfiguracji GitLab'a


Środowisko Google Cloud Platform 
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

W Google Cloud Platform znajduje się całość aplikacji zarówno środowiska testowe jak i aplikacji. Całość ma połączenie pomiędzy prywatnymi sieciami w celu zachowania odpowiedniego bezpieczeństwa.

instance-1
""""""""""

Maszyna z systemem opracyjnym Debian 

1,7 GB Memory / 10 GB Disk / europe-west3-c - Ubuntu 18.04.3 (LTS) x64

Na instancji zainstalowany jest Docker.

Maszyna będzie odpowiedzialna za hostowanie środowisk testowych bez użycia wysokiej dostępności i skalowalności.

TODO: Instrukcja instalacji środowiska testowego. Dla Mongo. Sprawdzić czy nie można podłączyć gitlaba do GCP żeby mógł klonować maszyny. Mongo może być przecież spreparowane na nasze potrzeby i zapisana w naszym rejestrze :) Nie musimy za kaym razem korzystać z sieciowej wersji bazy danych. Ba możemy nawet załadować specjalne dane, żeby już były załadowane. 

