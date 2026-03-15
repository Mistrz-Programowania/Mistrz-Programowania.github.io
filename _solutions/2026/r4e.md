---
layout: solution
title: Zamiatanie
edition: Mistrz Programowania 2026
round: 4
level: E
author: Krzysztof Witkowski
tags:
  - matematyka, modulo
statement: https://szkopul.edu.pl/problemset/problem/35KFBwC_lkq3RPaTgBX80aIN/site/?key=statement
---

Zastanówmy się dla danego elementu, kiedy będzie on na *prawie dobrej pozycji*. *Prawie dobra pozycja* to taka, że wszystkie elementy mniejsze od niego są na lewo od niego. Możemy zauważyć, że jeśli uda nam się policzyć czas potrzebny na ustawienie elementu na *prawie dobrej pozycji* i weźmiemy maksimum z tych czasów, to uzyskamy czas potrzebny na faktyczne posortowanie permutacji.

Teraz zastanówmy się, jak policzyć ten czas dla danego elementu. Wykorzystamy częsty trik polegający na zastąpieniu elementów mniejszych od rozważanego zerami, a większych jedynkami. Wtedy to, co robi operacja U, to przesunięcie pierwszej jedynki na koniec ciągu, a operacja D przesuwa ostatnie zero na początek.

Z tego wynika bardzo ważny fakt - kolejność wykonywania operacji nie ma znaczenia, bo jeśli użyję *x* razy operacji U i *y* razy operacji D, to niezależnie od kolejności pierwsze *x* jedynek przesuwa się na koniec i ostatnie *y* zer przesuwa się na początek.

### Rozwiązanie wolne $O(n^2*m)$

Możemy po prostu dla każdej ilości U i D w ciągu operacji (których jest maksymalnie $O(m)$) zasymulować w dowolnej kolejności (zgodnie z obserwacją wyżej) kolejne ciągi operacji, dopóki permutacja nie jest posortowana. Jeśli okaże się, że potrzeba *x* użyć ciągu operacji, to w tablicy wynikowej w komórce *x* dodajemy $\binom{#?}{#U}$, gdzie #U to liczba dodanych U poprzez zmianę znaku ? na U. Posortowanie może wymagać nawet n iteracji, dlatego złożoność to $O(n^2*m)$.

### Rozwiązanie wzorcowe wolne $O(n*m*log)$

Możemy dalej wykorzystać obserwację o przesuwaniu pierwszej jedynki i ostatniego zera. Chcemy dla dowolnej ilości operacji U i D liniowo stwierdzić, czy permutacja będzie posortowana. Jeśli nam się to uda, to możemy użyć wyszukiwania binarnego dla każdej ilości U i D w ciągu operacji, by znaleźć liczbę potrzebnych iteracji.

Załóżmy, że chcemy sprawdzić, czy ciąg będzie posortowany po *x* operacjach U i *y* operacjach D. Warunek dla danego elementu to wtedy max(pozycja elementu, pozycja *x* jedynki) > pozycja *y* zera od końca. Ten max wynika z tego, że jeśli nie ma jedynki przed rozważanym elementem, to przesuwamy go do następnej jedynki na prawo. Problem znajdowania k-th większego lub mniejszego elementu możemy rozwiązać liniowo, idąc 2 razy w kolejności posortowanej po wartości, trzymając jakiś wskaźnik na aktualną k-tą jedynkę/zero.

### Rozwiązanie bonusowe $O(n*m)$

Istnieje wiele podejść do tego, zazwyczaj prostszych od tego co tu pokażę, ale ono też nie jest złe.

Zamiast wyszukiwania binarnego możemy zastosować inne podejście. Trzymamy jakąś zmienną *wynik* i przechodzimy w jakiejś kolejności po elementach permutacji. W danym elemencie wykonujemy następującą procedurę: dopóki warunek dla niego nie jest spełniony przy aktualnym *wyniku* -> zwiększ *wynik* o jeden. Zwiększeń będzie maksymalnie $O(n)$, więc jeśli sprawdzenie, czy warunek jest spełniony, i zwiększanie *wyniku* o 1 wykonamy w amortyzowanym czasie $O(n)$, to rozwiążemy to zadanie w $O(n*m)$.

Będziemy iterowali się po elementach w kolejności rosnącej. Będziemy utrzymywali dwa wskaźniki na #U * *wynik* jedynkę i #D * *wynik* zero od końca. Wtedy potrzebujemy umieć robić następujące operacje:

- Dodaj element do zbioru zer.
- Usuń element ze zbioru jedynek.
- Przesuń wskaźnik jedynek do następnej jedynki.
- Przesuń wskaźnik zer do następnego zera.
- Przesuń wskaźnik zer do poprzedniego zera.

Te operacje nam wystarczają, bo zwiększenie *wyniku* to po prostu przesunięcie obu wskaźników do przodu o #U i #D pozycji, a zmiana elementu z jedynki na zero to ewentualne przesunięcie wskaźnika jedynek o jeden do przodu i wskaźnika zer o jeden do tyłu.

Strukturą, która pozwoli nam na takie operacje, jest lista, gdzie każdy element trzyma wskaźnik do następnego i poprzedniego. Usuwanie z takiej listy jest proste, bo wiemy, jakie wskaźniki trzeba poprzestawiać. Małym problemem jest dodawanie, gdzie w klasycznej liście trzeba przejść liniowo po niej, by znaleźć poprzedni i następny element od dodawanego, ale w tym przypadku dzięki temu, że kolejność dodań i usunięć jest cały czas taka sama, możemy na początku policzyć dla każdego elementu wartości w stylu następny i poprzedni mniejszy od niego, by od razu wiedzieć, jakie wskaźniki trzeba pozmieniać w tej liście. Ten następny i poprzedni mniejszy możemy policzyć kolejką monotoniczną.

Podczas trzymania wskaźników trzeba uważać na to, aby nie wyjść poza tablicę. Można np. trzymać wartość *jak bardzo wskaźnik wyszedł już poza tablicę*, czyli ile razy chciałem wykonać operację, która by to spowodowała.


## Przykładowe implementacje

### C++

```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
#define ll long long
#define ld long double
#define ull unsigned long long
#define ff first
#define ss second
#define pii pair<int,int>
#define pll pair<long long, long long>
#define vi vector<int>
#define vl vector<long long>
#define pb push_back
#define rep(i, b) for(int i = 0; i < (b); ++i)
#define rep2(i,a,b) for(int i = a; i <= (b); ++i)
#define rep3(i,a,b,c) for(int i = a; i <= (b); i+=c)
#define count_bits(x) __builtin_popcountll((x))
#define all(x) (x).begin(),(x).end()
#define siz(x) (int)(x).size()
#define forall(it,x) for(auto& it:(x))
using namespace __gnu_pbds;
using namespace std;
typedef tree<int, null_type, less<int>, rb_tree_tag,tree_order_statistics_node_update> ordered_set;
//mt19937 mt;void random_start(){mt.seed(chrono::time_point_cast<chrono::milliseconds>(chrono::high_resolution_clock::now()).time_since_epoch().count());}
//ll los(ll a, ll b) {return a + (mt() % (b-a+1));}
const int INF = 1e9+50;
const ll INF_L = 1e18+40;
const ll MOD = 998244353;

vi vsort;
int nxt_less[100008];
int prev_less[100008];
int nxt[100008];
int prv[100008];
int cur_one = 0;
int cur_zero = 0;
int cnt_zero = 0;

void add(int x)
{
    nxt[prev_less[x]] = x;
    prv[x] = prev_less[x];
    prv[nxt_less[x]] = x;
    nxt[x] = nxt_less[x];
}

void move_up(int k)
{
    cnt_zero = max(0,cnt_zero-k);
    k -= cnt_zero;
    if(k > 0)
    {
        rep(i,k)
        {
            cur_zero = nxt[cur_zero];
        }
    }
}

void move_down(int k)
{
    if(cur_zero == 0)
    {
        cnt_zero += k;
        return;
    }
    rep(i,k)
    {
        cur_zero = prv[cur_zero];
        if(cur_zero == 0)
        {
            cnt_zero += (k-i);
            return;
        }
    }
}

int solve(vi& arr, int u, int d)
{
    int n = siz(arr)-1;
    rep(i,n+2)
    {
        nxt[i] = n+1;
        prv[i] = 0;
    }
    int ans = 0;
    cur_one = 0;
    cur_zero = 0;
    cnt_zero = 0;
    forall(it,vsort)
    {
        if(it <= cur_one)
        {
            cur_one++;
            while(cur_one <= n && arr[cur_one] <= arr[it]) cur_one++;
        }
        add(it);
        if(it >= cur_zero) move_up(1);
        while(max(it,cur_one) < cur_zero)
        {
            ans++;
            rep(i,u)
            {
                cur_one++;
                while(cur_one <= n && arr[cur_one] <= arr[it]) cur_one++;
                if(cur_one > n) break;
            }
            move_down(d);
        }
    }
    return ans;
}

ll ans[100008];
ll sil[100008];
ll odwsil[100008];

ll P(ll x)
{
    ll ans2 = 1;
    rep(bit,31)
    {
        if((1<<bit)&(MOD-2))
        {
            ans2 *= x;
            ans2 %= MOD;
        }
        x *= x;
        x %= MOD;
    }
    return ans2;
}

ll newton(ll a, ll b)
{
    return (((sil[a]*odwsil[b])%MOD)*odwsil[a-b])%MOD;
}

int main()
{
    ios_base::sync_with_stdio(0);cin.tie(0);cout.tie(0);
    //random_start();
    sil[0] = 1;
    odwsil[0] = 1;
    rep2(i,1,100007)
    {
        sil[i] = (sil[i-1]*i)%MOD;
        odwsil[i] = P(sil[i]);
    }
    int n,m;
    cin >> n >> m;
    string s;
    cin >> s;
    int Dcnt = 0;
    int Ucnt = 0;
    int Pcnt = 0;
    forall(it,s)
    {
        if(it == 'U') Ucnt++;
        if(it == 'D') Dcnt++;
        if(it == '?') Pcnt++;
    }
    vi p(n+2,0);
    rep2(i,1,n) cin >> p[i];
    stack<int> st;
    st.push({0});
    rep2(i,1,n)
    {  
        while(p[st.top()] > p[i]) st.pop();
        prev_less[i] = st.top();
        st.push(i);
    }
    st = {};
    st.push(n+1);
    for(int i = n; i >= 1; i--)
    {
        while(p[st.top()] > p[i]) st.pop();
        nxt_less[i] = st.top();
        st.push(i);
    }
    vector<pii> vsort2;
    rep2(i,1,n) 
    {
        vsort2.pb({p[i],i});
    }
    sort(all(vsort2));
    rep(i,n) 
    {
        vsort.pb(vsort2[i].ss);
    }
    p.pop_back();
    rep2(i,0,Pcnt)
    {
        int D2 = Dcnt+i;
        int U2 = Ucnt+(Pcnt-i);
        ans[solve(p,U2,D2)] += newton(Pcnt,i);
    }
    rep(i,n) cout << ans[i]%MOD << " ";
}
```

### Python

```py
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

MOD = 998244353
MAXN = 100008

nxt_less = [0] * MAXN
prev_less = [0] * MAXN
nxt = [0] * MAXN
prv = [0] * MAXN

cur_one = 0
cur_zero = 0
cnt_zero = 0

def add(x):
    nxt[prev_less[x]] = x
    prv[x] = prev_less[x]
    prv[nxt_less[x]] = x
    nxt[x] = nxt_less[x]
def move_up(k):
    global cur_zero, cnt_zero
    cnt_zero = max(0, cnt_zero - k)
    k -= cnt_zero
    if k > 0:
        for _ in range(k):
            cur_zero = nxt[cur_zero]
def move_down(k):
    global cur_zero, cnt_zero
    if cur_zero == 0:
        cnt_zero += k
        return
    for i in range(k):
        cur_zero = prv[cur_zero]
        if cur_zero == 0:
            cnt_zero += (k - i)
            return
def solve(arr, u, d, vsort):
    global cur_one, cur_zero, cnt_zero
    n = len(arr) - 1
    for i in range(n + 2):
        nxt[i] = n + 1
        prv[i] = 0
    ans = 0
    cur_one = 0
    cur_zero = 0
    cnt_zero = 0
    for it in vsort:
        if it <= cur_one:
            cur_one += 1
            while cur_one <= n and arr[cur_one] <= arr[it]:
                cur_one += 1
        add(it)
        if it >= cur_zero:
            move_up(1)
        while max(it, cur_one) < cur_zero:
            ans += 1
            for _ in range(u):
                cur_one += 1
                while cur_one <= n and arr[cur_one] <= arr[it]:
                    cur_one += 1
                if cur_one > n:
                    break
            move_down(d)

    return ans
sil = [1] * MAXN
odwsil = [1] * MAXN
for i in range(1, MAXN):
    sil[i] = (sil[i - 1] * i) % MOD
    odwsil[i] = pow(sil[i], MOD - 2, MOD)
def newton(a, b):
    return (((sil[a] * odwsil[b]) % MOD) * odwsil[a - b]) % MOD
n, m = map(int, input().split())
s = input().strip()
Dcnt = s.count('D')
Ucnt = s.count('U')
Pcnt = s.count('?')
p = [0] * (n + 2)
p[1:n+1] = map(int, input().split())
st = [0]
for i in range(1, n + 1):
    while p[st[-1]] > p[i]:
        st.pop()
    prev_less[i] = st[-1]
    st.append(i)
st = [n + 1]
for i in range(n, 0, -1):
    while p[st[-1]] > p[i]:
        st.pop()
    nxt_less[i] = st[-1]
    st.append(i)
vsort = sorted(range(1, n + 1), key=lambda x: p[x])
ans = [0] * (n + 1)
for i in range(Pcnt + 1):
    D2 = Dcnt + i
    U2 = Ucnt + (Pcnt - i)
    k = solve(p, U2, D2, vsort)
    ans[k] = (ans[k] + newton(Pcnt, i)) % MOD
print(*ans[:n])
```

## Uwagi

- Link do orginalnego zadania: https://uoj.ac/contest/102/problem/1004
