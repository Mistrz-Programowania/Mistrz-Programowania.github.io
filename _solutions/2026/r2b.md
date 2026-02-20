---
layout: solution
title: Noworoczne życzenia
edition: 2026
round: 2
level: b
author: Daniel Olkowski i Adam Duda
tags:
  - implementation
  - strings
statement: https://szkopul.edu.pl/problemset/problem/wDGVylW5CaYOygHCcECoYH7N/site/
---

Możemy zliczać sumę cyfr $n$, dopóki nie będzie ona mniejsza niż 9. 1'000'000 cyfr to zdecydowanie za dużo na jakikolwiek z dostępnych w standardowych bibliotekach typów liczbowych, więc możemy użyć ciągu znaków.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
#define ll long long
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    string n;
    cin >> n;

    ll suma;
    do {
        suma = 0;
        for (char c : n) suma += (c - '0');
        n = to_string(suma);
    } while (suma > 9);

    if (suma == 1) cout << "Szczesliwego Nowego Roku";
    else cout << suma;
    return 0;
}
```

## Uwagi

- Dla większych limitów moglibyśmy wykorzystać redukcję modulo 9 (reszta z dzielenia dowolnej liczby daje sumę jej cyfr). W tym zadaniu nie było to konieczne ze względu na małe limity do długości $n$.
