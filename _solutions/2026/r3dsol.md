---
layout: solution
title: Idealne drzewo
edition: 2026
round: 3
level: d
author: Maciej Wiśniewski
tags:
  - graphs
statement: https://szkopul.edu.pl/problemset/problem/it5A9WzDeJLCG4XpTQ5Ngz9T/site/ 
---

W zadaniu mamy podany graf oraz drzewo. Naszym zadaniem jest znaleźć drzewo w grafie, lub stwierdzić że nie istnieje. Ten problem, w ogólnym przypadku jest trudny, ale w tym zadaniu specificznie dobrane limity, pozwalały na sprytne rozwiązanie.

### Twierdzenie 1
Jeżeli **minimalny** stopień w grafie $G$ wynosi przynajmniej $t-1$ to można w nim znaleźć **dowolne** drzewo $T$ o $t$ wierzchołkach.

#### Dowód:
Ponieważ minimalny stopień w $G$ wynośi przynajmniej $t - 1$, to musi mieć też przynajmniej $t$ wierzchołków.
Weźmy dowolny wierzchołek $v \in G$ i przypiszmy go dowolnemu wierzchołkowi $u \in T$. 
Następnie będziemy powtarzać ten algorytm dopóki nie znajdziemy wszystkich $t$ wierzchołków:

Weźmy dowolny z już przypisanych wierzchołków $v \in G$, który w $T$ ma więcej sąsiadów niż na razie wybraliśmy.
Taki wierzchołek, na pewno ma sąsiada w $G$, który jeszcze nie został przypisany do żadnego wierzchołka w $T$ 
(bo zostało wybranych mniej niż $t$ wierzchołków, a on ma stopień przynajmniej $t - 1$).
W takim wypadku możemy go przypisać do sąsiada odpowiednika $v$ w $T$ i powtarzać proces.

### Twierdzenie 2
Przy limitach z zadania, w oryginalnym grafie $G$ da się wybrać podzbiór $S$ wierzchołków w taki sposób żeby w grafie złożonym tylko z wierzchołków $S$ i krawędzi które mają oba końce w $S$ minimalny stopień to co najmniej $t - 1$.

#### Dowód:
Niech $H$ będzie dopełnieniem $G$ (czyli krawędź $(u, v)$ istnieje w $H$ wtedy i tylko wtedy gdy **nie** istnieje w $G$). Zauważmy że $H$ ma $n$ wierzchołków i $m$ krawędzi. 

Chcemy znaleźć taki zbiór wierzchołków $S$, że w grafie $G[S]$ każdy wierzchołek ma stopień przynajmniej $t-1$.

Zauważmy, że dla dowolnego wierzchołka $v \in S$ zachodzi

$$ \deg_{G[S]}(v) = (|S|-1) - deg_{H[S]}(v)$$


A więc warunek

$$ \deg_{G[s]}(v) \geq t-1 $$

jest równoważny warunkowi

$$ \deg_{H[s]}(v) \leq |S|-t $$

Wystarczy więc znaleźć taki zbiór $S$, że w grafie $H[S]$ każdy wierzchołek ma stopień co najwyżej $|S|-t$.

Zaczynamy od zbioru wszystkich wierzchołków i dopóki istnieje wierzchołek $v$ taki, że

$$ \deg_{H[s]}(v) > |S|-t $$

usuwamy go ze zbioru $S$.

Jeżeli ten proces się zatrzyma, to dla każdego pozostałego wierzchołka $v \in S$ mamy

$$ \deg_{H[s]}(v) \leq |S|-t $$

Pozostaje tylko udowodnić, że po zakończeniu tego procesu w zbiorze $S$ zostanie co najmniej $t$ wierzchołków.
    
Załóżmy nie wprost, że usuwaliśmy wierzchołki tak długo, aż zostało ich mniej niż $t$.

Przeanalizujmy kolejne usunięcia.
Jeżeli w pewnym momencie aktualny zbiór miał rozmiar $s$, to usuwany wtedy wierzchołek miał w grafie $H[S]$ stopień większy niż $s-t$, czyli co najmniej

$$ s - t + 1$$

To znaczy, że w chwili usuwania znikało z grafu $H$ co najmniej $s-t+1$ krawędzi.

Jeżeli proces doszedłby aż do momentu, w którym zostałoby mniej niż $t$ wierzchołków, to liczba usuniętych krawędzi byłaby co najmniej

$$ 1 + 2 + 3 + \cdots + (n - t + 1) = \frac{(n - t + 1) (n - t + 2)}{2}$$

Z założeń zadania wiemy jednak, że $m \leq \frac{n^2}{8}$ orazz $t \leq \frac{n}{2}$. Stąd

$$ \frac{(n - t + 1) (n - t + 2)}{2} \geq \frac{(\frac{n}{2} + 1)(\frac{n}{2} + 2)}{2} > \frac{n^2}{8} >= m$$

Czyli żeby proces usuwania wierzchołków na końcu zostawił mniej niż $t$ wierzchołków musielibyśmy usunąć więcej niż $m$ krawędzi z $H$, a $H$ ma dokładnie $m$ krawędzi, więc jest to niemożliwe.

### Rozwiązanie wzorcowe
Tak jak udowodniliśmy wyżej w zadaniu zawsze da się znaleźć wynik. Robimy to najpierw usuwając wierzchołki które mają mało krawędzi aż dostaniemy wystarczająco gęsty graf (Twierdzienie 2), a następnie zachłannie przyporządkowując pozostałe wierzchołki do drzewa (Twierdzenie 1).
