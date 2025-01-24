---
layout: solution
title: Gambit wieży
edition: 2025
round: 4
level: d
author: Mateusz Wesołowski
tags:
  - dynamic programming
  - combinatorics
  - game theory
statement: https://szkopul.edu.pl/problemset/problem/Dke0Ue-v5bAcIUzL7TUaEYVo/site/
---

### Strategia wygrywająca

Niech $x$ oznacza liczbę pełnych pięter (na których są $3$ klocki, możemy wyciągnąć dowolny z tych trzech klocków), 
a $y$ - liczbę pięter, z których możemy wyciągnąć tylko jeden klocek (pojedyńczych pięter). 

Stosując indukcję matematyczną po możliwych pozycjach wykażemy, że pozycje $x,y$ parzyste są przegrywające, a pozostałe są wygrywające.

**Baza indukcji:** pozycja $x=y=0$ jest przegrywająca, bo nie możemy wykonać żadnego ruchu.

**Krok indukcyjny** Najpierw przeprowadzamy indukcję po możliwych wartościach $y$ dla danego $x$, potem zwiększamy $x$, 
w ten sposób przechodzimy indukcją po wszystkich interesujących nas parach $x,y$. 
Rozważmy parę $x_1,y_1$ i załóżmy, że dla każdej pary $x_1>x$ albo $y_1>y$ (jeśli $x=x_1$) przegrywamy wtedy i tylko wtedy,
gdy $x$ i $y$ są parzyste, a pozostałe pozycje są wygrywające.

Rozważmy kilka przypadków:

- $x$ parzyste, $y$ nieparzyste. Wtedy możemy usunąć jeden klocek z któregoś z y pojedyńczych pięter
i drugi gracz znajdzie się w stanie $x,y$ parzyste. Jeśli weźmiemy zamiast tego jakiś klocek z pełnego piętra,
to z założenia indukcyjnego drugi gracz znajdzie się w pozycji wygrywającej.
- $x,y$ nieparzyste. Wtedy możemy wyjąć boczny klocek z pełnego piętra, czym zwiększymy $y$ o $1$ i drugi gracz przejdzie do pozycji $x,y$ parzyste.
Jeśli weźmiemy klocek środkowy z pełnego piętra lub klocek z pojedyńczego piętra, to wtedy drugi gracz znajdzie się w pozycji wygrywającej.
- $x$ nieparzyste, $y$ parzyste. Wtedy możemy wziąć środkowy klocek z pełnego piętra i drugi gracz znajdzie się w stanie $x,y$ parzyste.
- $x,y$ parzyste. Usunięcie jakiegokolwiek klocka sprawi, że drugi gracz znajdzie się w pozycji, z której będzie mógł w jednym ruchu
wrócić do pozycji $x,y$ parzyste, która z założenia indukcyjnego będzie pozycją przegrywającą.

### Liczenie możliwych notacji
Użyjemy w tym celu programowania dynamicznego. Niech $dp[x][y]$ oznacza liczbę sposobów na otrzymanie pozycji $x,y$ modulo $10^9 + 7$. Dla początkowej pozycji $x,y$ $dp[x][y] = 1$. Podobnie jak w powyższym 
dowodzie, najpierw będziemy się iterować po coraz mniejszych $y$ dla danego $x$, a potem po coraz
mniejszych $x$. 

- Jeśli obecny gracz jest w pozycji $x$ nieparzyste, $y$ parzyste, to musi wyjąć środkowy klocek
  z pełnego piętra. Może to zrobić na $x$ sposobów, więc aktualizujemy $dp[x-1][y] = dp[x-1][y] +
  x\times dp[x][y]$.
- Jeśli obecny gracz jest w pozycji $x,y$ nieparzyste, to musi wyjąć boczny klocek
  z pełnego piętra. Może to zrobić na $2x$ sposobów, więc aktualizujemy $dp[x-1][y+1] = dp[x-1][y+1] +     2x \times dp[x][y]$.
- Jeśli obecny gracz jest w pozycji $x$ parzyste, $y$ nieparzyste, to musi wyjąć klocek
  z pojedyńczego piętra. Może to zrobić na $y$ sposobów, więc aktualizujemy $dp[x][y-1] = dp[x][y-1] + y   \times dp[x][y]$.
- Jeśli obecny gracz jest w pozycji $x,y$ parzyste, to niezależnie od ruchu przegra, więc
  wykonujemy (o ile to możliwe) aktualizacje $dp[x-1][y]$, $dp[x-1][y+1]$ i $dp[x][y-1]$ jak wyżej.

Policzenie rodzajów pięter pozwala nam na wyznaczenie zwycięzcy w czasie $O(1)$ i liczby możliwych notacji w $O(n^2)$, co dawało maksymalną liczbę punktów.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

constexpr ll M = 1e9+7;
constexpr ll N = 5e3+7;
constexpr int debug = 0;

inline ll mod(ll a){
    return ((a%M)+M)%M;
}

int main() {
    ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int n; cin >> n;
    int p = 0, np = 0;  
    for(int i = 0; i < n; i++){
        string ok;
        for(int j = 0; j < 3; j++){
            string a; cin >> a;
            ok += a;
            if(j < 2) ok += " ";
        }
        //if(debug) cout << ok << '\n';
        if(i == 0) continue;
        if(ok == "1 1 0" || ok == "0 1 1") np++;
        else if(ok == "1 1 1") p++;
    }
    ll dp[p+3][np+p+3];
    for(int i = 0; i < p+3; i++){
        for(int j = 0; j < np+p+3; j++) dp[i][j] = 0;
    }
    (((p&1) == (np&1)) && ((np&1) == 0)) ? cout << "B\n" : cout << "A\n";
    dp[p][np] = 1;
    for(ll i = p; i >= 0; i--){
        for(ll j = np+p; j >= 0; j--){
            int a = (i&1), b = (j&1);
            if(a == b){       
                if(a){
                    dp[i-1][j+1] = mod(dp[i-1][j+1] + dp[i][j]*2*i);
                }
                else{
                    if(i > 0){
                        dp[i-1][j] = mod(dp[i-1][j] + dp[i][j]*i);
                        dp[i-1][j+1] = mod(dp[i-1][j+1] + dp[i][j]*2*i);
                    }
                    if(j > 0){
                        dp[i][j-1] = mod(dp[i][j-1]+dp[i][j]*j);
                    }

                }
            }
            else{
                if(a){
                    dp[i-1][j] = mod(dp[i-1][j] + dp[i][j]*i);
                }
                else{
                    dp[i][j-1] = mod(dp[i][j-1] + dp[i][j]*j);
                }
            }  

            //if(debug) cout << "DEBUG " << i << ' ' << j << ' ' << dp[i][j] << '\n';
        }
    }
    cout << dp[0][0] << '\n';
    
    return 0;
}
```

### Python

```py

```
  
