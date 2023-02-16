V této části si zkusíme napas nějaké základní dotazy na naše data. `pandas` umožňují napsat dotazy podobně jako jazyk SQL, k práci ale jeho znalost vůbec ne potřebujeme.

Tentokrát si vyzkoušíme načíst data ze souboru ve formátu JSON. Konkrétně budeme pracovat s daty o státech světa, která jsou stažená ze služby [restcountries](https://restcountries.com/). Data si můžeš [stáhnout zde](assets/staty.json). Opět platí, že si je musíš stáhnout do adresáře, kde máš právě otevřený terminál!

## Indexy

Vytvoř si nový Python skript. Soubor načteme pomocí funkce `read_json`, kde jako první parametr zadáme název souboru. Data jsou opět vrácena jako `DataFrame` a my si je uložíme do proměnné `staty`. U dat o státech světa však můžeme přidat jedno zlepšení. Víme, že každý stát na světě má svůj název a ten název je **unikátní** a **identifikuje ho**. Můžeme tedy tento název použít jako **index**.

**K zamyšlení:** Jaký index bychom použili pro tabulku zaměstnanců ve firmě, tabulku obcí České republice a tabulku aut v autopůjčovně? Pamatuj, že index by měl být unikátní.

### Přidání indexu

To, jaký sloupec má být použit jako index, řeší funkce `set_index()`. Ta nám vrátí upravený `DataFrame`, který si můžeme uložit do původní proměnné `staty`.

```py
import pandas
staty = pandas.read_json("staty.json")
staty = staty.set_index("name")
```

Úspěch našeho počínání si můžeme zkontrolovat pomocí příkazu `staty.index`, který nám zobrazí informace o indexu.

```py
print(staty.index)
```

```shell
Index(['Afghanistan', 'Åland Islands', 'Albania', 'Algeria', 'American Samoa',
       'Andorra', 'Angola', 'Anguilla', 'Antarctica', 'Antigua and Barbuda',
       ...
       'Uruguay', 'Uzbekistan', 'Vanuatu',
       'Venezuela (Bolivarian Republic of)', 'Viet Nam', 'Wallis and Futuna',
       'Western Sahara', 'Yemen', 'Zambia', 'Zimbabwe'],
      dtype='object', name='name', length=250)
```

Podívejme se nejprve, jaké informace jsou v naší tabulce obsažené.

```py
staty.info()
```

```shell
<class 'pandas.core.frame.DataFrame'>
Index: 250 entries, Afghanistan to Zimbabwe
Data columns (total 8 columns):
 #   Column      Non-Null Count  Dtype
---  ------      --------------  -----
 0   alpha2Code  250 non-null    object
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

K nalezení řádku pomocí indexu použijeme `loc`, která funguje obdobně jako funkce `iloc`. Oproti ní však primárně používá námi zvolené indexy, zatímco funkce `iloc` pracuje s čísly řádků. Opět platí, že používáme hranaté závorky, protože `loc` není běžná funkce.

```py
print(staty.loc["Czech Republic"])
```

```shell
alpha2Code                CZ
alpha3Code               CZE
capital               Prague
region                Europe
subregion     Eastern Europe
population          10558524
area                   78865
gini                      26
Name: Czech Republic, dtype: object
```

Opět jsme vybrali jeden řádek, získáme tedy výsledek jako sérii. Můžeme však jít ještě dále a získat jednu konkrétní hodnotu. Funkci `loc` dodáme druhý parametr, který bude obsahovat jméno sloupce, ze kterého chceme hodnotu vybrat. Vyberme si třeba rozlohu, kterou uložíme do proměnné `rozloha`.

```py
rozloha = staty.loc["Czech Republic","area"]
```

### Výběr několika řádků

Funkce `loc()` si podobně jako `iloc()` dobře rozumí s dvojtečkou. Náš soubor je seřazený dle abecedy. Pokud tedy chceme vypsat všechny státy, jejich názvy v abecedě patří mezi Českou republikou a Dominikánskou republikou, vložíme tato jméno do uvozovek a dáme mezi ně dvojtečku.

```py
print(staty.loc["Czech Republic":"Dominican Republic"])
```

```shell
                   alpha2Code alpha3Code        capital    region        subregion  population     area  gini
name
Czech Republic             CZ        CZE         Prague    Europe   Eastern Europe    10558524  78865.0  26.0
Denmark                    DK        DNK     Copenhagen    Europe  Northern Europe     5717014  43094.0  24.0
Djibouti                   DJ        DJI       Djibouti    Africa   Eastern Africa      900000  23200.0  40.0
Dominica                   DM        DMA         Roseau  Americas        Caribbean       71293    751.0   NaN
Dominican Republic         DO        DOM  Santo Domingo  Americas        Caribbean    10075045  48671.0  47.2
```

Podobně se funkce chová, i když zadáme jen jednu hranici. Můžeme si třeba zkusit vypsat hodnoty od začátku po Andorru nebo od Uzbekistánu do konce.

```py
print(staty.loc[:"Andorra"])
print(staty.loc["Uzbekistan":])
```

Pokud by nás zajímaly informace o více řádcích, které spolu nesousedí, můžeme opět použít seznam. Index řádků, které nás zajímají, vložíme do seznamu a ten předáme jako první parametr funkci `loc`.

```py
print(staty.loc[["Czech Republic","Slovakia"]])
```

```shell
               alpha2Code alpha3Code     capital  region       subregion  population     area  gini
name
Czech Republic         CZ        CZE      Prague  Europe  Eastern Europe    10558524  78865.0  26.0
Slovakia               SK        SVK  Bratislava  Europe  Eastern Europe     5426252  49037.0  26.0
```

Pomocí seznamu se můžeme zeptat i na informace z více sloupců. Zkusme si třeba porovnat rozlohu a počet obyvatel sousedních států České republiky.

```py
print(staty.loc[["Slovakia","Poland","Germany","Austria"], ["area","population"]])
```

```shell
              area  population
name
Slovakia   49037.0     5426252
Poland    312679.0    38437239
Germany   357114.0    81770900
Austria    83871.0     8725931
```

## Cvičení
::exc[excs>ceska-jmena]
