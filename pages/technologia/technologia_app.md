---
title: Serwery aplikacyjne
keywords: NodeJS, oprogramowanie, linux, google, vue.js, frontend, backend
last_updated: June 3, 2020
tags: [oprogramowanie, NodeJs, JavaScript,  VUE.js]
summary: "W tej części przeczytasz trochę więcej o serwerach aplikacji i technologiach użytych do pisania samej aplikacji."
sidebar: mydoc_sidebar
permalink: technologia_app.html
folder: technologia
---
# Serwery aplikacyjne

Do pisania aplikacji użyłem środowisk Node.JS wraz z freamworkami Vue.js i Bootstrap. Uznałem, że napisanie takiej aplikacji w JavaSript będzie sensownym rozwiązaniem ponieważ jest to bardzo elastyczny język, łatwy w rozwoju i utrzymaniu. Zastosowanie powyższych frameworków ułatwiło pracę ponieważ można się skupić wyłącznie na tym co chcemy uzyskać bez wniania w elementy nam tak naprawdę niepotrzebne. Dodatkowym atutem tego jest fakt że aplikacje napisane w takim języku mogą być skonteneryzowane i możliwe szybkie uruchomienie aplikacji końcowej.

W trakcie analizy wyszło że będziemy musieli się borykać z dużym obciążeniem naszej aplikacji i w tym celu aplikacja będzie skonteneryzowana i uchomiona w klastrze Kubernates. Do konteneryzacji będę używać Docker. Konteneryzacja i podzielenie aplikacji na 3 części (API, FronEnd i BackEnd) umożliwia nam lepsze zabezpieczenie przed niepowołanym dostępem. Możemy API i BackEnd wydzielić do innych podsieci powodując że tylko mała część będzie dostępna dla użutkownika końcowego.


## API

Do napisania części odpowiadającej za wymianę danych pomiędzy aplikacją a bazą danych został użyty Node.JS z frameworkiem Express. Powodem wyboru takiej technologi jest fakt że aplikacja nie ma zbędnych funkcjonalności, a to co jest nam potrzeba jest dokładane jako wtyczka. W efekcie mamy szybko działającą aplikację, która może być uruchamiana na każdym środowisku. Dokładając do tego konteneryzację możemy powielać kontenery z działającym API. 

Użyte komponenty:
: **accesscontrol** - biblioteka użyta do kontrolowania dostępów użytkownika.
: **bcrypt** - biblioteka do szyfrowania haseł użytkowników
: **nodemailer** - biblioteka do wysyłania maili
: **email-templates** - biblioteka do generowania maili w formacie HTML
: **jsonwebtoken** - biblioteka do generowania i zarządzania tokenami sesji
: **mongoose** - zestaw bibliotek do zarządzania systemem baz danych MongoDB
: **gridfs-stream** - biblioteka do zapisywania obrazów w bazie MongoDB

## Aplikacje klienckie

Do napisania aplikacji klienckich został użyty framework Vue.js wraz z frameweorkiem Bootstrap. Powodem użycia tych technologi jest to, że aplikacje są bardzo lekkie i szybko skalowalne. Framework jest dosć prostym narzędziem do programowania w tym języku i nie ma tak jak w przypadku Node.JS za dużo zbędnych komponentów. Późniejsze wsparcie dla tego framweorka będzie dosyć proste. W przypadku Bootstrap możemy w szybki i łatwy sposób zmieniać wygląd dla całej strony, a jego popularność sprawia, że będzie dosć długo wspierany.

