---
layout: solution
title: Prostopadłościan
edition: 2025
round: 5
level: d
author: Maciej Wiśniewski
tags:
statement: https://szkopul.edu.pl/problemset/problem/cIINvTBirsKgMQhz8aZO2TI6/site/
---

Do rozwiązania zadania użyjemy techniki "grafu warstwowego", to znaczy że zamiast patrzeć
tylko na oryginalną planszę, będziemy patrzeć na jej 3 kopie (warstwy):

- Plansza zakładając że klocek stoi pionowo
- Plansza zakładając że klocek leży "lewo-prawo"
- Plansza zakładając że klocek leży "przód-tył"

W takiej reprezentacji, ruch klocka nie tylko zmienia nie tylko jego pozycję na planszy, ale też warstwę
w której się znajdujemy, czyli tak naprawdę poruszamy się po jednym ale 3 razy większym grafie, gdzie
wierzchołkiem jest para (miejsce na planszy, warstwa), a metą jest wierzchołek (Meta, warstwa pionowa).

Reszta to szczegóły implementacyjne, ja dla uproszczenia otoczyłem całą planszę dwa razy polami "dziura",
dzięki czemu nie trzeba było sprawdzać czy nie wychodzę za krawędź planszy, oraz przyjąłem konwencję, że
pole na którym stoi klocek to pole na którym jest jego lewy górny róg (w przypadku gdy leży na dwóch polach).

## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;
#define VERTICAL 0
#define FRONT_BACK 1
#define LEFT_RIGHT 2

#define MAX_N 1000
#define MAX_LAYERS 3

int dist[MAX_N + 7][MAX_N + 7][MAX_LAYERS];
int n, m;
vector<string> grid;

struct node {
    int x, y; // coordinates of the top left corner
    int layer; // 0 = vertical, 1 = front-back, 2 = left-right

    node(int x_, int y_, int layer_) : x(x_), y(y_), layer(layer_) {}

    void prt() {
        cerr << x << " " << y << " " << layer << endl;
    }

    bool legal() {
        if (layer == VERTICAL) {
            return grid[x][y] == 'S' || grid[x][y] == 'M' || grid[x][y] == '.';
        }

        int xx = x + (int)(layer == FRONT_BACK);
        int yy = y + (int)(layer == LEFT_RIGHT);

        if (grid[x][y] == '#') return false;
        if (grid[xx][yy] == '#') return false;

        bool has_floor = false;
        if (grid[x][y] == 'S' || grid[x][y] == '.') has_floor = true;
        if (grid[xx][yy] == 'S' || grid[xx][yy] == '.') has_floor = true;

        return has_floor;
    }

    int d() {
        return dist[x][y][layer];
    }

    void set_d(int d) {
        dist[x][y][layer] = d;
    }

    vector<node> adj() {
        vector<node> all_adj, legal_adj;

        if (layer == VERTICAL) {
            all_adj.push_back(node(x - 2, y, FRONT_BACK));
            all_adj.push_back(node(x + 1, y, FRONT_BACK));
            all_adj.push_back(node(x, y - 2, LEFT_RIGHT));
            all_adj.push_back(node(x, y + 1, LEFT_RIGHT));
        }

        if (layer == FRONT_BACK) {
            all_adj.push_back(node(x - 1, y, VERTICAL));
            all_adj.push_back(node(x + 2, y, VERTICAL));
            all_adj.push_back(node(x, y - 1, FRONT_BACK));
            all_adj.push_back(node(x, y + 1, FRONT_BACK));
        }

        if (layer == LEFT_RIGHT) {
            all_adj.push_back(node(x - 1, y, LEFT_RIGHT));
            all_adj.push_back(node(x + 1, y, LEFT_RIGHT));
            all_adj.push_back(node(x, y - 1, VERTICAL));
            all_adj.push_back(node(x, y + 2, VERTICAL));
        }

        for (node v : all_adj) {
            if (v.legal()) {
                legal_adj.push_back(v);
            }
        }

        return legal_adj;
    }
};

void input() {
    cin >> n >> m;

    grid.resize(n + 4);
    grid[0] = string(m + 4, '-');
    grid[1] = string(m + 4, '-');
    grid[n + 2] = string(m + 4, '-');
    grid[n + 3] = string(m + 4, '-');

    for (int i = 0; i < n; i++) {
        string s;
        cin >> s;
        grid[i + 2] = "--" + s + "--";
    }

    n += 4;
    m += 4;
}

node find_start() {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < m - 1; j++) {
            if (grid[i][j] == 'S') {
                if (grid[i][j + 1] == 'S') {
                    return node(i, j, LEFT_RIGHT);
                } else if (grid[i + 1][j] == 'S') {
                    return node(i, j, FRONT_BACK);
                } else {
                    return node(i, j, VERTICAL);
                }
            }
        }
    }
    return node(-1, -1, -1);
}

node find_finish() {
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < m - 1; j++) {
            if (grid[i][j] == 'M') {
                return node(i, j, VERTICAL);
            }
        }
    }
    return node(-1, -1, -1);
}

void bfs(node v) {
    queue<node> q;

    v.set_d(1);
    q.push(v);

    while (!q.empty()) {
        v = q.front();
        q.pop();

        for (node u : v.adj()) {
            if (u.d() == 0) {
                u.set_d(v.d() + 1);
                q.push(u);
            }
        }
    }
}

int main() {
    input();
    bfs(find_start());
    cout << (find_finish().d() - 1) << endl;
}

```
