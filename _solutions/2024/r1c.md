---
layout: solution
title: Obserwacje
edition: 2024
round: 1
level: c
author: Tomasz Kwiatkowski
tags:
  - data_structures
statement: https://szkopul.edu.pl/problemset/problem/0MGRAVCrG-kz7K10InrFNVNI/site/
video: https://youtu.be/qIraPa7A1wY?t=960
---

Zauważmy, że gdy osoba A obserwuje osobę B oraz osoba B obserwuje osobę A,
to równie dobrze te dwie osoby mogłyby się wzajemnie nie obserwować -- wynik
nie zmieniłby się.

Pozbądźmy się takich sytuacji z wejścia.
Będziemy pamiętać (przy pomocy mapy, seta, ...), jakie obserwacje już wystąpiły.
Gdy wczytujemy teraz, że osoba A obserwuje osobę B, to sprawdzamy,
czy wcześniej nie wystąpiła już sytuacja, gdzie osoba B obserwowała osobę A.
Jeżeli tak było, to zapominamy o obu obserwacjach.

Zobaczmy, że wśród pozostałych obserwacji, jeżeli osoba A jest obserwowana
przez osobę B, to na pewno A nie obserwuje B.
Pozostało zatem dla każdej osoby zliczyć, ile jest obserwacji danej osoby
wśród pozostałych par.

Dostajemy rozwiązanie w złożoności $O(n + m \log{m})$.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    set<pair<int, int>> edges;
    for (int i = 0; i < m; ++i) {
        int a, b;
        cin >> a >> b;
        auto it = edges.find({b, a});
        if (it != edges.end())
            edges.erase(it);
        else
            edges.insert({a, b});
    }
    vector<int> ans(n);
    for (const auto &[a, b] : edges)
        ++ans[b - 1];
    cout << ans[0];
    for (const int &val : ans | views::drop(1))
        cout << ' ' << val;
    cout << '\n';
    return 0;
}

```

### Python

```py
def main():
    n, m = map(int, input().split())
    edges = set()
    for _ in range(m):
        a, b = map(int, input().split())
        if (b, a) in edges:
            edges.remove((b, a))
        else:
            edges.add((a, b))
    ans = [0] * n
    for _, b in edges:
        ans[b - 1] += 1
    print(' '.join(map(str, ans)))

main()
```

## Uwagi

- Set oraz mapa to przydatne, nie tylko w kontekście Olimpiady, struktury danych
- Dokumentacja `std::set` -- [C++](https://en.cppreference.com/w/cpp/container/set)
oraz `set` -- [Python](https://docs.python.org/3/tutorial/datastructures.html#sets)
- Ciekawostka: to zadanie da się zrobić nieco szybciej. Czy potrafisz rozwiązać to zadanie w czasie $O(n+m)$?
