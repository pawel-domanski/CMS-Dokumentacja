---
title: Szczegółowy opis encji i atrybutów
keywords: database, baza danych, schemat, opis encji, encje
last_updated: June 3, 2020
tags: [database, baza danych, schemat, opis encji, encje]
summary: "Opis szczegłółowy szystkich encji w bazie wraz z dokłądnym opisem atrybutów"
sidebar: mydoc_sidebar
permalink: technologia_db_schemat.html
folder: technologia
---
# Szczegółowy opis encji i atrybutów

## Kolekcja **abouts**

> Kolekcja przetrzymująca informacje dotyczące portalu. Są tam zawarte ustawienia takie jak, nazwa portalu, adresy do mediów społecznościowych, polityka bezpieczeństwa i informacje o samym. portalu.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
| _id  | document  |  true |  PK |  Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system. |
|key|string|true||Klucz pod jakim znajduje się dane ustawienie.|
|value|string|true||Wartość ustawienia.|
|text|string|true||Opis wyświetlający się w aplikacji BackEnd|

## Kolekcja **articles**

> Kolekcja przetrzymująca dokumenty z artykułami dla systemu

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|keywords|array|true|FK|Tablica przetrzymująca wartości słów kluczowych z kolekcji keywords|
|createddate|document|true||Data utworzenia dokumentu. Jest wstawiana automatycznie przy tworzeniu dokumentu.|
|published|boolean|true||Czy dokument jest udostępniony publicznie. Pole przyjmuje wartości true lub false.|
|comment|array|true|FK|Tablica wartości ObjectID z komentarzami z kolekcji comments|
|body|string|true||Treść naszego artykułu.|
|image|string|true|FK|Wartość ObjectID z kolekcji fs.files|
|category|document|true|FK|Wartość ObjectID z kolekcji categories. Pole przetrzymuje informacje do jakiej kategorii został przypisany artykuł.|
|createdby|document|true|FK|Wartość ObjectID z kolekcji users. Pole przetrzymuje referencje dla autora artykułu.|
|__v|document|true||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|


## Kolekcja **keywords**

> Kolekcja przetrzymująca słowa kluczowe artykułów.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|name|string|true||Słowo kluczowe|
|__v|document|false||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|


## Kolekcja **categories**

> Kolekcja przetrzymująca informacje o kategoriach artykułów.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|key|string|true||Nazwa kategorii|

## Kolekcja **messagess** 

> Kolekcja przetrzymująca wiadomości wpisane w formularzu kontaktowym, a także odpowiedzi jakie zostały udzielone.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|status|string|true||Status wiadomości. Pole typu enum przyjmuje wartości ['new', 'active', 'closed'],|
|createdtime|document|true||Data utworzenia dokumentu.|
|name|string|true||Imię użytkownika wpisany w formularzu kontaktowym.|
|from|string|true||Adres email podany w formularzu kontaktowym przez użytkownika FrontEnd|
|subject|string|true||Temat wiadomości, którą wpisuje użytkownik w aplikacji FrontEnd|
|text|string|true||Wiadomość z formularza kontaktowego.|
|__v|document|true||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|
|message_reply|string|false||Wiadomość, któa została wysłana|
|user|document|false|FK|_id użytkownika, który odpowiedział na wiadomość|

## Kolekcja **newsletters**

> Kolekcja przetrzymująca informację o subskrybentach newslettera.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|__v|document|true||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|
|isPolicyTrue|boolean|true||Czy subskrybent zaakceptował potwierdzenie przeczytania polityki bezpieczeństwa.|
|status|boolean|true||Czy użytkownik jest aktywny i czy będzie dostawać wiadomości|
|date|document|true||Data wpisania sie do newsletter|
|name|string|true||Imię osoby wpisującej się na newsletter|
|email|string|true||E-mail, na który mma przychodzić newsletter|
|token|string|true||Token do potwierdzenia wpisania do newslettera|
|tokenExpiration|document|true||Data kiedy wygaśnie token|

## Kolekcja **news**

> Kolekcja przetrzymuje informacje o newsletterach jakie są dostępne w systemie.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|createddate|document|true||Data utworzenia newslettera.|
|status|string|true||Status newslettera. Atrybut typu enum przetrzymuje wartości ['new', 'send','error'],|
|subject|string|true||Temat naszego newslettera|
|message|string|true||Wiadomość do naszego newslettera.|
|createdby|document|true|FK|Data utworzenia newslettera.|
|senddate|document|false||Data wysłania newslettera.|
|__v|document|true||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|

## Kolekcja **comments**

> Kolekcja przetrzymuje komentarze do artykułów.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|text|string|true||Tekst komentarza.|
|name|string|true||Imię wpisującego komentarz.|
|email|string|true||E-mail wpisującego komentarz.|
|__v|document|true||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|

## Kolekcja **users**

> Kolekcja przetrzymuje informację o redaktorach i administratorach systemu.

|  Atrybut | Typ  | REQ  | KEY  | Opis  |
|---|---|---|---|---|
|_id|document|true|PK|Unikalny numer id (ObjectID) dokumentu. Wartość jest wstawiana automatycznie przez system.|
|__v|document|true||Pole przetrzymuje wersje dokumentu. Wartość jest generowana automatycznie przez system.|
|newsletter|boolean|true||Atrybut który przetrzymuje informację czy użytkownik (pracownik) chce otrzymywać newsletter.|
|status|string|true||Status użytkownika. Pole typu enum może przyjmować wartości ['OK', 'Locked', 'NoConfirm', 'Disabled'],|
|role|string|true||Rola jaką ma uzytkownik. Pole typu enum i może przyjmować wartości ['basic', 'autor', 'admin'],|
|createdtime|document|true||Data utworzenia użytkownika.|
|updatedtime|document|true||Data aktualizacji rekordu z uzytkownikiem.|
|email|string|true||E-mail użytkownika. Wykorzystywany jest zarówno jako login do systemu jak i adres newslettera.|
|password|string|true||Zakodowane hasło użytkownika.|
|name|string|true||Imię i nazwisko użytkownika. Może to być pseudonim pod którym pisze teksty.|
|photo|string|true||Nazwa obrazu.|
|accessToken|string|true||Access Token dla użytkownika. Wartość wymagana jest do tego, żeby użytkownik nie musiał się logować za każdym razem.|
