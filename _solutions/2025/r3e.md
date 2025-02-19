---
layout: solution
title: Komputer w domu
edition: 2025
round: 3
level: e
author: Maciej Wiśniewski
tags:
statement: https://szkopul.edu.pl/problemset/problem/H2EmADr4OxG4emstOQSxmScB/site/
---

Trudność zadania polega na tym że przejść w komputerze możemy używać zawsze o ile są legalne. Musimy więc zaprojektować je w taki sposób, aby były legalne tylko w sytuacjach w których są pożądane (checmy zasymulować krawędź), lub takich które nic nie psują (np. prowadzą do stanu który nie jest wierzchołkiem i już nigdy do żadnego wierzchołka nie wejdzie).

## Rozwiązanie nieoptymalne (d = n)

$i$-temu wierzchołkowi przypisujemy wektor który ma same zera, poza jedną jedynką na pozycji $i$. Wtedy każda krawędź $i \rightarrow j$ mogła być zareprezentowana, przez wektor samych zer poza jedynką na pozycji $j$ oraz minus jedynką na pozycji $i$.

## Rozwiązanie prawie optymalne (d = 3)

Załóżmy że mamy krawędź, którą checmy móc przejść tylko z jednego konkretnego wierzchołka. W jaki sposób możemy "rozpoznać" czy jesteśmy w tym wierzchołku na komputerze w domu? Musimy znaleźć jakąś częściową operację, która jest możliwa tylko z tego wierzchołka. W poprzednim rozwiązaniu, było to odjęcie $1$ na odpowiednim indeksie. Ponieważ tylko jeden wierzchołek ma tam dodatnią liczbę, to tylko z tego wierzchołka jest to możliwe. Niestety ta operacja zużywa $n$ wymiarów. Jeżeli natomiast zareprezentujemy wierzchołek $i$ jako parę $(i, n - i + 1)$, to możemy "rozpoznać" wierzchołek używając tylko dwóch wymiarów. Dla każdej pary wierzchołków, jeden z nich będzie miał większą pierwszą liczbę z pary, a drugi drugą, więc żaden wierzchołek nie będzie mógł użyć nie swojej operacji.

Niestety, oba wymiary "zużywamy" na "rozpoznanie" w którym wierzchołku jesteśmy, ale musimy jeszcze gdzieś zapisać informację do jakiego wierzchołka chcemy dojść. Nie możemy jednocześnie odejmować i dodawać liczb na tym samym indeksie. Rozwiązanie w $d = 4$ zapisuje wierzchołki jako $(i, n - i + 1, 0, 0)$, a dla każdej krawędzi odejmuje parę odpowiadającą pierwszemu wierzchołkowi z dwóch pierwszych indeksów oraz dodaje współrzędne drugiego wierzchołka na indeksach $3$ i $4$. Oprócz tego są pomocnicze przejścia, które pozwalają przenieść liczby z indeksów
$3$ i $4$ na indeksy $1$ i $2$.

## Rozwiązanie optymalne (d = 3)

$i$-temu wierzchołkowi przypisujemy wektor $(100\cdot (n - i + 1), i, 0)$
Do tego dla krawędzi $j$ zapisujemy sobie pomocnicze stany $t_j = (j, 0, 100 * (m - j + 1))$ oraz $s_j = (0, 100 * (m - j + 1), j)$.

Idea jest taka, że krawędź numer $j$, $u \rightarrow v$ rozbijemy na $3$ przejścia:

- $u \rightarrow t_j$
- $t_j \rightarrow u_j$
- $u_j \rightarrow v$

Zauważmy że w każdym przejściu mamy te same $3$ operacje, ale na różnych indeksach:

- Zmniejsz $100 * x_1$ do $y_2$
- Zmniejsz $y_1$ do 0
- Zwiększ $0$ do $100 * x_2$

Zauważmy że te dwa zmniejszenia gwarantują unikalność na tej samej zasadzie co rozwiązanie $d = 4$.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

#define pii pair<int, int>
#define fi first
#define se second

constexpr int MAXN = 20;
constexpr int MAXM = 20;
constexpr int C = 1e2;
constexpr int D = 3;

int n, m;

vector <pii> edges;

void input() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);
    cout.tie(0);

    cin >> n >> m;
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        edges.push_back({a, b});
    }
}

vector <vector<int>> states, instructions;

vector <int> state(int node) {
    return {C * (n - node + 1), node, 0};
}

vector <int> sub(vector <int> a, vector <int> b) {
    vector <int> c(D, 0);
    for (int i = 0; i < D; i++) {
        c[i] = a[i] - b[i];
    }
    return c;
}

vector <int> t(int id) {
    return {id, 0, C * (m - id + 1)};
}

vector <int> s(int id) {
    return {0, C * (m - id + 1), id};
}

void solve() {
    states = vector <vector<int>> (n, vector<int> (D, 0));

    for (int i = 0; i < n; i++) 
        states[i] = state(i + 1);

    for (int i = 0; i < m; i++) {
        pii e = edges[i];

        instructions.push_back(sub(t(i + 1), state(e.fi)));     // e.fi -> ti
        instructions.push_back(sub(s(i + 1), t(i + 1)));        // ti -> si
        instructions.push_back(sub(state(e.se), s(i + 1)));     // si -> e.se
    }
}

void print() {
    cout << D << "\n";
    for (int i = 0; i < n; i++) {
        for (int x : states[i]) {
            cout << x << " ";
        }
        cout << "\n";
    }
    cout << instructions.size() << "\n";
    for (auto instr : instructions) {
        for (int x : instr) {
            cout << x << " ";
        }
        cout << "\n";
    }    
}


int main() {
    input();
    solve();
    print();
}
```

## Uwagi
