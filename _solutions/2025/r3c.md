---
layout: solution
title: Bajtocja Entertainment
edition: 2025
round: 3
level: c
author: Tomasz Kwiatkowski
tags:
  - math
  - greedy
statement: https://szkopul.edu.pl/problemset/problem/YjIQYOyW3GO8jVLR-0RNAnrO/site/
---

W zadaniu należało podzielić ciąg na spójne fragmenty o równych sumach.

### Podział o zadanych sumach

Załóżmy, że zgadliśmy sumę, którą będzie miał dany fragment -- $s$. Możemy zachłannie brać elementy od lewej strony, aż nie uzyskamy sumy $s$. Mamy wtedy pierwszy fragment o sumie $s$. Zaczynamy kolejny fragment i powtarzamy procedurę.

Jeżeli ostatni fragment ma złą sumę, to podział na fragmenty o zadanej sumie $s$ jest niemożliwy, musimy spróbować inną sumę. Ale uwaga, jeżeli ostatni fragment ma sumę $0$, to możemy go "dokleić" do przedostatniego fragmentu -- nie zmienimy jego sumy. Powinniśmy jeszcze sprawdzić, czy na pewno podzieliliśmy ciąg na przynajmniej dwa fragmenty!

Taki algorytm zadziała w złożoności $O(n)$.

### Rozwiązanie wolne (podzadanie 4)

Jak zgadnąć sumę fragmentu? Sprawdźmy każdą możliwą, od $0$ do połowy sumy wszystkich wartości $S$.
Dla każdej z nich możemy wykonać wcześniej omówiony algorytm i sprawdzić, czy podział na fragmenty o zadanych sumach jest możliwy.

Otrzymamy rozwiązanie w złożoności $O(S \cdot n)$.

### Rozwiązanie wolne (podzadanie 5)

Aby przyspieszyć powyższe rozwiązanie zobaczmy, jakie sumy może mieć pierwszy fragment. Będzie on prefiksem ciągu, zatem jako kandydatów na sumę $s$, wystarczy sprawdzić tylko sumy pierwszych $i$ elementów dla $i = 1, 2, \ldots, n - 1$.

Otrzymamy rozwiązanie w złożoności $O(n^2)$.

### Pełne rozwiązanie

Aby w pełni rozwiązać zadanie, jeszcze bardziej ograniczymy liczbę kandydatów na sumę $s$.

Pomyślimy jednak o tym nieco inaczej niż do tej pory. Jeżeli dzielimy ciąg na $k$ elementów, każdy o sumie $s$, to naturalnie musi zachodzić $S = k \cdot s$. Wynika z tego, że $k$ musi być dzielnikiem $S$. Ponadto, $k$ nie może przekraczać $n$ -- nie podzielimy na więcej fragmentów, niż mamy liczb.

Ostatnią obserwacją jest to, że wystarczy ograniczyć się do $k$ będących liczbami pierwszymi. Jeżeli podzielimy ciąg na $6$ fragmentów, każdy o sumie $s$, to równie dobrze możemy podzielić ciąg na $2$ fragmenty, każdy o sumie $3s$.

Dzięki temu, dla ograniczeń z zadania, sprawdzimy maksymalnie $13$ wartości $k$. Znając $S$ oraz $k$, znamy również $s$ i możemy wykonać algorytm z poprzednich podzadań.

Otrzymamy rozwiązanie w złożoności $O(n \log n)$.

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

optional<vector<int>> split(const vector<int> &nums, int64_t sum) {
    vector<int> res;
    int64_t curr = 0;
    for (int i = 0; i < int(nums.size()); ++i) {
        curr += nums[i];
        if (curr == sum) {
            curr = 0;
            res.push_back(i);
        }
    }
    if (curr == 0) {
        res.back() = int(nums.size()) - 1;
        return res;
    }
    return nullopt;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    vector<int> nums(n);
    int64_t sum = 0;
    for (int &num : nums) {
        cin >> num;
        sum += num;
    }
    
    if (sum == 0) {
        auto res = split(nums, 0);
        if (res && int(res.value().size()) > 1) {
            int k = int(res.value().size());
            cout << "Tak\n";
            cout << k << '\n';
            for (int i = 0; i < k; ++i) {
                cout << res.value()[i] + 1 << " \n"[i == k - 1];
            }
        }
        else {
            cout << "Nie\n";
        }
        return 0;
    }

    int64_t x = abs(sum);
    for (int k = 2; k <= x && k <= n; ++k) {
        if (sum % k) { continue; }
        if (auto res = split(nums, sum / k)) {
            cout << "Tak\n";
            cout << k << '\n';
            assert(k == int(res.value().size()));
            for (int i = 0; i < k; ++i) {
                cout << res.value()[i] + 1 << " \n"[i == k - 1];
            }
            return 0;
        }
        while (x && x % k == 0) {
            x /= k;
        }
    }
    cout << "Nie\n";
    return 0;
}
```

### Python

```py
def split(nums, sum):
    res = []
    curr = 0
    for i in range(len(nums)):
        curr += nums[i]
        if (curr == sum):
            curr = 0
            res.append(i + 1)
    
    if curr == 0:
       res[-1] = len(nums)
       return res
    return None


def main():
    n = int(input())
    nums = list(map(int, input().split()))
    S = sum(nums)
    
    if S == 0:
        res = split(nums, 0)
        if res and len(res) > 1:
            print("Tak")
            print(len(res))
            print(" ".join(map(str, res)))
        else:
            print("Nie")
        return

    x = abs(S)
    k = 2
    while k <= x and k <= n:
        if S % k:
            k += 1
            continue
        res = split(nums, S / k)
        if res:
            print("Tak")
            print(len(res))
            print(" ".join(map(str, res)))
            return
        
        while x and x % k == 0:
            x /= k
        k += 1
    
    print("Nie")


main()
```

## Uwagi

- Należało uważać na przypadek, kiedy suma wszystkich wartości jest równa $0$. Wtedy każdy fragment również musi mieć sumę $0$.
- Zadanie wzorowane na zadaniu kwalifikacyjnym do Facebooka. W tamtej wersji wystarczyło stwierdzić, czy ciąg da się podzielić na dokładnie $3$ fragmenty o równej sumie. 
