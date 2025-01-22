---
layout: solution
title: Pomiary
edition: 2025
round: 5
level: c
author: Tomasz Kwiatkowski
tags:
  - pref
statement: https://szkopul.edu.pl/problemset/problem/Bh4iXTzXZ4-g19K_wwKTIq3t/site/
---

Zauważmy, że ustalając pierwszy element, drugi i każdy kolejny jest też ustalony.

### Rozwiązanie wolne

Możemy sprawdzić każdy możliwy pierwszy element $a_1$. Może on przyjmować wartości od $0$ do $s_1$.
Następnie możemy obliczyć wszystkie pozostałe wartości $a_i$. Wystarczy sprawdzić, czy każda wartość $a_i$ mieści się w zakresie $[0, 10^9]$. Jeżeli tak, odpowiedź jest pozytywna oraz mamy znaleziony ciąg. Jeżeli nie, to sprawdzamy kolejną wartość $a_1$. Może się zdarzyć, że żadna wartość nie da nam poprawnego ciągu -- wtedy odpowiedź jest negatywna.

Daje to rozwiązanie w złożoności $O(n \cdot \max s_i)$.

### Rozwiązanie wzorcowe

Przyspieszymy powyższe rozwiązanie. Aby to zrobić, dokładniej przyjrzyjmy się procesowi obliczania pozostałych wartości $a_i$ na podstawie $a_1 = x$.

| Indeks   | 1   | 2         | 3               | 4                     | ... |
|----------|-----|-----------|-----------------|-----------------------|-----|
| Ciąg a_i | $x$ | $s_1 - x$ | $s_2 - s_1 + x$ | $s_3 - s_2 + s_1 - x$ | ... |

Co wiemy o ciągu $a_i$? Jest nieujemny, zatem:

- $x \geq 0$
- $s_1 - x \geq 0$
- $s_2 - s_1 + x \geq 0$
- $s_3 - s_2 + s_1 - x \geq 0$
- ...

Dostaliśmy na przemian dolne i górne ograniczenia na $x = a_1$:

- $x \geq 0$
- $x \leq s_1$
- $x \geq s_1 - s_2$
- $x \leq s_1 - s_2 + s_3$
- ...

Zauważmy, że kolejne wyrażenia prawych stron nierówności rożnią się od siebie jednym wyrazem. Możemy zatem obliczyć ich wartości, podobnie do obliczania sum prefiksowych.

Jakie ograniczenie na $x = a_1$ dostaliśmy? Dolne ograniczenie, to największe z dolnych ograniczeń. A górne, to najmniejsze z górnych. Zatem $\max(0, s_1 - s_2, ...) \leq x \leq \min(s_1, s_1 - s_2 + s_3, ...)$.

Pozostało sprawdzić, czy istnieje jakikolwiek $x$ spełniający te dwie nierówności, a jeżeli tak, odtworzyć (jak w wolnym rozwiązaniu) ciąg.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

constexpr int maxv = 1e9;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    vector<int> S(n - 1);
    int64_t lo = 0, hi = maxv;
    int64_t prv = 0;
    for (int i = 0; int &s : S) {
        cin >> s;
        if (i % 2 == 0) {
            prv += s;
            hi = min(hi, prv);
        }
        else {
            prv -= s;
            lo = max(lo, prv);
        }
        ++i;
    }
    if (lo > hi) {
        cout << "Nie\n";
        return 0;
    }
    cout << "Tak\n";
    int64_t x = lo;
    cout << x;
    for (int &s : S) {
        x = s - x;
        cout << ' ' << x;
    }
    cout << '\n';
    return 0;
}
```

### Python

```py
maxv = 10**9


def main():
    _ = int(input())
    S = list(map(int, input().split()))

    lo, hi = 0, maxv
    prv = 0
    for i, s in enumerate(S):
        if i % 2 == 0:
            prv += s
            hi = min(hi, prv)
        else:
            prv -= s
            lo = max(lo, prv)

    if lo > hi:
        print("Nie")
        return
    print("Tak")
    x = lo
    print(x, end="")
    for s in S:
        x = s - x
        print("", x, end="")
    print()


main()
```
