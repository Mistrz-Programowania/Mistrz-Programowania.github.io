---
layout: solution
title: Dylatacja++
edition: 2025
round: 4
level: c
author: Tomasz Kwiatkowski
tags:
  - pref
statement: https://szkopul.edu.pl/problemset/problem/K1y1ltLw1VLnKZh3Wkkmw7x1/site/
---

Najpierw, dla każdego wiersza policzmy, gdzie znajdują się szczeliny -- dla każdego wiersza obliczmy sumy prefiksowe.

Naszym zadaniem jest znaleźć wartość (czyli numer kolumny), która występuje najczęściej (to oznacza, że w danej kolumnie jest najwięcej szczelin -- przetniemy najmniej paneli) oraz najrzadziej (analogicznie -- przetniemy najwięcej paneli). 

Liczby wystąpień poszczególnych sum prefiksowych można pamiętać na mapie. Wtedy łatwo dostaniemy największą liczbę szczelin w kolumnie.

Aby obliczyć najrzadziej występującą sumę prefiksową trzeba nieco bardziej uważać. Może to być wartość $0$ (istnieje kolumna bez żadnej szczeliny, wtedy nie mamy tego w mapie), ale jeżeli w każdej kolumnie występuje jakaś szczelina, to wartość będzie większa...

Ale zauważmy, że w maksymalnie pierwszych $P + 1$ (liczbie paneli $+ 1$) kolumnach wystąpi kolumna bez szczeliny.
Możemy zatem brutalnie sprawdzić każdą kolumnę o numerach np. w zakresie $[1, 500\,001]$.

Otrzymujemy rozwiązanie w złożoności $O(P \log{P})$.

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
    vector<vector<int>> rows(n);
    for (auto &row : rows) {
        int panel_n;
        cin >> panel_n;
        row.resize(panel_n);
        for (int &len : row) {
            cin >> len;
        }
    }

    map<int, int> pref;
    int total_len = 0;
    int maxx = 0;
    for (const auto &row : rows) {
        total_len = 0;
        for (int i = 0; i < int(row.size()); ++i) {
            total_len += row[i];
            ++pref[total_len];
            if (i != int(row.size()) - 1) {
                maxx = max(maxx, pref[total_len]);
            }
        }
    }

    int minn = n;
    for (int x = 1; x < min(total_len, 500007); ++x) {
        minn = min(minn, pref[x]);
    }
    cout << n - maxx << ' ' << n - minn << '\n';
    return 0;
}
```

### Python

```py
def main():
    n = int(input())
    rows = [list(map(int, input().split()[1:])) for _ in range(n)]

    pref = {}
    maxx = 0
    for row in rows:
        total_len = 0
        for i in range(len(row)):
            total_len += row[i]
            pref[total_len] = pref.get(total_len, 0) + 1
            if i != len(row) - 1:
                maxx = max(maxx, pref.get(total_len, 0))

    minn = n
    for x in range(1, min(total_len, 500007)):
        minn = min(minn, pref.get(x, 0))

    print(n - maxx, n - minn)


main()
```

## Uwagi

- Zadanie wzorowane na zadaniu kwalifikacyjnym do LinkedIn. W tamtej wersji wystarczyło wypisać tylko pierwszą liczbę.
