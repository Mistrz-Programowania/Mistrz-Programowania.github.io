---
layout: solution
title: Platformówka
edition: 2023
round: 1
level: b
author: Daniel Olkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/fvAOHZCmoSLCh8SWy9MCggN0/site/
---

Weźmy wcześniejszy z końców odcinków -- mniejszą z wartości $a_2, b_2$. Powiedzmy, że wcześniejszy koniec ma odcinek $b$ czyli $b_2<a_2$. Teraz wystarczy zastanowić się gdzie jest początek odcinka $a$, czyli gdzie jest $a_1$. Jeśli $a_1$ jest z lewej strony $b_2$ to odcinki nachodzą na siebie, gdyż koniec $b$ jest zawarty między początkiem i końcem $a$.

Podobnie możemy rozumować jeśli wcześniejszy koniec ma odcinek $a$ czyli $a_2<b_2$. Wówczas odcinki nachodzą na siebie jeśli $b_1$ jest z lewej $a_2$, inaczej nie nachodzą na siebie.

Wystarczy zatem sprawdzić czy najwcześniejszy koniec odcinków jest z lewej strony najpóźniejszego początku odcinków. Jeśli tak to odcinki nachodzą na siebie, inaczej nie nachodzą.


## Przykładowa implementacja

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
 long long a1, a2, b1, b2;
 cin >> a1 >> a2 >> b1 >> b2;

 long long dist = min(b2,a2) - max(b1,a1);
 if ( dist < 0 ) 
    cout << "NIE" << endl;
 else
    cout << dist << endl;

 return 0;
}
```

## Uwagi

- To realny problem, który mają twórcy gier komputerowych. Szybkim rozwiązywanim takich problemów zajmuje się oddzielna gałąź informatyki -- grafika komputerowa. Jest to przedmiot a często katedra czy też całe oddzielne studia na wydziałach informatycznych.
