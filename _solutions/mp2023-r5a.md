---
layout: solution
title: Ramka
edition: 2023
round: 5
level: a
author: Tomasz Kwiatkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/cRyyI_qzWJBH0DWzSsejOWkV/site/
---

Stworzymy funkcję, która przyjmuje parametr $n$
i wypisuje na przemian $n+1$ plusów oraz $n$ minusów.
Wywołamy ją, by uzyskać pierwszą i trzecią linijkę wyjścia.

Do wypisania drugiej linijki wykorzystamy pętlę `for`.
Dla każdego znaku wypiszemy najpierw `|`, a następnie dany znak.
Na koniec dopiszemy brakujący, ostatni znak `|`.

Do wczytania napisu w C++ skorzystamy z typu `string`.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

void print_border(int n) {
    for (int i = 0; i < n; ++i)
        cout << "+-";
    cout << "+\n";
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    string s;
    cin >> s;

    print_border(s.size());

    for (char c : s)
        cout << "|" << c;
    cout << "|\n";
    
    print_border(s.size());
    return 0;
}
```

### Python

```py
def print_border(n):
    print("+-" * n + "+")

def main():
    s = input()

    print_border(len(s))

    for c in s:
        print("|" + c, end = "")
    print("|")

    print_border(len(s))

main()
```

## Uwagi

- W tym przypadku można by dyskutować,
czy funkcja `print_border` jest potrzebna.
Ale w ogólności wydzielanie fragmentów kodu, który
powtarzamy wielokrotnie do oddzielnych funkcji jest dobrą praktyką
