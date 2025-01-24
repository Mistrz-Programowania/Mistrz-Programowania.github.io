---
layout: solution
title: Park rozrywki
edition: 2025
round: 2
level: e
author: XV Zhautykov Olympiad on Mathematics, Physics and Informatics
tags:
  - interactive
statement: https://szkopul.edu.pl/problemset/problem/I4m215kgpEa1viCE0MJvOuYe/site/
---

Oryginalna treść: [https://oj.uz/problem/view/IZhO19_xoractive](https://oj.uz/problem/view/IZhO19_xoractive)

Zapytajmy się najpierw jaka liczba stoi na pozycji $1$.
Teraz zauważmy, że jeżeli zadamy drugi rodzaj pytania o zbiór $A$,
który zawiera $1$, a następnie zadamy to samo pytanie, ale bez $1$,
to dowiemy się dokładnie jakie liczby składają się na ten zbiór indeksów.

Przykład (Liczby poniżej to indeksy, a nie wartości):

- Pytamy się o pierwszą liczbę.
- Pytamy się o zbiór $\{1, 2, 4\}$, dostajemy odpowiedź $\{1 \oplus 1, 1 \oplus 2, 1 \oplus 4, 2 \oplus 2, 2 \oplus 4, 4 \oplus 4\}$
- Pytamy się o zbiór $\{2, 4\}$, dostajemy odpowiedź $\{2 \oplus 2, 2 \oplus 4, 4 \oplus 4\}$
- Zauważmy że różnica tych zbiorów to $\{1 \oplus 1, 1 \oplus 2, 1 \oplus 4\}$ czyli $0$ oraz wszystkie liczby ze zbioru $\{2, 4\}$ zxorowane z $1$, ale ponieważ znamy wartość $1$, to możemy je odzyskać.

Czyli umiemy w 2 operacje znaleźć jakie liczby są na dowolnym podzbiorze indeksów, ale nadal nie wiemy nic o ich kolejności.
Aby ją poznać, rozłożymy indeksy na zapis binarny i zadamy następujące pytania:

- Jakie liczby stoją na indeksach które mają $1$ jako pierwszą cyfrę w zapisie binarnym
- Jakie liczby stoją na indeksach które mają $1$ jako drugą cyfrę w zapisie binarnym
- Jakie liczby stoją na indeksach które mają $1$ jako trzecią cyfrę w zapisie binarnym

i tak dalej. Dla $i$-tego pytania, dla każdej z liczb którą otrzymamy zapisujemy, że jej indeks ma $1$ na $i$-tej pozycji w jego zapisie binarnym, pozostałe liczby będą miały tam $0$. Po zadaniu wszystkich
pytań, będziemy znali wszystkie liczby w ciągu wraz z ich indeksami.

Pytań maksymalnie zadamy $7 \cdot 2 + 1 = 15$:

- $7$ to maksymalna liczba cyfr indeksów w systemie binarnym dla $n = 100$
- $2$ ponieważ każde "pytanie" o zbiór liczb składa się z dwóch operacji opisanych wyżej
- $1$ ponieważ potrzebujemy jednego pytania żeby zapytać wprost o liczbę na indeksie $1$.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

multiset<int> get_subset(vector <int> ids) {
    cout << "2 " << ids.size() + 1 << " 1";
    for (int i : ids)
        cout << " " << i;
    cout << endl;

    multiset <int> subset;
    for (size_t i = 0; i < (ids.size() + 1) * (ids.size() + 1); i++) {
        int x;
        cin >> x;
        subset.insert(x);
    }

    cout << "2 " << ids.size();
    for (int i : ids)
        cout << " " << i;
    cout << endl;

    for (size_t i = 0; i < ids.size() * ids.size(); i++) {
        int x;
        cin >> x;
        subset.erase(subset.find(x));
    }
    return subset;
}

int main() {
    int n;
    cin >> n;
    vector<int> v(n);
    
    cout << "1 1" << endl;
    cin >> v[0];

    map<int, int> mp;
    for (int i = 0; i < 7; i++) {
        multiset<int> subset;
        vector <int> subset_ids;
        for (int j = 1; j < n; j++)
            if ((j & (1 << i)))
                subset_ids.push_back(j + 1);
        if (subset_ids.empty())
            continue;
        subset = get_subset(subset_ids);

        for (auto e : subset)
            mp[e] |= (1 << i);
    }

    for (auto e : mp)
        if (e.first)
            v[e.second] = e.first ^ v[0];

    cout << "3";
    for (int i : v)
        cout << " " << i;
    cout << endl;
    return 0;
}
```

## Uwagi

- Jest to zadanie interaktywne, trzeba pamiętać o opróżnianiu bufora wyjściowego.
  Jeżeli używamy `cout`, to wystarczy po każdym wypisaniu opróżnić:
  `cout << flush` lub `cout << endl`, który pod spodem wykonuje operację `flush`.
  W Pythonie wystarczy ustawić argument `flush=True` w funkcji `print`.
  W przeciwnym wypadku, wypisane wartości mogą nie trafić do programu sprawdzającego.
- Trzeba uważać żeby każdy indeks uwzględnić w którymś z pytań przynajmniej raz, inaczej nie będziemy znać liczby która tam stoi. W szczególności może się to zdarzyć jeżeli postanowimy indeksować od $0$,
ponieważ $0$, nie ma żadanej $1$ w zapisie binarnym, to nigdy o nie nie zapytamy.
