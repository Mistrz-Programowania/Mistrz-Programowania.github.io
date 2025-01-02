---
layout: solution
title: Asystent
edition: 2023
round: 4
level: c
author: Mikołaj Bulge
tags:
  - math
  - pref
statement: https://szkopul.edu.pl/problemset/problem/cn5EG4LiOODQup2AfIwBmbQx/site/
---

Zauważmy, że iloczyn wszystkich elementów poza $i$-tym, to jest iloczyn wszystkich elementów z przedziału $[1, i - 1]$ oraz $[i + 1, n]$. Dzięki temu, aby uzyskać wszystkie odpowiedzi wystarczy policzyć zawczasu iloczyny każdego prefiksu oraz każdego sufiksu ciągu z wejścia. Warto pamiętać, aby wszystkie obliczenia wykonywać modulo $10^9 + 7$, aby uniknąć problemów z overflow.

## Przykładowe implementacje

### C++

```cpp
#include<bits/stdc++.h>
using namespace std;

constexpr int M = 5e5 + 7;
constexpr int mod = 1e9 + 7;

long long t[M];
long long px[M];
long long sx[M];

int main() {
    ios_base::sync_with_stdio(0); 
    cin.tie(0); 
    cout.tie(0);

    int n;
    
    cin >> n;
    for(int i = 1; i <= n; i++) cin >> t[i];
    
    px[0] = sx[n+1] = 1;
    for(int i = 1; i <= n; i++) px[i] = (px[i - 1] * t[i]) % mod;
    for(int i = n; i >= 1; i--) sx[i] = (sx[i + 1] * t[i]) % mod;

    for(int i = 1; i <= n; i++) cout << (px[i - 1] * sx[i + 1]) % mod << ' ';

    return 0;
}
```

### Python

```py
MOD = 10**9 + 7

def main():
    n = int(input())
    arr = list(map(int, input().split()))
    suf = [1]
    for i in range(n - 1):
        suf.append((suf[-1] * arr[-i - 1]) % MOD)
    pref = 1
    for i in range(n):
        print((suf[-i - 1] * pref) % MOD, end=' ')
        pref = (pref * arr[i]) % MOD
    print()

main()
```
