---
layout: solution
title: Łowca promocji
edition: 2024
round: 2
level: c
author: Tomasz Kwiatkowski
tags:
  - sweep
  - sortings
statement: https://szkopul.edu.pl/problemset/problem/-T25qNAvUMugFGqNtkAYorq5/site/
video: https://youtu.be/oMfDh98RN5w?t=472
---

W zadaniu mamy tak naprawdę zdarzenia postaci: w dniu $x$ cena produktu $y$ zmienia się o $z$.
Łatwo możemy przekształcić wejście do takich zdarzeń.
Każda promocja to dwa zdarzenia -- w dniu $p$ cena produktu spada, w dniu $k+1$ cena produktu
powraca do początkowej wartości.

Mając $2\cdot m$ takich zdarzeń, zasymulujemy przebieg promocji w czasie.
Będziemy przeglądać zdarzenia od najwcześniejszego, do najpóźniejszego.
Będziemy trzymać aktualną oraz najlepszą obniżkę cen produktów wraz
z pierwszym dniem, w którym została osiągnięta.

Gdy rozpatrujemy dane zdarzenie, aktualizujemy aktualną sumę obniżek
i sprawdzamy, czy nasza suma jest większa od poprzedniej najlepszej.

Otrzymujemy rozwiązanie w złożoności $O(n + m \log m)$.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    cin >> n >> m;
    vector<int> prices(n);
    for (auto &price : prices)
        cin >> price;
    
    // {day, price_delta}
    vector<pair<int, int>> events;
    for (int i = 0; i < m; i++) {
        int prod, start, end, offer_price;
        cin >> prod >> start >> end >> offer_price;
        events.emplace_back(start, prices[prod - 1] - offer_price);
        events.emplace_back(end + 1, -(prices[prod - 1] - offer_price));
    }
    sort(events.begin(), events.end());

    pair<long long, int> best_discount = {0, -1};
    long long current_discount = 0;
    for (const auto &[day, price_delta] : events) {
        current_discount += price_delta;
        best_discount = max(best_discount, {current_discount, -day});
    }

    long long total_price = 0;
    for (const auto &price : prices)
        total_price += price;
    cout << total_price - best_discount.first << ' ' << -best_discount.second << '\n';
    return 0;
}
```

### Python

```py
def main():
    n, m = map(int, input().split())
    prices = list(map(int, input().split()))
    events = []
    for _ in range(m):
        prod, start, end, offer_price = map(int, input().split())
        events.append((start, prices[prod - 1] - offer_price))
        events.append((end + 1, -(prices[prod - 1] - offer_price)))

    best_discount = (0, -1)
    current_discount = 0
    for day, price_delta in sorted(events):
        current_discount += price_delta
        best_discount = max(best_discount, (current_discount, -day))
    print(sum(prices) - best_discount[0], -best_discount[1])


main()
```
