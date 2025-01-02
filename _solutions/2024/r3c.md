---
layout: solution
title: Po co nam para uszu?
edition: 2024
round: 3
level: c
author: Tomasz Kwiatkowski
tags:
  - interactive
  - bs
statement: https://szkopul.edu.pl/problemset/problem/5I5CNZUFv3Ri9b2lwuPpiBw-/site/
video: https://youtu.be/VrMk1y1y_F0?t=681
---

Rozwiążmy najpierw podzadanie, w którym źródło znajduje się na osi OX ($y=0$).
Powiedzmy, że zawsze będziemy patrzeć "do góry" (tj. w kierunku rosnących x-ów).

Jeżeli słyszymy dźwięk z lewej strony, to pójdziemy w lewo, jeżeli z prawej strony, to w prawo.
Pozostaje pytanie, o ile w lewo/prawo należy się przesunąć.
Odpowiedzią na to pytanie jest: o połowę możliwego dystansu.
Tym sposobem każde pytanie zmniejsza nam potencjalną liczbę położeń źródła o połowę.

Aby rozwiązać zadanie w całości, najpierw w opisany wyżej sposób znajdziemy
pozycję x-ową źródła, a następnie analogicznie znajdziemy y-ową.

Otrzymujemy rozwiązanie w złożoności $O(\log{n})$.

## Przykładowe implementacje

### C++

```cpp
#include "r3clib.h"

int find_pos(auto go_higher) {
    int lo = -1e9, hi = 1e9;
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (go_higher(mid))
            lo = mid + 1;
        else
            hi = mid;
    }
    return lo;
}

int main() {
    int x = find_pos([](int a) { return sluchaj(a, 0, a, 1) > 0; });
    int y = find_pos([](int a) { return sluchaj(0, a, -1, a) > 0; });
    odpowiedz(x, y);
    return 0;
}
```

### Python
```py
import r3clib

def find_pos(go_higher):
    lo = -10**9
    hi = 10**9
    while lo < hi:
        mid = lo + (hi - lo) // 2
        if go_higher(mid):
            lo = mid + 1
        else:
            hi = mid
    return lo

def main():
    x = find_pos(lambda a: r3clib.sluchaj(a, 0, a, 1) > 0)
    y = find_pos(lambda a: r3clib.sluchaj(0, a, -1, a) > 0)
    r3clib.odpowiedz(x, y)

main()
```

## Uwagi

- W rozwiązaniu zastosowaliśmy dwukrotnie *wyszukiwanie binarne* -- bardzo silny i ważny algorytm.
- Implementując rozwiązanie w C++ należało być ostrożnym podczas dzielenia przez $2$ w wyszukiwaniu binarnym. Kiedy dzielimy dwie liczby całkowite, C++ domyślnie zaokrągla w stronę $0$ -- dla liczb dodatnich "w dół", ale dla liczb ujemnych "w górę"!
