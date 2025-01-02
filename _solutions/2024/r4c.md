---
layout: solution
title: Literatura
edition: 2024
round: 4
level: c
author: Tomasz Kwiatkowski
sol_author: Szymon Hajderek
tags:
  - strings
  - greedy
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/8OisdmQQ0R46NgS5tFFF771Z/site/
video: https://youtu.be/8xexhyiKHsQ&t=998
---

### Rozwiązanie (wszystkie słowa zawierają wyłącznie literkę *a*)

Niezależnie od kolejności, w jakiej ułożymy słowa, długość najdłuższego przedziału takich samych literek będzie równa sumie długości wszystkich słów. Więc jest to oczywiście nasza odpowiedź.

### Rozwiązanie $(n=1)$

Jeżeli $n=1$, to wynikiem jest najdłuższy przedział takich samych literek, zawierający się w danym słowie. Aby wyznaczyć jego długość, przejdziemy się po kolei po literkach słowa, zwiększając licznik o 1 w przypadku gdy aktualna literka jest równa poprzedniej, albo ustawiając licznik na 1 w przeciwnym wypadku. Wynikiem jest maksymalna spośród wszystkich osiągniętych wartości licznika.

### Rozwiązanie $(n=2)$

Jeżeli $n\geq2$, to w pierwszej kolejności zastosujemy dla każdego słowa z osobna algorytm dla $n=1$ (bo być może najdłuższy możliwy fragment, jaki możemy uzyskać, zawiera się od razu w którymś ze słów). Może się jednak zdarzyć, iż optymalny fragment będzie leżał na połączeniu pewnych dwóch słów. Aby obsłużyć ten przypadek, rozpatrzymy po kolei dwie sytuacje. Poniższe rozumowanie będzie od teraz nazywane *Algorytmem dla $(n=2)$*:

1. **jeżeli słowo $(1)$ ma na końcu taką samą literkę co słowo $(2)$ na początku**: wynikiem potencjalnie może być najdłuższy suffix $(1)$ składający się z tych samych literek + najdłuższy prefix słowa $(2)$ składający się z tej samej literki.
2. **jeżeli słowo $(2)$ ma na końcu taką samą literkę co słowo $(1)$ na początku**: wynikiem potencjalnie może być najdłuższy suffix słowa $(2)$ składający się z tych samych literek + najdłuższy prefix słowa $(1)$ składający się z tej samej literki.

Naszym wynikiem jest maksimum ze wszystkich przypadków.

### Rozwiązanie $(n\leq3)$

Jeżeli $(n=3)$, sytuacja się lekko komplikuje. Najprostszym rozwiązaniem jest zastosowanie algorytmu dla $(n=1)$ na wszystkich możliwych kolejnościach słów i wzięcie maksymalnego wyniku. Otrzymamy w ten sposób kilka dodatkowych punktów, jednak to rozwiązanie nie zaprowadzi nas już niestety wiele dalej.

### Rozwiązanie $(s \leq 100$ / $n\leq 1000)$

Przypatrzmy się dokładniej przypadkowi $(n=3)$. Co różni go od przypadku $(n=2)$? Mogłoby się np zdarzyć, iż dane słowa wyglądają np. tak: *aaxx*, *a* oraz *bba*. Wtedy stosując *algorytm dla $(n=2)$* dla każdej pary słów, dostaniemy wynik 3 (czyli przedział literek *a* zawierający się w słowie $bbaaaxx$). Czemu nie jest on poprawny? Ponieważ moglibyśmy dokleić słowo *a* pomiędzy słowem *bba* oraz *aaxx*, uzyskując słowo *bbaaaaxx*, czego nie obsługujemy. W tym momencie przydatne jest wprowadzenie podziału słów na *mosty* oraz *końce*. *Mostem* nazwiemy słowo, składające się w całości z jednego rodzaju literki. *Końcami* nazwiemy wszystkie pozostałe słowa. Dla każdego rodzaju literki zsumujmy długości wszystkich *mostów* składających się z tej literki. Następnie możemy się przejść po każdej parze **różnych!** *końców* i zastosować *algorytm dla $(n=2)$*, powiększając wynik o sumę długości *mostów* składających się wyłącznie z literki kończącej lewy *koniec* (czyli również rozpoczynającej prawy *koniec*). Rozwiązanie to jest poprawne nie tylko dla $(n<=3)$. Gdy $n$ się zwiększa, może się co najwyżej zwiększyć liczba *mostów*, natomiast wciąż możemy je "upchnąć" między co najwyżej dwa *końce*. Dostajemy więc już w pełni poprawne rozwiązanie! Tylko zależnie od naszej implementacji, wciąż może być ono za wolne na niektóre z podzadań (w szczególności z pewnością na ostatnie dwa).

### Rozwiązanie pełne

No okej... Ale po co właściwie rozpatrywać każdą parę *końców*??? Przecież wystarczy połączyć najdłuższy suffix jakiegoś *końca* z najdłuższym kompatybilnym prefixem innego *końca*. W takim razie dla każdego rodzaju literki będziemy trzymali najdłuższy prefix jakiegoś *końca* składający się z tej literki oraz najdłuższy suffix jakiegoś *końca* składający się z tej literki. Następnie do sumy tych dwóch wartości dodajemy sumę *mostów* złożonych z danej literki i voilà! Mamy rozwiązanie -- szybkie i poprawne. Prawie... Niestety łatwo zapomnieć o przypadku, w którym zarówno najdłuższy prefix, jak i suffix należą do tego samego *końca*. W takim wypadku nasz algorytm "połączy słowo w kółko" -- co nie jest oczywiście poprawne. Aby tego uniknąć, będziemy trzymali dwa najdłuższe prefixy oraz dwa najdłuższe suffixy dla każdej literki, wraz z indeksem słowa, do którego należą. Reszta to kilka ifów (:

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

struct MaxRes { int length = 0, word_id = (-1); };

void update_max_res(tuple<MaxRes, MaxRes>& max_res, MaxRes const& new_res) {
  if(new_res.length >= get<0>(max_res).length) {
    get<1>(max_res) = get<0>(max_res);
    get<0>(max_res) = new_res;
  }
}

int main() {

  cin.tie(0)->sync_with_stdio(0);
  int n; cin >> n;

  // bridge[X] - sum of length of strings consisting only of characer X
  vector<int> bridge(26, 0);

  // pref[X][0] - longest prefix consisting only of X, pref[X][1] - second longest ...
  vector pref(26, tuple<MaxRes, MaxRes>{});

  // suff[X] - longest suffix consisting only of X, suff[X][1] - second longest ...
  vector suff(26, tuple<MaxRes, MaxRes>{});

  int res = 1;

  for(int t = 0; t < n; t++) {

    string s; cin >> s;
    int k = int(s.size());
    int cfirst = s[0] - 'a';
    int clast = s[k-1] - 'a';

    for(int i = 1, curr = 1; i < k; i++) { // what if the longest same-letter substring is "inside" of the strings?
      if(s[i] != s[i-1]) curr = 0;
      res = max(res, ++curr);
    }

    // Find maximum prefix of equal characters
    int mp = 0;
    while(mp < k && s[mp] == s[0]) mp++;

    // But what if the whole word consists of equal letters?
    if(mp == k) {
      bridge[cfirst] += k; // we can only use this word as a bridge
      continue;
    }

    // Find maximum suffix of equal characters
    int ms = k-1;
    while(ms >= 0 && s[ms] == s[k-1]) ms--;
    ms = (k - 1 - ms);

    update_max_res(pref[cfirst], { mp, t });
    update_max_res(suff[clast], { ms, t });

  }

  // we have to keep two longest prefixes / suffixes in case both longest prefix and suffix belong to the same word.
  for(int i = 0; i < 26; i++) {
    if(get<0>(pref[i]).word_id != get<0>(suff[i]).word_id) {
      res = max(
        res, 
        get<0>(pref[i]).length + bridge[i] + get<0>(suff[i]).length
      );
    }
    else {
      res = max(
        res,
        max(
          get<0>(pref[i]).length + bridge[i] + get<1>(suff[i]).length,
          get<1>(pref[i]).length + bridge[i] + get<0>(suff[i]).length
        )
      );
    }
  }

  cout << res << '\n';

}
```

### Python

```py
A = "abcdefghijklmnopqrstuvwxyz"

def process_word(word, data):
    splits = [-1] + [i for i in range(len(word) - 1) if word[i] != word[i + 1]] + [len(word) - 1]
    res = max(
        splits[1] - splits[0] + data["right"][word[0]] + data["all"][word[0]],
        splits[-1] - splits[-2] + data["left"][word[-1]] + data["all"][word[-1]],
    )
    data["left"][word[0]] = max(data["left"][word[0]], splits[1] - splits[0])
    data["right"][word[-1]] = max(data["right"][word[-1]], splits[-1] - splits[-2])
    for l, r in zip(splits[:-1], splits[1:]):
        res = max(res, r - l)
    return res

def main():
    n = int(input())
    tmp = [input() for _  in range(n)]

    data = {
        "left": dict.fromkeys(A, 0),
        "right": dict.fromkeys(A, 0),
        "all": dict.fromkeys(A, 0),
    }

    words = []
    for word in tmp:
        if all([c == word[0] for c in word[1:]]):
            data["all"][word[0]] += len(word)
        else:
            words.append(word)

    ans = max(data["all"].values())
    for word in words:
        ans = max(ans, process_word(word, data))
    print(ans)

main()
```
