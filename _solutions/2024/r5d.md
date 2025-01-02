---
layout: solution
title: Gangi i Sekty
edition: 2024
round: 5
level: d
sol_author: Maciej Wiśniewski
statement: https://szkopul.edu.pl/problemset/problem/mm8ZYsQRcg6UeXkAs5ooePZr/site/
video: https://youtu.be/YDD-yt7Y_tE&t=1408
---

Weźmy dowolne $k$ wierzchołków i odpalmy na nich dfsa. Jeżeli znajdziemy cykl, będzie on miał
długość conajwyżej $k$, więc będzie poprawnym gangiem. Jeżeli nie znajdziemy cyklu, to te $k$ wybranych
wierzchołków tworzą drzewo (albo zbiór drzew), które możemy **dwukolorować**. Jededn z kolorów będzie miał
przynajmniej $\lceil \frac{k}{2} \rceil$ wierzchołków, więc będzie można z niego wybrać poprawną sektę.

### Dwukolorowanie
Dwukolorowanie grafu polega na pomalowaniu jego wierzchołków na dwa kolory, tak aby żadne dwa wierzchołki w jednym kolorze
nie były sąsiadami. Nie we wszystkich grafach jest ono możliwe, ale w drzewach jest wręcz proste. Dajemy dowolnemu wierzchołkowi
kolor 1, wszystkim jego sąsiadom kolor 2, ich sąsiadom znowu 1, itd. na zmianę aż pomalujemy całe drzewo.
