---
title: Aplikacja BackEnd dla redaktorów i administratorów
keywords: dokumentacja, instrukcja, backend, admin
last_updated: August 14, 2020
tags: [dokumentacja, backend, instrukcja]
summary: "Pełna dokumentacja użytkownika administracyjnego."
sidebar: mydoc_sidebar
permalink: docs_backend.html
folder: docs
---

# Aplikacja BackEnd dla redaktorów i administratorów

## Logowanie do aplikacji

Po uruchomieniu aplikacji wejściu na adres administratora. Najpierw musimy się zalogować. Do panelu mają dostęp wyłącznie osoby, które mają odpowiednie uprawnienia i posiadają konto w systemie. 

![Logowanie do aplikacji](/images/docs/docs_admin_01.png)

## Okno główne

Pierwszym oknem po zalogowaniu jest główne okno naszej aplikacji. Dopierow w tym miejscu możliwa jest dalsza nawigacja.

![Dashboard aplikacji](/images/docs/docs_admin_02.png)

## Lista artukułów

Po wejściu w zakładkę **Artykuły** mamy dostępną listę wszystkich artykułów jakie są dostępne w systemie i mamy podgląd na wszystko co się dzieje. Z tego okna możemy kasować artykuły z systemu lub akceptować do publikacji (opcje dostępne wyłącznie dla administratorów). Możemy także wejść w edycję lub dodać nowy artukuł do bazy.

![Lista artykułów](/images/docs/docs_admin_03.png)

> W momencie jakiegokolwiek kasowania danych dostaniemy poniższy komunikat ***(opcja kasowania dostępna wyłącznie dla administratorów)***
> ![Okno modalne kasowania](/images/docs/docs_admin_03a.png)

## Dodawanie i edycja artukułu.

Jeśli jesteśmy w jednej z opcji możemy zmienić wszystkie parametry dokumentu. Opcje wyłączone są elementami, które są dodawane automatycznie. W polu **Tekst artykułu** możemy stosować normalne formatowanie jakie jest dostępne w edytorach.

> W przypadku zmiany lub załadowania obrazka wyróżniającego należy wybrać plik, a następnie załaduj obrazek dopiero zapisywać dokument. 

![Dodawanie/edycja artykułu](/images/docs/docs_admin_04.png)

## Lista użytkowników

***(opcja wyłącznie dla administratora)***
Poniżej wygląd listy użytkowników któzy widnieją w naszym systemie. Operacje jakie możemy wykonywać są identyczne jak w przypadku artukułu. 

W aplikacji mamy do dyspozycji 2 użytkowników jest to Redaktor i Administrator.

![Lista użytkowników](/images/docs/docs_admin_05.png)

## Dodawanie i edycja użytkownika

Analogicznie jak w przypadku artukułu edytujemy użytkownika

![Dodawanie/edycja użytkownika](/images/docs/docs_admin_06.png)

## Zarządzanie kategoriami

Możemy przedlądać i z tego poziomu możemy zarówno dodawać jaki i kasować kategorie ***(opcja kasowania wyłącznie dla administratora)***

![Zarządzanie kategoriami](/images/docs/docs_admin_07.png)

## Zarządzanie słowami kluczowymi

Możemy przedlądać i z tego poziomu możemy zarówno dodawać jaki i kasować słowa kluczowe ***(opcja kasowania wyłącznie dla administratora)***

![Zarządzanie słowami kluczowymi](/images/docs/docs_admin_08.png)

## Ustawienia portalu

W tym miejscu możemy zarządzać ustawieniami naszego portalu. Co jest widoczne na stronie **O nas**, zmodyfikować **Politykę bezpieczeństwa**, czy też zmienić wersję lub linki do portali społecznościowych.

![Ustawienia portalu](/images/docs/docs_admin_09.png)

## Edycja ustawień

***(opcja wyłącznie dla administratora)***
Możemy zmienić wyłącznie wartość dla danego parametru.

![Edycja ustawień portalu](/images/docs/docs_admin_10.png)

## Lista subsktybentów newslettera

Możemy w tym miejscu przeglądać listę wszystkich subskrybentów naszego newslettera. Z tego poziomu możemy kasować subskrybenta lub zaakceptować ręcznie ***(obie opcje wyłącznie dla administratora)***

![Subskrybenci newslettera](/images/docs/docs_admin_11.png)

## Lista wiadomości newslettera

Po wejściu w tą cześć aplikacji widzimy listę wszystkich wiadomości do użytkowników newslettera. Z poziomu tego okna mamy możliwość zarówno wejść w dodawanie nowej wiadomości jak i wejść w edycję, skasować lub wysłać wiadomość *** (kasowanie i wysyłanie wyłącznie dla administratorów)

![Lista wiadomości newslettera](/images/docs/docs_admin_12.png)

> Poniżej komunikat z potwierdzeniem wysłania newslettera.
> ![Komunikat potwierdzenia wysłania newslettera](/images/docs/docs_admin_12a.png)

Wiadomość zostanie wysłana do wszystkich użytkowników zapisanych do newslettera, jak i użytkowników (pracownikó), którzy zaznaczyli chęć dostawania newslettera.

## Dodawanie i edycja wiadomości

W tym miejscu mamy możliwość wpisania tematu i wiadomości. W przypadku, kiedy wiadomość została już wysłana nie można edytować.

![Dodawanie newslettera](/images/docs/docs_admin_13.png)

## Lista wiadomości

Z aplikacji FrontEnd jest możliwość wysłania przez każdego oglądającego wiadomości do prowadzących serwis. W tym miejscu są one dostępne.

![Lista wiadomości ze strony](/images/docs/docs_admin_15.png)

## Edycja i wysłanie odpowiedzi

Po tym jak na poprzednim ekranie naciśniemy opcję z tym, że chcemy odpowiedzieć zostaniemy przeniesieni do ekranu pisania odpowiedzi. Po zakończeniu wpisywania odpowiedzi zatwierdzamy klawiszem **Wyślij**, odpowiedź zostanie automatycznie wysłana na e-mail podany podczas wpisywania w aplikacji FrontEnd. Jeśli odpowiedź została udzielona dopowiedź i przez kogo. 


![Wysyłanie odpowiedzi do czytelnika](/images/docs/docs_admin_16.png)