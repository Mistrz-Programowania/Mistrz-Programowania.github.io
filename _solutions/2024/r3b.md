---
layout: solution
title: Weryfikator wejścia
edition: 2024
round: 3
level: b
author: Maciej Wiśniewski
statement: https://szkopul.edu.pl/problemset/problem/ydt-9dQtD9VrxqWti26az5Cx/site/
video: https://youtu.be/VrMk1y1y_F0?t=457
---

W zadaniu chcemy po kolei sprawdzić czy test zalicza się do kolejnych podzadań.

Aby sprawdzić pierwsze podzadanie tworzymy zmienną bool, która na początku jest $true$.
Przechodzimy pętlą for po wszystkich liczbach $a_i$ i sprawdzamy czy którakolwiek jest
inna niż $1$. Jeżeli tak, to ustawiamy naszą zmienną na $false$. Jeżeli na końcu zmienna
nadal jest $true$, to znaczy że wszystkie $a_i$ były jedynkami i test zalicza się do pierwszego
podzadania.

Drugie podzadanie sprawdzamy tak samo, ustawiając zmienną na $false$ jeżeli wartość $a_i$ jest większa od $2$.

Podzadania od $3$ do $6$ sprawdzamy porównując $n$ w if-ie. Aby sprawdzić parzystość $n$ w C++ oraz Pythonie możemy użyć znaku % czyli
reszty z dzielenia. Jeżeli reszta z dzielenia $n/2$ (czyli $n\%2$) jest równa $0$ to $n$ jest parzyste.
 
Jeżeli żaden z poprzednich warunków nie był spełniony to musimy wypisać $7$. Ale taka sytuacja nie powinna nigdy zajść,
ponieważ $n$ zawsze jest albo parzyste, albo nieparzyste, więc przynajmniej jeden warunek musi być spełniony.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(0);

    int subtask = 7;

    int n;
    cin >> n;

    if (n <= 100)
        subtask = 3;
    else if (n <= 1000)
        subtask = 4;
    else if (n % 2 == 0)
        subtask = 5;
    else
        subtask = 6;

    bool is_sub_1 = true, is_sub_2 = true;
    for (int i = 0; i < n; i++) {
        int a;
        cin >> a;

        if (a > 1) 
            is_sub_1 = false;
        if (a > 2) 
            is_sub_2 = false;
    }

    if (is_sub_2)
        subtask = 2;
    if (is_sub_1)    
        subtask = 1;


    cout << subtask << "\n";
    return 0;
}
```

### Python

```py
n = int(input())
maxA = max(list(map(int, input().split()))[::])
subtask = 7
if n % 2 == 1:
    subtask = 6
if n % 2 == 0:
    subtask = 5
if n <= 1000:
    subtask = 4
if n <= 100:
    subtask = 3
if maxA <= 2:
    subtask = 2
if maxA <= 1:
    subtask = 1
    
print(subtask)
```
