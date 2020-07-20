---
title: Baza danych
keywords: rdbms, MongoDB, Systemy Baz Danych
last_updated: July 15, 2020
tags: [MongoDB, Bazy danych]
summary: "Tutaj skupiam się nad tym żeby przedstawić dlaczego wybrałem bazę danych Mongo DB i podstawowe cechy jakie będą przydatne w naszym projekcie"
sidebar: mydoc_sidebar
permalink: technologia_db.html
folder: technologia
---
## Systemy Baz Danych

W systemie zarządzania trescią musiałem wziąć pod uwagę kilka kwestii. Sterowanie współbierznością w rozproszonych bazach danych jest dość skomplikowane musimy być pewni że zreplikowane kopie tego samego rekordu-dokumentu są wszędzie identyczne i są zmowyfikowane w ten sam sposób.

Wiemy co to jest już baza danych i znamy już jaka była historia powstania baz danych ale zanim przejdziemy dalej muszę napisać o teorii CAP. Akronim CAP pochodzi od początkowych liter Consistency (spójność), Availability (dostępność) i Partition tolerance (podział). Zanim przejdziemy do tego, którą bazę wybierzemy należy się zastanowić i wykonać analizę które z tych trzech cech. Niestety nie ma takiego systemu baz danych żeby pokrywały wszystkie cechy i z reguły należy wybrać dwa, które będą najważniejsze. Co oznaczają poszczególne cechy.

**Consistency** - Spójność. Oznacza, że wszyscy klienci widzą te same dane w tym samym czasie, bez względu na to, z którym węzłem się łączą. Aby tak się stało, za każdym razem, gdy dane są zapisywane w jednym węźle, muszą zostać natychmiast przesłane lub zreplikowane do wszystkich innych węzłów w systemie, zanim zapis zostanie uznany za „zakończony powodzeniem”.

**Availability** - Dostępność. Dostępność oznacza, że każdy klient żądający danych otrzymuje odpowiedź, nawet jeśli jeden lub więcej węzłów nie działa. Innym sposobem na stwierdzenie tego - wszystkie działające węzły w systemie rozproszonym zwracają prawidłową odpowiedź na każde żądanie, bez wyjątku.

**Partition tolerance** - Tolerancja podziału. Partycja to przerwa w komunikacji w systemie rozproszonym - utracone lub czasowo opóźnione połączenie między dwoma węzłami. Tolerancja podziału oznacza, że klaster musi nadal działać pomimo dowolnej liczby awarii komunikacji między węzłami w systemie.

W moim przypadku największy nacisk musiałem położyć na wysoką dostępność, ponieważ system ma być czynny 24/7 co oznacza że musimy całe środowisko utrzymywać systemy właściwie bez wyłączania. Drugą cechą to przy utracie części maszyn lub braku dostępów do nich dane muszą być dostępne dla użytkownika. Można wybierać z wielu systemów bardziej lub mniej egzotyczne ale akurat MongoDB będzie tutaj odpowiednim wyborem.

W tym miejscu dochodzimy do sedna i wielu powodów dla których wybrałem bazę NoSQL. Pamiętajmy, że moim projektem jest system zarządzania treścią  i muszę się skupić na innych aspektach..
W systemach nierelacyjnych nacisk przede wszystkim jest położony na wysoką dostępność dlatego będziemy musieli zastanowić się nad tym w jaki sposób możemy replikować nasze dane. Do tego jeszcze musimy przewidzieć w jaki sposób musimy zabezpieczyć się przed przetrzymywaniem dużej ilości danych.


Jak już wcześniej napisałem zgodnie z Teorią CAP w takim systemie rozproszonym nie da się uzyskać wszystkich trzech porządanych cech i w każdym przypadku musimy przyjąć jedynie dwie dominujące cechy, którymi będzie się harakteryzować nasz system. O ile w przypadku gdzie musimy zadbać o spójność danych będzie prriorytetem użylibyśmy transakcyjnej bazy danych takiej jak Oracle czy Microsoft SQL Server to właśnie ta cecha w naszym przypadku będzie najmniej znacząca więc musiałem użyć systemu baz danych, który odporny na dostępność i częściową utratę partycji. Dlatego sensownym wyborem będzie MongoDB. System taki będzie nie tylko odpowrny na wcześniej wymienione cechy ale także będzie możliwość rozbudowy albo przebudowy środowiska w trakcie kiedy środowisko będzie już działało produkcyjnie. 

Dokumentowe systemy baz danych przechodują dane jako kolekcje dokumentów i jako zaletę takiego systemu jest fakt, że nie trzeba tworzyć, a co za tym idzie utrzymywać schematu bazy. Jest to wielkie ułatwienie, ponieważ w wielu przypadkach odchodzi wdrożenie po stronie bazy danych. Jak możemy przeczytać w niektórych źródłach takie bazy są samoopisujące. Oznacza to że atrybuty dokumentu jak i sama jego struktura nie wymaga dodatkowego słownika. 

Kolekcje są to dokumenty do siebie podobe ale nie identyczna. Dokumenty między sobą się mogą różnić ponieważ jak wcześniej wspomniałem nie posiadają stałego schematu. Pomimo tego że mamy w danej kolekcji dokument z artykułem to może się okazać, że w środku mamy zupełne inne dane z różnymi atrybutami.

## Cechy systemu rozproszonego MongoDB

Aby zapewnić niepodzielność i spójność transakcji w rozproszonej bazie danych MongoDB korzysta z zatwierdzania dwufazowego.

Aby przejść dalej należy wyjaśnić czym jest zatwierdzanie dwufazowe, niepodzielność i spójność transakcji.

# Niepodzielność i spójność transakcji

Nie będę opisywać wszystkiego z teorii baz danych ponieważ musiałbym przepisać dużą część książki i za mocno zagłębić się w działanie systemów baz danych. W naszym przypadku do celów projektu należy tylko przedstawić kontekst dla zobrazowania rozwiązanego problemu. Żeby zrozumieć jak działa system MongoDB jako rozproszony system musimy zrozumieć jak to działa. 

Jest to jedna z właściwości, która cechuje się właśnie taki system i jest jedną z czterech właściwości samej transakcji. W literaturze spotykamy się z akronimem ACID, który pochodzi od pierwszych liter tychże właściwości i są to:

**Atomicity** - nazywaną atomowością lub zamiennie niepodzielnością. Oznacza to że transakcja jest niepodzielną jednostką przetwarzania i jest wykonywana w całości bądź też w ogóle.

**Consistency** - spójność to nic innego jak to, że dane w bazie przed i po transakcji stanowią określony prawdziwy stan. Transakcja nieprzerwana przez inne transakcje przenosi stan bazy z jednego w drugi.

**Isolation** - izolacja. Pznacza to, że to co robimy w jednej transakcji jest odseparowana od tego co robi lub w jakim stanie jest inna transakcja. Oznacza to że transakcja nie powinna kolidować ze współbieżnym wykonywaniem innych transakcji.

**Durability** - trwałoś. Oznacza to że jeśli wykonamu transakcje to zmiany (przeniesienie bazy z jednego stanu w drugi) są trwałe i nie mogą zostać utracone.

Wracając do naszej niepodzielności to system, który wybierzemy musi zapewnić to że naza transakcja zostanie wykonana od początku do końca. Za to właśnie odpowiedzialny jest RDBMS. Transakcja jeśli zostanie wykonana czyli zmiany występujące w trakcie twania transakcji muszą zostać utrwalone zgodnie z 4 właściwością. Jest to zatwierdzanie (commitowanie) naszych transakcji.
W przypadku kiedy transakcja nie może być wykonana do końca w wyniku anulowania lub błędu aplikacji (awarii systemu) mechanizm odwracania musi wszystkie zmiany wycofać (wykonać rollback) aby baza była spójna, a dane muszą być takie same jak z przed transakcji.

Każdy system baz danych w inny sposób to realizuje i dla naszego projektu to w tej chwili nie ma znaczenia.

# Zatwierdzanie dwufazowe

W dzisiejszym artykule porozmawiamy o drugim ważnym aspekcie, który muszę wziąć pod uwagę wybierając odpowiedni silik do mojego projektu.

Jest to zatwierdzanie dwufazowe. W typowych serwerach transakcyjnych zakładamy że jedna transakcja ma dostęp do jednej bazy danych. W tym przypadku środowisku jest zbudowane w taki sposób, że dane mogą i nawet będą przechowywane w wielu fizycznych miejscach w sieci. Będziemy mieli w takim przypadku do czynienia z rozproszoną bazą danych, gdzie poszczególne jej elementy są umieszczone w odrębnych miejscach połączonych siecią.

W takim przypadku aby zachować niepodzielność transacji (o czym pisałem wcześniej) nasz system musi posiadać mechanizm dwupoziomowego zatwierdzania. Do tych celów mamy globalny proces koordynujący przechowywanie informacji potrzenmych do do odtwarzania, natomiast menadżery lokalne przechowują dziennik i tablice.

Jak sama nazwa wskazuje mamy dwie częći (fazy) zatwierdzania. 

Faza 1 - jest wtedy kiedy wszystkie bazy biorące udział w transakcji zasygnalizują koordynatorowi, ż ich część transakcji została wykonana, następnie koordynator do każdej z nich przesyła komunikat przygotowania do zatwierdzenia. Każda baza kiedy otrzyma taki komunikat wymusza zapis wszystkich rekordów dziennika  oraz informacji wymaganych do odtwarzania dla lokalnego mechanizmu odtwarzania. W tym samym czasie lokalna baza wysyła do globalnego koordynatora sygnaturę potwierdzającą zapisy, które wykonały się lokalnie.
W przypadku błędu lub braku odpowiedzi przez lokalną bazę koordynator zakłada, że transakcja zwróciła status nie OK

Faza 2 - Dla wszystkich pozytywnie zakończonych transakcji w bazach współdzielonych koordynator także przyjmuje status OK. Transakcja kończy się sukcesem i koordynator globalny wysyła komunikat do baz o zatwierdzeniu.
W przypadku kiedy choćby jedna lokalna transakcja prześle do koordynatora informację o błędzie lub nie prześle informacji. GLobalny proces prześle do reszty baz komunikat o wycofaniu transakcji. Po wycofaniu transakcji proces globalny kończy się błędem.

W efekcie dwóch tych faz jest albo zatwierdzenie danych przez wszystkie bazy albo niezatwierdzenie jej przez żadną.

## Replikacja w MongoDB

Jest to sposób przetrzymywania, tworzenia wielu kopii tego samego pliku w różnych miejscach zwanymi węzłami. W tym celu mamy do syspozycji zbiory replik. Replikacja w takim modelu odbywa się na zasadzie master-slave. W o ile niedużych systemach możne to zdawać dobry egzamin ale w naprawdę dużych narzut na obsługę, koszt składowana i aktualizacji tośnie wraz z ilością replik.

## Sharding w MongoDB

Jak już wcześniej wspomniałem dla małej ilości danych można się pokusić na to by użyć replikacji jednakże w dużych systemach należy zastanowić się nad innym sposobem przetrzymywania danych. W naszym projekcie na początek danych będzie niewiele, jednakże w przyszłości to może się znacząco zmienić i należałoby na samym początku przyjąć że będę użwać schardeningu. Sharding jest podziałem poziomym dokumentów w kolekcji i jest to podział na odrębne partycje. Dzięki temu możemy dodawać nowe węzły i przechowywać podzielone na mniejsze kawałki dokumenty w odrębnych węzłach aby zrównoważyć obciążenie. W takim przypadku każdy węzeł przetrzymuje tylko operacje dotyczące danego składowanego dokumentu w danym węźle. Skutkuje to wzrostem wydajnośći.

W MongoDB występują dwa sposoby podziałuy kolekcji na shardy i są to 
 - podział na zakresy
 - podział z kluczem mieszanym .

W obu przypadkach system wymaga od nas jakiego atrybutu którego będzie używał do podziału (shared key) i musi występować on w każdym dokumencie w kolekcji, dodatkowo na tym aktrybucie musi być założony indeks.