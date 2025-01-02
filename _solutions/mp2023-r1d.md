---
layout: solution
title: Bez kontekstu
edition: 2023
round: 1
level: d
author: Maciej Wiśniewski
tags:
  - strings
statement: https://szkopul.edu.pl/problemset/problem/YUOgYI-OUv9BndBQytRYu_9R/site/
---

Aby 2 słowa były anagramami wystarczy żeby miały tyle samo każdej litery.
Sprwadźmy zatem ile razy każda litera występuje w szukanym fragmencie, a potem ile
razy występuje w pierwszych $m$ literach tekstu i zapiszmy to odpowiednio w tablicach
$A[26]$ i $B[26]$. Jeżeli te tablice będą się zgadzać, to dodamy 1 do wyniku, jeżeli nie, 
to nic nie dodamy.

Teraz spróbujmy pójść o 1 literę dalej w głównym tekście. Wszystkie litery będą takie same,
z dwoma wyjątkami, pierwsza litera nam odpadnie oraz ostatnia dojdzie nowa. Musimy więc
zmienić tylko 2 wartości w tablicy $B$ i znowu dodajemy 1 jeżeli $A = B$.

To Rozwiązanie działa w czasie $O(n\cdot \omega)$, gdzie $\omega$ to rozmiar alfabetu czyli $26$, 
ponieważ co krok musimy porównać tablice $A$ i $B$. Ale zauważmy, że tablica $A$ nie zmienia się wcale,
a tablica $B$ tylko w dwóch miejscach. Możemy więc trzymać sobie liczbę liter których jest tyle samo w $A$
co w $B$ i ją aktualizować. Jeżeli wcześniej $A[i] = B[i]$, ale $B[i]$ się zmieniło, to jedna litera mniej
będzie się zgadzać. Analogicznie jeżeli wcześniej $A[i] \neq B[i]$ ale teraz $B[i]$ się zmieniło i już się zgadza.
Wtedy dodajemy do wyniku 1, jeżeli widzimy że zgadza się 26 liter. To rozwiązanie działa w $O(n)$.
