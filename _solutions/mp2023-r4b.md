---
layout: solution
title: Potęgi klucz
edition: 2023
round: 4
level: b
author: Daniel Olkowski
tags:
  - math
statement: https://szkopul.edu.pl/problemset/problem/d_AZMQVbGBFVqZX0Oz1aArVl/site/
---

W zadaniu mamy podane dwa kolejne wyrazy pewnego ciągu $a^0$, $a^1$, $a^2$, $a^3$, $a^4$, ... Musimy powiedzieć jaki będzie trzeci kolejny.

Na przykład weźmy ciąg $5^0$, $5^1$, $5^2$, $5^3$, $5^4$, ... czyli $1$, $5$, $25$, $125$, $625$, ...
Jeśli mamy dane $25$ i $125$ to jaki będzie kolejny wyraz ciągu?

Kolejne wyrazy są mnożone przez ten sam czynnik. Wystarczy podzielić $125$ przez $25$ by dowiedzieć się, że wyrazy w naszym ciągu są mnożone przez $5$. Zatem w naszym przykładzie kolejny wyraz to będzie $125 \cdot 5$ czyli $625$.

## Przykładowa implementacja

### C++

```cpp
#include <iostream>
using namespace std;

int main() {

 long long pierwsza_liczba, druga_liczba;
 long long iloraz, wynik;

 cin >>	pierwsza_liczba >> druga_liczba;
 iloraz = druga_liczba / pierwsza_liczba;
 wynik = druga_liczba*iloraz;
 
 cout << wynik << endl;
 
 return 0;
}
```
