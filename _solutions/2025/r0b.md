---
layout: solution
title: Mistrzostwa w brydża dla olimpijczyków
edition: 2025
round: 0
level: b
author: Mikołaj Bulge
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/TE3Ga-gbceEEst3mL-gFw_AT/site
---

Celem zadania jest walidacja opisu kart oraz zsumowanie ich punktacji zgodnie z 
zasadami opisanymi w treści. 

Sprawdzenie czy wartości kart są legalne oraz czy jest ich dokładnie trzynaście 
jest proste i wymaga kilku instrukcji warunkowych. Nieco większym wyzwaniem jest
potwierdzenie, czy każda z wartości występuje co najwyżej cztery razy. Naturalnym
rozwiązaniem tego problemu jest użycie `std::map` w C++ lub `dict()` w Pythonie.

Aby uniknąć dwukrotnego przetwarzania wejścia, można sumować punkty podczas 
sprawdzenia czy dana karta jest legalna.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

map<string, int> mapa;

void oszust() {
    cout << "OSZUST!\n";
    exit(0);
}

int main() {
    int n, res = 0;
    string s;
    
    cin >> n;
    
    if(n != 13) oszust();
    
    while(n--) {
        cin >> s;
        
        mapa[s]++;
        if(mapa[s] > 4) oszust();
        
        if(s.size() == 1 && s[0] >= '2' && s[0] <= '9') continue;
        else if(s.size() == 2 && s == "10") continue;
        else if(s.size() == 1 && s[0] == 'J') res += 1;
        else if(s.size() == 1 && s[0] == 'Q') res += 2;  
        else if(s.size() == 1 && s[0] == 'K') res += 3;
        else if(s.size() == 1 && s[0] == 'A') res += 4;
        else oszust();
    }
    
    cout << res << '\n';
     
    return 0;
}
```

### Python

```py
POINTS = {
    "2": 0,
    "3": 0,
    "4": 0,
    "5": 0,
    "6": 0,
    "7": 0,
    "8": 0,
    "9": 0,
    "10": 0,
    "J": 1,
    "Q": 2,
    "K": 3,
    "A": 4,
}


def main():
    n = int(input())
    if n != 13:
        print("OSZUST!")
        return

    counter = {}
    points = 0
    for _ in range(n):
        card = input()
        if card not in POINTS:
            print("OSZUST!")
            return

        counter[card] = counter.get(card, 0) + 1
        if counter[card] > 4:
            print("OSZUST!")
            return
        
        points += POINTS[card]

    print(points)


main()
```
