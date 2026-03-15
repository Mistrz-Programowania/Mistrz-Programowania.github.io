---
layout: solution
title: Zamiatanie
edition: Mistrz Programowania 2026
round: 4
level: E
author: Krzysztof Witkowski
tags:
  - math, nt
statement: https://szkopul.edu.pl/problemset/problem/35KFBwC_lkq3RPaTgBX80aIN/site/?key=statement
---

Zastanówmy się dla danego elementu, kiedy będzie on na *prawie dobrej pozycji*. *Prawie dobra pozycja* to taka, że wszystkie elementy mniejsze od niego są na lewo od niego. Możemy zauważyć, że jeśli uda nam się policzyć czas potrzebny na ustawienie elementu na *prawie dobrej pozycji* i weźmiemy maksimum z tych czasów, to uzyskamy czas potrzebny na faktyczne posortowanie permutacji.

Teraz zastanówmy się, jak policzyć ten czas dla danego elementu. Wykorzystamy częsty trik polegający na zastąpieniu elementów mniejszych od rozważanego zerami, a większych jedynkami. Wtedy to, co robi operacja U, to przesunięcie pierwszej jedynki na koniec ciągu, a operacja D przesuwa ostatnie zero na początek.

Z tego wynika bardzo ważny fakt - kolejność wykonywania operacji nie ma znaczenia, bo jeśli użyję *x* razy operacji U i *y* razy operacji D, to niezależnie od kolejności pierwsze *x* jedynek przesuwa się na koniec i ostatnie *y* zer przesuwa się na początek.

### Rozwiązanie wolne $O(n^2 \cdot m)$

Możemy po prostu dla każdej ilości U i D w ciągu operacji (których jest maksymalnie $O(m)$) zasymulować w dowolnej kolejności (zgodnie z obserwacją wyżej) kolejne ciągi operacji, dopóki permutacja nie jest posortowana. Jeśli okaże się, że potrzeba *x* użyć ciągu operacji, to w tablicy wynikowej w komórce *x* dodajemy $\binom{#?}{#U}$, gdzie #U to liczba dodanych U poprzez zmianę znaku ? na U. Posortowanie może wymagać nawet n iteracji, dlatego złożoność to $O(n^2 \cdot m)$.

### Rozwiązanie wzorcowe wolne $O(n\cdot m \cdot \log)$

Możemy dalej wykorzystać obserwację o przesuwaniu pierwszej jedynki i ostatniego zera. Chcemy dla dowolnej ilości operacji U i D liniowo stwierdzić, czy permutacja będzie posortowana. Jeśli nam się to uda, to możemy użyć wyszukiwania binarnego dla każdej ilości U i D w ciągu operacji, by znaleźć liczbę potrzebnych iteracji.

Załóżmy, że chcemy sprawdzić, czy ciąg będzie posortowany po *x* operacjach U i *y* operacjach D. Warunek dla danego elementu to wtedy max(pozycja elementu, pozycja *x* jedynki) > pozycja *y* zera od końca. Ten max wynika z tego, że jeśli nie ma jedynki przed rozważanym elementem, to przesuwamy go do następnej jedynki na prawo. Problem znajdowania k-th większego lub mniejszego elementu możemy rozwiązać liniowo, idąc 2 razy w kolejności posortowanej po wartości, trzymając jakiś wskaźnik na aktualną k-tą jedynkę/zero.

### Rozwiązanie bonusowe $O(n \cdot m)$

Istnieje wiele podejść do tego, zazwyczaj prostszych od tego co tu pokażę, ale ono też nie jest złe.

Zamiast wyszukiwania binarnego możemy zastosować inne podejście. Trzymamy jakąś zmienną *wynik* i przechodzimy w jakiejś kolejności po elementach permutacji. W danym elemencie wykonujemy następującą procedurę: dopóki warunek dla niego nie jest spełniony przy aktualnym *wyniku* -> zwiększ *wynik* o jeden. Zwiększeń będzie maksymalnie $O(n)$, więc jeśli sprawdzenie, czy warunek jest spełniony, i zwiększanie *wyniku* o 1 wykonamy w amortyzowanym czasie $O(n)$, to rozwiążemy to zadanie w $O(n \cdot m)$.

Będziemy iterowali się po elementach w kolejności rosnącej. Będziemy utrzymywali dwa wskaźniki na #U * *wynik* jedynkę i #D * *wynik* zero od końca. Wtedy potrzebujemy umieć robić następujące operacje:

- Dodaj element do zbioru zer.
- Usuń element ze zbioru jedynek.
- Przesuń wskaźnik jedynek do następnej jedynki.
- Przesuń wskaźnik zer do następnego zera.
- Przesuń wskaźnik zer do poprzedniego zera.

Te operacje nam wystarczają, bo zwiększenie *wyniku* to po prostu przesunięcie obu wskaźników do przodu o #U i #D pozycji, a zmiana elementu z jedynki na zero to ewentualne przesunięcie wskaźnika jedynek o jeden do przodu i wskaźnika zer o jeden do tyłu.

Strukturą, która pozwoli nam na takie operacje, jest lista, gdzie każdy element trzyma wskaźnik do następnego i poprzedniego. Usuwanie z takiej listy jest proste, bo wiemy, jakie wskaźniki trzeba poprzestawiać. Małym problemem jest dodawanie, gdzie w klasycznej liście trzeba przejść liniowo po niej, by znaleźć poprzedni i następny element od dodawanego, ale w tym przypadku dzięki temu, że kolejność dodań i usunięć jest cały czas taka sama, możemy na początku policzyć dla każdego elementu wartości w stylu następny i poprzedni mniejszy od niego, by od razu wiedzieć, jakie wskaźniki trzeba pozmieniać w tej liście. Ten następny i poprzedni mniejszy możemy policzyć kolejką monotoniczną.

Podczas trzymania wskaźników trzeba uważać na to, aby nie wyjść poza tablicę. Można np. trzymać wartość *jak bardzo wskaźnik wyszedł już poza tablicę*, czyli ile razy chciałem wykonać operację, która by to spowodowała.


