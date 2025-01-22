---
layout: solution
title: Woda Czyści
edition: 2025
round: 1
level: b
author: Daniel Olkowski
sol_author: Tomasz Kwiatkowski
tags:
  - implementation
  - strings
statement: https://szkopul.edu.pl/problemset/problem/jAqYLpzf_o8fRGM8PW37L6zJ/site/
---

Możemy na bieżąco wczytywać słowo i od razu wypisywać jego pierwszą literę.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        string line;
        cin >> line;
        cout << line[0];
    }
    cout << '\n';
    return 0;
}
```

### Python

```py
def main():
    n = int(input())
    for _ in range(n):
        line = input()
        print(line[0], end='')
    print()


main()
```

## Uwagi

Te dwie linijki w C++ wpływają na szybkość wczytywania i wyświetlania danych, warto je dać, jeżeli w zadaniu mamy dużo operacji wczytywania lub wypisywania.
1. `std::ios_base::sync_with_stdio(false);`
    - Domyślnie: Strumienie standardowego wejścia/wyjścia w C++
      (`std::cin`, `std::cout`) są zsynchronizowane
      ze strumieniami wejścia/wyjścia w C (`scanf`, `printf`).
      Dzięki temu można używać obu tych mechanizmów równocześnie.
    - Co robi ta linijka: Wyłącza tę synchronizację, co przyspiesza
      operacje wejścia/wyjścia w C++, ponieważ `std::cin`
      i `std::cout` działają wtedy szybciej bez narzutu
      wynikającego z synchronizacji z biblioteką C.
2. `std::cin.tie(nullptr);`
    - Domyślnie: `std::cin` jest "połączone" (`tied`)
    z `std::cout`. To oznacza, że zanim `std::cin` coś wczyta,
    `std::cout` automatycznie "wypisze" wszystkie dane,
    które czekają w buforze.
    - Co robi ta linijka: Rozłącza `std::cin` i `std::cout`.
    Dzięki temu `std::cin` nie musi czekać,
    aż `std::cout` wypisze dane,
    co także przyspiesza działanie programu.
