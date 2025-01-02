---
layout: solution
title: Platforma
edition: 2023
round: 3
level: e
sol_author: Mikołaj Bulge
tags:
  - segment_trees
statement: https://szkopul.edu.pl/problemset/problem/ap1YUQsxZoA9zDP62ps0ufdO/site/
---

Rozwiązaniem założonym przez autora było utworzenie drzewa przedziałowego punkt--przedział, które umożliwia wykonanie operacji dodania w punkcie oraz którego każdy węzeł zna sumę swojego poddrzewa. W węzłach drzewa początkowo znajduje się wartość $0$. Po pojawieniu się na platformie wartości $x$, należy dodać $1$ do $x$-tego liścia. Teraz, aby znaleźć $k$-tą najmniejszą wartość spośród dotychczas dodanych do drzewa, należy wykonać poniższy algorytm:

- Ustaw `cnt` = $k$
- Zaczynając od korzenia powtarzaj: jeśli w lewym poddrzewie suma wynosi co najmniej `cnt`, to przejdź do niego. W przeciwnym wypadku odejmij od `cnt` sumę lewego poddrzewa i przejdź do prawego poddrzewa.
- Algorytm zakończ po dotarciu do liścia. Jeśli jest to $x$-ty liść od lewej, to znaczy, że $x$ jest $k$-tą najmniejszą wartością z dotychczas wstawionych do drzewa wartości.

W ten sposób po dodaniu każdego elementu można łatwo znaleźć $\frac{n}{2}$-tą najmniejszą wartość (oraz $\frac{n}{2}+1$, jeśli potrzeba).

Alternatywnie do rozwiązania zadania można użyć struktury `ordered_set`, która jest opisana w [tym](https://codeforces.com/blog/entry/11080) linku.

Oba rozwiązania mają jednakową złożoność obliczeniową, która wynosi $\Theta(n\cdot log_2(n))$.
