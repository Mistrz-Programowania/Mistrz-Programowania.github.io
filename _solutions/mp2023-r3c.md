---
layout: solution
title: BANANA
edition: 2023
round: 3
level: c
author: Maciej Wiśniewski
tags:
  - math
statement: https://szkopul.edu.pl/problemset/problem/4Buls0IA8e3DFOXtt63ShAbt/site/
---

Zadanie polega na zapisaniu liczby w systemie $26$-stkowym, z drobną
modyfikacją że w tym systemie nie ma zera. Będziemy chcieli odczytać tą liczbę
po jednej cyfrze (literze) od najmniej znaczących.

To co byśmy zrobili w normalnym systemie $26$-stkowym, to powiedzieli że ostatnią cyfrą (literą)
jest $n \mod 26$ (reszta z dzielenia $n / 26$), i potem podzielili $n$ przez $26$ zaokrąglając w dół, 
żeby sprawdzić następne cyfry(litery). Ze względu na drobną modyfikację systemu w naszym zadaniu, musimy
przed odczytaniem ostatniej cyfry najpierw zmniejszyć $n$ o $1$. Czyli całe rozwiązanie można zapisać w 4 krokach:

1. zmniejsz $n$ o $1$.
2. dopisz jako kolejną cyfrę (literę) $n\mod 26$.
3. podziel $n / 26$ zaokrąglając w dół.
4. jeżeli $n$ jest większe od $0$, to powtórz wszystkie kroki.

W zależności od implementacji, być może słowo trzeba będzie na końcu odwrócić.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;


string solve(long long nr) {
    if (nr == 1) return "A";

    string s = "";

    while (nr) {
        nr--;
        char c = (nr % 26) + 'A';
        s += c;
        nr /= 26;
    }

    reverse(s.begin(), s.end());
    return s;
}

int main()
{
    ios_base::sync_with_stdio(0), cin.tie(0);

    int tcase;
    cin >> tcase;

    while (tcase--) {
        long long nr;
        cin >> nr;
        cout << solve(nr) << "\n";
    }
    return 0;
}
```

### Python

```py
def sheet(col):
    res = ""
    while col:
        res += chr(65 + (col - 1) % 26)
        col = (col - 1) // 26
    return res[::-1]

def main():
    for _ in range(int(input())):
        print(sheet(int(input())))

main()
```
