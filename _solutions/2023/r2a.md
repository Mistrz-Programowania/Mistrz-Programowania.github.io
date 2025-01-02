---
layout: solution
title: Usuwanie bugów
edition: 2023
round: 2
level: a
author: Daniel Olkowski
tags:
  - implementation
  - math
statement: https://szkopul.edu.pl/problemset/problem/NN2iidtGdkjvqGDCo7tSaSOu/site/
---

Mając daną liczbę musimy wypisać wartość o połowę większą. Czyli do wartości liczby dodać jej połowę.
 
Zadanie jest oczywiste przy czym trzeba zwrócić uwagę na dwie rzeczy:

1. W C++ trzeba użyć `long long` -- zakres do $10^{18}$

2. Zaokrąglenie w górę. Domyślne zaokrąglenie w C++ przy działaniach na liczbach całkowitych jest w dół.
$17/2$ daje $8$ w C++ -- odcina część ułamkową
My potrzebujemy zaokrąglenia w górę czyli by $17/2$ dawało $9$.
Najprościej możemy osiągnąć zaokrąglenie w górę dodając 1 do liczby którą będziemy dzielić przez 2.
Faktycznie $(17+1)$ na $2$ daje $9$.
Z kolei $30$ podzielone na $2$ to będzie $(30+1)$ podzielone na $2$ a to da nam $15$ -- część ułamkowa zostanie odrzucona przez C++ przy dzieleniu $31/2$.

## Przykładowa implementacja

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
 long long liczba;
 
 cin >>	liczba;
 liczba += (liczba+1)/2;

 cout << liczba;
  
 return 0;
}
```
