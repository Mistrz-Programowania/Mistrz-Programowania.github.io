---
layout: solution
title: Budzik
edition: 2026
round: 4
level: a
author: Tomasz Kwiatkowski
tags:
  - math
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/lYH2bczG96FTYeFamgGoPkMb/site/
---

Możemy zamienić godzinę na liczbę minut od północy.
Na przykład `5:45`, to $5 * 60 + 45 = 345$ minut po północy.

Skoro budzik dzwoni po raz $k$-ty, to minęło $(k - 1) \cdot 10$ minut.
Dodajmy ten czas do obliczonej liczby minut.

Pozostało tylko zamienić z powrotem na godzinę.
Musimy jednak pamiętać, że ostatni budzik może dzwonić innego dnia, niż pierwszy.
Dzień ma $24 * 60 = 1440$ minut. Wystarczy, że weźmiemy resztę z dzielenia przez $1440$.
Na przykład, jeżeli liczba minut wyszła nam równa $1467 = 1440 + 27$, to odpowiada to godzinie `0:27`.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
    int h, m;
    cin >> h >> m;
    int64_t k;
    cin >> k;

    int64_t minutes = h * 60 + m;
    minutes += (k - 1) * 10;
    minutes %= 24 * 60;
    cout << minutes / 60 << ' ' << minutes % 60 << '\n';
    return 0;
}
```

### Python

```py
def main():
    h, m = map(int, input().split())
    k = int(input())

    minutes = h * 60 + m
    minutes += (k - 1) * 10
    minutes %= 24 * 60
    print(minutes // 60, minutes % 60)


main()
```

### Rust

```rs
use std::io::{self, Read};

fn main() {
    let mut input = String::new();
    io::stdin().read_to_string(&mut input).unwrap();
    let mut it = input.split_whitespace();

    let h: i64 = it.next().unwrap().parse().unwrap();
    let m: i64 = it.next().unwrap().parse().unwrap();
    let k: i64 = it.next().unwrap().parse().unwrap();

    let mut minutes: i64 = h * 60 + m;
    minutes += (k - 1) * 10;
    minutes %= 24 * 60;

    println!("{} {}", minutes / 60, minutes % 60);
}
```
