---
layout: solution
title: Plan Konwój
edition: 2026
round: 1
level: d
author: Maciej Mączka
tags:
  - dsu
  - lca
statement: https://szkopul.edu.pl/problemset/problem/PhhFxukOD9iIXmhCfwvGqNSK/site/
---
### Treść
W zadaniu mamy podany graf ważony i należy odpowiedzieć na q pytań o minimalne takie X, że da się dojść z a do b, używając krawędzi o wagach $\leq X$.

### Podzadanie 1 - Rozwiązanie wolne - $O(n*m*logn + q)$
Zdefiniujmy wartość ścieżki jako:

##### Def 1:
$val = max(c_1, c_2, ..., c_k)$

Teraz okazuje się, że klasyczna odmiana Dijkstry odpowiednio policzy nam najkrótsze ścieżki. Wystarczy puścić ją n razy i odpowiadać w czasie stałym na zapytania.

### Podzadanie 2 - Wagi krawędzi to albo 0, albo 1 - $O(nlogn)$
Tutaj wystarczy użyć techniki Find and Union i połączyć wszystkie krawędzie o wadze 0 ze sobą. Gdy chcemy odpowiedzieć na pytanie między wierzchołkami a i b, to sprawdzamy, czy są w tej samej składowej spójnej. Jeśli tak, to odpowiadamy 0, jeśli nie, to odpowiadamy 1.

### Rozwiązanie wzorcowe 1 - $O(nlogn + qlogn)$
Najpierw zaproponuję rozwiązanie zachłanne, a potem dowiodę, dlaczego to działa.

##### Algorytm:
1) Posortuj krawędzie wagami niemalejąco.  
2) Stwórz graf z n wierzchołkami bez krawędzi.  
3) Idź po kolei posortowanymi krawędziami i jeśli jakaś krawędź łączy 2 wierzchołki, do których wcześniej nie dało się dojść, to ją dodaj; w przeciwnym przypadku nic nie rób.  
4) W ten sposób otrzymaliśmy drzewo.

Lemat 1: Ścieżka w drzewie między a i b jest najkrótszą ścieżką (wg Def 1) między a i b w głównym grafie.
 
Dowód: Popatrzmy na krawędź o największej wadze na ścieżce między a i b w drzewie. Jako że jej waga jest największa, to została dodana jako ostatnia, a więc przed jej dodaniem (po dodaniu wszystkich krawędzi o wagach mniejszych od niej) nie istniała ścieżka między a i b, ponieważ były w osobnych składowych spójnych (inaczej nasza krawędź nie zostałaby dodana). Z tego otrzymujemy, że nie istnieje lepsza ścieżka.

5) Korzystając z Lematu 1, odpowiadamy na każde pytanie między wierzchołkami a i b, znajdując maksimum na ścieżce między nimi w naszym drzewie.

##### Szczegóły techniczne:
Do tworzenia drzewa używamy Find and Union (to drzewo jest również MST naszego grafu powstałym poprzez użycie **Algorytmu Kruskala**). Aby odpowiadać na zapytania, musimy umieć znajdować w czasie logarytmicznym maksimum na ścieżce, co można zrobić, używając jump pointerów w podobny sposób, jak znajdujemy LCA 2 wierzchołków.

### Rozwiązanie wzorcowe 2 - O(nlogn + qlogn)
Stwórzmy drzewo D, które na początku ma n liści niepołączonych ze sobą krawędziami. Teraz przeiterujmy się po krawędziach w kolejności niemalejących wag. Gdy dodajemy krawędź między a i b, które wcześniej były w osobnych składowych spójnych, to w drzewie D tworzymy nowy wierzchołek X i łączymy go z wierzchołkami o indeksach $S_a$ oraz $S_b$. Następnie ustawiamy $S_a := X$ oraz $S_b := X$ (na początku $\forall i: S_i = i$). Na koniec ustawiamy $W_X$ na wagę krawędzi, którą właśnie dodaliśmy. Możemy zaobserwować, że odpowiedź na zapytanie (a, b) to $W_L$, gdzie L = lca(a, b) (dla drzewa ukorzenionego w $S_1$ – ostatnim dodanym wierzchołku reprezentującym cały spójny graf). Dowód tego jest analogiczny jak w rozwiązaniu 1, gdyż przedstawiona w tym rozwiązaniu struktura jest tak naprawdę alternatywą do odpowiadania na zapytania o maksimum na ścieżce w drzewie (struktura ta nazywa się DSU tree i jest ona bardziej przydatna w bardziej zaawansowanych problemach, lecz zachęcam do zrozumienia jej działania na tym prostym przykładzie).

### Bonus: Rozwiązanie O(nlogn + q)
W rozwiązaniu wzorcowym 2 logn dla każdego zapytania bierze się z wyliczania LCA, które umiemy wyliczyć w czasie stałym.

### Bonus 2: Inne alternatywne rozwiązania do rozwiązania pierwszego.
Z rozwiązania 1 możemy wnioskować, że odpowiedź zawsze jest ścieżką w drzewie rozpinającym naszego grafu i okazuje się, że każde minimalne drzewo rozpinające to spełnia.

Dowód:
```py
Zróbmy dowód przez sprzeczność.
W MST ścieżka z A do B ma największą wagę równą w,
a istnieje ścieżka P z A do B, której maksymalna waga krawędzi jest mniejsza niż w.
Dodajmy teraz do MST krawędź ze ścieżki P, która jeszcze w nim nie występuje i ma wagę mniejszą niż w,
tak by spowodowała powstanie cyklu zawierającego krawędź o wadze w.
Usuwając z tego cyklu krawędź o wadze w, otrzymujemy ponownie drzewo, ale o mniejszej sumie wag, co prowadzi do sprzeczności.