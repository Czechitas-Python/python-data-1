Opět si zde načteme data ze souboru ve formátu JSON. Konkrétně budeme pracovat s daty o státech světa, která jsou stažená ze služby [restcountries](https://restcountries.com/). Data si můžeš [stáhnout zde](assets/staty.json). Opět platí, že si je musíš stáhnout do adresáře, kde máš právě otevřený terminál!

## Indexy

Vytvoř si nový Python skript. Soubor načteme pomocí funkce `read_json`, kde jako první parametr zadáme název souboru. Data jsou opět vrácena jako `DataFrame` a my si je uložíme do proměnné `staty`. U dat o státech světa však můžeme přidat jedno zlepšení. V našem datasetu má každý stát svůj kód, který je **unikátní** a **identifikuje ho**. Můžeme tedy tento kód použít jako **index**.

**K zamyšlení:** Jaký index bychom použili pro tabulku zaměstnanců ve firmě, tabulku obcí České republice a tabulku aut v autopůjčovně? Pamatuj, že index by měl být unikátní.

### Přidání indexu

To, jaký sloupec má být použit jako index, řeší funkce `set_index()`. Ta nám vrátí upravený `DataFrame`, který si můžeme uložit do původní proměnné `staty`.

```py
import pandas
staty = pandas.read_json("staty.json")
staty = staty.set_index("alpha2Code")
```

Úspěch našeho počínání si můžeme zkontrolovat pomocí příkazu `staty.index`, který nám zobrazí informace o indexu.

```py
print(staty.index)
```

```shell
Index(['AF', 'AX', 'AL', 'DZ', 'AS', 'AD', 'AO', 'AI', 'AQ', 'AG',
       ...
       'UY', 'UZ', 'VU', 'VE', 'VN', 'WF', 'EH', 'YE', 'ZM', 'ZW'],
      dtype='object', name='alpha2Code', length=250)
```

Podívejme se nejprve, jaké informace jsou v naší tabulce obsažené.

```py
staty.info()
```

```shell
<class 'pandas.core.frame.DataFrame'>
Index: 250 entries, AF to ZW
Data columns (total 8 columns):
 #   Column      Non-Null Count  Dtype
---  ------      --------------  -----
 0   name        250 non-null    object
 1   alpha3Code  250 non-null    object
 2   capital     250 non-null    object
 3   region      250 non-null    object
 4   subregion   250 non-null    object
 5   population  250 non-null    int64
 6   area        240 non-null    float64
 7   gini        153 non-null    float64
dtypes: float64(2), int64(1), object(5)
memory usage: 17.6+ KB
```

### Výběr konkrétního řádku a hodnoty

Z názvů sloupců bychom mohli odvodit, jaké informace se v našem `DataFrame` nacházejí, ale možná se zorientujeme lépe, když se podíváme na nějaký konkrétní řádek.

K nalezení řádku pomocí indexu použijeme `loc`, který funguje obdobně jako `iloc`. Oproti němu však primárně používá námi zvolené indexy, zatímco `iloc` pracuje s čísly řádků. Opět platí, že používáme hranaté závorky, protože `loc` není běžná funkce.

```py
print(staty.loc["CZ"])
```

```shell
name          Czech Republic
alpha3Code               CZE
capital               Prague
region                Europe
subregion     Eastern Europe
population          10558524
area                 78865.0
gini                    26.0
Name: CZ, dtype: object
```

Opět jsme vybrali jeden řádek, získáme tedy výsledek jako sérii. Můžeme však jít ještě dále a získat jednu konkrétní hodnotu. `loc` dodáme druhý parametr, který bude obsahovat jméno sloupce, ze kterého chceme hodnotu vybrat. Vyberme si třeba rozlohu, kterou uložíme do proměnné `rozloha`.

```py
rozloha = staty.loc["CZ", "area"]
```

### Výběr několika řádků

`loc` si podobně jako `iloc` dobře rozumí s dvojtečkou. Náš soubor je seřazený dle abecedy podle názvu státu. Pokud tedy chceme vypsat všechny státy, jejich názvy v abecedě patří mezi Českou republikou a Dominikánskou republikou, vložíme tato jméno do uvozovek a dáme mezi ně dvojtečku.

```py
print(staty.loc["CZ":"DO"])
```

```shell
                          name alpha3Code        capital    region        subregion  population     area  gini
alpha2Code
CZ              Czech Republic        CZE         Prague    Europe   Eastern Europe    10558524  78865.0  26.0
DK                     Denmark        DNK     Copenhagen    Europe  Northern Europe     5717014  43094.0  24.0
DJ                    Djibouti        DJI       Djibouti    Africa   Eastern Africa      900000  23200.0  40.0
DM                    Dominica        DMA         Roseau  Americas        Caribbean       71293    751.0   NaN
DO          Dominican Republic        DOM  Santo Domingo  Americas        Caribbean    10075045  48671.0  47.2

```

Podobně funguje, i když zadáme jen jednu hranici. Můžeme si třeba zkusit vypsat hodnoty od začátku po Andorru nebo od Uzbekistánu do konce.

```py
print(staty.loc[:"AD"])
print(staty.loc["UZ":])
```

Pokud by nás zajímaly informace o více řádcích, které spolu nesousedí, můžeme opět použít seznam. Index řádků, které nás zajímají, vložíme do seznamu a ten předáme jako první parametr funkci `loc`.

```py
print(staty.loc[["CZ", "SK"]])
```

```shell
                      name alpha3Code     capital  region       subregion  population     area  gini
alpha2Code
CZ          Czech Republic        CZE      Prague  Europe  Eastern Europe    10558524  78865.0  26.0
SK                Slovakia        SVK  Bratislava  Europe  Eastern Europe     5426252  49037.0  26.0
```

Pomocí seznamu se můžeme zeptat i na informace z více sloupců. Zkusme si třeba porovnat rozlohu a počet obyvatel sousedních států České republiky.

```py
print(staty.loc[["SK", "PL", "DE", "AT"], ["area", "population"]])
```

```shell
                area  population
alpha2Code
SK           49037.0     5426252
PL          312679.0    38437239
DE          357114.0    81770900
AT           83871.0     8725931
```

## Cvičení
::exc[excs/ceska-jmena]
