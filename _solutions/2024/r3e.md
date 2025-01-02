---
layout: solution
title: Skakanie
edition: 2024
round: 3
level: e
author: Szymon Hajderek
statement: https://szkopul.edu.pl/problemset/problem/oMxyFl4BNXMj9_G1qVoPNvPw/site/
video: https://youtu.be/VrMk1y1y_F0?t=1409
---

Wpierw możemy oczywiście napisać powolne rozwiązanie rekurencyjne. Takie rozwiązanie pozwoli bez problemu uzyskać wynik 15 - 20 punktów.
Jedną z kluczowych obserwacji prowadzących do rozwiązania wzorcowego jest zauważenie, iż $Skakanie(A, B)$ można zrealizować na tyle samo sposobów, co $Skakanie(C, D)$, jeżeli $|B - A| = |D - C|$. Wynika to z faktu, że dowolną sekwencję ruchów w przedziale $[A, B]$ możemy odwzorować na przedziale $[C, D]$, zachowując długości kolejnych skoków oraz momenty dygresji. Wynika z tego w szczególności, iż $Skakanie(A, B)$ można wykonać na tyle samo sposobów co $Skakanie(B, A)$. Wystarczy się więc zajmować jedynie długością rozpatrywanego skakania. Wprowadzimy więc notację $dp_n$, oznaczającą liczbę sposobów na wykonanie skakania długości $n$. Zastanówmy się co się mogło stać, zanim wskoczyliśmy na $n$-ty schodek:

1. Skoczyliśmy ze schodka o numerze $(n-1)$ bez wykonania dygresji
2. Skoczyliśmy ze schodka o numerze $(n-2)$ bez wykonania dygresji
3. Skoczyliśmy ze schodka o numerze $(n-1)$ wykonując uprzednio dygresję
4. Skoczyliśmy ze schodka o numerze $(n-2)$ wykonując uprzednio dygresję

W przypadkach (1) oraz (2) wykonujemy po prostu skok. Czyli każde dotychczasowe skakanie rozszerzamy na jeden sposób -- możemy to zrobić na $dp_{n-1}$ (1) + $dp_{n-2}$ (2) sposobów. W przypadkach (3) oraz (4) musimy jeszcze domnożyć $dp_{n-1}$ oraz $dp_{n-2}$ przez odpowiednie liczby sposobów na wykonanie dygresji. Zgodnie z definicją dygresji, jeżeli stoimy na $K$-tym schodku, dygresja obliguje nas do wykonania bezpośrednio po sobie $Skakania(K, 0)$ oraz $Skakania(0, K)$. Ponieważ są to skakania niezależne, więc możemy je wykonać na $dp_k \cdot dp_k$ sposobów. Tym samym przypadki (3) oraz (4) doliczają nam łącznie $(dp_{n-1} \cdot {dp_{n-1}}^2 + dp_{n-2} \cdot {dp_{n-2}}^2)$ sposobów. Ostateczną liczbę sposobów na wykonanie skakania długości $n$ możemy więc zapisać, łącząc wszystkie przypadki, jako $dp_n = dp_{n-1} \cdot (1 + {dp_{n-1}}^2) + dp_{n-2} \cdot (1 + {dp_{n-2}}^2)$. Rozwiązanie wykorzystujące powyższy wzór, działające w złożoności $O(n)$ pozwoli uzyskać 50 punktów. Wykorzystując preprocessing np. "co milion", możemy uzyskać nawet 70 punktów. Od rozwiązania na 100 punktów dzieli nas już jednak niewiele. Zauważmy, że stan $dp_n$ wyliczamy na podstawie dwóch sąsiednich stanów $dp_{n-1}$ oraz $dp_{n-2}$, których wartości są ograniczone przez modulo z treści. W związku z tym, w pewnym momencie wartości zaczną się powtarzać w swego rodzaju "cyklu". Jak długi może on być? Pesymistycznie jego wielkość możemy oszacować przez $modulo^2$, gdyż tyle różnych par może maksymalnie istnieć. W praktyce jednak okazuje, iż długości tych cykli są zazwyczaj znacznie krótsze. Przyglądając się dokładnie modulo danemu w treści zadania, zauważyć można, iż jest ono co najmniej *nietypowe*: $1019043214942321 = P1(100043) \cdot P2(100853) \cdot P3(100999)$. Możemy sprawdzić, że jesteśmy w stanie szybko wyznaczyć osobno cykle modulo $P1$, $P2$ oraz $P3$. Czy jednak w czymkolwiek to pomaga? Z pomocą przychodzi w tym momencie *Twierdzenie chińskie o resztach* [^1]. Mówi ono, iż znając resztę z dzielenia $x$ przez $a$ oraz przez $b$, gdzie $a$ jest względnie pierwsze z $b$, jesteśmy w stanie jednoznacznie wyznaczyć resztę z dzielenia $x$ przez $a \cdot b$, co pozwala nam uzyskać wynik 100 punktów, łącząc rozwiązania wyliczone osobno dla krótszych cykli modulo $P1$, $P2$ oraz $P3$.

## Przykładowa implementacja

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

using ll = int64_t;
using i128 = __int128_t;
#define loop(i, a, b) for(int i = a; i <= b; i++)
#define sz(x) int(x.size())
#define pb push_back

i128 M = 1019043214942321LL;
static void setm(i128 m) { M = m; }
static void unsetm() { M = 1019043214942321LL; }

inline ll mul(i128 a, i128 b) { return (a * b) % M; }
inline ll add(i128 a, i128 b) { return (a + b >= M ? a + b - M : a + b); }
inline ll sub(i128 a, i128 b) { return (a - b < 0 ? a - b + M : a - b); }
inline ll cub(i128 x) { return mul(x, mul(x, x)); }

/* extended euclidian algorithm */
struct EuclidianRes { i128 x, y, gcd; };
static EuclidianRes euc(i128 a, i128 b) {
  if(b == 0) return { 1, 0, a };
  auto [ x, y, gcd ] = euc(b, a % b);
  return { y, x - (a / b) * y, gcd };
}

struct Cycle { vector<ll> sequence; ll cycle_beg, cycle_end, cycle_len; };

/* given a cycle, get n-th value from that cycle */
ll solve(ll n, Cycle const& c) {
  if(n < sz(c.sequence)) {
    return c.sequence[n - 1];
  }
  else {
    return c.sequence[c.cycle_beg + (n - c.cycle_beg) % c.cycle_len - 1];
  }
}

/* helper function used to update dp state from n to n+1 */
void step_rec(ll &vold, ll &vcurr) {
  ll vnew = add(add(cub(vcurr), vcurr), add(cub(vold), vold));
  vold = vcurr, vcurr = vnew;
}

/* function used to find a cycle in the recursive sequence */
Cycle calc_cycle(ll mod) {
  setm(mod);
  vector<map<int, int>> v(mod);
  v[1][3] = 1;

  Cycle res;
  res.sequence.pb(1), res.sequence.pb(3);
  ll vold = 1, vcurr = 3, curr = 2;

  while(true) {
    step_rec(vold, vcurr);
    if(v[vold].count(vcurr)) {
      res.sequence.pop_back(); break;
    }
    res.sequence.pb(vcurr);
    v[vold][vcurr] = curr++;
  }

  res.cycle_end = curr - 1;
  res.cycle_beg = v[vold][vcurr];
  res.cycle_len = res.cycle_end - res.cycle_beg + 1;
  unsetm();

  return res;

}

int main() {
  cin.tie(0)->sync_with_stdio(false);
  ll n; cin >> n;

  /* prime factors of the weird modulo from the task :P */
  vector<ll> mods = { 100043, 100853, 100999 };
  vector<ll> partial_res(sz(mods));

  /* calculating value for each modulo independently */
  loop(i, 0, sz(mods)-1) {
    Cycle cycle = calc_cycle(mods[i]);
    partial_res[i] = solve(n, cycle);
  }

  i128 a1 = partial_res[0], m1 = mods[0];

  /* chinesse remainder theorem */
  loop(i, 1, sz(mods)-1) {
    i128 a2 = partial_res[i], m2 = mods[i];
    auto [ n1, n2, _ ] = euc(m1, m2);
    a1 = (a1*n2*m2 + a2*n1*m1) % (m1*m2);
    if(a1 < 0) a1 += (m1*m2);
    m1 = m1 * mods[i];
  }

  cout << ll(a1) << '\n';

}
```


## Uwagi
[^1]: Więcej o twierdzeniu chińskim o resztach można przeczytać na [wikipedii](https://pl.wikipedia.org/wiki/Chi%C5%84skie_twierdzenie_o_resztach) oraz [cp-algorithms](https://cp-algorithms.com/algebra/chinese-remainder-theorem.html).
- W C++ wzór rekurenycjny wymaga wykorzystania typu `__int128_t`, gdyż typ `long long` może nie zmieścić wyniku mnożenia.
- Stojąc na schodku "zero" nie można wykonać dygresji
