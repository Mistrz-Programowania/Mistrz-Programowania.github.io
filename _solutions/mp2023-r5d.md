---
layout: solution
title: W trosce o środowisko
edition: 2023
round: 5
level: d
author: Tomasz Kwiatkowski
tags:
  - strings
  - patterns
statement: https://szkopul.edu.pl/problemset/problem/Ro8p1Q32GyFmOc2wLF6GaCV5/site/
---

### Rozwiązanie wolne

Dla danego słowa-zapytania będziemy przechodzić się po tekście i utrzymywać, której aktualnie literki ze słowa-zapytania szukamy. Jeżeli po przejściu całego tekstu zmienna będzie wskazywała poza słowo-zapytanie, to odpowiedź dla danego słowa-zapytania to `TAK`.

Złożoność tego podejścia to $O(nA + S)$, gdzie $A$ to długość tekstu, $S$ to suma długości słów-zapytań. Takie rozwiązanie przechodziło dwa pierwsze podzadania.

### Rozwiązanie wzorcowe

Aby przyśpieszyć rozwiązanie, będziemy równolegle wyszukiwać wszystkie słowa-zapytania. Dla każdego trzymamy indeks znaku, który aktualnie chcemy z niego znaleźć (na początku $0$). Dla każdej literki trzymamy również listę indeksów słów-zapytań, które szukają danej literki. Przechodząc tekst znamy, które słowa-zapytania aktualnie szukają danej literki z tekstu. Zwiększamy indeks szukanego znaku dla tych słów-zapytań oraz aktualizujemy listę wrzucając do list odpowiednich znaków indeksy tych słów. Aby odpowiedzieć na zapytania, podobnie jak w rozwiązaniu wolnym, będziemy patrzyli, czy szukany indeks wskazuje poza słowo.

Złożoność tego podejścia to $O(A + S)$. Każdy znak z tekstu przejdziemy raz oraz każdy znak z każdego słowa przejrzymy dokładnie raz.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MAXN = 5e5;
const int ALPHA = 60;

string message[MAXN];
size_t curr_pos[MAXN];
vector<int> events[ALPHA];

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    string s;
    cin >> s;
    int n;
    cin >> n;
    for (int i = 0; i < n; ++i) {
        cin >> message[i];
        events[message[i][0] - 'A'].push_back(i);
    }
    
    for (char c : s) {
        vector tmp = events[c - 'A'];
        events[c - 'A'].clear();
        for (int msg_ind : tmp) {
            ++curr_pos[msg_ind];
            if (curr_pos[msg_ind] >= message[msg_ind].size())
                continue;
            char next_c = message[msg_ind][curr_pos[msg_ind]];
            events[next_c - 'A'].push_back(msg_ind);
        }
    }

    for (int i = 0; i < n; ++i)
        cout << (curr_pos[i] == message[i].size() ? "TAK" : "NIE") << '\n';
    return 0;
}
```

### Python

```py
def main():
    s = input()
    n = int(input())

    message = [""] * n
    curr_pos = [0] * n
    events = [[] for _ in range(60)]
    for i in range(n):
        message[i] = input()
        events[ord(message[i][0]) - 65].append(i)

    for c in s:
        tmp = events[ord(c) - 65]
        events[ord(c) - 65] = []
        for msg_ind in tmp:
            curr_pos[msg_ind] += 1
            if curr_pos[msg_ind] >= len(message[msg_ind]):
                continue
            next_c = message[msg_ind][curr_pos[msg_ind]]
            events[ord(next_c) - 65].append(msg_ind)
        
    for pos, msg in zip(curr_pos, message):
        if pos == len(msg):
            print("TAK")
        else:
            print("NIE")
    
main()
```
