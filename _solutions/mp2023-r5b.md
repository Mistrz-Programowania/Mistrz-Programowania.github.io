---
layout: solution
title: Dłuższy dzień
edition: 2023
round: 5
level: b
author: Daniel Olkowski
tags:
  - implementation
statement: https://szkopul.edu.pl/problemset/problem/Kz45VU4-hb8V8ofkbm8aQtEJ/site/
---

Przede wszystkim musimy sprawdzić w jakiej fazie roku jesteśmy:

1. Mamy najkrótszy dzień w roku
2. Kolejny dzień będzie dłuższy
3. Kolejny dzień będzie krótszy

Dla 1.
Wypisujemy odpowiedni komunikat: "NOC SWIETOJANSKA"

Dla 2.
Wypisujemy kolejny dzień.
 
Dla 3.
Kolejny najkrótszy dzień będzie symetryczny względem 21 grudnia. Musimy obliczyć ile jest dni do 21 grudnia. Jeśli do 31 grudnia jest 5 dni, to najbliższy dłuższy dzień będzie szóstym dniem po 21 grudnia.

Jak to zrobić? Najwygodniej operować na numerach dniach w roku zamiast na miesiącach i dniach miesiąca.

## Przykładowe implementacje

### C++

```cpp
#include <iostream>
#include <string>
using namespace std;

int days[12] = {31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
string month_labels[12] = {
    "styczen",
    "luty",
    "marzec",
    "kwiecien",
    "maj",
    "czerwiec",
    "lipiec",
    "sierpien",
    "wrzesien",
    "pazdziernik",
    "listopad",
    "grudzien",
};

int date_to_num(int day, string month) {
    int res = day;
    for (int i = 0; month_labels[i] != month; ++i)
        res += days[i];
    return res;
}

string num_to_date(int num) {
    num = (num - 1) % 366 + 1;
    int i = 0, sum = 0;
    for (; sum + days[i] < num; ++i)
        sum += days[i];
    return to_string(num - sum) + " " + month_labels[i];
}

const int LONGEST = date_to_num(21, "czerwiec");
const int SHORTEST = date_to_num(21, "grudzien");

int main() {
    int day;
    string month;
    cin >> day >> month;
    int num = date_to_num(day, month);
    if (LONGEST == num) {
        cout << "NOC SWIETOJANSKA!\n";
    } else if (LONGEST < num && num < SHORTEST) {
        cout << num_to_date(2 * SHORTEST - num + 1) << '\n';
    } else {
        cout << num_to_date(num + 1) << '\n';
    }
    return 0;
}
```

### Python

```py
days = [31, 29, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
month_labels = [
    "styczen",
    "luty",
    "marzec",
    "kwiecien",
    "maj",
    "czerwiec",
    "lipiec",
    "sierpien",
    "wrzesien",
    "pazdziernik",
    "listopad",
    "grudzien",
]


def date_to_num(date):
    day, month = date.split()
    return sum(days[: month_labels.index(month)]) + int(day)


def num_to_date(num):
    num = (num - 1) % sum(days) + 1
    month_nr = 0
    while sum(days[: month_nr + 1]) < num:
        month_nr += 1
    return f"{str(num - sum(days[: month_nr]))} {month_labels[month_nr]}"


LONGEST = date_to_num("21 czerwiec")
SHORTEST = date_to_num("21 grudzien")


def main():
    date = input()
    num = date_to_num(date)
    if LONGEST == num:
        print("NOC SWIETOJANSKA!")
    elif LONGEST < num < SHORTEST:
        print(num_to_date(2 * SHORTEST - num + 1))
    else:
        print(num_to_date(num + 1))


main()
```
