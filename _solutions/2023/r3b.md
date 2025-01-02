---
layout: solution
title: Bez walki
edition: 2023
round: 3
level: b
author: Mikołaj Bulge
sol_author: Daniel Olkowski
tags:
  - greedy
statement: https://szkopul.edu.pl/problemset/problem/haschIaQ6AzXw4HwUewSJEOt/site/
---

Jeśli w dniu numer 10 chcemy sprzedać akcje to kupić powinniśmy w tym dniu od 1 do 9, w którym były najtańsze.

Wystarczy zatem przejść po wszystkich dniach pamiętając cenę, po której mogliśmy najtaniej kupić akcje do tej pory. Jeśli dla danego dnia otrzymujemy większy zysk ze sprzedaży akcji niż w przypadku wcześniejszych dni, to aktualizujemy maksymalny możliwy zysk.

## Przykładowa implementacja

### C++

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
 ios_base::sync_with_stdio(0);
 cin.tie(0);
 cout.tie(0);

 int liczba_dni, i;
 long long cena_akt,cena_min;
 long long max_zysk;
 
 cin >>	liczba_dni;

 cin >> cena_min; 
 max_zysk = 0;
 for (i=2; i<=liczba_dni; ++i) {
   cin >> cena_akt;
   max_zysk = max (max_zysk, cena_akt-cena_min);
   cena_min = min (cena_min, cena_akt);
 }
 
 if ( max_zysk > 0)
    cout << max_zysk << endl;
 else
    cout << "Nie ma zysku, to ci sie nie oplaca" << endl;
 
 return 0;
}
```
