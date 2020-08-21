---
title: Architektura systemu
keywords: oprogramowanie, linux, google, vue.js, frontend, backend, Architektura
last_updated: June 3, 2020
tags: [oprogramowanie, architektura]
summary: "Poniżej opis architektury systemu."
sidebar: mydoc_sidebar
permalink: technologia_arch.html
folder: technologia
---
# Architektura systemu

Aplikacja zarówno każdy jej element czyli API, Aplikacje klienckie jak większość oprogramowania oparta jest o model MVC (Model View Controller) i jest przez to najpopularniejszym wzorcem projektowym. Podążając za definicją model opiera się na trzech składowych elementach 
: **Model** - jest to reprezentacja logiki biznesowej opisująca strukturę danych, powiązania pomiędzy innymi jak i mapowania relacyjno-obiektowe.   
: **View**- jest to nic innego jak nasza wartswa prezentacji. Widoki aplikacji pobierają dane z widoków przedstawiają dane z model użytkownikowi, a także  modyfikuje je.   
: **Controller** - jest to cała logika obsługująca warstwę użytkownika. Mapuje odpowiednie metody Modelu i definiuje warstwę abstrakcji


![MVC](/images/technologia/technologia_arch.png)

# Architektura infrastruktury

