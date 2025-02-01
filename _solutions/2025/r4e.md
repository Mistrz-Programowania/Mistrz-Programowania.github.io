---
layout: solution
title: Bajtex S.A.
edition: 2025
round: 4
level: e
author: Szymon Hajderek
tags:
  - data_structures
  - small_to_large
  - persistent
  - dsu
---

W zadaniu musimy obsłużyć trzy typy zapytań:

  1. Połącz zbiory $A$ oraz $B$, takie, że $x \in A$ oraz $y \in B$ (dane są $x$ oraz $y$)
  2. Cofnij $c$ ostatnich operacji $(1)$ (jest gwarantowane, iż jest to wykonalne, czyli było wykonanych przynajmniej $c$ operacji typu $(1)$)
  3. Podaj minimalne $c$ takie, że po wykonaniu operacji *"$(2)$ $c$"*, elementy $x$ oraz $y$ należą do różnych zbiorów (jest gwarantowane, iż $x \neq y$)

Początkowo $\forall_{1 \leq x \leq n}$ istnieje zbiór $X = \{ x \}$.

---

W celu rozwiązania zadania, zbudujemy zmodyfikowaną struturę *DSU*. Przez $rep[v]$ oznaczymy krawędź wychodzącą z wierzchołka $v$, a przez $siz[v]$ rozmiar spójnej, której *korzeniem* jest $v$.

Ponieważ musimy umieć wykonywać operację *"$(2)$ $c$"*, możemy zrezygnować z wykorzystania kompresji ścieżek - i tak niewiele by nam dała, gdyż potencjalnie ciągle byśmy musieli ją wycofywać. Możemy natomiast użyć często spotykanej techniki łączenia *"mniejszy do większzego"*. Dzięki temu uzyskamy strukturę o głębokości $O(log(n))$.

---

Cofnięcie pojedynczej operacji możemy wykonać w złożoności $O(1)$ - wystarczy utrzymywać stos wykonanych operacji $(1)$, utrzymujący parę wartości $( v, s)$, oznaczającą iż w wyniku danej operacji zmieniła się krawędź wychodząca z wierzchołka $v$, a rozmiar zbioru $X$, do którego należał $v$ przed tą operacją wynosił $s$. W takim razie cofnięcie realizujemy następująco:

  - $siz[rep[v]]$ -= $s$ (odejmujemy rozmiar, który już nie należy do zbioru)
  - $rep[v] = v$ ($v$ jest spowrotem swoim korzeniem)
  - $siz[v]$ = $s$ (przywracamy stary rozmiar)

Cofnięcie $c$ operacji możemy realizować jako $c$ pojedynczych cofnięć - każda operacja może być cofnięta co najwyżej raz, więc wykonamy łącznie co najwyżej $O(m)$ operacji.

---

Gdy łączymy dotychczas rozłączne zbiory $(A, B)$, tworzymy krawędź miedzy ich dotychczasowymi *"korzeniami"* (powiedzmy $a$ oraz $b$). Załóżmy *b.s.o.*, że krawędź jest poprowadzona z $a$ do $b$. W takim razie, dla każdej pary $\{ x, y \}$ $x \in A, y \in B$, aktualny moment jest pierwszym momentem, w którym $x$ jest w tym samym zbiorze co $y$. Dla par $(x \in A, y \in A)$ (analogicznie dla $B$) nic się natomiast nie zmienia.

Oznacza to, że zmieniło się coś jedynie dla elementów, dla par wierzchołków $(x, y)$, dla których $lca(x, y) = b$ Możemy więc umieścić na dodawanej właśnie krawędzi informację o tym, w którym momencie została dodana. (przez $lca(x, y)$ rozumiemy pierwszy wierzchołek, który jest osiągalny zarówno z $x$ jak i z $y$).

---

Będziemy więc podczas łączenia ustawiać wartości na krawędziach w naszym *DSU* - waga będzie oznaczała momemt dodania. $time[v] = t$, oznacza, iż krawędź wychodząca z wierzchołka $v$ została dodana w czasie $t$.

Zapytanie typu $(3)$ o parę $(x, y)$ możemy teraz zrealizować znajdując krawędź, którą $x$ osiąga $lca(x, y)$ - nazwijmy ją $e_1$ oraz krawędź, którą $y$ osiąga $lca(x, y)$ - nazwijmy ją $e_2$. Powiedzmy, że aktualny moment (czyli liczba niecofniętych operacji typu $(1)$) to $t$. Wtedy naszym wynikiem jest: $(t - max(time[e_1], time(e_2)))$

---

## Uwagi:

-  Trzeba uważnie rozpatrzeć przypadki, w których jeden z wierzchołków z pary z zapytania jest bezpośrednio osiągalny z drugiego.
-  Należy pamiętać, iż może zajść sytuacja, w której łączone wierzchołki już są łączone - łatwo popełnić w takiej sytuacji drobne błędy, które mogą prowadzić do pogorszenia złożoności obliczeniowej bądź spowodować udzielanie niepoprawnych odpowieedzi.
- Nie przypadkowo w zadaniu jest dość ciasny limiit pamięci - ma to zmusić do staranności. Trzeba choć trochę uważać, by go nie przekroczyć - spore benefity, zarówno czasowe, jak i (przede wszystkim) pamięciowe, powoduje użycie funkcji `.reserve(max_rozmiar)` w przypadku wykorzystania wektorów ze standardowej biblioteki c++. Jeżeli tego nie zrobimy, `vector` może (aby zapewnić szybką złożoność zamoryzowaną działania funkcji `.push_back`), zaalokować `200%` pamięci, której faktycznie używa. Jak widać w omówieniu, do rozwiązani wystarczy jedynie kilka tablic / `vector`-ów - nie ma sensu trzymać tablic, które nie wnoszą żadnej nowej informacji - a jeżeli już coś takiego robimy, zachęcam do upewnienia się, czy na pewno nie zbliżamy się zbytnio do limitu pamięci.

- zużycie pamięci możemy mierzyć lokalnie na kilka sposobów:
  1. `/usr/bin/time ./program < dane_wejsciowe  > wynik_programu` - mówi ile czasu działał program i ile pamięci zużył (w `kB`).

  2. `./oiejq.sh ./program < dane_wejsciowe > wynik_programu` - to samo, ale mierzy jak na olimpiadzie - skrypt ten można pobrać na stronie `oi.edu.pl`.

  3. jeżeli używasz `windowsa`, zacznij używać `linuxa` 
  (najprostszym na to sposobem jest `wsl` - warto podczytać o tym na internecie) (;