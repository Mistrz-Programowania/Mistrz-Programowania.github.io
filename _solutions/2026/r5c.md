---
layout: solution
title: Samotny egoista
edition: 2026
round: 5
level: c
author: Daniel Olkowski
sol_author: Tomasz Kwiatkowski
tags:
  - graphs
statement: https://szkopul.edu.pl/problemset/problem/zCigN5L04yX9VZgTOsPr539u/site/
---

Spójrzmy na relację "myśli o" jak na graf skierowany, w którym każdy wierzchołek ma dokładnie jedną krawędź wychodzącą.
Dla osoby `x` mamy krawędź `x -> t_x`.

Osoba `x` jest samotnym egoistą wtedy i tylko wtedy, gdy:
- wskazuje na siebie (`t_x = x`),
- jej stopień wejściowy wynosi `1` (jedyną osobą myślącą o `x` jest ona sama).

To podpowiada, żeby utrzymywać:
- tablicę `t` (obecne wskazania),
- tablicę `deg_in` (stopnie wejściowe),
- aktualną odpowiedź `ans` (ile osób jest samotnymi egoistami).

### Zapytanie

W zapytaniu `a b` zmieniamy jedną krawędź: `a -> stary` na `a -> b`.
Gdzie `stary = t_a` sprzed zmiany.

Status samotnego egoisty może się zmienić tylko dla:
- `a` (bo tylko dla niej mogło zmienić się równanie `t_v = v`),
- `stary` (bo jej stopień wejściowy maleje o `1`),
- `b` (bo jej stopień wejściowy rośnie o `1`).

Wystarczy więc zbiór `S = {a, stary, b}` (z usunięciem duplikatów).

Przed modyfikacją odejmujemy wkład wszystkich `v` z `S` od `ans`, potem wykonujemy aktualizację krawędzi i stopni wejściowych, a na końcu dodajemy nowy wkład tych samych `v`.

Każde zapytanie obsługujemy w czasie stałym.

### Algorytm

1. Wczytaj `t` i policz wszystkie `deg_in`.
2. Policz początkowe `ans` z warunku `t_v = v` i `deg_in[v] = 1`.
3. Dla każdego zapytania `a b`:
   - `stary = t[a]`,
   - `S = {a, stary, b}`,
   - odejmij z `ans` wszystkie osoby z `S`, które są samotnymi egoistami,
   - wykonaj zmianę:
     - `deg_in[stary]--`,
     - `t[a] = b`,
     - `deg_in[b]++`,
   - dodaj do `ans` nowy wkład osób z `S`,
   - wypisz `ans`.

### Złożoność

- Czas: `O(n + q)`.
- Pamięć: `O(n)`.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>

int main() {
    std::ios_base::sync_with_stdio(false);
    std::cin.tie(nullptr);

    int n, q;
    std::cin >> n >> q;
    std::vector<int> T(n);
    for (auto& t : T) {
        std::cin >> t;
        --t;
    }

    std::vector<int> deg_in(n);
    for (auto t : T) {
        ++deg_in[t];
    }

    auto is_special = [&](int v) {
        return T[v] == v && deg_in[v] == 1;
    };

    int ans = 0;
    for (int v = 0; v < n; ++v) {
        ans += is_special(v);
    }

    for (int i = 0; i < q; ++i) {
        int a, b;
        std::cin >> a >> b;
        --a; --b;
        auto targets = std::set{a, T[a], b};
        for (auto v : targets) {
            ans -= is_special(v);
        }
        --deg_in[T[a]];
        T[a] = b;
        ++deg_in[T[a]];
        for (auto v : targets) {
            ans += is_special(v);
        }
        std::cout << ans << '\n';
    }
    return 0;
}
```

### Python

```py
def main():
    n, q = map(int, input().split())
    T = list(map(lambda x: int(x) - 1, input().split()))

    deg_in = [0] * n
    for t in T:
        deg_in[t] += 1

    is_special = lambda v: T[v] == v and deg_in[v] == 1

    ans = 0
    for v in range(n):
        ans += is_special(v)

    out = []
    for _ in range(q):
        a, b = map(lambda x: int(x) - 1, input().split())
        targets = {a, T[a], b}
        for v in targets:
            ans -= is_special(v)
        deg_in[T[a]] -= 1
        T[a] = b
        deg_in[T[a]] += 1
        for v in targets:
            ans += is_special(v)
        out.append(str(ans))
        
    print("\n".join(out))


main()
```

### Rust

```rs
use std::io::{self, Read};

fn main() {
    let mut input = String::new();
    io::stdin().read_to_string(&mut input).unwrap();
    let mut it = input.split_whitespace();

    let n: usize = match it.next() {
        Some(v) => v.parse().unwrap(),
        None => return,
    };
    let q: usize = it.next().unwrap().parse().unwrap();

    let mut t = vec![0usize; n + 1];
    let mut indeg = vec![0i32; n + 1];
    for i in 1..=n {
        let v: usize = it.next().unwrap().parse().unwrap();
        t[i] = v;
        indeg[v] += 1;
    }

    let mut ans: i32 = 0;
    for v in 1..=n {
        if t[v] == v && indeg[v] == 1 {
            ans += 1;
        }
    }

    let mut out = String::new();
    for _ in 0..q {
        let a: usize = it.next().unwrap().parse().unwrap();
        let b: usize = it.next().unwrap().parse().unwrap();
        let old = t[a];

        let mut affected = Vec::with_capacity(3);
        for v in [a, old, b] {
            if !affected.contains(&v) {
                affected.push(v);
            }
        }

        for &v in &affected {
            if t[v] == v && indeg[v] == 1 {
                ans -= 1;
            }
        }

        indeg[old] -= 1;
        t[a] = b;
        indeg[b] += 1;

        for &v in &affected {
            if t[v] == v && indeg[v] == 1 {
                ans += 1;
            }
        }

        out.push_str(&format!("{ans}\n"));
    }

    print!("{out}");
}
```

## Uwagi

- Trzeba uważać na duplikaty w zbiorze `S = {a, stary, b}` (np. gdy `a = b` albo `stary = b`).
- W C++ i Pythonie najwygodniej użyć `set`.
