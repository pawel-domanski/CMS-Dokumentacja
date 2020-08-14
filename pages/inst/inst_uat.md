---
title: Instalacja środowiska testowego
keywords: linux, infra, instalacja
last_updated: August 11, 2020
tags: [oprogramowanie, system, docker, ngingx]
summary: "Przechodzimy do instalacji oprogramowania i wszyskitgo co jest z tym związane."
sidebar: mydoc_sidebar
permalink: inst_uat.html
folder: insta
---
# Instalacja środowiska UAT

## Instalacja środowiska bazodanowego

Środowisko do testów jest znacząco różne od tego co znajduje się na środowisku produkcyjnym. Wyszedłem z założenia, że nie ma konieczności tworzenia duplikatu środowiska produkcyjnego ponieważ w przypadku dużych środowisk produkcyjnych nie jest możliwe aby takie środowisko przygotować. Dlatego poniżej znajduje się opis instalacji środowiska do testów. Rżnica jest taka, że nie ma w tym środowisku obsługi wyokiej dostępności.

Przed przystąpieniem do instalacji musimy przygotować maszynę na której będzie instalowana aplikacja. Minimalne wymagania takiej maszyny to:

> 1 CPU         
> 2GB Pamięci RAM       
> 10GB miejsca na dane i aplikacje      
> sieć z przydzielonym IP       
> dostęp do internetu.          


Nasze środowisko będziemy instalować w chmurze Goolgle Cloud. Może to być lokalna maszyna z systemem operacyjnym Linux. Instrukcja jest przygotowana dla dystrybucji Ubuntu.

### Czynności przygotowawcze

Przygotowujemy sobie śdodowisko do instalacji. Wszystkie informacje do instalacji zawarte są w odpowiednich katalogach na **GitHub**
Połączenie do repozytorium odbywa się przy pomocy kluczy __SSH__ 

### Instalacja Docker

Przed przystąpieniem do instalacji musimy na maszynie dodać źródła, z których będziemy mogli zainstalować Docker.

W tym celu należy uruchomić poniższe polecenie. Polecenie wymaga uprwnień administratora.

`sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
         $(lsb_release -cs) \
        stable"
`
Wykonujemy update pakietów

`sudo apt-get update`

I instalujemy oprogramowanie Docker

`sudo apt-get install docker-ce`

Po zainstalowaniu musimy wprowadzić zmianę w konfiguracji Docker'a. Do pliku z marametrami musimy dodać informację o tym, żebyśmy mogli zarządzać instancją z internetu.
W tym celu należy uruchomić

`sudo vi /lib/systemd/system/docker.service`

wyszukać **ExecStart** i dodać na końcu **-H tcp://192.168.186.27:4243**

Zapisać plik i wyjść z edytora. Żeby zmiany były widoczne należy zrestartować serwis Docker.

### Konfiguracja sieci

Aby można było bezpiecznie korzystać z naszego oprogramowania należy dla aplikacji i bazy danych wydzielić sieć w ramach, której będą się komunikować. W tym celu użyjemy polecenia:

`sudo docker network create --subnet=10.11.0.0/16  --opt com.docker.network.driver.mtu=9216  --opt encrypted=true mongo_default`

Musimy wcześniej ustalić jaka sieć będzie możliwa do ustawienia. W naszym przypadku będzie to 10.11.0.0 

### Konfuguracja docker pod MongoDB

Przed przystąpieniem do instalacji bazy danych należy utworzyć volumen, który będzie nam służyć do transerowania danych pomiędzy poszczególnymi kontenerami. Będą to kliki instalacyjne lub konfiguracyjne wspólne dla wszystkich kontenerów.

`docker volume create --name mongo_storage`

> Warto sobie utworzyć także tymczasowy kontener do przegrywania tam danych. 

Przegrywamy na volumen mongo_storage do katalogu /opt/keyfile/ plik ./software/scripts/mongo/keys/mongo-key, a następnie zmieniamy właściciela na 999 i uprawnienia do tego pliku na 600. W przeciwnym przypadku nie będziemy mogli zainstalować bazy MongoDB.

Instalujemy na maszynie docker-compose, które pomoże w szybszej instalacji maszyn i możemy utuchomić 

`sudo docker-compose -f ./software/scripts/mongo/init_dev_db.yaml run -d `

Polecenie uruchomi nam konfigurator instancji MongoDB zgodnie z naszymi wymaganiami. Nazwa kontenera nazywa się ***mongodev***

### Konfiguracja bazy MongoDB pod instalację aplikacji

Logujemy się na maszynę poleceniem 

`sudo docker exec -it mongodev /bin/bash`

Następnie logujemy się do instancji MongoDB

`mongo admin`

I uruchamiamy skrypt tworzący użytkownika na prawach administratora.

`db.createUser({user: 'administrator', pwd: '<P@ss0rd>', roles:[{role:'root',db:'admin'}]});`

Używając hasła powyżej loggujemy się do instancji MongoDB

`mongo admin -u administrator -p <P@ss0rd>`

Przechodzimy do bazy cms. Uzycie ***use nazwa_bazy*** spowoduje połączenie się do bazy danych. Jeśli tej bazy danych nie ma klient utworzy ją. Jeśli jednak nie utworzymy żadnego obiektu w nowej bazie zmiany nie zostaną zapisane i nie zostanie utworzona nowa baza.

`use cms `

Następne polecenie utowrzy nam użytkownika **cms**, który będzie nam służyć do połączenia się naszej aplikacji do bazy danych.

`db.createUser({user: 'cmsuser', pwd: 'ChangeOnInstall', roles:[{role:'dbOwner', db:'cms'}]})`

Z katalogu ./software/database przegrywamy na naszego dockera mongodev pliki abouts.json i users.json. Logujemy się do kontenera i uruchamiamy poniższe polecenia. 
Polecenia mają zaimportować informacje potrzebne do uruchomienia aplikacji. Jak widzimy nie jest to dużo. Po zaimportowaniu tak naprawdę wszystkie informacje są już dostępne. Jest to zaleta baz dokumentowych, że dopiero po tym jak zapiszemy choćby jeden dokument dopiero tworzona jest kolekcja.

`mongoimport --db cms --collection abouts --authenticationDatabase cms --username cmsuser  --password ULqcXWKbf6fMLGxebueuXXJWHdjdjNc --file ./abouts.json`

`mongoimport --db cms --collection users --authenticationDatabase cms --username cmsuser  --password ULqcXWKbf6fMLGxebueuXXJWHdjdjNc --file ./users.json`

Wynik polecenia będzie jak poniżej:

![Wynik polecenia importu danych](/images/insta/insta_db_import.png)

Możemy teraz sprawdzić czy mamy zaimportowane dane używając polecenia

`show collections`

WYnik takiego zapytania poniżej

![Wynik polecenia sprawdzenia importu](/images/insta/insta_db_show_coll.png)


### Budowa środowisk

Należy przygotować plik w katalogu *./software/deploy/conf*  o nazwie naszego środowiska np test.env możemy w tym miejscu sterować jaki plik użyjemy do jakiej kompilacji. Przedrostek będzie jednocześnie pierwszym argumentem naszego skryptu do budowy obrazów. 

Możemy przystąpić do budowania obrazów Docker dla API naszej aplikacji.

Do tego celu na maszynie **instance-1** przechodzimy do katalogu ./software/deploy, z któego uruchamiamy odpowiedni skrypt, 

`./build_test.sh test 1.4` 

Skrypt przyjmuje dwie wartości pierwszą jest to zmienna, o której wspominałem wcześniej. Drugim parametrem jest tag z jakim będziemy umieszczać nasze obrazy. 

Po udanej kompilacji dostaniemy w ostatniej lini informację o naszym obrazie w rejestrze.
np
> 1.4: digest: sha256:3fe1a85d2c9f8629f7e953342721e162f6be01245d77a7dc783acc14aff74fbc size: 1994

Możemy się zalogować do rejestru (tutaj przykład dla Visual Studio Code)
![Obrazy w rejestrze](/images/insta/insta_api_registry.png)

W tej właśnie chwili mamy przygotowane oprogramowanie gotowe do instalacji na środowiska. Użycie rejestru spowoduje, że mamy jednakowe wersje na każdym środowisku.

> Istnieje możliwość zmiany parametrów w pliku konfiguracyjnym (.env) już po instalacji na konkretnym środowisku. 

Proces przeprowadzi budowanie dla całej aplikacji zaróno API jak i FrontEnd i BackEnd.

### Instalacja API

Logujemy się na masznę gdzie mamy mieć zainstalowane API do naszej operacji i wystarczy zalogować się do repozytorium obrazów i uruchomić polecenie 

`sudo docker run -p 3000:3000 --name cmsapi registry.dexterlab.pl/cmsapi:<wersja API>`

Możemy sprawdzić poprawność działania używając polecenia

`wget <ip lub nazwa naszego hosta>:3000`

### Instalacja BackEnd

Cała aplikacja została już wcześniej zbudowana i znajduje się w naszym rejestrze więc wystarczy jak załadujemy odpowiedni obraz.

`sudo docker run -p 8080:80 --name cmsbackend registry.dexterlab.pl/cmsbackend:<wersja BackEnd>`

### Instalacja FrontEnd

Tak samo jak w przypadku API i BackEnd wystarczy że załadujemy odpowiedni obraz używając polecenia

`sudo docker run -p 80:80 --name frontend registry.dexterlab.pl/cmsfrontend:<wersja FrontEnd>`

## Ustawienia dodatkowe

Należy skopiować plik utils.js z katalogu CMS-api do odpowiedniego miejsca na kontenerze

`sudo docker cp utils.js cmsapi:/app/node_modules/mongodb/lib`

Ustawienia do strony ustarotwej i API znajduje się w pliku 

`public/static/config.json`


Po załadowaniu obrazu do instancji Docker należy zmienić plik nginx.conf 
W tym celu na FrontEnd i BackEnd należy zmienić skopiować plik ***default.conf***
znajdujący się w lokalizacji ***PracaDyplomowa/software/deploy/conf/***

`sudo docker cp default.conf cmsfrontend:/etc/nginx/conf.d/default.conf`

`sudo docker cp default.conf cmsbackend:/etc/nginx/conf.d/default.conf`

Na koniec restartujemy kontenery.

Strona z aplikacją z wersją **UAT** znajduje się:

Aplikacja dla użytkowników - http://backend.uat.dexterlab.pl/

Aplikacja dla redaktorów i administratorów - http://backend.uat.dexterlab.pl:8080

 

