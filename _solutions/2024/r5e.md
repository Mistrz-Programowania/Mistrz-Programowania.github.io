---
layout: solution
title: Game of Bugs
edition: 2024
round: 5
level: e
sol_author: Maciej Wiśniewski
statement: https://szkopul.edu.pl/problemset/problem/I1-1PgPGN4AskrBkVhwZ_O5E/site/
video: https://youtu.be/YDD-yt7Y_tE&t=1758
---

Nazwijmy "otaczającym prostokątem" najmniejszy prostokąt, który zawiera wszystkie żywe komórki.
Zauważmy że co iterację, niezależnie od błędu maszyny, otaczający prostokąt zawsze poszerzy się
o przynajmniej 1 kratkę w każdą z czterech stron. Ta obserwacja ogranicza nam wynik przez połowę
wysokości i połowę szerokości otaczającego prostokąta końcowego stanu (Możliwość wypisania "inf",
oraz podawanie wyniku modulo było tylko podpuchą).

Załóżmy na razie że maszyna nie popełnia błędów. Wtedy dla każdego stanu który nie jest początkowym,
moglibyśmy jednoznacznie odczytać poprzedni stan, rząd po rzędzie. Każdy z napotkanych po drodze stanów
mógłby być początkowym oraz żaden inny nie mógłby być. Sprawdzenie jednej iteracji wstecz zajmuje $O(nm)$,
a iteracji jest co najwyżej $\min(\lceil\frac{n}{2}\rceil, \lceil\frac{m}{2}\rceil)$, więc cały algorytm ma
złożoność $O(nm (n+m))$.

Wracając do możliwości jednego błędu na iterację. Okazuje się że jeżeli wystąpił conajwyżej jeden błąd, również
możemy go znaleźć. Próbujemy odtworzyć poprzedni stan zakładając że błędu nie było, a jeżeli błąd był, to w pewnym miejscu
algorytm każe nam wpisać żywą komórkę w miejscu które powinno być poza otaczającym prostokątem poprzedniego stanu.
Możemy w ten sposób wykryć w którym wierszu wystąpił błąd, a robiąc to samo kolumna po kolumnie, w której kolumnie. 
Znając wiersz i kolumnę w której wystąpił błąd, możemy go naprawić.


Dokładny algorytm odtwarzania poprzedniego stanu jest opisany w omówieniu oryginalnego zadania (G): 
[https://icpc.global/worldfinals/problems/2017-ICPC-World-Finals/finals2017solutions.pdf](https://icpc.global/worldfinals/problems/2017-ICPC-World-Finals/finals2017solutions.pdf).
