---
layout: solution
title: Puk
edition: 2025
round: 4
level: b
author: Daniel Olkowski
sol_author: Tomasz Kwiatkowski
tags:
  - math
  - dp
statement: https://szkopul.edu.pl/problemset/problem/6INoGyjOCMCao68VXg0rPNIy/site/
---

Możemy iteracyjnie obliczyć kolejne wartości ciągu $A$ utrzymując je w tablicy i obliczając kolejne wartości na podstawie poprzednich.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<int> A(max(2, n));
    A[0] = A[1] = 1;
    for (int i = 2; i < n; ++i) {
        A[i] = A[i - A[i - 1]] + A[i - A[i - 2]];
    }
    cout << A.back() << endl;
    return 0;
}
```

### Python

```py
def main():
    n = int(input())
    A = [0] * max(2, n)
    A[0] = A[1] = 1
    for i in range(2, n):
        A[i] = A[i - A[i - 1]] + A[i - A[i - 2]]
    print(A[-1])


main()
```

## Uwagi

- Rozwiązanie rekurencyjne, nieostrożnie napisane, będzie działać bardzo wolno i przejdzie tylko pierwsze dwa podzadania. 
- Rozważany w zadaniu ciąg to [Hofstadter Q-sequence](https://oeis.org/A005185).
