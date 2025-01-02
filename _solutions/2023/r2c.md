---
layout: solution
title: Kucharz 2
edition: 2023
round: 2
level: c
author: Tomasz Kwiatkowski
tags:
  - pref
statement: https://szkopul.edu.pl/problemset/problem/ZvwXouDiRBunQjxByKfECOX7/site/
---

### Rozwiązanie wolne

Naturalnym podejściem może być skorzystanie z rozwiązania zadania Kucharz. Będziemy mieli wówczas masy pojedynczych produktów. Dla każdego zapytania pętlą można obliczyć sumę danych produktów. Niestety to podejście nie dostanie maksymalnej liczby punktów -- jest zbyt wolne. Zauważmy, że gdyby każde zapytanie pytało o masę wszystkich produktów, to dla każdego zapytania, których jest $q$, musielibyśmy zsumować $n$ liczb.

Możemy powiedzieć, że _złożoność_ tego podejścia to $O(nq)$. Dla dużych testów, wykonalibyśmy około $5\cdot 10^{11}$ operacji. To dużo. Zależnie od złożoności tych operacji, możemy przyjmować, że w ciągu sekundy sprawdzarka może wykonać $10^7$ -- $10^8$ operacji. Zatem nasze rozwiązanie mogłoby pesymistycznie działać ponad $16$ minut!

### Rozwiązanie wzorcowe

Zastanówmy się jednak czy nie możemy zrobić tego lepiej. Co dokładnie mówi nam $k$-te wskazanie wagi? Sumaryczną masę pierwszych $k$ produktów.

Spójrzmy na przykład. Chcemy poznać sumaryczną masę produktów od $20$-tego do $23$-go. Gdybyśmy **przed** dodaniem $20$-tego wytarowali wagę (wyzerowali wskazanie), to po dodaniu $23$-go produktu, na wadze otrzymalibyśmy oczekiwaną sumę produktów. Co tak naprawdę zrobiliśmy? Wzięliśmy masę pierwszych $23$-ech produktów i odjęliśmy w pewnym momencie masę pierwszych $19$-stu produktów (ale nie $20$-stu, bo chcemy zważyć $20$-ty produkt!).

Formalniej, mając zapytanie o masę produktów do $a$ do $b$ odejmujemy $a-1$-sze wskazanie wagi od $b$-tego.

W tablicy `pref` zapamiętamy kolejne wskazania wagi. Odpowiadając na zapytania odejmiemy odpowiednie dwie wartości tablicy `pref`.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
using namespace std;

const int MAXN = 1e6;
int pref[MAXN + 1];

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr);

    int n;
    cin >> n;
    for (int i = 1; i <= n; ++i) {
        int w;
        cin >> w;
        pref[i] = w;
    }
    int q;
    cin >> q;
    for (int i = 0; i < q; ++i) {
        int a, b;
        cin >> a >> b;
        cout << pref[b] - pref[a - 1] << '\n';
    }
    return 0;
}
```

### Python

```py
def main():
    _ = int(input())
    pref = [0] + list(map(int, input().split()))
    q = int(input())
    for i in range(q):
        a, b = map(int, input().split())
        print(pref[b] - pref[a - 1])

main()
```

## Uwagi

- Co do rozwiązania wolnego, ktoś mógłby powiedzieć "ale przecież nie musi być takich dużych, pesymistycznych testów -- rozwiązanie będzie szybkie". Niestety, z bardzo dużym prawdopodobieństwem, można założyć, że na Olimpiadzie, czy innych tego typu konkursach, będą duże, pesymistyczne testy.
- W rozwiązaniu pierwsze wskazanie wagi trzymam w `pref[1]` (a nie `pref[0]` -- tam trzymam $0$ -- czyli _zerowe_ wskazanie wagi). Dzięki temu nie muszę oddzielnie rozpatrywać zapytań z $a = 1$. Ten zerowy element czasem nazywany jest _strażnikiem_.
- To zadanie pokazuje technikę _sum prefiksowych_ -- bardzo ważnej, w kontekście Olimpiady, tego typu konkursów i przydatnej w wielu innych miejscach. Tablica `pref` to tablica _sum prefiksowych_ -- dzięki niej możemy szybko odpowiadać na zapytania np. o sumę liczb na przedziale. Jeżeli jeszcze nie znasz tej techniki, zachęcam do jej poznania!
