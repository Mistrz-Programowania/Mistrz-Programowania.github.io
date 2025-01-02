---
layout: solution
title: Kod weryfikacyjny
edition: 2024
round: 5
level: c
author: Tomasz Kwiatkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/RQsU8YR2L68blA-wmZ4gLmej/site/
video: https://youtu.be/YDD-yt7Y_tE&t=1081
---

Spróbujmy najpierw znaleźć cyfry, które mają jakąś unikalną literę.
Przykładowo, tylko cyfra $1$ ma literę `j`.
Dzięki temu, możemy łatwo obliczyć, ile cyfr $1$ jest w kodzie -- wystarczy zliczyć wystąpienia `j`.
Podobnie, możemy znaleźć liczbę wystąpień cyfr $2$ oraz $5$.

A co z pozostałymi cyframi?
Zauważmy, że tylko cyfry $2$ oraz $9$ mają w zapisie słownym literę `w`.
Policzyliśmy wcześniej, ile jest cyfr $2$ (a każda zawiera dokładnie jedno `w`),
zatem możemy też obliczyć, ile jest cyfr $9$.

Podobnie postępując dla pozostałych cyfr, możemy policzyć ile razy występują.
Teraz, pozostało wypisać odpowiednią liczbę dziewiątek, ósemek, ..., jedynek i zer.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    string line;
    cin >> line;
    
    vector<int> cnt_c('z' + 1, 0);
    for (const char &c : line)
        cnt_c[c] += 1;

    vector<int> cnt_d(10, 0);
    cnt_d[1] = cnt_c['j'];
    cnt_d[2] = cnt_c['a'];
    cnt_d[5] = cnt_c['p'];
    cnt_d[9] = cnt_c['w'] - cnt_d[2];
    cnt_d[7] = cnt_c['d'] - cnt_d[1] - cnt_d[2] - cnt_d[9];
    cnt_d[8] = cnt_c['m'] - cnt_d[7];
    cnt_d[0] = cnt_c['o'] - cnt_d[8];
    cnt_d[6] = (cnt_c['s'] - cnt_d[7] - cnt_d[8]) / 2;
    cnt_d[4] = cnt_c['c'] - cnt_d[5] - cnt_d[6] - cnt_d[9];
    cnt_d[3] = cnt_c['t'] - cnt_d[4];

    for (int d = 9; d >= 0; --d)
        for (int i = 0; i < cnt_d[d]; ++i)
            cout << d;
    cout << '\n';
    return 0;
}
```

### Python

```py
from collections import Counter

def main():
    n = int(input())
    line = input()

    cnt_c = Counter(line)

    cnt_d = [0] * 10
    cnt_d[1] = cnt_c['j']
    cnt_d[2] = cnt_c['a']
    cnt_d[5] = cnt_c['p']
    cnt_d[9] = cnt_c['w'] - cnt_d[2]
    cnt_d[7] = cnt_c['d'] - cnt_d[1] - cnt_d[2] - cnt_d[9]
    cnt_d[8] = cnt_c['m'] - cnt_d[7]
    cnt_d[0] = cnt_c['o'] - cnt_d[8]
    cnt_d[6] = (cnt_c['s'] - cnt_d[7] - cnt_d[8]) // 2
    cnt_d[4] = cnt_c['c'] - cnt_d[5] - cnt_d[6] - cnt_d[9]
    cnt_d[3] = cnt_c['t'] - cnt_d[4]

    for d in range(9, -1, -1):
        print(str(d) * cnt_d[d], end = '')
    print()

main()
```

## Uwagi

- Zadanie powstało na podstawie pytania z rozmowy kwalifikacyjnej firmy Slack.
