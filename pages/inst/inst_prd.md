---
title: Instalacja środowiska produkcyjnego
keywords: linux, infra, instalacja, produkcja
last_updated: August 20, 2020
tags: [oprogramowanie, system, docker, ngingx]
summary: "Finalna wersja instrukcji instalacji ."
sidebar: mydoc_sidebar
permalink: inst_prd.html
folder: insta
---
# Instalacja środowiska produkcyjnego

## Instalacja środowiska bazodanowego

Środowisko oparte jest na replikach a także na scharedningu danych pomiędzy nodami. W tym przypadku wymadane jest aby w każdej Replice były przynajmniej 3 nody, przynajmniej 2 repliki, a także 3 serwery konfiguracji i przynajmniej jeden serwer dla miejsca gdzie łączą się klienci. Na fakt że na razie nie potrzebujemy aż tak rozbudowanego środowiska wystarczy, że rozwiązanie będzie oparte na instanacjach bazy danych MongoDB skonteneryzowanych. Zasada działania konfiguracji jest podobna.

Przed przystąpieniem do instalacji musimy przygotować maszyny na której będzie instalowana aplikacja. Minimalne wymagania takiej maszyny to:

> 1 CPU         
> 2GB Pamięci RAM       
> 10GB miejsca na dane i aplikacje      
> sieć z przydzielonym IP       
> dostęp do internetu.          


Nasze środowisko będziemy instalować w chmurze Goolgle Cloud. Może to być lokalna maszyna z systemem operacyjnym Linux. Instrukcja jest przygotowana dla dystrybucji Ubuntu.

Opis przygotowania maszyn podany był we wcześniejszym rozdziale. 

### Czynności przygotowawcze

Przygotowujemy sobie śdodowisko do instalacji. Wszystkie informacje do instalacji zawarte są w odpowiednich katalogach na **GitHub**
Połączenie do repozytorium odbywa się przy pomocy kluczy __SSH__ 

Na każdej maszynie zakłądamy katalog deploy i do niego robimy 

`git clone git@github.com:pawel-domanski/PracaDyplomowa.git`

Wszystkie prace i skrypty potrzebne do instalacji są w katalogu PracaDyplomowa/software/mongo/scripts

### Konfuguracja docker pod MongoDB na każdej maszynie

Przed przystąpieniem do instalacji bazy danych należy utworzyć volumen, który będzie nam służyć do transerowania danych pomiędzy poszczególnymi kontenerami. Będą to kliki instalacyjne lub konfiguracyjne wspólne dla wszystkich kontenerów.

`docker volume create --name mongo_storage`

> Warto sobie utworzyć także tymczasowy kontener do przegrywania tam danych. 

Przegrywamy na volumen mongo_storage do katalogu /opt/keyfile/ plik ./software/scripts/mongo/keys/mongo-key, a następnie zmieniamy właściciela na 999 i uprawnienia do tego pliku na 600. W przeciwnym przypadku nie będziemy mogli zainstalować bazy MongoDB.

Instalujemy na maszynie docker-compose, które pomoże w szybszej instalacji maszyn i możemy utuchomić 

`sudo docker-compose -f ./software/scripts/mongo/init_dev_db_.yaml run -d `

Polecenie uruchomi nam konfigurator instancji MongoDB zgodnie z naszymi wymaganiami. Nazwa kontenera nazywa się ***mongodev***

Następnie należy zalogować się do maszyny Docker 

`sudo docker exec -it mongodev /bin/sh`

i zmienić uprawnienia na 400 dla pliku /opt/keyfile/mongo-key a także zmienić właściciela na 999 i grupę 999. W przeciwnym przypadku nie będziemy mogli uruchomić żadnej instancji bazy.

### Instance-3 - ReplicaSet1

Wszystkie informacje potrzebne do zainstalowania i przygotowania kontenrów dla instancji zawarta jest w pliku mongo_replicaSet1.yaml i należy uruchomić tylko

`sudo docker-compose -f mongo_replicaSet1.yaml run -d` 

Wszystko co jest potrzebne zostanie uruchomione i skonfigurowane.

Łączymy się do naszej maszyny

`sudo docker exec -it mongo-1-1 /bin/sh`

i uruchamiamy 

`mongo admin` 

Wykonujemy po koleii polecnenia poniżej

{% highlight json %}
var cfg = {
        "_id": "rs1" ,
        "protocolVersion": 1,
        "members": [
            {
                "_id": 0,
                "host": "10.154.0.2:30011"
            },
            {
                "_id": 1,
                "host": "10.154.0.2:30012"
            },
            {
                "_id": 2,
                "host": "10.154.0.2:30013"
            }
        ]
    };

rs.initiate(cfg, { force: true });
    
db.createUser( {
	user: "siteRootAdmin",
	pwd: "7wYogULnZVvynyME33jNBmja9TjX6ZR",
	roles: [ { role: "root", db: "admin" } ]
});

db.getSiblingDB("admin").auth("siteRootAdmin", "7wYogULnZVvynyME33jNBmja9TjX6ZR" );

rs.reconfig(cfg, { force: true });
admin = db.getSiblingDB("admin");

admin.createUser(
  {
    user: "siteUserAdmin",
    pwd: "u9ZZtvF8D4R7FH3PUPAKx9ycK3rDgfT",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

db.getSiblingDB("admin").auth("siteUserAdmin", "u9ZZtvF8D4R7FH3PUPAKx9ycK3rDgfT" )


db.getSiblingDB("admin").createUser(
  {
    "user" : "replicaAdmin",
    "pwd" : "3kx8wmQGnzUoffu4j4ghbskUWiN2Fit",
    roles: [ { "role" : "clusterAdmin", "db" : "admin" } ]
  }
)

{% endhighlight %}


Wystarczy że wykonamy to na jednej nodze w replice, dane się zreplikują pomiędzy siebie.

### Instance-4 - ReplicaSet2

Wszystkie informacje potrzebne do zainstalowania i przygotowania kontenrów dla instancji zawarta jest w pliku mongo_replicaSet1.yaml i należy uruchomić tylko

`sudo docker-compose -f mongo_replicaSet2.yaml run -d` 

Wszystko co jest potrzebne zostanie uruchomione i skonfigurowane.

Łączymy się do naszej maszyny

`sudo docker exec -it mongo-2-1 /bin/sh`

i uruchamiamy 

`mongo admin` 

Wykonujemy po koleii polecnenia poniżej

{% highlight json %}
var cfg = {
        "_id": "rs2" ,
        "protocolVersion": 1,
        "members": [
            {
                "_id": 0,
                "host": "10.154.0.4:30021"
            },
            {
                "_id": 1,
                "host": "10.154.0.4:30022"
            },
            {
                "_id": 2,
                "host": "10.154.0.4:30023"
            }
        ]
    };

rs.initiate(cfg, { force: true });
    
db.createUser( {
	user: "siteRootAdmin",
	pwd: "7wYogULnZVvynyME33jNBmja9TjX6ZR",
	roles: [ { role: "root", db: "admin" } ]
});

db.getSiblingDB("admin").auth("siteRootAdmin", "7wYogULnZVvynyME33jNBmja9TjX6ZR" );

rs.reconfig(cfg, { force: true });
admin = db.getSiblingDB("admin");

admin.createUser(
  {
    user: "siteUserAdmin",
    pwd: "u9ZZtvF8D4R7FH3PUPAKx9ycK3rDgfT",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

db.getSiblingDB("admin").auth("siteUserAdmin", "u9ZZtvF8D4R7FH3PUPAKx9ycK3rDgfT" )


db.getSiblingDB("admin").createUser(
  {
    "user" : "replicaAdmin",
    "pwd" : "3kx8wmQGnzUoffu4j4ghbskUWiN2Fit",
    roles: [ { "role" : "clusterAdmin", "db" : "admin" } ]
  }
)

{% endhighlight %}


Wystarczy że wykonamy to na jednej nodze w replice, dane się zreplikują pomiędzy siebie.

### Instance-2 - serwery konfiguracji i Mongos

#### Serwery konfiguracji

Wszystkie informacje potrzebne do zainstalowania i przygotowania kontenrów dla instancji zawarta jest w pliku mongo_replicaSet1.yaml i należy uruchomić tylko

`sudo docker-compose -f mongo_replicaConfig.yaml run -d` 

Wszystko co jest potrzebne zostanie uruchomione i skonfigurowane.

Łączymy się do naszej maszyny

`sudo docker exec -it mongo-cnf-1 /bin/sh`

i uruchamiamy 

`mongo admin` 

Wykonujemy po koleii polecnenia poniżej

{% highlight json %}

db.createUser( {
        user: "siteRootAdmin",
        pwd: "7wYogULnZVvynyME33jNBmja9TjX6ZR",
        roles: [ { role: "root", db: "admin" } ]
});

db.getSiblingDB("admin").auth("siteRootAdmin", "7wYogULnZVvynyME33jNBmja9TjX6ZR" );

admin = db.getSiblingDB("admin");

rs.initiate();

admin.createUser(
  {
    user: "siteUserAdmin",
    pwd: "u9ZZtvF8D4R7FH3PUPAKx9ycK3rDgfT",
    roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
  }
)

db.getSiblingDB("admin").auth("siteUserAdmin", "u9ZZtvF8D4R7FH3PUPAKx9ycK3rDgfT" )


db.getSiblingDB("admin").createUser(
  {
    "user" : "replicaAdmin",
    "pwd" : "3kx8wmQGnzUoffu4j4ghbskUWiN2Fit",
    roles: [ { "role" : "clusterAdmin", "db" : "admin" } ]
  }
)

db.getSiblingDB("admin").auth("replicaAdmin", "3kx8wmQGnzUoffu4j4ghbskUWiN2Fit" )

   var cfg = {
        "_id": "cnf-serv",
        "configsvr": true,
        "protocolVersion": 1,
        "members": [
            {
                "_id": 100,
                "host": "10.156.0.3:30101"
            },
            {
                "_id": 101,
                "host": "10.156.0.3:30102"
            },
            {
                "_id": 102,
                "host": "10.156.0.3:30103"
            }
        ]
    };
    rs.initiate(cfg, { force: true });
    rs.reconfig(cfg, { force: true });

{% endhighlight %}


Wystarczy że wykonamy to na jednej nodze w replice, dane się zreplikują pomiędzy siebie.

#### Serwery Mongos

Jest to serwer gdzie łączą się użytkownicy i on jest opdowiedzialny za prawidłową komunikację poniędzy replikami. Instalacja jest bardzo podobna jak powyżej. 
Wszystkie informacje potrzebne do zainstalowania i przygotowania kontenrów dla instancji zawarta jest w pliku mongo_replicaSet1.yaml i należy uruchomić tylko

`sudo docker-compose -f mongo_replicaRouter.yaml run -d` 

Wszystko co jest potrzebne zostanie uruchomione i skonfigurowane.

Łączymy się do naszej maszyny

`sudo docker exec -it mongo-router /bin/sh`

i uruchamiamy 

`mongo admin` 

logujemy się jako ReplicaAdmin 

`db.getSiblingDB("admin").auth("replicaAdmin", "3kx8wmQGnzUoffu4j4ghbskUWiN2Fit" )`

I podłączamy obie repliki

`sh.addShard( "rs1/10.154.0.2:30011,10.154.0.2:30012,10.154.0.2:30013" );`

`sh.addShard( "rs2/10.172.0.2:30030,10.172.0.2:30031,10.172.0.2:30032" );`

Możemy sprawdzić całość poleceniem

`sh.status();`

System gotowy jest do pracy.

#### Załadowanie danych do bazy i włączenie schardeningu

Musimy zalogować się do **mongo-router**  

`sudo docker exec -it mongo-router /bin/sh`

Załadować wszystkie tabele jakie mamy w systmie

`mongoimport --db cms --collection abouts --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./abouts.json`

`mongoimport --db cms --collection users --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./users.json`

`mongoimport --db cms --collection articles --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./articles.json`

`mongoimport --db cms --collection comments --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./comments.json`

`mongoimport --db cms --collection keywords --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./keywords.json`

`mongoimport --db cms --collection messages --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./messages.json`

`mongoimport --db cms --collection news --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./news.json`

`mongoimport --db cms --collection newsletters --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./newsletters.json`

`mongoimport --db images --collection fs.chunks --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./fs.chunks.json`

`mongoimport --db images --collection fs.files --authenticationDatabase admin --username siteRootAdmin  --password 7wYogULnZVvynyME33jNBmja9TjX6ZR --file ./fs.files.json`

Jest to konieczne ponieważ musimy utworzyć indeksy, które będą wymagane do uruchomienia shardeningu.

Następnie uruchomiamiamy

`mongo admin` 

i zalogować się jako _siteRootAdmin_

`db.getSiblingDB("admin").auth("siteRootAdmin", "7wYogULnZVvynyME33jNBmja9TjX6ZR" );`

tworzymy rolę dla użytkownika aplikacyjnego

{% highlight json %}
db.createRole( 
      {    
         role: "cmsrole",   
        privileges: [  
        { resource: { cluster: true }, actions: [ "killop", "inprog" ] },    
          { resource: { db: "cms", collection: "" }, actions: [ "killCursors" ] }  
         ], 
        roles: [{role:'dbOwner', db:'cms'}]    
     }    
)
{% endhighlight %}


tworzymy użytkownika aplikacyjnego

{% highlight json %}
db.createUser(
    {user: 'cmsuser', pwd: 'fp4Kfh}ex8T^>kn&BqX&AVZz8=9iC', 
            roles:[
                {role:'dbOwner', db:'cms'},
                {role:'dbOwner', db:'images'}
            ]})
{% endhighlight %}

Włączamy shardening dla baz images i cms



{% highlight json %}
use cms
sh.enableSharding("cms")

use images
sh.enableSharding("images")
{% endhighlight %}



Zakładamy indeksy na kolekcjach i włączamy na tabelach schardening

{% highlight json %}
use cms 


b.articles.createIndex( { createddate: "hashed" } )
sh.shardCollection("cms.articles" , { createddate : "hashed" } )

db.categories.createIndex( { _id: "hashed" } )
sh.shardCollection("cms.categories" , { _id : "hashed" } )

db.comments.createIndex( { _id: "hashed" } )
sh.shardCollection("cms.comments" , { _id : "hashed" } )

db.keywords.createIndex( { _id: "hashed" } )
sh.shardCollection("cms.keywords" , { _id : "hashed" } )

db.messages.createIndex( { createdtime: "hashed" } )
sh.shardCollection("cms.messages" , { createdtime : "hashed" } )

db.news.createIndex( { createddate: "hashed" } )
sh.shardCollection("cms.news" , { createddate : "hashed" } )

db.newsletters.createIndex( { _id: "hashed" } )
sh.shardCollection("cms.newsletters" , { _id : "hashed" } )

db.users.createIndex( { _id: "hashed" } )
sh.shardCollection("cms.users" , { _id : "hashed" } )

db.abouts.createIndex( { _id: "hashed" } )
sh.shardCollection("cms.abouts" , { _id : "hashed" } )


use images

db.fs.chunks.createIndex( { _id: "hashed" } )
sh.shardCollection("images.fs.chunks" , { _id : "hashed" } )

{% endhighlight %}

Baza danych jest już gotowa do pracy z aplikacją.


## Budowa środowisk

Budowa środowisk odbywa się analogicznie jak w przypadku środowisk testowych. Poniżej przedstawię wyłaczenie części, które są wyłącznie dla środowiska produkcyjnego. 

--- 
### Instance-5 - Serwer API i Backend

Po tym jak już zbudujemy nasze środowisko łączymy się do maszyny i wykonujemy deployment aplikacji z rejestru.

Dla API

`sudo docker run -p 3000:3000 --name cmsapi registry.dexterlab.pl/cmsapi:<wersja API>`

Dla BackEnd

`sudo docker run -p 80:80 --name cmsbackend registry.dexterlab.pl/cmsbackend:<wersja BackEnd>`

Następnie w usłudze Cloudflare robimy wpis A dla backend.dexterlab.pl na odpowiedni IP gdzie mamy zainstalowaną aplikację.

Aplikacja dostępna jest pod adresem http://backend.dexterlab.pl/

### cluster-1 - aplikacja FrontEnd

W przypadku klastra kubernates musimy zbudować odpowiednio obraz dockera jak to omawialiśmy w przypadku środisk API i BackEnd ale wdrożenie wygląda już trochę inaczej ponieważ musimy skonfigurować odpowiednio klaster (w naszym prazypadku korzystam z wzorca dostępnego w Google Cloud Platform). Do wdrożenia aplikacji należy zainstalować na maszynie wykonującej wdrożenie aplikacji klienta _kubectl_. W tym celu należy uruchomić polecenie

`sudo apt install kubectl`

Następnie należy zainstalować SDK Google, które służy do komunikacji z chmurą Google. Wszystkie potrzebne informacje jak zainstalować i skonfigurować klienta można zlaleźć pod adresem https://cloud.google.com/sdk

Instalujemy aplikację

`kubectl create deployment frontend-app --image=<nazwa obrazu w rejestrze>`

Zmieniamy że ma utworzyć 3 PODy aplikacji przy starcie

`kubectl scale deployment frontend-app --replicas=3`

Włączamy autoskalowanie aplikacji

`kubectl autoscale deployment frontend-app --cpu-percent=80 --min=1 --max=5` 

Powyższe polecenie ustawi nam że następne PODy zostaną utworzone przy utylizacji CPU na poziomie 80% i ma ich być minimum 1 a maksimum 5 (opcje te można zmieniać w trakcie pracy)

Żeby aplikacja była widoczna z internetu musimy utworzyć serwis typu LoadBalancer, który będzie nam przekierowywać ruch z sieci na wewnętrzny ip klastra.

`kubectl expose deployment  frontend-app --name=frontend-app-service --type=LoadBalancer --port 80 --target-port 80`

Aby sprawdzić czy wsystko jest w porządku uruchamiamy polecenie

`kubectl get pods`

Polecenie powinno zwrócić coś podobnego do poniższego listingu

> NAME                            READY   STATUS    RESTARTS   AGE  
> frontend-app-58d4596789-cgcsd   1/1     Running   0          31s  
> frontend-app-58d4596789-gmx8z   1/1     Running   0          51s  
> frontend-app-58d4596789-gnxlk   1/1     Running   0          31s  

Tworzenie serwisów i wystawianie może potrwać trochę. Sprawdzenie jaki mamy zewnętrzny ip wykonujemy przez polecenie

`kubectl get service`

> NAME                   TYPE           CLUSTER-IP   EXTERNAL-IP    PORT(S)        AGE  
> frontend-app-service   LoadBalancer   10.0.10.63   34.78.105.75   80:31772/TCP   54s    
> kubernetes             ClusterIP      10.0.0.1     <none>         443/TCP        2d22h  

Następnie w usłudze Cloudflare robimy wpis A dla backend.dexterlab.pl na odpowiedni IP gdzie mamy zainstalowaną aplikację.

Aplikacja dostępna jest pod adresem http://cms.dexterlab.pl/