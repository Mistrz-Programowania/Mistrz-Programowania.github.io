---
layout: solution
title: Zapałki
edition: 2024
round: 1
level: d
sol_author: Maciej Wiśniewski
statement: https://szkopul.edu.pl/problemset/problem/T_NMOlZmmubRC5aK7lSF7lbF/site/
video: https://youtu.be/qIraPa7A1wY?t=1341
---

### Podzadanie 4

Ponieważ cyfra $4$ nie może być zamieniona na żadną inną cyfrę przestawiając jedną zapałkę, a mamy gwarancję, że działanie
na wejściu nie jest poprawne, to każdy test z tego podzadania miał odpowiedź "no".

### Rozwiązanie wolne $O(n^2)$

Równanie jest prawdziwe, wtedy i tylko wtedy gdy wartość jego lewej strony ($L$) minus wartość jego prawej strony ($R$) jest równa zero.
Przesuwając jedną zapałkę, możemy zmienić co najwyżej dwie cyfry. Zmiana cyfry $A$ na cyfrę $B$, zmienia wartość ($L-R$) o:
$\pm$ (w zależności od znaku przed liczbą w której jest ta cyfra, oraz tego po której stronie równania się znajduje) $(A - B) \cdot 10^x$
gdzie $x$ to pozycja cyfry w liczbie.

Samo przekładanie zapałki można zrobić wewnątrz jednej cyfry (np. z 6 można zrobić 9), lub między cyframi 
(np. zabrać zapałkę z 7 tworząc 1 i dołożyć ją do 0 tworząc 8). Legalnych przełożeń zapałki jest niewiele, można je wszystkie
wypisać na kartce.

Nasuwa się więc następujące rozwiązanie:

1. Policzmy $L-R$, aby wiedzieć o ile dokładnie musimy tą wartość zmienić żeby była równa zero.
2. Sprawdźmy każdą cyfrę oraz wszystkie jej legalne przełożenia zapałki wewnątrz samej siebie. 
3. Sprawdźmy każdą parę cyfr, oraz wszsytkie legalne przełożenia zapałek między nimi. 

### Rozwiązanie wzorcowe $O(n)$

Jeżeli chcemy zadanie rozwiązać liniowo, nie możemy sobie pozwolić na sprawdzanie każdej pary cyfr.
Zamiast tego rozbijemy przekładanie zapałki na dwie niezależne części: zabranie zapałki z jednej cyfry oraz
dodanie jej do drugiej. W ten sposób możemy liniowo sprawdzić jakie zmiany w $L-R$ są możliwe do osiągnięcia
zabierając jedną zapałkę oraz zapisać je na np. hash-mapie. Następnie, również liniowo, spróbujemy dodać zapałkę 
do każdej cyfry i sprawdzić czy udało nam się wcześniej osiągnąć odpowiednią zmianę tak, aby razem
z aktualnie sprawdzanym dodaniem zapałki naprawiły równanie.

### Za proste?

Zadanie da się rozwiązać w liniowym czasie oraz stałej dodatkowej pamięci (bez żadnych map, tablic, tylko jeden string na wejście).
Zostawiam to jako ćwiczenie dla czytelnika.
