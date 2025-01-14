---
layout: solution
title: Zakupy
edition: 2022
round: 0
author: Tomasz Kwiatkowski
tags:
  - implementation
---

W zadaniu należało wczytać dwie liczby całkowite i wypisać ich różnicę.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
    int m, k;
    cin >> m >> k;
    cout << k - m << '\n';
    return 0;
}
```

### Python

```py
m, k = map(int, input().split())
print(k - m)
```
