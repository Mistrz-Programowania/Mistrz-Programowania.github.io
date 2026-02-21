---
layout: solution
title: Plan Włamanie
edition: 2026
round: 1
level: e
author: Maciej Mączka
tags:
  - data_structures
  - convex_hull_trick
  - li_chao_tree
  - treap
  - segment_trees
statement: https://szkopul.edu.pl/problemset/problem/2W2tfEiqi3abjZtetXPnHkfg/site/
---
### Treść
W zadaniu mamy podane 2 ciągi o liczbach 1,2,3 odpowiednio o długości n i m. Każda z liczb 1,2,3 ma odpowiednio koszt A,B,C i mamy policzyć najdłuższy wspólny niemalejący podciąg o maksymalnym koszcie, a potem odpowiedzieć na q hipotetycznych zmian A lub C i wypisać same maksymalne koszty tych podciągów.

### Podzadanie 6 - q=0
W tym podzadaniu musimy wyliczyć tylko główny wynik bez odpowiadania na zapytania.

Oznaczmy occ1[x][k] - ilość wystąpień liczby k na przedziale 0..x w ciągu 1 i analogicznie wyliczamy occ2[x][k] w czasie $O(n+m)$.

Zachłanna obserwacja: Jeśli nasze rozwiązanie będzie zawierało x jedynek, to może ono parować pierwsze x jedynek z pierwszego ciągu i pierwsze x jedynek z drugiego. Analogicznie dla trójek. Idea dowodu jest taka, że chcemy, aby ostatnie jedynki z obu ciągów były jak najwcześniej, gdyż wtedy będziemy mogli zagarnąć więcej dwójek i trójek.

Niech jedynki[i] = {a,b}, gdzie a to pozycja i-tej jedynki od lewej w ciągu 1, a b to pozycja i-tej jedynki w ciągu 2, jedynki[0] = {0,0}.  
Analogicznie trójki[i] = {a,b}, gdzie a to pozycja i-tej trójki od prawej w ciągu 1, a b to pozycja i-tej trójki w ciągu 2, trójki[0] = {n-1, m-1}.

Spróbujmy dla każdej pary jedynek określić wynik, jeśli będzie ona ostatnią parą jedynek w odpowiedzi.

Zaproponuję algorytm korzystający z pewnej struktury danych, której implementację omówimy później.  
Operacje na strukturze wyglądają następująco:
1. Dołóż i-tą parę trójek.
2. Dodaj dwójkę z ciągu 1.
3. Dodaj dwójkę z ciągu 2.
4. Znajdź maksymalną wartość z zebranych do tej pory dwójek i trójek.

Dla rozjaśnienia nasza struktura danych trzyma takie trójki liczb {c, b1, b2}, gdzie każda z nich przechowuje jakieś możliwe dokończenie rozwiązania, które będzie miało c trójek oraz min(b1,b2) dwójek i wartość takiej trójki liczb $val(c,b1,b2) = C*c + B*min(b1,b2)$,  
a operacje odpowiadają odpowiednio:
1. Dodaj trójkę liczb do zbioru {x, 0, 0}.
2. Do każdego b1 dodaj 1.
3. Do każdego b2 dodaj 1.
4. Policz maksymalny val po wszystkich elementach zbioru.

Zakładając, że mamy taką strukturę, która działa w rozsądnym czasie, możemy obliczyć wynik naszego zadania tym algorytmem:
```cpp
int obecna_trójka = 0;
Operacja1(0);
int ans = 0;
int idx1 = n-1;
int idx2 = m-1;
for(int i=jedynki.size()-1; i>=0; --i) {
   while(trójki[obecna_trójka+1].first > jedynki[i].first && trójki[obecna_trójka+1].second > jedynki[i].second) {
       obecna_trójka++;
       /* przejdź iteratorami idx1 i idx2 na pozycje trójki[obecna_trójka], wykonując operacje 2 i 3,
          jeśli napotkamy jakieś dwójki po drodze */
   }
   /* Przejdź iteratorami idx1 i idx2 na pozycje jedynki[i], dodając przy tym napotkane dwójki */
   ans = max(ans, i*A + Operacja4());
}
return ans;
```

Teraz pozostaje nam wymyślić, jaka struktura danych pozwoli nam na te operacje. Okazuje się, że najprostszym sposobem wymyślenia rozwiązania bez większych obserwacji będzie wykorzystanie kopcodrzewa (Treap - https://cp-algorithms.com/data_structures/treap.html).

Mianowicie zamieńmy nasze trójki {c,b1,b2} na pary {$C*c + B*b1, C*c + B*b2$}, gdzie new_val({a,b}) = min(a,b).  
To okazuje się już banalnie proste do zrobienia przy użyciu kopcodrzewa, gdyż tak będzie działała operacja:
1. Insertujemy parę {$C*x$, $C*x$} - kopcodrzewo pozwala na to w czasie $O(logn)$.
2. Do pierwszych elementów wszystkich par dodaj B - używając lazy propagation możemy to wykonać w czasie $O(logn)$.
3. Do drugich elementów wszystkich par dodaj B - analogicznie jak operacja 2, $O(logn)$.
4. Z wyliczaniem może się pojawić mały problem, bo nie do końca umiemy utrzymywać maxa po min(a,b) i dodawać do a i b, ale nasze wątpliwości rozwiewają się po zrobieniu sprytnej obserwacji, że pary {a,b} w kopcodrzewie możemy trzymać posortowane po wartości (a-b). Wtedy dodanie do wszystkich a lub b nadal utrzymuje ten porządek, a po wykonaniu splita na kopcodrzewie względem liczby 0 nasze zapytania możemy podzielić na 2 typy: max spośród par, gdzie a >= b, oraz max spośród par, gdzie a < b. Rozwiązujemy je poprzez znalezienie max po pierwszych liczbach w parach i max po drugich liczbach w parach. Wynikiem jest większa z tych dwóch liczb.

Odtwarzanie wyniku jest jedynie problemem implementacyjnym, gdyż dla każdej pary {a,b} musimy pamiętać, ile trójek ona dostała na początku oraz przy znajdowaniu maksa również musimy utrzymywać, ile miała trójek para, która dała największą odpowiedź.

Implementację tego można znaleźć w plikach wla.cpp i wlas1.cpp, które będą umieszczone na dole tego edytorialu, jak i w Rozwiązaniach Wzorcowych paczki Szkopuł.

### Rozwiązanie wzorcowe
Zauważmy, że musimy umieć odpowiadać tylko na zapytania o A, gdyż zapytania o C są analogiczne.

Wróćmy do naszego pseudokodu, a dokładniej do linijki:
```cpp
ans = max(ans, i*A + Operacja4());
```
Dorzućmy przed główną pętlą:
```cpp
vector<pair<int, int>> func;
```
oraz po linijce wyliczania ans:
```cpp
func.push_back({i, Operacja4()});
```
Okazuje się, że jeśli pytamy o wartość dla jakiegoś hipotetycznego A, oznaczmy go przez x, to wynosi ona maksimum po każdym $a,b \in$ func: $a*x + b$. Sprowadzamy więc problem do następującego:

Mamy $O(n)$ funkcji liniowych i musimy odpowiadać na maksymalną wartość spośród ich wszystkich dla jakiegoś całkowitego x. Ten problem można rozwiązać Convex Hull Trickiem lub drzewem Li Chao (https://cp-algorithms.com/geometry/convex_hull_trick.html) w czasie logarytmicznym od zapytania. W pliku wla.cpp zostało zaimplementowane rozwiązanie z Convex Hull Trickiem.

To już ostatni krok naszego zadania, a całkowita złożoność czasowa wynosi: $O((n+m+q)*logn)$, a złożoność pamięciowa $O(n+m)$.

Ciekawostka: Istnieją również rozwiązania (podzadania 6), które używają łatwiejszych w implementacji struktur danych, jak na przykład drzewo przedziałowe lub zmodyfikowana kolejka, która pozwala to rozwiązać w czasie liniowym, lecz uznałem, że opisany wyżej sposób jest łatwiejszy i przyjemniejszy do zrozumienia (i wcale nie taki ciężki do zaklepania).