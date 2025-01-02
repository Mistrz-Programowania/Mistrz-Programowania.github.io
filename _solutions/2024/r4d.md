---
layout: solution
title: Robociki w paczce
edition: 2024
round: 4
level: d
author: Tomasz Kwiatkowski
tags:
  - graphs
  - dp
statement: https://szkopul.edu.pl/problemset/problem/Hsp583jBs0FgHjj25z-4BN_0/site/
video: https://youtu.be/8xexhyiKHsQ&t=1708
---

Niech robociki będą wierzchołkami, a zależności między nimi krawędziami w grafie.
Każdy wierzchołek ma pewną wartość -- poziom umiejętności danego robocika.

### Obserwacja

Każdą spójną składową bierzemy w całości albo wcale.
Inaczej, robociki przestaną działać i sumaryczny poziom umiejętności wyniesie 0.

### Przekształcenie problemu

Każda spójna składowa charakteryzuje się dwoma wartościami:
liczbą wierzchołków oraz sumą wartości w wierzchołkach.
Wyobraźmy sobie, że *wagą* spójnej jest liczba jej wierzchołków,
a *wartością* -- suma wartości jej wierzchołków.
Możemy to policzyć dowolnie przechodząc się po spójnych składowych grafu (np. DFS'em)

Chcemy teraz wziąć spójne o sumarycznej masie nieprzekraczającej $k$,
w taki sposób, aby suma ich wartości była najwyższa możliwa.

### Rozwiązanie problemu

Przekształciliśmy problem z zadania do klasycznego problemu plecakowego.
Możemy go rozwiązać korzystając z programowania dynamicznego.

Otrzymamy rozwiązanie w złożoności $O(nk + m)$.
