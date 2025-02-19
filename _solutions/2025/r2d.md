---
layout: solution
title: Sok z kumkwatu
edition: 2025
round: 2
level: d
author: Mateusz Wesołowski
tags:
  - segment_trees
  - math
statement: https://szkopul.edu.pl/problemset/problem/dPRQ8n3DBZqD1q-mK27vsCB3/site/
---

W rozwiązaniu będziemy traktowali operacje zwolnienia na stanowisku $x$ jako ustawienie jego prędkości ($v_x$) na 0;
oraz liczby wyprodukowanych przez niego dotychczas litrów soku na $0$. Zatrudnienie pracownika oznacza $x$ oznacza ustawienie $v_x$ na nową prędkość.

### $n=1$

Oznaczmy początkową prędkość pracownika jako $v_0$, $t_0 = 0$, prędkość po $i$-tej zmianie $i$ jako $v_i$ a dzień $i$-tej zmiany jako $t_i$.
Liczba litrów wyprodukowanego przez danego pracownika soku po $q$ operacjach wynosi $v_0\times t_0 + \sum_{i=0}^q (t_{i+1}-t_i)\times v_{i+1}$ (I).

### Pełne rozwiązanie

Możemy przekształcić wzór (I) do postaci $t_q*v_q \sum_{i=0}^{q-1} (v_i-v_{i+1})\times t_{i}$. Będziemy trzymać prędkości każdego pracownika,
oraz wartości $\sum_{i=0}^{q-1} (v_i-v_{i+1})\times t_{i}$ na drzewach punkt-przedział. Aktualizacja pracownika sprowadza się zatem
do dodania w odpowiadającym mu punkcie drugiego drzewa $(v_i-v_{i+1})\times t_{i}$, a następnie zmianę prędkości $v_i$ na $v_{i+1}$.
Odpowiedzią na zapytania o przedział jest suma na przedziale w drugim drzewie + $t\times$ suma prędkości na przedziale w pierwszym drzewie.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

constexpr int base = 1<<20;

ll tree[2*base+7][2];          //0 to drzewo predkosci, 1 to drzewo poprawek

void update(ll a, ll v, bool t){
    a += base;
    tree[a][t] = v;
    while(a /= 2){
        tree[a][t] = tree[2*a][t] + tree[2*a+1][t];
    }
}

ll query(ll a, ll b, bool t){
    ll sc = 0;
    a += base-1; b += base+1;
    while(a/2 != b/2){
        if(!(a&1)){
            sc += tree[a+1][t];
        }
        if(b&1){
            sc += tree[b-1][t];
        }
        a /= 2; b /= 2;
    }
    return sc;
}

int main() {
    ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int n, q;
    cin >> n >> q;
    for(int i = 1; i <= n; i++){
        ll v1; cin >> v1;
        update(i, v1, 0);
    }
    ll a1, a2, t;
    while(q--){
        char question; cin >> question;
        if(question == 'Q'){
            cin >> a1 >> a2 >> t;
            cout << query(a1, a2, 0)*t + query(a1, a2, 1) << '\n';
        }
        else if(question == 'V'){
            cin >> a1 >> a2 >> t;
            ll ok = tree[a1+base][0];
            ll nu = tree[a1+base][1];
            update(a1, nu+(ok-a2)*t, 1);
            update(a1, a2, 0);
        }
        else if(question == 'F'){
            cin >> a1 >> t;
            update(a1, 0, 0);
            update(a1, 0, 1);
        }
        else{
            cin >> a1 >> a2 >> t;
            update(a1, -a2*t, 1);
            update(a1, a2, 0);
        }
    }
    
    return 0;
}
```

### Python

```py

```

### Uwagi
- Drzewa przedziałowe można przyspieszyć korzystając z operacji bitowych. Dzielenie przez 2 można zastąpić przesunięciem bitowym (>>),
  a sprawdzenie parzystości - operacją AND (&).
- Istnieje również rozwiązanie, które nie wymaga przekształcenia wzoru (I).
  Czy potrafisz je znaleźć?
  
