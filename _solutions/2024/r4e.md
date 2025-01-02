---
layout: solution
title: Wróg Mojego Wroga
edition: 2024
round: 4
level: e
sol_author: Maciej Wiśniewski
statement: https://szkopul.edu.pl/problemset/problem/QKT4MZq40YwEwvmlzSerLho2/site/
video: https://youtu.be/8xexhyiKHsQ&t=2156
---

### Obserwacja 1

Królowie są podzieleni na dwie spójne przyjaźni (odpowiadające dwukolorowaniu drzewa).

### Obserwacja 2

Jeżeli król wie o imprezie którą robi inny król, to w niezależnie od tego czy są przyjaciółmi,
jego status zmieni się o dokładnie $1$. Od tego czy są przyjaciółmi zależy tylko czy wzrośnie czy zmaleje.

### Obserwacja 3

Możemy założyć że jeżeli imprezę organizuje król z pierwszej spójnej przyjaźni, to wszystkim królom
którzy o niej wiedzą dodajemy $1$ do statusu (niezależnie od tego czy się przyjaźnią), a jeżeli organizator należy do drugiej spójnej,
to wszystkim królom odejmujemy $1$ od statusu. Na koniec mnożymy status królów z drugiej spójnej razy $-1$. Można sprawdzić,
że otrzymamy w ten sposób dokładnie te same statusy. Zamieniliśmy w ten sposó trudną operację dodawania i odejmowania na zmianę, na prostszą "dodaj wszystkim".

Sprowadziliśmy zadanie do następującej treści: Dane jest drzewo, obsługuj operacje:

1. wyjmij wierzchołek
2. wsadź wierzchołek spowrotem
3. powiedz ile wierzchołków jest ciągle połączonych z $v$ i dodaj $\pm 1$ do statusu wszystkich wierzchołków połączonych z $v$

Główny pomysł w zadaniu polega na tym że operację 3, będziemy robić na poddrzewach. Gdy dostaniemy operację typu 3 dla wierzchołka $v$,
rozbijemy ją na 3 kroki:

1. Znajdź $u$ - najwyższego przodka $v$ który jest z nim jeszcze połączony.
2. Sprawdź ile jest wierzchołków połączonych z $u$.
3. Dodaj $\pm 1$ do **całego** poddrzewa $u$.

Pierwszą część możemy rozwiązać wyszukiwaniem binarnym, jeżeli będziemy trzymać w każdym wierzchołku liczbę jego wyjętych przodków,
co możemy osiągnąć dodając $1$ na poddrzewie wierzchołka którego wyjmujemy, oraz odejmując $1$ gdy go wkładamy.
 
### Obserwacja 4

Wierzchołek $u$ jest korzeniem drzewa, albo jego bezpośredni ojciec jest wyjęty.

Dzięki obserwacji 4, do rozwiązania drugiej części wystarczy trzymać dla każdego wierzhołka ile wierzchołków **z poddrzewa** danego wierzchołka jest z nim połączone. 
Gdy wyjmujemy wierzchołek, odejmujemy jego liczbę połączonych z nim wierzchołków na ścieżce do korzenia, a gdy go wkładamy dodajemy ją spowrotem.

Trzecia część, może zmienić wartość wierzchołków które nie są połączone z $u$. Pomysł jest taki, że jeżeli jakiś wierzchołek z poddrzewa $u$
(powiedzmy $x$), nie jest połączony z $u$, to musi na ścieżce między nimi być wyjęty wierzchołek $y$. Wartość $x$ naprawimy w momencie gdy będziemy
wkładać spowrotem $y$. Gdy wyciągamy dowolny wierzchołek, zapisujemy jego status, gdy go wkładamy spowrotem różnica w jego aktualnym statusie oraz
tym z momentu wyjęcia to suma wszystkich opearcji które go dotyczyły w czasie gdy był wyjęty. Te same operacje dotyczyły też całego jego poddrzewa,
zatem aby naprawić wszystkie wartości wystarczy odjąć tą różnicę od całego poddrzewa. Na koniec zadania możemy sztucznie włożyć wszystkie wyjęte wierzchołki,
aby naprawić statusy.

Aby obsłużyć wszystkie opisane operacje potrzebujemy drzewa przedziałowego lub Fenwicka, pre-post order do wykonywania operacji na poddrzewach
oraz Heavy-Light Decomposition do wykonywania operacji na ścieżce do korzenia. Całe rozwiązanie ma złożoność $O(n \log^2n)$
