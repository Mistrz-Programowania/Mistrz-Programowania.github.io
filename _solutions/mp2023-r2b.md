---
layout: solution
title: Nuda
edition: 2023
round: 2
level: b
author: Daniel Olkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/h_CUbmgiARwURoxoJ5GsSelL/site/
---

Zadanie wymaga narysowania w kolejnych liniach znaków dolara $. W każdej kolejnej linii mają być dwa znaki więcej.

Implementacyjnie potrzebujemy dwóch pętli:
 * Pętla 1 która idzie po kolejnych wypisywanych liniach
 * Pętla 2 która idzie po kolejnych znakach $ w danej linii

W momencie w którym skończą się nam znaki $ musimy przestać je wypisywać, nawet jeśli jesteśmy w środku linii.
Dlatego dla drugiej pętli wygodnie jest zaimplementować osobną funkcję z której wyjdziemy jeśli znaki dolara się skończą.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

bool NarysujLinie (int &ile_jest_lacznie_znakow, int liczba_znakow_w_akt_linii, int max_liczba_znakow) {
 int i;
 
 for (i=1; i<=liczba_znakow_w_akt_linii; ++i) {
    cout << "$ ";
    ++ile_jest_lacznie_znakow;
    if (ile_jest_lacznie_znakow >= max_liczba_znakow ) {
       cout << endl;
       return false;
    }
 }

 cout << endl;
 return true;
}

int main() {
 int liczba_znakow_w_akt_linii, max_liczba_znakow;
 int ile_jest_lacznie_znakow;
  
 cin >>	liczba_znakow_w_akt_linii >> max_liczba_znakow;

 ile_jest_lacznie_znakow = 0;
 while ( true ) {
    if ( NarysujLinie (ile_jest_lacznie_znakow, liczba_znakow_w_akt_linii, max_liczba_znakow) == false )
        break;
    liczba_znakow_w_akt_linii += 2;
 }
  
 return 0;
}
```

### Python

```py
def main():
    k, n = map(int, input().split())
    while n > 0:
        print(" ".join(["$"] * min(n, k)))
        n -= k
        k += 2

main()
```
