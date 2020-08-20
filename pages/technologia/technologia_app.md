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
## Serwery aplikacjyjne

Do pisania aplikacji użyłem środowisk Node.JS wraz z freamworkami Vue.js i Bootstrap. Uznałem, że napisanie takiej aplikacji w JavaSript będzie sensownym rozwiązaniem ponieważ jest to bardzo elastyczny język, łatwy w rozwoju i utrzymaniu. Zastosowanie powyższych frameworków ułatwiło pracę ponieważ można się skupić wyłącznie na tym co chcemy uzyskać bez wniania w elementy nam tak naprawdę niepotrzebne. Dodatkowym atutem tego jest fakt że aplikacje napisane w takim języku mogą być skonteneryzowane i możliwe szybkie uruchomienie aplikacji końcowej.

W trakcie analizy wyszło że będziemy musieli się borykać z dużym obciążeniem naszej aplikacji i w tym celu aplikacja będzie skonteneryzowana i uchomiona w klastrze Kubernates. Do konteneryzacji będę używać Docker. Konteneryzacja i podzielenie aplikacji na 3 części (API, FronEnd i BackEnd) umożliwia nam lepsze zabezpieczenie przed niepowołanym dostępem. Możemy API i BackEnd wydzielić do innych podsieci powodując że tylko mała część będzie dostępna dla użutkownika końcowego.