---
title: Technologie użyte podczas tworzenia środowisk
keywords: rdbms, MongoDB, Systemy Baz Danych, NodeJS, oprogramowanie, linux, google
last_updated: June 3, 2020
tags: [oprogramowanie, NodeJs, MongoDB, Bazy danych]
summary: "W tej sekcji można przeczytać na temat oprogramowania użytego podczas pisania pracy dyplomowej wraz z odsyłaczami, których uzyłem podczas wyboru. WYbór technologii nie jest prosty, szczególnie w takim projekcie."
sidebar: mydoc_sidebar
permalink: technologia_intro.html
folder: technologia
---
### Technologie użyte w procesie budowania środowisk

## Systemy Bazy Danych

Jako miejsca do zapisu artykułów a także innych potrzebnych informacji do działania aplikacji będzie użyta technologia dokumentowych systemów baz danych. W tym przypadku wybrałem że to będzie baza MongoDB. Jako jedna z niewielu baz danych pozwala zbudować środowisko skalowanie horyzontalnie i można połączyć pojedyńcze bazy danych w jeden większy system do zarządzania treścią.

W systemie zarządzania trescią musiałem wziąć pod uwagę kilka kwestii. Sterowanie współbierznością w rozproszonych bazach danych jest dość skomplikowane musimy być pewni że zreplikowane kopie tego samego rekordu-dokumentu są wszędzie identyczne i są zmowyfikowane w ten sam sposób.

-->> Teoria CAP -->> opisać

Jak już wcześniej napisałem zgodnie z Teorią CAP w takim systemie rozproszonym nie da się uzyskać wszystkich trzech porządanych cech i w każdym przypadku musimy przyjąć jedynie dwie dominujące cechy, którymi będzie się harakteryzować nasz system. O ile w przypadku gdzie musimy zadbać o spójność danych będzie prriorytetem użylibyśmy transakcyjnej bazy danych takiej jak Oracle czy Microsoft SQL Server to właśnie ta cecha w naszym przypadku będzie najmniej znacząca więc musiałem użyć systemu baz danych, który odporny na dostępność i częściową utratę partycji. Dlatego sensownym wyborem będzie MongoDB. System taki będzie nie tylko odpowrny na wcześniej wymienione cechy ale także będzie możliwość rozbudowy albo przebudowy środowiska w trakcie kiedy środowisko będzie już działało produkcyjnie. 

Dokumentowe systemy baz danych przechodują dane jako kolekcje dokumenów.

## Serwery Aplikacyjne

## Oprogramowanie użyte do pisania aplikacji