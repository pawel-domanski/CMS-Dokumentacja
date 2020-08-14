---
title: Technologie użyte podczas tworzenia środowisk
keywords: rdbms, MongoDB, Systemy Baz Danych, NodeJS, oprogramowanie, linux, google
last_updated: June 3, 2020
tags: [oprogramowanie, NodeJs, MongoDB, Bazy danych]
summary: "W tej sekcji można przeczytać na temat oprogramowania użytego podczas pisania pracy dyplomowej wraz z odsyłaczami, których uzyłem podczas wyboru. Wybór technologii nie jest prosty, szczególnie w takim projekcie."
sidebar: mydoc_sidebar
permalink: technologia_intro.html
folder: technologia
---
### Technologie użyte w procesie budowania środowisk

## Systemy Bazy Danych

Jako miejsca do zapisu artykułów a także innych potrzebnych informacji do działania aplikacji będzie użyta technologia dokumentowych systemów baz danych. W tym przypadku wybrałem bazę MongoDB. Jako jedna z niewielu baz danych pozwala zbudować środowisko skalowanie horyzontalnie i można połączyć pojedyńcze bazy danych w jeden większy system do zarządzania treścią.

## Serwery Aplikacyjne

Oprogramowanie będzie napisane pod NodeJS dla API i Vue.JS dla aplikacji FrontEnd i BackEnd. 
Aplikacje będą uruchamiane w Docker, który będzie potem mógł być podłączony do klastrów Kubernates w celu zapewnienia wysokiej dostępności.
Aby odseparować wszystkie warstwy i zabezpieczyć całość połączenie będzie odbywało się poprzez silnik NGinx. Na środowisku testowym będzie można się podłączyć do aplikacji bezpośrednio. W przypadku środowisk produkcyjnych będzie użyty load balancer służący do rozdzielania ruchu pomiędzy poszczególne klastry Kubernates. W przypadku naszej instalacji będzie postawiony jeden klaster Kubernates ale będzie można w przyszłości rozwijać środowisko.



## Oprogramowanie użyte do pisania aplikacji

Do pisania aplikacji tak naprawdę wystarczy nam zwykły notatnik ponieważ oprogramowanie jest pisane w JavaSript, jednakże aby pisało się to przyjemniej i żeby kod był od razu sprawdzany i formatowany do pisania użyłem następujących narzędzi:

Do API użyty był Visual Code z zainstalowanym pligine Prettier, ESLint, Vetur, VS COde for Node.js.
Zaletą tego programy jest fakt że jest to narzędzie darmowe i działa na wszystkich systemach ze środowiskiem Desktop.

Do części FrontEnd i BackEnd użyłem WebStorm w podstawowej wersji. Umożliwił mi dodatkowo uruchamianie i sprawdzanie na bieżąco całego kodu. 

Do bazy danych nie ma potrzeby stosowania dodatkowego narzędzia ponieważ sama aplikacja zakłada konkretne kolekcje w bazie. Do przeglądania obketów w bazie użyłem narzędzia **MongoDB Compass**. Bardzo pomocnym narzędziem okazał się serwis https://cloud.mondodb.com w którym zbudowałem wersję deweloperską aplikacji. Pozwoliło mi to uniknąć instalowania dodatkowego opgroramowania i mogłem spokojnie devalopować aplikację z każdego komputera, na którym pracowałem.

![Diagram przypadków dla użykowników BackEnd](/images/technologia/Technologia_tools_MongoDB_01.png)

