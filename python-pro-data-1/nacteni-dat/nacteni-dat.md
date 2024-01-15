## Základní práce s DataFrame

V Pandas většinou pracujeme s datovou strukturou zvanou `DataFrame`. Je to tabulková datová struktura a funguje podobně jako tabulka v Excelu nebo v databázi. Můžeme jej považovat za další datový typ vedle slovníků a seznamů. `DataFrame` obsahuje data ve sloupcích, kde každý sloupec může mít různý datový typ, tedy například číslo, desetinné číslo, řetězec, pravdivostní hodnota a jiné.

**Poznámka:** Pokud znáš základy objektově orientovaného programování, pak věz, že `DataFrame` je ve skutečnosti třída a my na jejím základě budeme vytvářet objekty.

Abychom si práci s DataFrame vyzkoušeli, vrátíme se k naší tabulce s potravinami. Protože data jsou poměrně rozsáhlá, použijeme pouze 100 náhodně vybraných řádků, abychom se v datech snáze orientovali.

### Načítání dat

Data si můžete stáhnout v souboru [food_sample_100.csv](assets/food_sample_100.csv). Důležité je, že si soubor musíš uložit nebo zkopírovat do **stejného adresáře**, v jakém právě pracuješ ve Visual Studiu! To si ověříš pomocí příkazu `dir` ve Windows nebo `ls` v MacOS nebo Linuxu. Tento příkaz ti vypíše obsah aktuální adresáře. V přehledu souborů bys měla vidět soubor `food_sample_100.csv`.

Abychom tabulku načetli jako `DataFrame`, vytvoříme si nový Python skript, importujeme modul `pandas` a načteme CSV soubor pomocí funkce `read_csv().`

```py
import pandas as pd
food = pd.read_csv('food_sample_100.csv')
```

**Poznámka:** Modul `pandas` nabízí obrovské množství možností. Nemusíš si samozřejmě vše pamatovat, protože vše najdeš přehledně popsáno [v dokumentaci](https://pandas.pydata.org/docs/). Například funkce `read_csv` je [popsána zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html). Dokumentaci k samotnému DataFrame najdeš [zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.html).

Funkce `read_csv` má spoustu nepovinných parametrů, o kterých si můžeme přečíst [v dokumentaci](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html). Například se tam dočteme, že `pandas` standardně nastavuje jako oddělovač sloupců čárku (parametr `sep`). Protože my většinou používáme středník, budeme muset tento parametr často nastavit. Náš soubor `food_sample_100.csv` ale používá čárku, takže nyní nic měnit nemusíš.

Celý DataFrame vypíšeme na obrazovku pomocí funkce `print()`.

Všimni si, že `pandas` nám přidal nový sloupec s číslem řádku. Jedná se o **index**, se kterým budeme později pracovat. Index je hodnota, která identifikuje řádek. V některých případech nemusíme jako index používat číslo řádku, ale můžeme jako index vybrat některý ze sloupců. Obdobnou funkci má v databázích **primární klíč**. Jako *best practice* se většinou uvádí, že index by měl být **unikátní**, i když to `pandas` (na rozdíl od právě databází) nevyžadují. Na druhou stranu, práce s tabulkou, kde index není unikátní, je [pomalejší](https://stackoverflow.com/q/16626058/4693904). 

My bychom si jako index mohli zvolit sloupec `fdc_id`. To bychom provedli pomocí metody `set_index()`.

```py
food = food.set_index("fdc_id")
```

Pandas nabízí kromě funkce `read_csv()` také funkci pro čtení formátu JSON `read_json()` nebo dokonce funkci pro čtení přímo Excelových tabulek `read_excel()`.

### Základní informace o tabulce

Jakmile máme tabulku načtenou, budeme o ní chtít vědět nějaké úplně základní údaje. K tomu nám pomůže metoda `info()`, která vrací souhrnné informace o celé tabulce: názvy sloupců, datové typy, počet neprázdných hodnot atd.

```py
food.info()
```

Výsledek je vidět níže. Takto vypadá výsledek v případě, že nenastavíme sloupec `fdc_id` jako index. Pokud bychom to udělali, v seznamu sloupců `fdc_id` neuvidíme.

```shell
<class 'pandas.core.frame.DataFrame'>
Index: 100 entries, 0 to 7858
Data columns (total 5 columns):
 #   Column            Non-Null Count  Dtype  
---  ------            --------------  -----  
 0   fdc_id            100 non-null    int64  
 1   data_type         100 non-null    object 
 2   description       100 non-null    object 
 3   food_category_id  99 non-null     float64
 4   publication_date  100 non-null    object 
dtypes: float64(1), int64(1), object(3)
memory usage: 4.7+ KB
```

**Poznámka:** Pokud znáš základy objektově orientovaného programování, pak věz, že `info` je ve skutečnosti metoda třídy `DataFrame`.

Počet řádků a sloupců můžeme získat z vlastnosti `shape`:

```py
print(food.shape)
```

Výsledek je opět níže.

```shell
(100, 5)
```

`pandas` nám vrací výsledky v sekvenci, která se jmenuje `tuple`. Nám stačí vědět, že si z ní data můžeme načíst stejně jako ze seznamu. Na prvním místě je vždy počet řádků a na druhém počet sloupců. Pokud by nás třeba zajímal jen počet řádků, napíšeme:

```py
print(food.shape[0])
```

Výsledkem je celé číslo (typ hodnoty `int`).

```py
100
```

Názvy všech sloupců pak z vlastnosti `columns`:

```py
print(food.columns)
```

Níže je výstup příkazu. Opět platí, že kdybychom nastavili sloupec `fdc_id` jako index, tak tímto příkazem vypsán nebude.

```shell
Index(['fdc_id', 'data_type', 'description', 'food_category_id', 'publication_date'], dtype='object')
```

### Začátek a konec

Na prvních a posledních několik řádků se chceme podívat často, hlavně v případě, když moc dobře neznáme strukturu dat. Můžeme k tomu použít metody `head` a `tail`.

```py
print(food.head())
```

```shell
    fdc_id        data_type    description  food_category_id publication_date
0  2644829  sub_sample_food   lentils, dry              16.0       2023-10-19
1  2347263  sub_sample_food    heavy cream               1.0       2022-10-28
2  2261954  sub_sample_food  Flour, potato              11.0       2022-04-28
3   321470  sub_sample_food  Salt, Iodized               2.0       2019-04-01
4   322951  sub_sample_food  Hot dogs beef               7.0       2019-04-01
```

Metoda `head` má parametr `n`, což je počet řádků, které mají být vypsány. Tento parametr je ale *nepovinný*. Nepovinné parametry mají vždy nějakou výchozí hodnotu, v případě parametru `n` metody `head` je tato výchozí hodnota 5. Můžem ale zvolit libovolnou vlastní, například 20.

Často je užitečné podívat se spíše na konec souboru. Pokud jsou data seřazená podle času, uvidíme na konci souboru nejnovější data, která nás často (např. u kurzu měn nebo akcií) zajímají víc než dávná historie.

```py
print(food.tail())
```

Metoda `tail` má také nepovinný parametr `n` s výchozí hodnotou 5.
