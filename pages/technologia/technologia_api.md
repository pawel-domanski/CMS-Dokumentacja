---
title: Szczegółowy opis API
keywords: api, technologie, projekt, frontend, backend
last_updated: August 21, 2020
tags: [api]
summary: "Poniżej przedstawiam kompletny opis API zastosowane w projekcie."
sidebar: mydoc_sidebar
permalink: technologia_api.html
folder: technologia
---
# Szczegółowy opis API

API jest napisane w oparciu o język JavaScript i działa na Node.JS. Opis użytych technologii opisana jest we wcześniejszych rozdziałach tego dokumentu. Poniżej jest przedstawiona funkcjonalność każdej funkcji API.

## root
    Główny adres dla API
**/**   
> Wartoś standardowa i odpowiedź z serwera po uruchomieniu ścieżki Root

## about/
    Funkcje obsługujące wszystko co jest związane z artykułami w systemie.

**get /**   
> Zwraca warości dla ustawień strony.

**get /:key**   
> Zwraca pojedyczy dokument warosć dla podanego atrybutu key.

**get /setting/:key**   
> Zwraca nam wartość pojedyczego dokumentu po podaniu ID dokumentu.

**post /:key**   
> Zapisuje atrybut value dla odpowiedniego ID dokumentu. Zwraca status polecenia.

## articles:    
    Funkcje obsługi artykułów w systemie

**post /**   
> Zapisuje nowy artykół do bazy. Zwraca obiekt i status zapisu.

**get /**   
> Powiera listę opublikowanych artykułów.

**get /all**   
> Pobiera wszystkie artykuły z bazy dla panelu administratora.

**get /:idArticle**   
>  Pobiera pojedyńczy artykuł z bazy.

**post /comment/:idArticle**   
>  Zapisuje komentarz dla podanego artukułu.

**get /comment/:idArticle**   
> Pobiera komentarze dla artykułu o podanym ID.

**post /:idArticle**   
> Aktualizuje atykuł o podanym ID.

**post /publish/:idArticle**   
> Zmienia wartość published na true dla artykułu o podanym ID.

**post /unpublish/:idArticle**   
> Zmienia wartość published na false dla artykułu o podanym ID.

**post /delete/:idArticle**   
> Kasuje artykuł z bazy o podanym ID.

**get /top/:top**   
> Wyświetla listę top 10 najbardziej poczytnych artykułów.

**get /category/:idCategory**   
> Wyświetla listę artykułów dla danej kategorii

**get /keywords/:idKeyword**   
> Wyświetla listę artykułów dla danego słowa kluczowego


## user/
    Funkcje obsługujące wszystko co jest związane z użytkownikiem.

**post /signup**   
> Tworzy użytkownika o podanych parametrach

**post /login**   
> Loguje użytkownika do systemu po podaniu email i hasła

**get /:userId**   
> Pobiera informację o użytkowniku o podanym ID

**post /**   
> Funkcja tworząca użytkownika na potrzeby BackEndu

**get /**   
> Pobiera listę dokumentów z użytkownikami

**post /:userId**   
> Aktualizacja użytkownika o podanym ID

**post /delete/:userId**   
> Skasowanie użytkownika o podanym ID

## message/
    Funkcje obsługujące wiadomości wysyłane poprzez użytkownika z aplikacji FrontEnd

**post /**   
    Wysłanie wiadomości

**get /**   
> Wyświetlenie listy wiadomości 

**post /:messageId**   
> Udzielenie odpowiedzi na wiadomość wysłaną z aplikacji FrontEnd. Wysyłanie wiadomości o podanym ID

**get /:messageId**   
> Pobranie dokumentu z wiadomością o podanym ID

## image/
    Funkcje do obsługi obrazów w bazie danych.

**post /**   
> Zapisanie obrazu w bazie danych

**get /:id**   
> Pobranie obrazu z bazy

## newsletter:
    Funkcje obsługujące subskrybenów w systemie.

**post /:token**   
> Potwierdzenie uczestnictwa w newsletterze przez użytkownika. Zatwierdzanie poprzez podanie parametru jakim jest tokoen generowany w systemie i wysyłanie mailem do użytkownika.

**post /unregister/:email**   
> Wyrejestrowanie się z newslettera dla podanego adresu e-mail

**post /unregister/confirm/:token**   
> Potwierdzenie wyrejestrowania się z  newslettera przez użytkownika. Zatwierdzanie poprzez podanie parametru jakim jest tokoen generowany w systemie i wysyłanie mailem do użytkownika.


## keywords/
    Funkcje obsługujące słownik słów kluczowych w systemie.

**get /**   
> Pobranie listy dostępnych słów kluczowych

**post /**   
> Dodanie do słownika nowego słowa kluczowego


## category/
    Funkcje obsługujące słownik kategorii w systemie.


**get /**   
> Pobranie listy dostępnych kategorii.

**post /**   
> Dodanie do słownika nowej katagorii.

**post /delete/:idCat**   
> Wykasowanie kategorii o podanym ID

## subscribers/
    Obsługa subskrybentów z poziomu aplikacji BackEnd

**get /**   
> Pobranie listy wszystkich subskrybentów

**post unaccept/:_id**   
> Cofnięcie akceptacji dla subskrybenta o podanym ID.

**post /accept/:_id**   
> Zaakceptowanie subskrybenta o podanym ID.

**post /delete/:_id**   
> Skasowanie subskrybenta o podanym ID

## news:
    Funkcje obsługi newsletterów w aplikacji.

**get /**   
    Pobranie listy wszystkich newsletterów znajdujących się w bazie.

**post /:newsId**   
> Aktualizacja newslettera w bazie. Nie jest to jednoznaczne z wysłaniem do subskrybentów.

**post /delete/:newsId**   
> Skasowanie newslettera o podanym ID z systemu.

**get /:newsId**   
> Pobranie dokumentu o newsletterze o podanym ID

## send/
    Funkcje obsługujące wysyłanie newsletterów.

**post /:newsId**   
> Wysłanie newslettera o podanym ID i odznaczenie go jako wysłany.