---
layout: solution
title: Czas
edition: 2023
round: 4
level: a
author: Daniel Olkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/vtWmO3zoqWjYMtjjvApkMjsL/site/
---

Suma czasów wykonywanych czynności jest większa od 24 -- czasu trwania doby. Ten naddatek ponad 24h to czas który musi być wspólny. Wystarczy zatem zsumować czas wszystkich czynności i odjąć 24.

## Przykładowa implementacja

### C++

```cpp
#include <iostream>
using namespace std;

int main() {

 int czas_szkoly, czas_snu, czas_dla_siebie;
 int zysk_na_czasie_szkoly_i_snu;
 
 cin >>	czas_szkoly >> czas_snu >> czas_dla_siebie;
 zysk_na_czasie_szkoly_i_snu = czas_szkoly + czas_snu + czas_dla_siebie - 24;

 cout << zysk_na_czasie_szkoly_i_snu << endl;
 
 return 0;
}

```
