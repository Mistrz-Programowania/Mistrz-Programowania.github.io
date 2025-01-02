---
layout: solution
title: Puzzle
edition: 2023
round: 2
level: d
author: Maciej Wiśniewski
tags:
  - math
  - nt
statement: https://szkopul.edu.pl/problemset/problem/rZa6uMvtCyNIUH1_U0GNM8SU/site/
---

Najlepszym rozwiązaniem będzie ułożyć z puzzli prostokąt który będzie jak najbardziej przypominał 
kwadarat. Ponieważ ten prostokąt musi być pełny, to jego boki muszą być dzielnikami $n$. Zatem musimy znaleźć
największy dzielinik $n$ który jest nie większy niż $\sqrt{n}$. Możemy zrobić sito Eratostenesa do $MAXN$,
z takim wyjątkiem że nie będziemy pomijać liczb złożonych. W takim sicie zamiast trzymać informacji czy liczba
jest pierwsza będziemy trzymać informację o największym dzielniku tej liczby, który jest mniejszy niż jej pierwiastek.
Ponieważ nie pomijamy liczb złożonych, to napewno dla każdej liczby znajdziemy najlepszą wynik, a złożoność takiego 
rozwiązania to $O(T + MAXN \cdot ln(MAXN))$.

## Uwagi

- Jeżeli ktoś jest zainteresowany dowodem złożoności takiego sita, wpiszcie w Google hasło "szereg harmoniczny".
