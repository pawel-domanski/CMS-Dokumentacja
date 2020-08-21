---
title: Infrastruktura
keywords: NodeJS, oprogramowanie, linux, google, vue.js, frontend, backend, Infrastruktura, Docker
last_updated: June 3, 2020
tags: [oprogramowanie, NodeJs, JavaScript,  VUE.js]
summary: "Tutaj wybór technologii do infrastruktury."
sidebar: mydoc_sidebar
permalink: technologia_infra.html
folder: technologia
---
# Infrastruktura

W dużej mierze w projekcie oparłem się na tym, żeby środowiska były mocno skalowalne, czyli tak że jeśli jest większe obciążenie to w szybki sposób możemy reagować i powoływać nowe maszyny do życia. W przypadku systemu zarządzania treścią nie jesteśmy w stanie określić czasu kiedy będziemy mieli większe lub mniejsze obciążenie. W tym przypadku jest wiele czynników niezależnych, a co za tym idzie środowisko w większości czasu może być przeszacowane a w niewielkim jego czasie być wykorzystywane z pełną mocą. Zastosowanie tradycyjnego podejścia do budowania środowisk będziemy musieli zastosować sporo środków pieniężnych do uruchomienia środowiska. Wszystkie serwery fizycznie usytuowane w siedzibie klienta, wraz z jego ekspoatacją i potem utrzymaniem może być dość duża. Dodatkowo w przypadku kiedy mamy znaczący wzrost obciążenia nie jesteśmy szybko uruchamiać szybko nowych serwerów. Czas w trakcie którego będą problemy wydajnościowe może się odbić finansowo na firmie. Kolejnym bardzo ważnym faktem jest to, że projekt może nie mieć takiego zainteresowania i może być zamknięty i wtedy zostajemy z maszynami, które nie są do niczego potrzebne.

Zastosowanie konteneryzacji i usług działających w chmurze może nam szybko przeciwdziałać takim problemom. W przypadku takim nie jesteśmy właścicielami maszyn, nie działają one w naszej sieci i naszej serwerowni. Jako firma wynajmujemy je na określony czas z określonymi parametrami, a co za tym idzie możemy w przypadku fiaska zrezygnować z usług lub w przypadku sukcesu szybko się rozwijać, ponieważ powołowywanie nowych maszyn jest bardzo szybkie. 

#### Czym jest konteneryzacja? 

Tutaj będziemy mówić o kontenerach Docker'a.  Docker jest to platforma, która pozwoli hostować odseparowane aplikacje, bazy danych na jednym hoście. Uporządkowanie jest poprzez tworzenie dla danej aplikacji w poszczególnych kontenerach, które łatwiej jest administrować. Pojedynczy kontener jest swojego rodzaju odrębnym systemem. Jest to jak dla mnie bardzo ciekawe rozwiązanie ponieważ nie ma tutaj samej warstwy systemu. W przypadku Dockera nie mamy takiego problemu jak instalacja i utrzymanie systemów operacyjnych dla każdej sytuacji.

Poniżej porównanie Dockera do standardowej wirtualizacji.

![VM vs Docker](/images/technologia/technologia_docker.png)

Dodatkowo w przypadku standaryzacji aplikacji i użyciu kontenerów, infrastruktura jest bardzo elastyczna i w projekcie użyłem zarówno kontenerów dla aplikacji, które mają małe obciążenie i instancje działają jako stand-alone konetnery z aplikacją, jak i w klastrach Kubernates jak to jest dla aplikacji działającej na produkcji.


