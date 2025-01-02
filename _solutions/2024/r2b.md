---
layout: solution
title: Zaproszenie
edition: 2024
round: 2
level: b
author: Mateusz Wesołowski
statement: https://szkopul.edu.pl/problemset/problem/3LR6PKfQvkVzsQ4YWBTJN7lb/site/
video: https://youtu.be/oMfDh98RN5w?t=227
---

Za pomocą pętli wczytujemy co najmniej pierwsze 2 linijki z wejścia jako ciąg znaków 
(tyle potrzebujemy do ustalenia konturu i wypełnienia, ale możemy wczytać wszystkie). 
Następnie za pomocą podwójnej pętli for wypisujemy kwadrat o podanej długości boków z osobnym
wypisywaniem konturu i wypełnienia.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main(){
   ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
   int stary_rozmiar, nowy_rozmiar;
   int x, y;
 
   cin >> stary_rozmiar >> nowy_rozmiar;	
   char kontur_znak, tresc_znak;

   string wiersze_stare[stary_rozmiar]; 
 
   for(y = 0; y < stary_rozmiar; y++)
      cin >> wiersze_stare[y];

   kontur_znak = wiersze_stare[0][0];
   tresc_znak = wiersze_stare[1][1];

   for(x = 0; x < nowy_rozmiar; x++)
      cout << kontur_znak;
   cout << endl;

   for(y = 1; y < nowy_rozmiar-1; y++) {
      cout << kontur_znak;
      for(x = 1; x < nowy_rozmiar-1; x++)
         cout << tresc_znak;
      cout << kontur_znak << endl;
   }

   for (x = 0; x < nowy_rozmiar; x++)
      cout << kontur_znak;
   cout << endl;

 
   return 0;
}
```

### Python

```py
a1, a2 = map(int, input().split())
ob = 0
s = []
while True:
    try:
        s.append(input())
        ob += 1
    except EOFError:
        break

kontur = s[0][0]
wyp = s[1][1]

for i in range(a2):
    print(kontur, end='')
print()

for i in range(1, a1-1):
    print(kontur, end='')
    for j in range(1, a2-1):
        print(wyp, end='')
    print(kontur)

for i in range(a2):
    print(kontur, end='')
print()
```
