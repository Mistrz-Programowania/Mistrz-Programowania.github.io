---
layout: solution
title: Slider
edition: 2023
round: 3
level: a
author: Daniel Olkowski
tags:
  - math
statement: https://szkopul.edu.pl/problemset/problem/aglr789jaEUgz2pk9BWToAIW/site/
---

Kluczową obserwacją jest to, że minimalna liczba pól między sliderami -- tak by nie się wzajemnie nie biły -- to dwa pola.
Zatem możemy zachłannie, umieszczać slidery:
Slider numer 1 na pierwszym polu, slider numer 2 na polu 4, slider numer 3 na polu 7, itd.

Wniosek: Na każdej trójce pól możemy umieścić jeden slider.
Wystarczy zatem podaną liczbę pól podzielić przez 3, by otrzymać maksymalną możliwą liczbę sliderów które możemy umieścić by wzajemnie się nie biły.

Musimy jeszcze zauważyć, że
1. dla liczby pól 1, 2, 3 możemy umieścić jeden slider podczas gdy podzielenie 1/3 daje zero po obcięciu reszty, 2/3 daje 0 po obcięciu reszty, 3/3 daje 1 po obcięciu reszty
2. dla liczby pól 4, 5, 6 możemy umieścić dwa slidery podczas gdy podzielenie 4/3 daje 1 po obcięciu reszty, 5/3 daje 1 po obcięciu reszty, 6/3 daje 2 po obcięciu reszty

Wystarczy, że do liczby pól dodamy 2. Wówczas podzielenie przez 3 -- w C++ dzielenie na liczbach całkowitych obcina część ułamkową -- da nam odpowiednią ilość sliderów. Dla przypadku 1. będzie to 1 slider, dla przypadku 2. będą to 2 slidery, i tak dalej.

## Przykładowa implementacja

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
 long long liczba_pol, wynik;
 
 cin >>	liczba_pol;
 wynik = (liczba_pol+2)/3; 
 cout << wynik << endl;
  
 return 0;
}
```
