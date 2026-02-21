---
layout: solution
title: Powrót
edition: 2026
round: 0
level: a
author: Daniel Olkowski
sol_author: Tomasz Kwiatkowski
tags:
  - math
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/kijKHUM5X2wEACkA3-bSvad2/site/
---

Podczas 3 dni nauczyciel przerobi 4 działy. Zatem podczas jednego dnia nauczyciel przerobi $\frac{4}{3}$ działu. Czyli podczas $n$ dni przerobi $\frac{4n}{3}$ działu. Tę wartość należy podać zaokrąglając w dół, czyli pomijając część niecałkowitą.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
    int64_t n;
    cin >> n;

    cout << (4 * n) / 3 << '\n';
    return 0;
}
```

### Python

```py
def main():
    n = int(input())

    print((4 * n) // 3)


main()
```

### Rust

```rs
use std::io::{self, Read};

fn main() {
    let mut input = String::new();
    io::stdin().read_to_string(&mut input).unwrap();
    
    let n: i64 = input.trim().parse().unwrap();
    
    println!("{}", (4 * n) / 3);
}
```
