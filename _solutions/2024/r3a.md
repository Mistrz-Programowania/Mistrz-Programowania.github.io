---
layout: solution
title: Problem 1 hetmana
edition: 2024
round: 3
level: a
author: Mateusz Wesołowski
statement: https://szkopul.edu.pl/problemset/problem/W2pcU2Ko0R-MwOGcT4sYAmAc/site/
video: https://youtu.be/VrMk1y1y_F0?t=142
---

- Pole docelowe jest na tym samym polu co hetman (sprawdzamy, czy współrzędne są te same) - odpowiedź to 0.
- Pole docelowe jest na tej samej kolumnie lub tym samym wierszu (sprawdzamy, czy odpowiednia współrzędna jest ta sama) - odpowiedź to 1.
- Pole docelowe jest na tej samej linii ukośnej (sprawdzamy, czy wartości bezwzględne różnicy współrzędnej x i y pól są te same) - odpowiedź to 1.
- W przeciwnym razie nie może wykonać legalnego ruchu przesuwającego hetmana z początkowej pozycji na pole docelowe. Wtedy odpowiedź to 2.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
 ios_base::sync_with_stdio(0);
 cin.tie(0);
 cout.tie(0);

 long long hx, hy, sx, sy;
 long long delta_x, delta_y;

 cin >> hx >> hy;
 cin >> sx >> sy;

 delta_x = abs(hx-sx);
 delta_y = abs(hy-sy);

 if ( (hx==sx) && (hy==sy) ) {
	   cout << 0;
	   return 0;
 }

 if ( (hx==sx) || (hy==sy) || (delta_x==delta_y) ) {
	   cout << 1;
	   return 0;
 }

 cout << 2;
 return 0;
}
```

### Python

```py
a1, a2 = map(int, input().split())
b1, b2 = map(int, input().split())
if a1 == b1:
    if a2 == b2:
        print(0)
    else:
        print(1)
else:
    if a2 == b2:
        print(1)
    elif abs(a1 - b1) == abs(a2 - b2):
        print(1)
    else:
        print(2)
```
