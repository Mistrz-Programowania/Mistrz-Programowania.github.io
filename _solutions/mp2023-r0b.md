---
layout: solution
title: Mniej niż 7
edition: 2023
round: 0
level: b
author: Jargalan Myagmardorj
sol_author: Wiktor AKA Pomocny Dzik
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/7OEBSJ0oHwCi_d5o10kSHTfs/site/
---

Omówienie C++: [https://youtu.be/CnOETic_wwA](https://youtu.be/CnOETic_wwA)

Omówienie Python: [https://youtu.be/gulo4s7OUJs](https://youtu.be/gulo4s7OUJs)

## Przykładowa implementacja

### C++

```cpp
#include <iostream>
using namespace std;

int main() {
  ios_base::sync_with_stdio(false);
  cin.tie(nullptr);

  int n;
  cin >> n;
  int res = 0;
  for (int i = 0; i < n; ++i) {
    int ph;
    cin >> ph;
    res += (ph < 7);
  }
  cout << res << '\n';
  return 0;
}
```
