---
layout: solution
title: Górnicy
edition: 2024
round: 2
level: d
author: Mateusz Wesołowski
statement: https://szkopul.edu.pl/problemset/problem/LJKh5oUhMYWn54j65f6--XtF/site/
video: https://youtu.be/oMfDh98RN5w?t=1017
---

Rozwiązanie dla $N=1$ sprowadza się do obliczenia minimum z numerów wierzchołków, bo wtedy mamy po prostu do czynienia
ze ścieżką, na której większy numer jest niżej. 
Dla stałego $N$ i $V \leq 10^6$ możemy użyć algorytmu na LCA. To algorytm znany i często wykorzystywany w zadaniach z drzewami,
warto się z nim zapoznać w ramach przygotowań do olimpiady informatycznej.

Rozwiązanie wzorcowe dla $N\geq 2$ korzysta z obserwacji, że przedostatni syn wierzchołka o numerze $k$ ma numer $N\cdot k$.
Dla $k=1$ dowód jest oczywisty
Głębokością wierzchołka będziemy nazywać odległość od korzenia. Głębokość korzenia wynosi 0. 
Zauważmy, że jeśli spełnia ten warunek chociaż jedno $k$ na danej głębokości, 
to pozostałe $k$ na tej głębokości też go spełniają (po dodaniu/odjęciu $N$ do syna $k$
przejdziemy do przedostatniego syna odpowiednio $k+1$ lub $k-1$, 
bo każdy wierzchołek ma N synów, a jego numer będzie równy odpowiednio $N\cdot (k-1)$ lub $N\cdot (k+1)$). 
Możemy zatem pokazać, że zachodzi to dla przedostatniego $k$ na głębokości $l$

Numer ostatniego wierzchołka na głebokości $l-1$ jest równy $A=\sum^{l-1}\_{s=0} N^s$, wynika to z tego, że
na głębokości $s$ będzie zawsze $N^s$ wierzchołków, gdyż na każdej głębokości jest $N$ razy więcej niż na poprzedniej.
Numer przedostatniego wierzchołka będzie wyniesie: $A+N^l-1 = \sum^{l}_{s=1} N^s = A\cdot N$, co kończy dowód tego faktu.

Korzystając z niego możemy w czasie stałym określić ojca wierzchołka o danym numerze. 
Określmy funkcję $f(k) = \lfloor \frac{k+N-2}{N} \rfloor$ 
(wszyscy synowie dowolnego $s$ mają numery od $N\cdot (s-1)+2$ do $N\cdot s+1$, 
dodanie N-2 do wszystkich synów oznacza, że wszyscy synowie mają numery od $N\cdot s$ do $N\cdot (s+1)-1$, 
więc podłoga z dzielenia tak uzyskanej liczby przez N da $s$). 
Teraz przejdziemy do szukania najniższego wspólnego przodka wierzchołków o numerach $k_1$ i $k_2$.
Bez straty ogólności przyjmijmy, że $k_1\leq k_2$ Teraz zapamiętujemy, ile jest równe $k_1$ 
(ponieważ jest $\leq k_2$, będzie na tej samej lub niżeszej głębokości od $k_2$) 
i sprawdzamy, czy obecna wartość $k_2$ jest równa którejś z poprzednich wartości $k_1$ 
(nie musi przechodzić w każdym ruchu po wszystkich zapamiętanych wartościach, 
ponieważ w tym procesie $k_1$ i $k_2$ maleją, więc jeśli wartość $k_2$ będzie mniejsza od którejś z zapamiętanych wartości $k_1$, może przejść do następnej mniejszej równej zapamiętanej wartości). 
Następnie ustawiamy obie liczby na $f(k_1)$ i $f(k_2)$. Powtarzamy proces, aż $k_2$ jest równe którejś z zapamiętanych
$k_1$, i tak dojdziemy do najniższego przodka początkowych $k_!$ i $k_2$ (idziemy po ścieżkach od $k$ do korzenia, ich
wszyscy przodkowie znajdują się na tych przodkach, a w algorytmie szukamy, kiedy te ścieżki się przetną), 
czyli mamy to, czego szukaliśmy. Ilość takich ruchów dla $N\geq2$ i $k\leq10^{16}$ nie przekroczy 64. 
Dla $N \geq 2$ złożoność obliczeniowa wyniesie $O(log_N(k))$, 
gdyż każdą liczbę podzielimy przez N maksymalnie tyle razy, ile jest to możliwe zanim dojdziemy do 1.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
#define ll long long
using namespace std;

ll N;

void fun(ll &licz){
    licz = (licz+N-2)/N;
}

void solve(){
    ll a, b; 
    cin >> N >> a >> b;
    if(N == 1){
        cout << min(a,b) << '\n';
        return;
    }
    if(a < b) swap(a,b);                //BSO a > b

    vector<ll> memo;
    int ob = 0;
    memo.push_back(b);

    while(a != memo[ob]){
        fun(a); fun(b);
        memo.push_back(b);
        if(a < memo[ob]) ob++;
    }
    cout << a << '\n';
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);

    int q;
    cin >> q;
    
    while(q > 0){
        solve();
        q--;
    }

    return 0;
}
```

### Python

```py
def fun(ok, N):
    return (ok + N - 2) // N

def solve():
    N, a, b = [int(x) for x in input().split()]
    if N == 1:
        print(min(a, b))
        return
    if a < b:
        a, b = b, a
    memo = []
    ob = 0
    memo.append(b)
    while a != memo[ob]:
        a = fun(a,N)
        b = fun(b,N)
        memo.append(b)
        if a < memo[ob]:
            ob += 1
    print(a)

q = int(input())
while q > 0:
    solve()
    q -= 1
```
