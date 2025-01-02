---
layout: solution
title: Mistrz Programowania 2024
edition: 2023
round: 5
level: c
author: Jerzy Fronczyk
sol_author: Daniel Olkowski
tags:
  - sortings
statement: https://szkopul.edu.pl/problemset/problem/Pa5p1j1wDlfh53BYYBRYxNuk/site/
---

Zadanie ma 3 wyzwania: Sposób przechowywania danych, Niestandardowe posortowanie oraz Wypisanie rankingu tak by osoby z tą samą oceną miały identyczny numer. Omówiona zostanie implementacja C++.

### Sposób przechowywania danych
Najwygodniej dane dotyczące pojedynczego użytkownika (identyfikator, punkty, ...) trzymać we strukturze (struct) C++. Następnie towrzymy vector (tablicę) której pojedynczym elementem jest właśnie zdefiniowany struct. Teraz sortując vector sortujemy jednocześnie całość informacji dotyczących pojedynczego użtykownika.

### Niestandardowe sortowanie
C++ posiada możliwośc podanie funkcji definiującej który z obiektów sortowanego vectora jest mniejszy - poiwnien być wcześniej w tablicy. U nas poniżej to funkcja `CzyZawdodnikAZLewejStrony`

### Wypisanie rankingu
Generowanie fair rankingu -- osoby z tą samą oceną mają ten sam numer rankingu -- najłatwiej wykonać poprzez 2 zmienne. Globalny numer uczestnika oraz pozycja w rankingu. Wypisując posortowanych uczestników, sprawdzamy czy aktualny użytkownik ma tą samą ocenę co poprzedni. Jeśli TAK jego wypisywana pozycja nie ulega zmianie. Jeśli NIE to wypisywana pozycja ma globalny numer uczestnika.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int LICZBA_RUND = 5;
const int MAX_PUNKTOW_RUNDA = 500;

struct typ_zawodnik {
 string identyfikator;
 vector<int> wyniki_rund;
 int w_ilu_rundach;
 int ile_maksow;
 int suma_punktow;
};

bool CzyZawdodnikAZLewejStrony (const typ_zawodnik &zawodnikA, const typ_zawodnik &zawodnikB) {
 if ( zawodnikA.w_ilu_rundach != zawodnikB.w_ilu_rundach )
    return zawodnikA.w_ilu_rundach > zawodnikB.w_ilu_rundach;
 if ( zawodnikA.ile_maksow != zawodnikB.ile_maksow )
    return zawodnikA.ile_maksow > zawodnikB.ile_maksow;
 if ( zawodnikA.suma_punktow != zawodnikB.suma_punktow )
    return zawodnikA.suma_punktow > zawodnikB.suma_punktow;
 return zawodnikA.identyfikator < zawodnikB.identyfikator;
}

bool CzyZawodnicyTacySami (const typ_zawodnik &zawodnikA, const typ_zawodnik &zawodnikB) {
 if ( zawodnikA.w_ilu_rundach != zawodnikB.w_ilu_rundach )
    return false;	
 if ( zawodnikA.ile_maksow != zawodnikB.ile_maksow )
    return false;
 if ( zawodnikA.suma_punktow != zawodnikB.suma_punktow )
    return false;
 return true;
}
int main() {
 ios_base::sync_with_stdio(0);
 cin.tie(0);
 cout.tie(0);

 int ile_uczestnikow, i, j;
 int akt_numer_zawodnika, numer_globalny_zawodnika;
 vector <typ_zawodnik> zawodnicy;

 cin >>	ile_uczestnikow;
 zawodnicy.resize(ile_uczestnikow);

 for (i=0; i<ile_uczestnikow; ++i) {
    cin >> zawodnicy[i].identyfikator; 
    zawodnicy[i].wyniki_rund.resize(LICZBA_RUND);
    zawodnicy[i].suma_punktow = zawodnicy[i].w_ilu_rundach = zawodnicy[i].ile_maksow = 0;
    for (j=0; j<LICZBA_RUND; ++j) {
       cin >> zawodnicy[i].wyniki_rund[j]; 
       zawodnicy[i].suma_punktow += zawodnicy[i].wyniki_rund[j];        
       if ( zawodnicy[i].wyniki_rund[j] == MAX_PUNKTOW_RUNDA )
          ++zawodnicy[i].ile_maksow;
       if ( zawodnicy[i].wyniki_rund[j] > 0 )
          ++zawodnicy[i].w_ilu_rundach;
    }
 }

 sort (zawodnicy.begin(), zawodnicy.end(), CzyZawdodnikAZLewejStrony );

 akt_numer_zawodnika = numer_globalny_zawodnika = 1;
 for (i=0; i<ile_uczestnikow; ++i) {
    cout << akt_numer_zawodnika << " " << zawodnicy[i].identyfikator << " " << zawodnicy[i].suma_punktow << " ";
    for (j=0; j<LICZBA_RUND; ++j)
       cout << zawodnicy[i].wyniki_rund[j] << " "; 
    cout << endl;
    ++numer_globalny_zawodnika;
    if ( numer_globalny_zawodnika > ile_uczestnikow )
       break;
    if ( CzyZawodnicyTacySami (zawodnicy[i], zawodnicy[i+1]) == false )
       akt_numer_zawodnika = numer_globalny_zawodnika;
 }
 
 
 return 0;
}
```

### Python

```py
def main():
    n = int(input())
    to_sort = []
    for _ in range(n):
        row = input().split()
        """
        sort order:
        a. number of non-zero rounds
        b. number of maxed round
        c. total points
        d. lexicographically
        """
        to_sort.append(
            (
                -(5 - row.count("0")),
                -row.count("500"),
                -sum(map(int, row[1:])),
                row[0],
                row[1:],
            )
        )

    to_sort.sort()
    nr = 1
    for i in range(n):
        print(
            nr,
            to_sort[i][3],
            sum(list(map(int, to_sort[i][4]))),
            " ".join(to_sort[i][4]),
        )
        if i < n - 1 and to_sort[i + 1][:3] != to_sort[i][:3]:
            nr = i + 2

main()
```

## Uwagi

- Własne sortowanie to coś co jest bardzo potrzebne w realnym programowaniu -- na przykład sklepy internetowe gdzie sortujemy po cenie, popularności, itp. Ale też przydaje się w konkursach i olimpidach informatycznych -- zadanie Metro z Olimpiady Informatycznej Juniorów.
