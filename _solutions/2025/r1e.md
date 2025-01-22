---
layout: solution
title: Kartograf i kawa
edition: 2025
round: 1
level: e
author: Tomasz Kwiatkowski
tags:
  - graphs
  - dijkstra
  - bs
statement: https://szkopul.edu.pl/problemset/problem/XNM5SohfyqZRv-Qv-qPSvLgs/site/
---

W zadaniu, mamy dany skierowany graf ważony, którego niektóre wagi są ukryte. Naszym celem jest tak dobrać ukryte wagi, aby najkrótsza ścieżka od $1$ do $n$ miała długość dokładnie $t$ lub powiedzieć, że się nie da.

### Wszystkie krawędzie mają znaną wagę (podzadanie 2)

Kiedy znamy wagę wszystkich krawędzi, nie mamy dużego pola do popisu. Musimy sprawdzić, czy faktycznie najkrótsza ścieżka między $1$ a $n$ ma dokładnie $t$. Służy do tego algorytm Dijkstry.

Otrzymujemy rozwiązanie w złożoności $O(n \log{n})$.

### Pełne rozwiązanie

Ustalmy, że każda nieznana krawędź ma najmniejszą możliwą wagę $1$. Jeżeli nadal najkrótsza ścieżka od $1$ do $n$ jest dłuższa niż $t$ (lub w ogóle nie istnieje), to odpowiedź w zadaniu będzie negatywna.

Jeżeli jej długość wynosi dokładnie $t$, to mamy odpowiedź. Załóżmy jednak, że tak nie jest, musimy zwiększyć przypisane wagi.

**Obserwacja:** Jeżeli wybierzemy dowolną nieznaną krawędź i zwiększymy jej wagę, to odległość z $1$ do $n$ na pewno nie zmaleje (wzrośnie o $1$ lub pozostanie taka, jak była).

To prowadzi do rozwiązania! Możemy zwiększać kolejne nieznane krawędzie, aż do maksymalnej wagi $10^9$. Aby zrobić to efektywnie, można skorzystać z wyszukiwania binarnego. Jeżeli otrzymamy za długą ścieżkę, to powinniśmy zmniejszyć wagi, jeżeli za krótką, to zwiększyć.

Dzięki temu, otrzymujemy rozwiązanie w złożoności $O(n \log{n} \log{max_c})$.

## Przykładowe rozwiązanie

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

constexpr int min_val = 1;
constexpr int max_val = 1e9;
constexpr int64_t max_time = 1e14;

int64_t dijkstra(const vector<vector<tuple<int, int, int>>> &adj, auto edge_val) {
    vector<int64_t> dist(adj.size(), max_time + 1);
    priority_queue<pair<int64_t, int>> Q;
    Q.emplace(0, 0);
    dist[0] = 0;
    while (Q.size()) {
        auto [d, node] = Q.top();
        Q.pop();
        for (auto [neigh, len, i] : adj[node]) {
            len = (len != -1 ? len : edge_val(i));
            if (dist[node] + len < dist[neigh]) {
                dist[neigh] = dist[node] + len;
                Q.emplace(-dist[neigh], neigh);
            }
        }
    }
    return dist.back();
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n, m;
    int64_t t;
    cin >> n >> m >> t;
    vector<tuple<int, int, int>> edges(m);
    vector<vector<tuple<int, int, int>>> adj(n);
    for (int i = 0; auto &[u, v, w] : edges) {
        cin >> u >> v >> w;
        --u; --v;
        adj[u].emplace_back(v, w, i++);
    }

    int upper_weight = *ranges::upper_bound(views::iota(1, max_val), t, {}, [&](int val) {
        return dijkstra(adj, [&](int) { return val; });
    });

    int ind = *ranges::lower_bound(views::iota(0, m), t, {}, [&](int val) {
        return dijkstra(adj, [&](int i) { return i < val ? upper_weight : upper_weight - 1; });
    });
    
    if (dijkstra(adj, [&](int i) { return max(min_val, i < ind ? upper_weight : upper_weight - 1); }) != t) {
        cout << "-1\n";
        return 0;
    }

    for (int i = 0; i < m; ++i) {
        if (get<2>(edges[i]) != -1) {
            cout << get<2>(edges[i]) << '\n';
        }
        else {
            cout << max(min_val, i < ind ? upper_weight : upper_weight - 1) << '\n';
        }
    }
    return 0;
}
```

### Python

```py
import heapq

min_val = 1
max_val = 10**9
max_time = 10**14

def dijkstra(adj, edge_val):
    dist = [max_time + 1] * len(adj)
    Q = []
    heapq.heappush(Q, (0, 0))
    dist[0] = 0
    while len(Q):
        _, node = heapq.heappop(Q)
        for neigh, length, i in adj[node]:
            length = length if length != -1 else edge_val(i)
            if dist[node] + length < dist[neigh]:
                dist[neigh] = dist[node] + length
                heapq.heappush(Q, (dist[neigh], neigh))
    
    return dist[-1]


def upper_bound(adj, value):
    lo, hi = 1, max_val
    while lo < hi:
        mid = (lo + hi) // 2
        if dijkstra(adj, lambda _: mid) <= value:
            lo = mid + 1
        else:
            hi = mid
    return lo


def lower_bound(adj, m, upper_weight, value):
    lo, hi = 0, m
    while lo < hi:
        mid = (lo + hi) // 2
        if dijkstra(adj, lambda i: upper_weight if i < mid else upper_weight - 1) < value:
            lo = mid + 1
        else:
            hi = mid
    return lo


def main():
    n, m, t = map(int, input().split())
    edges = [tuple(map(int, input().split())) for _ in range(m)]
    adj = [[] for _ in range(n)]
    for i, edge in enumerate(edges):
        a, b, c = edge
        adj[a - 1].append((b - 1, c, i))

    upper_weight = upper_bound(adj, t)
    ind = lower_bound(adj, m, upper_weight, t)
    
    if dijkstra(adj, lambda i: max(min_val, upper_weight if i < ind else upper_weight - 1)) != t:
        print(-1)
    else:
        for i in range(m):
            if edges[i][2] != -1:
                print(edges[i][2])
            else:
                print(max(min_val, upper_weight if i < ind else upper_weight - 1))


main()
```

## Uwagi

- Wyzwanie: potrafisz "zbić" jeden logarytm i otrzymać złożoność $O(n \log{n})$?
