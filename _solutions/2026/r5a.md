---
layout: solution
title: Autobus
edition: 2026
round: 5
level: a
author: Daniel Olkowski
sol_author: Tomasz Kwiatkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/NZ3CnnwoszjH7-R_q70ZRBoN/site/
---

W zadaniu należy stwierdzić, czy podany na wejściu ASCII-art autobusu zawiera drzwi, czy nie.

Szczęśliwie się składa, że w drugimi wierszu znajdują się pionowe kreski (`|`) wtedy i tylko wtedy, gdy autobus ma drzwi!

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    array<string, 4> bus;
    for (auto& line : bus) {
        getline(cin, line);
    }

    if (ranges::contains(bus[1], '|')) {
        cout << "W prawo, na pizze!\n";
    }
    else {
        cout << "W lewo, na sprawdzian!\n";
    }
    return 0;
}
```

### Python

```py
def main():
    bus = [input() for _ in range(4)]

    if '|' in bus[1]:
        print("W prawo, na pizze!")
    else:
        print("W lewo, na sprawdzian!")


main()
```

### Rust

```rs
use std::io::{self, Read};

fn main() {
    let mut input = String::new();
    io::stdin().read_to_string(&mut input).unwrap();

    let mut iter = input.lines();
    let mut bus = [String::new(), String::new(), String::new(), String::new()];
    for line in bus.iter_mut() {
        *line = iter.next().unwrap_or("").to_string();
    }

    if bus[1].contains('|') {
        println!("W prawo, na pizze!");
    } else {
        println!("W lewo, na sprawdzian!");
    }
}
```

## Uwagi

- W C++ należy uważać, aby wczytać całe wiersze, `std::cin >> ...` wczytuje tylko do białego znaku!
- W rozwiązaniu można było również pominąć wczytywanie trzeciego i czwartego wiersza.
