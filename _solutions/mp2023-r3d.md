---
layout: solution
title: Nieudany szyfr
edition: 2023
round: 3
level: d
sol_author: Tomasz Kwiatkowski
tags:
  - dp
  - combinatorics
statement: https://szkopul.edu.pl/problemset/problem/sarFyLVwJIS1CC9V7YKV50As/site/
---

Zadanie rozwiążemy dynamicznie. Niech `dp[i]` oznacza ile słów daje szyfr złożony z pierwszych $i$ cyfr wejścia. Dla zeru znaków mamy $1$ sposób -- puste słowo, `dp[0] = 1`.

Dalej zastanówmy się, jak obliczyć `dp[i]`. Mamy dwie opcje albo ostatni znak ma kod jednocyfrowy, albo dwucyfrowy. Zatem jeżeli ostatnia cyfra nie jest zerem, to do `dp[i]` dodajemy `dp[i - 1]`. Jeżeli ostatnie dwie cyfry tworzą liczbę z zakresu $[10, 26]$, to do `dp[i]` dodajemy `dp[i - 2]` (oczywiście, jeżeli $i > 1$). Wynik będzie w ostatniej komórce `dp`.

Zauważmy, że tak naprawdę nie potrzebujemy pamiętać wszystkich wartości tablicy `dp`. Wystarczą nam dwie ostatnie. W rozwiązaniu używam dwóch zmiennych -- `a` oznaczającej `dp[i - 2]` oraz `b` oznaczającej `dp[i - 1]`. Funkcja `ok_char` zwraca wartość logiczną i mówi, czy ostatnie $d+1$ cyfr odpowiada jakiemuś znakowi lub nie.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1e9 + 7;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    string s;
    cin >> s;
    int a = 1, b = 1;
    auto ok_char = [&](int i, int d) {
        return (d == 0 && s[i] > '0') ||
               (d == 1 && s[i - 1] == '1') ||
               (d == 1 && s[i - 1] == '2' && s[i] < '7');
    };
    for (int i = 1; i < int(s.size()); ++i) {
        int c = (ok_char(i, 1) * a + ok_char(i, 0) * b) % MOD;
        a = b;
        b = c;
    }
    cout << b << '\n';
    return 0;
}
```

### Python

```py
MOD = 1000000007

def main():
    s = input()
    a = b = 1
    for c1, c2 in zip(s[:-1], s[1:]):
        a, b = b, (a * (c1 == '1' or (c1 == '2' and c2 < '7')) + b * (c2 > '0')) % MOD
    print(b)

main()
```

## Uwagi

- W C++ znak to też liczba (tzn. np. `'0'` odpowiada jej kodowi ASCII -- $48$). Dzięki temu nie musimy pamiętać kodów ASCII znaków, wystarczy, że porównujemy zmienne po prostu z danym znakiem.
- W Pythonie też można tak porównywać znaki.
