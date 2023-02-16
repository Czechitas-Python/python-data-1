## Výběr řádků pomocí čísla řádku

Jak už víme, v `pandas` má každý řádek přiřazený index. Jako index můžeme zvolit některý ze sloupců. Pokud však tabulku načteme bez toho, abychom specifikovali index, `pandas` nám vytvoří **číselný index** automaticky. Je to něco podobného jako číslování řádků v Excelu.

K vybrání jednoho konkrétního řádku můžeme použít `iloc[]`. `iloc` nám umožní ptát se na konkrétní záznam podobně jako u sekvencí, jsou zde přítomné i hranaté závorky. `iloc` tedy ve skutečnosti není funkce, ale kromě jiného typu závorek s ní pracujeme jako s funkcí.

Zkusme si zobrazit třeba **čtvrtý** nákup. Číslujeme tradičně od nuly, jistě tě tedy nepřekvapí, že napíšeme `nakupy.iloc[3]`.

```py
print(nakupy.iloc[3])
```

```shell
jmeno         Libor
datum    2020-03-05
vec            Pivo
cena            124
Name: 3, dtype: object
```

Všimni si, že když jsme chtěli pouze jeden řádek, vypsal se nám výsledek jinak orientovaný. Výběr jednoho řádku nám vrátí Sérii stejně jako v případě výběru jediného sloupce. Pohled na tento řádek pak máme orientovaný na výšku.

Metoda `iloc[]` umožňuje pro výběr řádků použít rozsah ve formátu `od:do`. K tomu používáme **dvojtečku**. Před dvojtečku píšeme první řádek, který chceme vypsat a za dvojtečku první řádek, který již vy výpisu nebude. Pokud tedy například napíšeme `nakupy.iloc[3:5]`, získáme řádky s indexy 3 a 4, ale už ne řádek s indexem 5.

```py
print(nakupy.iloc[3:5])
```

```shell
   jmeno       datum               vec  cena
3  Libor  2020-03-05              Pivo   124
4   Petr  2020-03-18  Pytel na odpadky    75
```

Pokud se chceme podívat třeba na první tři řádky, nemusíme před dvojtečku psát 0, stačí napsat `iloc[:3]`.

```py
print(nakupy.iloc[:3])
```

```shell
   jmeno       datum             vec  cena
0   Petr  2020-02-05    Prací prášek   399
1  Ondra  2020-02-08            Savo    80
2   Petr  2020-02-24  Toaletní papír    65
```

Podobně si můžeme nechat vypsat poslední tři řádky. Pokud víme, že řádků je 10, chceme vypsat řádky od osmého dále. Nyní se nabízí napsat číslo před dvojtečku. Píšeme tam ale 8, protože řádek, jehož číslo je před dvojtečkou, je vždy součástí výpisu.

```py
print(nakupy.iloc[8:])
```

```shell
    jmeno       datum    vec  cena
8   Zuzka  2020-06-05   Savo    80
9   Pavla  2020-06-13  Máslo    50
10  Ondra  2020-07-25   Káva   300
```

Nevýhodou postupu je, že si musíme předem zjistit, jak kolik řádků máme. U seznamů už ale existoval trik použití záporného čísla. Ten můžeš použít i v `pandas`. Pokud napíšeš `iloc[-3:]`, získáš též poslední tři řádky.

```py
print(nakupy.iloc[-3:])
```

```shell
    jmeno       datum    vec  cena
8   Zuzka  2020-06-05   Savo    80
9   Pavla  2020-06-13  Máslo    50
10  Ondra  2020-07-25   Káva   300
```

### Výběr řádků a sloupců podle čísla

Kromě řádků si často chceme vybrat jen některé sloupce, protože mnoho tabulek obsahuje spoustu různých informací a ne všechny nás musejí zajímat. Čísla sloupců zadáváme jako druhý parametr funkce `iloc`.

Pokud chceš například vypsat jména u prvních pět nákupů, jako první parametr napiš `:5` a jako druhý `0`.

```py
print(nakupy.iloc[:5,0])
```

```shell
0     Petr
1    Ondra
2     Petr
3    Libor
4     Petr
Name: jmeno, dtype: object
```

U sloupců ale často narazíme na to, že jich chceme několik, ale ony nutně nemusí být vedle sebe. nás u nákupů asi bude nejvíce zajímat jméno a částka. Abychom dali dohromady dvě čísla, která neleží vedle sebe, můžeme použít seznam. Pro prvních pět nákupů tedy jako druhý parametr napíšeme `[0,3]`.

```py
print(nakupy.iloc[:5,[0,3]])
```

```shell
   jmeno  cena
0   Petr   399
1  Ondra    80
2   Petr    65
3  Libor   124
4   Petr    75
```

Pokud bys chtěla vidět všechny řádky, jako první parametr napiš pouze dvojtečku.

```py
print(nakupy.iloc[:,[0,3]])
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
