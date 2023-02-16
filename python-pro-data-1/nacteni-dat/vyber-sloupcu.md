## Výběr sloupců

V některých případech nás jako první při práci s daty napadne nějak si data zjednodušit. Například budeme chtít v DataFrame vybrat pouze některé sloupce, a to co nás nezajímá, můžeme zahodit.

K tomu použijeme výběr sloupců pomocí hranatých závorek. Zápis připomíná práci se seznamy - hranatou závorku napíšeme přímo za název proměnné, kde máme uložený `DataFrame`, a do ní vepíšeme název sloupce, který nás zajímá.

```py
print(nakupy['vec'])
```

```shell
0         Prací prášek
1                 Savo
2       Toaletní papír
3                 Pivo
4     Pytel na odpadky
5     Utěrky na nádobí
6       Toaletní papír
7         Pečící papír
8                 Savo
9                Máslo
10                Káva
Name: vec, dtype: object
```

Zde je důležité říct, že pokud vybíráme pouze jeden sloupec, vrátí se nám takzvaná **Série** (`Series`), což je jiný datový typ než DataFrame. Sérii si představme jako jednorozměrnou tabulku.

Pro výběr více sloupců musíme do indexace DataFrame vložit seznam s názvy sloupců.

```py
print(nakupy[['jmeno', 'cena']])
```

```shell
    jmeno  cena
0    Petr   399
1   Ondra    80
2    Petr    65
3   Libor   124
4    Petr    75
5    Míša   130
6   Ondra   120
7    Míša    30
8   Zuzka    80
9   Pavla    50
10  Ondra   300
```

Tady se nám již vrátil datový typ DataFrame. Tohoto triku můžeme využít, když chceme získat pouze jeden sloupec, ale nechceme ho v datovém typu Série, ale jako DataFrame.

```py
print(nakupy[['vec']])
```

```shell
                 vec
0       Prací prášek
1               Savo
2     Toaletní papír
3               Pivo
4   Pytel na odpadky
5   Utěrky na nádobí
6     Toaletní papír
7       Pečící papír
8               Savo
9              Máslo
10              Káva
```
