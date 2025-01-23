---
layout: solution
title: Gambit wieży
edition: 2025
round: 4
level: d
author: Mateusz Wesołowski
tags:
  - dynamic programming
  - combinatorics
  - game theory
statement: https://szkopul.edu.pl/problemset/problem/Dke0Ue-v5bAcIUzL7TUaEYVo/site/
---

###DOKOŃCZYĆ!!!

###Strategia wygrywająca

Niech $x$ oznacza liczbę pełnych pięter (na których są $3$ klocki, możemy wyciągnąć dowolny z tych trzech klocków), 
a $y$ - liczbę pięter, z których możemy wyciągnąć tylko jeden klocek (pojedyńczych pięter). 

Stosując indukcję matematyczną po możliwych pozycjach wykażemy, że pozycje $x,y$ parzyste są przegrywające,
a pozostałe są wygrywające.

**Baza indukcji:** pozycja $x=y=0$ jest przegrywająca, bo nie możemy wykonać żadnego ruchu.

**Krok indukcyjny** Najpierw  

Rozważmy kilka przypadków:

- $x,y$ nieparzyste. Wtedy możemy wyjąć środkowy klocek z pełnego piętra i drugi gracz przejdzie do pozycji $x,y$ parzyste.
Jeśli weźmiemy klocek boczny z pełnego piętra, to 
- $x$ parzyste, $y$ nieparzyste. Wtedy możemy usunąć jeden klocek z któregoś z y pojedyńczych pięter
i drugi gracz znajdzie się w stanie $x,y$ parzyste. Jeśli weźmiemy zamiast tego jakiś klocek z pełnego
piętra, to drugi gracz może wykonać jeden z ruchów wymienionych powyżej i to my znajdziemy się w pozycji $x,y$ parzyste



Przypomnijmy cechę podzielności przez $9$ -- liczba jest podzielna przez $9$
wtedy i tylko wtedy, gdy suma jej cyfr jest podzielna przez $9$.
Zatem, równoważnie, naszym celem jest sprawić, aby końcowa liczba miała sumę cyfr podzielną przez $9$.

Drugi gracz zawsze ma strategię wygrywającą, ponieważ wypisuje ostatnią cyfrę.
Może dowolnie wybrać ostanią cyfrę -- a zatem również resztę z dzielenia sumy cyfr przez $9$.
Przykładowo, jeżeli do tej pory suma cyfr była równa $3$ -- wystarczy wpisać $6$.

W rozwiązaniu wypiszemy $n-1$ dowolnych cyfr, zapamiętamy ich sumę oraz sumę cyfr wypisanych przez przeciwnika.
Na koniec wystarczy wypisać cyfrę `9 - suma % 9` (warto też zauważyć,
że wynik tego odejmowania będzie jednocyfrowy).

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
    int n;
    cin >> n;
    
    int sum;
    cin >> sum;
    for (int i = 0; i < n - 1; ++i) {
        cout << 0 << endl;
        int opponent;
        cin >> opponent;
        sum += opponent;
    }
    cout << 9 - sum % 9 << endl;
    return 0;
}
```

### Python

```py
def main():
    n = int(input())

    sum = int(input())
    for i in range(n - 1):
        print(0, flush=True)
        opponent = int(input())
        sum += opponent
    print(9 - sum % 9, flush=True)


main()
```

## Uwagi

- Jest to zadanie interaktywne, trzeba pamiętać o opróżnianiu bufora wyjściowego.
  Jeżeli używamy `cout`, to wystarczy po każdym wypisaniu opróżnić:
  `cout << flush` lub `cout << endl`, który pod spodem wykonuje operację `flush`.
  W Pythonie wystarczy ustawić argument `flush=True` w funkcji `print`.
  W przeciwnym wypadku, wypisane wartości mogą nie trafić do programu sprawdzającego.
- Alternatywnym rozwiązaniem, które pozwala otrzymać jeszcze krótszy kod, jest za każdym
  razem wypisywanie cyfry "przeciwnej" do cyfry wypisanej przez przeciwnika.
  Jeżeli przeciwnik wypisał `d`, to my wypiszemy `9 - d`.
  Łatwo zauważyć, że na koniec suma cyfr będzie podzielna przez $9$.
  Nie trzeba wtedy pamiętać dotychczasowej sumy cyfr.
