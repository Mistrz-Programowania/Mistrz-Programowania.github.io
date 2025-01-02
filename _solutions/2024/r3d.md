---
layout: solution
title: Gra
edition: 2024
round: 3
level: d
author: Tomasz Kwiatkowski
tags:
  - graphs
  - dp
statement: https://szkopul.edu.pl/problemset/problem/XLgnBZlSu1_awphJpdcJ14Sn/site/
video: https://youtu.be/VrMk1y1y_F0?t=1010
---

Wyobraźmy sobie, że nie ruszamy planszy, tylko przesuwamy piłeczkę.
W pierwszym ruchu piłeczkę możemy przesuwać tylko lewo/prawo,
w drugim tylko góra/dół, w trzecim znowu tylko lewo/prawo, itd.
To nasuwa pomysł, aby stworzyć graf warstwowy z naszej planszy.

Każde wolne pole na planszy będzie wierzchołkiem występującym
w dwóch stanach -- kiedy możemy poruszać się lewo/prawo oraz góra/dół.
Zaczynamy z pierwszego stanu, a do końcowego pola możemy dojść w dowolnym stanie.
Każdy ruch zmienia stan wierzchołka, do którego idziemy.

Ponieważ chcemy policzyć najkrótszą ścieżkę w grafie nieważonym,
skorzystamy z algorytmu BFS. Pozostaje tylko pytanie, w jaki sposób
szybko stworzyć graf.

Moglibyśmy dla każdego wolnego pola dla każdego z czterech kierunków
przejść się pętlą dopóki pole jest wolne i w ten sposób stworzyć graf.
Takie rozwiązanie będzie miało złożoność $O(nm\cdot(n+m))$ i otrzyma 63 punkty.

Aby to przyspieszyć, skorzystamy z podejścia dynamicznego.
Powiedzmy, że dla pewnego wolnego pola chcemy policzyć
gdzie przemieści się piłeczka, jeżeli przesuniemy ją w lewo.
Jeżeli pole jest puste i znamy dla niego tę wartość,
to piłeczka przejdzie właśnie w to miejsce (i zmieni stan). W przeciwnym razie,
piłeczka pozostanie w danym wierzchołku (tylko zmieniając stan).
Powtarzając to podejście czterokrotnie i obliczając wartości w dobrej kolejności,
stworzymy graf liniowo.

Otrzymujemy liniowe rozwiązanie w złożoności $O(nm)$.

## Uwagi

- Grafy warstwowe to bardzo ciekawe podejście. Często pozwalają w elegancki sposób zamodelować pewien problem.
