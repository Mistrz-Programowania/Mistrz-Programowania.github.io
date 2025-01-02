---
layout: solution
title: Rozliczenie
edition: 2024
round: 0
level: b
author: Tomasz Kwiatkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/DZpNuDDf_-ScywcWcTDJEI-3/site/
video: https://youtu.be/RzUsfk1zWOA?t=280
---

### Rozwiązanie -- idea

Wyobraźmy sobie, że po każdych zakupach na kartce zapisujemy kwotę,
którą wydaliśmy, a następnie dzielimy ją po równo między wszystkie
osoby będące na wyjeździe.

Teraz, jeżeli pewna osoba wyjedzie, to wiemy dokładnie, ile póki co
wydała każda osoba -- kwota na kartce! Zatem w szczególności wiemy,
ile wydała wyjeżdżająca osoba. Co więcej, wiemy, że ta osoba już za
nic więcej nie zapłaci na tym wyjeździe -- bo po prostu jej nie będzie.
Czyli możemy dla niej ostatecznie powiedzieć, ile wydała!

### Rozwiązanie

Spróbujmy przenieść nasz pomysł na kod.
Łączną kwotę na kartce będziemy reprezentować przez zmienną
`current_total`. Potrzebujemy też wiedzieć, ile aktualnie
osób jeszcze jest na wyjeździe: `people_currently`.
W końcu, potrzebujemy również tablicy na wyniki dla poszczególnych
osób: `total`.

1. Początkowo mamy $n$ osób: `people_currently = n`
i nic nie wydaliśmy: `current_total = 0.0`.
2. Jeżeli robimy zakupy za kwotę `k`, to musimy zaktualizować
liczbę na kartce:
`current_total = current_total + k / people_currently`.
3. Jeżeli osoba `o` wyjeżdża, to musimy powiedzieć, ile wydała:
`total[o] = current_total`.

To tyle! Wystarczy wypisać wartości tablicy `total`.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

constexpr int MAXN = 1e5 + 1;
long double total[MAXN];

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    int people_currently = n;
    long double current_total = 0.0L;
    for (int i = 0; i < m; ++i) {
        char type;
        cin >> type;
        if (type == 'Z') {
            int k;
            cin >> k;
            current_total += (long double)k / people_currently;
        } else {
            int o;
            cin >> o;
            --people_currently;
            total[o] = current_total;
        }
    }

    cout << fixed << setprecision(12);
    for (int i = 1; i <= n; ++i)
        cout << total[i] << (i < n ? " " : "");
    cout << '\n';
    return 0;
}
```

### Python
```py
def main():
    n, m = map(int, input().split())
    total = [0.0] * n
    people_currently = n
    current_total = 0.0
    for _ in range(m):
        line = input().split()
        if line[0] == 'Z':
            k = int(line[1])
            current_total += k / people_currently
        else:
            o = int(line[1])
            people_currently -= 1
            total[o - 1] += current_total
        
    for tot in total:
        print(f"{tot:.12f}", end=" ")

main()
```
