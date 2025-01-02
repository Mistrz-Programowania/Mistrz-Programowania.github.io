---
layout: solution
title: Bajtek nie lubi się powtarzać
edition: 2023
round: 4
level: e
sol_author: Maciej Wiśniewski
tags:
  - implementation
  - greedy
  - sortings
  - strings
statement: https://szkopul.edu.pl/problemset/problem/X8O2Ff_d5gx9_3LaO8OVheVB/site/
---

Zauważmy, że są 3 możliwe najlepsze powtarzalności: 
1. jeżeli wszystkie litery są takie same, to słowo będzie miało powtarzalność $n-1$ i nie możemy nic zmienić. (np. "aaaaaaaaa")
2. jeżeli istnieje chociaż jedna litera która występuje w tym słowie dokładnie raz, to możemy ją dać na pierwsze 
miejsce i powtarzalność będzie równa 0. Możemy więc posortować leksykograficznie resztę słowa. (np. "caabbefg")
3. w przeciwnym wypadku istnieje przynajmniej jedna litera która występuje conajmniej 2, ale nie więcej niż $n/2 + 1$ razy. 
Możemy więc dać 2 te litery na początek, a potem na zmianę tą literę i jakąś inną, otrzymując powtarzalność 1.

Jedyny nietrywialny przypadek to ten trzeci, możemy go rozbić na 3 mniejsze przypadki:

1. jeżeli najmniejsza leksykograficznie litera występująca w słowie nie występuje więcej niż $n/2 + 1$ razy to dajemy 2 te litery
na początek, resztę sortujemy leksykograficznie i dajemy na zmianę inną literę i tą najmniejszą aż się skończą. (np. "aabacadadefg")
2. w przeciwnym wypadkum, jeżeli w całym słowie występują dokładnie 2 różne litery, dajemy jedną najmniejszą leksykograficznie literę na początek, 
potem wszystkie drugiego rodzaju i na końcu resztę tego pierwszego. (np. "abbbbbaaaaaaaa").
3. w przeciwnym wypadku, dajemy jedną najmniejszą leksykograficznie literę na początek, potem jedną drugą najmniejszą, potem resztę
pierwszej i na końcu całą resztę posortowaną leksykograficznie. (np. "abbbbbaaaaaaaa").
