---
layout: solution
title: Proceduralna generacja terenu
edition: 2024
round: 1
level: e
author: Szymon Hajderek
statement: https://szkopul.edu.pl/problemset/problem/mqFpdWrqdAd32CQndmM3o8Be/site/
video: https://youtu.be/qIraPa7A1wY?t=2053
---

Warto napisać na początku rozwiązanie brutalne, przechodzące po wszystkich permutacjach (seedach) $n$-elementowych, wyznaczające dla każdego seedu jego ciekawość wprost z definicji. Takie rozwiązanie możemy zrealizować prosto w złożoności $O(n \cdot n! \cdot S)$, gdzie $S$ oznacza sumę ciekawości wszystkich seedów. Pozwala ono uzyskać 10 punktów oraz umożliwia weryfikację poprawności naszych kolejnych rozwiązań. Przyjrzyjmy się jednak nieco dokładniej temu co się dzieje, gdy dokonujemy zmieszania seedu $a$ z seedem $b$. Wiemy, że dla każdego $i$, $c(i) = a(b(i))$. Oznacza to, że na $i$-tej pozycji wynikowej permutacji, będzie stała $b(i)$-ta wartość permutacji $a$. Możemy więc to graficznie zinterpretować jako "strzałki", tudzież krawędzie w grafie -- z pozycji $b(i)$ do pozycji $i$. Rysując parę przykładów, nietrudno się przekonać, iż narysowane krawędzie formują rozłączne cykle. Zauważmy, iż w definicji ciekawości seedu, dokonujemy cały czas zmieszania aktualnego seedu z tą samą permutacją $s$. W związku z tym zbiór tak zdefiniowanych krawędzi nie ulega zmianie, a zmieszanie aktualnego seedu z seedem $s$ jest jedynie cyklicznym przesunięciem wartości znajdujących się na poszczególnych pozycjach każdego cyklu (wartości przesuwamy na pozycje wskazywane przez strzałki). Kiedy po raz pierwszy wrócimy do sytuacji, gdy każdy z cykli będzie w swoim początkowym stanie? Niech długość $i$-tego cyklu wynosi $d_i$. Wtedy $i$-ty cykl wraca oczywiście do swojego początkowego stanu po $m \cdot d_i$ zmieszaniach (gdzie $m$ jest dowolną liczbą całkowitą nieujemną). Oczywiście najmniejsza liczba przemieszań, jaką musimy wykonać, aby dojść do wspólnej wielokrotności długości wszystkich cykli to $NWW(d_1, d_2, \dots)$. W związku z tym właśnie tyle wynosi ciekawość seedu. Nasze zadadnie sprowadza się więc teraz do zliczenia seedów $n$ elementowych, których NWW długości cykli wynosi $k$. Możemy już teraz napisać backtrack'a, który poprawnie zaimplementowany, pozwoli nam się cieszyć 40 punktami. 

### Rozwiązanie wzorcowe

Aby usprawnić nasze rozwiązanie jeszcze bardziej, wykorzystamy programowanie dynamiczne. Niech $dp_{n,k}$ oznacza liczbę seedów $n$ elementowych, których NWW długości cykli wynosi $k$. Będziemy dokonywali przejść, "dokładając" kolejne cykle do dotychczasowych seedów. Mogłoby się wydawać, iż wystarczyłoby proste przejście, w którym wartość $dp_{n,k} \cdot \binom{n+c}{c} \cdot (c-1)!$ "wypychamy" do $dp_{n+c, NWW(c, k)}$ (interpretacja -- wybieramy spośród $n+c$ dostępnych elementów cykl długości $c$, a następnie wybieramy dowolnie jego krawędzie [na $(c-1)!$ sposobów]). Niestety jednak, wzór ten nie jest w pełni poprawny -- w przypadku dołożenia cyklu długości $c$ do seedu już zawierającego cykl długości $c$, zliczymy pewne rozwiązania wielokrotnie. Istnieje wiele sposobów, aby uniknąć takiej sytuacji. Możemy np. dokładać wszystkie cykle długości $c$ "naraz". Aby to zrealizować, możemy np. iterować się w pierwszej kolejności po dokładanej wielkości ($c$) cyklu, w drugiej po kolejności po parametrach $n,k$, a następnie po tym ile razy ($m$) chcemy dołożyć cykl długości $c$. Wystarczy wtedy podzielić "wypychaną" liczbę możliwości przez $m!$ -- zapobiegamy w ten sposób rozróżnianiu nierozróżnialnych od siebie cykli tej samej długości. Takie rozwiązanie jest wystarczająco szybkie i otrzymuje 100 punktów. Przykładową implementację przedstawiamy poniżej.

## Przykładowa implementacja

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

#define loop(i, a, b) for(int i = a; i <= b; i++)
#define loop_rev(i, a, b) for(int i = a; i >= b; i--)
using ll = int64_t;

constexpr int MAX_N = 500 + 1;
constexpr int MAX_VAL = 1e5 + 1;
constexpr int MAX_DIVS = 150 + 1; // no number in range [1, MAX_VAL) has more than 134 divisors.

ll f[MAX_N] = { 1 }, f_inv[MAX_N] = { 1 };
int lcm_prep[MAX_DIVS][MAX_DIVS];
ll dp[MAX_N][MAX_DIVS];
int div_id[MAX_VAL];
ll divs[MAX_DIVS];

constexpr ll MOD = 1e9 + 7;
inline ll mulm(ll a, ll b) { return a * b % MOD; }
inline ll addm(ll a, ll b) { return (a + b) >= MOD ? a + b - MOD : a + b; }
inline ll subm(ll a, ll b) { return (a - b) < 0 ? a - b + MOD : a - b; }
inline ll newton(ll n, ll k) { return mulm(f[n], mulm(f_inv[k], f_inv[n-k])); }
inline int scaled_lcm(ll a, ll b) { return div_id[lcm(a, b)]; }

ll qpow(ll a, ll b) {
  ll res = 1;
  while(b) {
    if(b&1) {
      res = mulm(res, a);
    }
    a = mulm(a, a);
    b /= 2;
  }
  return res;
}

int main() {
  cin.tie(0)->sync_with_stdio(false);
  int n, k; cin >> n >> k;

  loop(i, 1, n)
    f[i] = mulm(f[i-1], i);

  f_inv[n] = qpow(f[n], MOD-2);
  loop_rev(i, n, 1)
    f_inv[i-1] = mulm(f_inv[i], i);

  int num_divs = 0;
  loop(i, 1, k) {
    if(k % i == 0) {
      divs[num_divs] = i;
      div_id[divs[num_divs]] = num_divs;
      num_divs++;
    }
  }

  loop(i, 0, num_divs-1)
    loop(j, i, num_divs-1)
      lcm_prep[i][j] = lcm_prep[j][i] = scaled_lcm(divs[i], divs[j]);

  dp[0][0] = 1;

  loop(new_cycle_i, 0, num_divs-1) { // length of the cycle we are going to append
    ll new_cycle_len = divs[new_cycle_i];
    ll max_new_cycle_cnt = n / new_cycle_len;

    loop_rev(curr_space, n, 0) { // current length of the seed
      loop(current_lcm_i, 0, num_divs-1) { // current lcm of the seed's cycles
        ll coef = 1;
        ll space_needed = new_cycle_len;
        ll space_to_choose_from = (n - curr_space);

        loop(new_cycle_cnt, 1, max_new_cycle_cnt) { // how many times we are adding the new cycle
          if(space_to_choose_from < new_cycle_len) break;

          // number of ways to choose `new_cycle_cnt` cycles of length `new_cycle_len` from `(n - i)` elements.
          coef = mulm(coef, mulm(newton(space_to_choose_from, new_cycle_len), f[new_cycle_len - 1]));

          // pushing the new options
          dp[curr_space + space_needed][lcm_prep[current_lcm_i][new_cycle_i]] = addm(
            dp[curr_space + space_needed][lcm_prep[current_lcm_i][new_cycle_i]],
            mulm(dp[curr_space][current_lcm_i], mulm(coef, f_inv[new_cycle_cnt]))
          );

          space_needed += new_cycle_len;
          space_to_choose_from -= new_cycle_len;

        }

      }
    }
  }

  cout << mulm(dp[n][num_divs-1], qpow(f[n], MOD-2)) << '\n';

}
```
