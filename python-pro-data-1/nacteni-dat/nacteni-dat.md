## Základní práce s DataFrame

V Pandas většinou pracujeme s datovou strukturou zvanou `DataFrame`. Je to tabulková datová struktura a funguje podobně jako tabulka v Excelu nebo v databázi. Můžeme jej považovat za další datový typ vedle slovníků a seznamů. `DataFrame` obsahuje data ve sloupcích, kde každý sloupec může mít různý datový typ, tedy například číslo, desetinné číslo, řetězec, pravdivostní hodnota a jiné.

**Poznámka:** Pokud znáš základy objektově orientovaného programování, pak věz, že `DataFrame` je ve skutečnosti třída a my na jejím základě budeme vytvářet objekty.

Abychom si práci s DataFrame vyzkoušeli, vrátíme se k naší tabulce se seznamem nákupů.

| Jméno   | Datum      | Věc              | Částka v korunách |
|:--------|:-----------|:-----------------|-----:|
| Petr    | 2020-02-05 | Prací prášek     |  399 |
| Ondra   | 2020-02-08 | Savo             |   80 |
| Petr    | 2020-02-24 | Toaletní papír   |   65 |
| Libor   | 2020-03-05 | Pivo             |  124 |
| Petr    | 2020-03-18 | Pytel na odpadky |   75 |
| Míša    | 2020-03-30 | Utěrky na nádobí |  130 |
| Ondra   | 2020-04-22 | Toaletní papír   |  120 |
| Míša    | 2020-05-05 | Pečící papír     |   30 |
| Zuzka   | 2020-06-05 | Savo             |   80 |
| Pavla   | 2020-06-13 | Máslo            |   50 |
| Ondra   | 2020-07-25 | Káva             |  300 |

### Načítání dat

Tabulku výše si můžete stáhnout ve [formátu CSV](assets/nakupy.csv). Důležité je, že si soubor musíš uložit nebo zkopírovat do **stejného adresáře**, v jakém právě pracuješ ve Visual Studiu! To si ověříš pomocí příkazu `dir` ve Windows nebo `ls` v MacOS nebo Linuxu. Tento příkaz ti vypíše obsah aktuální adresáře. V přehledu souborů bys měla vidět soubor `nakupy.csv`.

Abychom tabulku načetli jako `DataFrame`, vytvoříme si nový Python skript, importujeme modul `pandas` a načteme CSV soubor pomocí funkce `read_csv().`

```py
import pandas
nakupy = pandas.read_csv('nakupy.csv')
print(nakupy)
```

**Poznámka:** Modul `pandas` nabízí obrovské množství možností. Nemusíš si samozřejmě vše pamatovat, protože vše najdeš přehledně popsáno [v dokumentaci](https://pandas.pydata.org/docs/). Například funkce `read_csv` je [popsána zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html). Dokumentaci k samotnému DataFrame najdeš [zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html).

Funkce `read_csv` má spoustu nepovinných parametrů, o kterých si můžeme přečíst [v dokumentaci](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html). Například se tam dočteme, že `pandas` standardně nastavuje jako oddělovač sloupců čárku (parametr `sep`). Protože my většinou používáme středník, budeme muset tento parametr často nastavit. Náš soubor `nakupy.csv` ale používá čárku, takže nyní nic měnit nemusíš.

Celý DataFrame vypíšeme na obrazovku pomocí funkce `print()`.

```shell
    jmeno       datum               vec  cena
0    Petr  2020-02-05      Prací prášek   399
1   Ondra  2020-02-08              Savo    80
2    Petr  2020-02-24    Toaletní papír    65
3   Libor  2020-03-05              Pivo   124
4    Petr  2020-03-18  Pytel na odpadky    75
5    Míša  2020-03-30  Utěrky na nádobí   130
6   Ondra  2020-04-22    Toaletní papír   120
7    Míša  2020-05-05      Pečící papír    30
8   Zuzka  2020-06-05              Savo    80
9   Pavla  2020-06-13             Máslo    50
10  Ondra  2020-07-25              Káva   300
```

Všimni si, že `pandas` nám přidal nový sloupec s číslem řádku. Jedná se o **index**, se kterým budeme později pracovat. Index je hodnota, která identifikuje řádek. V některých případech nemusíme jako index používat číslo řádku, ale můžeme jako index vybrat některý ze sloupců. Obdobnou funkci má v databázích **primární klíč**. Jako *best practice* se většinou uvádí, že index by měl být **unikátní**, i když to `pandas` (na rozdíl od právě databází) nevyžadují. Mohli bychom si tedy jako index zvolit například sloupec `Jmeno`, ale tím bychom si zadělávali na problém do budoucna (například v tom, že by práce s `DataFrame` byla [pomalejší](https://stackoverflow.com/q/16626058/4693904)).

Pandas nabízí kromě funkce `read_csv()` také funkci pro čtení formátu JSON `read_json()` nebo dokonce funkci pro čtení přímo Excelových tabulek `read_excel()`.

### Základní informace o tabulce

Jakmile máme tabulku načtenou, budeme o ní chtít vědět nějaké úplně základní údaje. K tomu nám pomůže metoda `info()`, která vrací souhrnné informace o celé tabulce: názvy sloupců, datové typy, počet neprázdných hodnot atd.

```py
import pandas
nakupy = pandas.read_csv('nakupy.csv')
nakupy.info()
```

```shell
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 11 entries, 0 to 10
Data columns (total 4 columns):
 #   Column  Non-Null Count  Dtype
---  ------  --------------  -----
 0   jmeno   11 non-null     object
 1   datum   11 non-null     object
 2   vec     11 non-null     object
 3   cena    11 non-null     int64
dtypes: int64(1), object(3)
memory usage: 480.0+ bytes
```

Základní statistické veličiny u číselných hodnot získáme metodou `describe()`:
```py
nakupy.describe()
```

```shell
             cena
count   11.000000
mean   132.090909
std    114.017064
min     30.000000
25%     70.000000
50%     80.000000
75%    127.000000
max    399.000000
```

Počet řádků a sloupců můžeme získat z vlastnosti `shape`:

```py
print(nakupy.shape)
```

```shell
(11, 4)
```

**Poznámka:** Pokud znáš základy objektově orientovaného programování, pak věz, že `info` je ve skutečnosti funkce třídy `DataFrame`.

`pandas` nám vrací výsledky v sekvenci, která se jmenuje `tuple`. Nám stačí vědět, že si z ní data můžeme načíst stejně jako ze seznamu. Na prvním místě je vždy počet řádků a na druhém počet sloupců. Pokud by nás třeba zajímal jen počet řádků, napíšeme:

```py
print(nakupy.shape[0])
```

```shell
11
```

Názvy všech sloupců pak z vlastnosti `columns`:

```py
print(nakupy.columns)
```

```shell
Index(['jmeno', 'datum', 'vec', 'cena'], dtype='object')
```

### Začátek a konec

Na prvních a posledních několik řádků se chceme podívat často, hlavně v případě, když moc dobře neznáme strukturu dat. Můžeme k tomu použít metody `head` a `tail`.

```py
print(nakupy.head())
```

```shell
   jmeno       datum               vec  cena
0   Petr  2020-02-05      Prací prášek   399
1  Ondra  2020-02-08              Savo    80
2   Petr  2020-02-24    Toaletní papír    65
3  Libor  2020-03-05              Pivo   124
4   Petr  2020-03-18  Pytel na odpadky    75
```

Často je užitečné podívat se spíše na konec souboru. Pokud jsou data seřazená podle času, uvidíme na konci souboru nejnovější data, která nás často (např. u kurzu měn nebo akcií) zajímají víc než dávná historie.

```py
print(nakupy.tail())
```

```shell
    jmeno       datum             vec  cena
6   Ondra  2020-04-22  Toaletní papír   120
7    Míša  2020-05-05    Pečící papír    30
8   Zuzka  2020-06-05            Savo    80
9   Pavla  2020-06-13           Máslo    50
10  Ondra  2020-07-25            Káva   300
```
