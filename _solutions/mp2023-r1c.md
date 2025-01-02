---
layout: solution
title: Kucharz
edition: 2023
round: 1
level: c
author: Tomasz Kwiatkowski
tags:
  - pref
statement: https://szkopul.edu.pl/problemset/problem/t890BSFA76k23x1SBDaIdrbM/site/
---

Zauważmy, że masa pierwszego produktu to pierwsze wskazanie wagi. To oczywiste, ponieważ w misce jest wówczas tylko jeden produkt -- pierwszy, zatem waga wskazuje jego masę.

A jak obliczyć masę drugiego produktu? Co prawda, nie mamy podanej jej wprost. Ale znamy masę pierwszych dwóch produktów -- jest to drugie wskazanie wagi. Znamy też masę pierwszego produktu, stąd, odejmując drugie wskazanie wagi od pierwszego, otrzymujemy masę drugiego produktu.

Dalej robimy analogicznie. Znając masę produktów od $1$-szego do $i$-tego oraz od $1$-szego do $i-1$-szego, możemy obliczyć masę $i$-tego produktu.

Zadanie możemy rozwiązać pamiętając poprzednie wskazanie wagi (na początku $0$), w pętli wczytując aktualne wskazanie wagi i obliczając masę kolejnych produktów odejmując od siebie odpowiednie wartości.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    int prv = 0;
    for (int i = 0; i < n; ++i) {
        int w;
        cin >> w;
        cout << w - prv << ' ';
        prv = w;
    }
    cout << '\n';
    return 0;
}
```

### Python

```py
def main():
    _ = int(input())
    prv = 0
    for w in input().split():
        print(int(w) - prv)
        prv = int(w)

main()
```

## Uwagi

- Ograniczenie liczby rzeczy globalnych w Pythonie może znacząco przyspieszyć działanie programu
