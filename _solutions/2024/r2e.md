---
layout: solution
title: Kumkwaty
edition: 2024
round: 2
level: e
sol_author: Maciej Wiśniewski
statement: https://szkopul.edu.pl/problemset/problem/22zn-64CqHK92GMijnMOE_eP/site/
video: https://youtu.be/oMfDh98RN5w?t=2070
---

### Nazewnictwo

Niech kupki o nieparzystych numerach będą czarne, a te o parzystych białe.
Niech $B$ będzie sumą wszystkich kupek kumkwatów o nieparzystych numerach,
a $W$ sumą wszystkich kupek kumkwatów o parzystych numerach.

### Parzyste $n$

Pierwszy gracz może zagwarantować sobie wynik $B$ oraz $W$ biorąc odpowiednio pierwszą,
lub ostatnią kupkę. Natomiast drugi gracz może zagwarantować sobie wynik $B$ jeżeli pierwszy
zaczął od białej kupki (idąc w prawo o ile się da), lub $W$ jeżeli zaczął od czarnej (idąc w lewo o ile się da).
Zatem pierwszy gracz zdobędzie $\max(B,W)$ kumkwatów.

### Nieparzyste $n$

Pierwszy gracz może zagwarantować sobie wynik $B$ oraz drugi gracz może zagwarantować wynik $W$ jeżeli pierwszy zacznie
od czarnej kupki (analogicznie do parzystego $n$). Sytuacja wygląda inaczej jeżeli pierwszy gracz zacznie od białej kupki.

Gdy pierwszy gracz wybiera białą kupkę, drugi gracz wybiera czy idzie na lewo czy na prawo od niej. Wszystkie białe kupki po 
wybranej przez drugiego gracza stronie zabierze pierwszy gracz, a wszystkie czarne drugi. Gdy gracze dojdą do początku lub końca kupek,
w grze znowu zostanie spójny podciąg nieparzystej liczby kupek i będzie kolej pierwszego gracza.

Gra będzie przebiegała według następującego schematu:

1. Gracz 1 wybiera czy chce z pozostałych kupek zabrać $B$ i skończyć grę, czy próbuje osiągnąć więcej biorąc białą kupkę.
2. Jeżeli gracz 1 wybrał białą kupkę, to gracz 2 wybiera jedną ze stron (lewa/prawa) tej kupki.
3. Z wybranej przez gracza 2 strony, gracz 1 zabiera wszystkie białe kupki, a gracz 2 wszystkie czarne.
4. Wróć do punktu 1.

Strategia pierwszego gracza może być opisana drzewem binarnym. Korzeniem drzewa jest biała kupka którą gracz 1 bierze w pierwszym ruchu.
Jeżeli wierzchołek ma lewego syna, to jest to biała kupka którą gracz 1 bierze jeżeli w poprzednim wierzchołku gracz 2 zdecydował się
pójść w lewo. Jeżeli wierzchołek nie ma lewego syna, oznacza to że jeżeli gracz 2 zdecyduje się pójść w lewo, gracz 1 zakończy grę biorąc
pozostałe czarne kupki. Analogicznie dla prawego syna.

Zauważmy że pierwszy gracz zawsze weźmie wszyskie $k$ białych kupek którym odpowiadają pewne wierzchołki w drzewie. Te białe kupki
podzielą cały ciąg na $k+1$ spójnych fragmentów, z których we wszystkich poza ostatnim gracz 1 dostanie wszystie białe kupki, a gracz 2 wszystkie czarne.
Wybór tego "ostatniego" fragmentu należy do gracza 2.

Pierwszy gracz dostanie więc $W + X$ kumkwatów, gdzie $X$ to różnica $W - B$ w ostatnim fragmencie. Zatem gracz 1 powinien tak wybrać
$k$ oraz $k$ białych kupek, aby minimum tej różnicy ze wszystkich fragmentów między wybranymi kupkami było maksymalne. Da się policzyć
optymalną strategię gracza 1, robiąć wyszukiwanie binarne po wyniku po $X$ oraz sprawdzenie czy dany $X$ jest osiągalny liniowoym dynamikiem.
Ostateczna złożoność: $O(n\log(n))$ 
