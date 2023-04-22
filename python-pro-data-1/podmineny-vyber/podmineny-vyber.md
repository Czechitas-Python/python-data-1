## Podmíněný výběr

Nyní si vyzkoušíme jednu z hlavních prací datového analytika, a to je **psaní dotazů**. Logika psaní dotazů je v různých prostředích stejná, liší se pouze to, jak ji provádíme. Podívejme se na konkrétní příklady.

### Výběr sloupců

Již jsme si říkali, že ne vždy nás zajímají všechna data. Zopakujme si, že pokud chceme vybrat jen sloupec a zachovat všechny řádky, zpravidla použijeme výběr sloupců pomocí hranatých závorek.

```py
print(staty["population"])
```

Pokud nás zajímá více sloupců, můžeme opět použít seznam, do kterého sloupce vepíšeme.

```py
print(staty[["population", "area"]])
```

### Co vlastně umí série

Asi se teď říkáš, k čemu je to vlastně dobré. Zkusme si jednoduchý příklad: Chceme zjistit, kolik lidí žije ve všech státech světa. Bude nás tedy zajímat pouze sloupec `population`, kde máme sérii s počty obyvatel jednotlivých států. Série sama o sobě umí zajímavé věci, například umí sama spočítat svůj součet a vrátit výsledek jako číslo. K tomu slouží funkce `sum`. K jejímu volání opět použijeme tečkovou notaci.

```py
populace = staty["population"]
print(populace.sum())
```

```shell
7349137231
```

Oba kroky můžeme spojit do jednoho.

```py
print(staty["population"].sum())
```

Série umí spočítat řadu dalších věcí, jako třeba průměr (funkce `mean`), minimum a maximum (funkce `min` a `max`) nebo medián (funkce `median`). Přehled všech funkcí najdeš [v dokumentaci](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html).

### Podmínky (ano, už zase)

V datové analýze podmínkám rozhodně neutečeš. Podmínky jsou velmi užitečné, protože bez nich bychom museli pracovat se všemi daty, co jsme dostali, což není vždy žádoucí.

- Data často obsahují chyby, která vzniknou třeba špatným nastavením stroje nebo překlepem pracovníka, který je zadával. Pokud bychom chyby nechali v datech a dále s nimi pracovali, udělaly by nám tam pěknou paseku.
- Často chceme zpracovat jen část dat. Pokud máme například firmu s obchůdky v několika městech, můžeme chtít zpracovat jen data z jednoho města, pro nějaké konkrétní zboží nebo časové období.

V jazyce SQL píšeme podmínky za klíčové slovo `WHERE`, v Excelu můžeme použít funkce Filtr atd. V `pandas` používáme funkci `query`. Název této funkce si ale pamatovat nemusíš, protože namísto ní opět můžeme použít hranaté závorky.

 Začněme s tím, že se podíváme na nejmenší státy, které na světě jsou. Nechme si například vypsat státy, které mají méně než 1000 obyvatel. Postup si vysvětlíme ve dvou krocích.

 Nejprve potřebujeme formulovat podmínku. Ta bude vypadat takto `staty["population"] < 1000`. V podmínce máme sloupec, na který se ptáme, a porovnání s číselnou hodnotou. Používáme nám již známý operátor menší než (`<`). Zkusme si zadat samotnou podmínku do terminálu a podívejme se na výsledek.

```py
print(staty["population"] < 1000)
```

```shell
name
Afghanistan          False
Åland Islands        False
Albania              False
Algeria              False
American Samoa       False
                     ...
Wallis and Futuna    False
Western Sahara       False
Yemen                False
Zambia               False
Zimbabwe             False
Name: population, Length: 250, dtype: bool
```

Pokud si vzpomeneš na hodnoty typu `bool`, víš, že ty mohly nabývat pouze dvou hodnot: `True` a `False`. Při použití operátorů pro porovnávání vždy získáme hodnotu typu `bool`. Pokud bychom například měli proměnnou `znamka` a napsali do terminálu `znamka < 3`, získáme jednu hodnotu `True` nebo `False`, a to podle toho, jakou hodnotu v proměnné `znamka` máme.

My v naší tabulce ale máme 250 států s různými počty obyvatel, proto nám náš dotaz vrací 250 hodnot, z nichž některé jsou `True` a některé `False`. Vytvořili jsme tedy jakýsi polotovar. My nyní chceme vidět jen ty řádku, kde máme hodnotu `True`, což nám dá státy s počtem obyvatel menší než 1000. K tomu využijeme poměrně zvláštní zápis - naši podmínku vložíme do hranatých závorek.

```py
pidistaty = staty[staty["population"] < 1000]
print(pidistaty[["population", "area"]])
```

```shell
                                              population     area
name
Bouvet Island                                          0    49.00
United States Minor Outlying Islands                 300      NaN
Cocos (Keeling) Islands                              550    14.00
French Southern Territories                          140  7747.00
Heard Island and McDonald Islands                      0   412.00
Holy See                                             451     0.44
Pitcairn                                              56    47.00
South Georgia and the South Sandwich Islands          30      NaN
```

Tento podivný zápis má ve skutečnosti svoji logiku. My jsme v našem polotovaru získali sloupec, kde máme 250 hodnot typu `bool`. `pandas` teď jednoduše udělají to, že vypíšou ty řádky řádky, kde má náš polotovar hodnotu `True` a ty, které mají hodnotu `False`, před námi skryjí.

V tabulce vidíme několik států a kromě Holy See (tj. Vatikánu) jsme o nich asi ani nikdy neslyšeli. V některých řádcích vidíme hodnotu `NaN`. To značí, že pro daný řádek hodnotu nemáme, pro některé státy tedy nemáme zadanou rozlohu. Měli bychom si tedy rozmyslet, zda s takovými státy v nějaké analýze vůbec pracovat.

### Spojení více podmínek

Často narazíme na případ, kdy chceme zkombinovat více podmínek, například chceme tržby v jedné prodejně a pro letošní rok. Při kombinaci se musíme rozhodnout, zda chceme, aby ke zobrazení řádku byly splněné obě, nebo zda stačí, aby byla splněna pouze jedna z nich.

Pokud chceme, aby musely být splněny obě podmínky, vložíme mezi ně symbol `&`. Uvažujme dvě podmínky:

- Stát musí mít alespoň 20 milionů obyvatel: `(staty["population"] > 20000000)`
- Stát se musí nacházet v Evropě: `staty["region"] == "Europe")`

Obě tyto podmínky napíšeme do závorek a vložíme mezi ně symbol `&`. Následně použijeme již známé hranaté závorky, které přidáme hned za proměnnou `staty`.

```py
lidnate_evropske_staty = staty[(staty["population"] > 20000000) & (staty["region"] == "Europe")]
print(lidnate_evropske_staty["population"])
```

```shell
name
France                                                   66710000
Germany                                                  81770900
Italy                                                    60665551
Poland                                                   38437239
Russian Federation                                      146599183
Spain                                                    46438422
Ukraine                                                  42692393
United Kingdom of Great Britain and Northern Ireland     65110000
Name: population, dtype: int64
```

Pokud chceme, aby stačilo splnění jedné podmínky, použijeme symbol `|`. Zde vypisujeme státy, které mají buď více než miliardu obyvatel nebo rozlohu větší než 3 miliony kilometrů čtverečních.

```py
print(staty[(staty["population"] > 10_000_000_000) | (staty["area"] > 3_000_000)])
```

```shell
                         alpha2Code alpha3Code           capital    region                  subregion  population        area  gini
name
Antarctica                       AQ        ATA                       Polar                                   1000  14000000.0   NaN
Australia                        AU        AUS          Canberra   Oceania  Australia and New Zealand    24117360   7692024.0  30.5
Brazil                           BR        BRA          Brasília  Americas              South America   206135893   8515767.0  54.7
Canada                           CA        CAN            Ottawa  Americas           Northern America    36155487   9984670.0  32.6
China                            CN        CHN           Beijing      Asia               Eastern Asia  1377422166   9640011.0  47.0
India                            IN        IND         New Delhi      Asia              Southern Asia  1295210000   3287590.0  33.4
Russian Federation               RU        RUS            Moscow    Europe             Eastern Europe   146599183  17124442.0  40.1
United States of America         US        USA  Washington, D.C.  Americas           Northern America   323947000   9629091.0  48.0
```

**Poznámka:** Abychom zpřehlednili zápis velkých čísel, použili jsme podtržítko.

### Použití seznamu v podmínce

Uvažujme, že bychom chtěli vypsat všechny státy, které leží v západní nebo východní Evropě. Na to bychom mohli použít operátor `|`, ale při dotazu na tři nebo čtyři hodnoty by se takový zápis extrémně protáhl.

V seznamu operátorů na porovnávání jsme měli ještě operátor `in`, kterým jsme ověřovali, jestli je nějaký prvek přítomný v kolekci. Tento operátor nám v `pandas` supluje funkce `isin()`. Pokud tuto funkci aplikujeme na jeden konkrétní sloupec, vrátí ním `True` pro všechny řádky, pro které je hodnota přítomná v seznamu. Náš dotaz na země východní a západní Evropy bychom tedy napsali jako `isin(["Western Europe", "Eastern Europe"])`.

```py
print(staty[staty["subregion"].isin(["Western Europe", "Eastern Europe"])])
```

```shell
                      alpha2Code alpha3Code     capital  region       subregion  population         area  gini
name
Austria                       AT        AUT      Vienna  Europe  Western Europe     8725931     83871.00  26.0
Belarus                       BY        BLR       Minsk  Europe  Eastern Europe     9498700    207600.00  26.5
Belgium                       BE        BEL    Brussels  Europe  Western Europe    11319511     30528.00  33.0
Bulgaria                      BG        BGR       Sofia  Europe  Eastern Europe     7153784    110879.00  28.2
Czech Republic                CZ        CZE      Prague  Europe  Eastern Europe    10558524     78865.00  26.0
France                        FR        FRA       Paris  Europe  Western Europe    66710000    640679.00  32.7
Germany                       DE        DEU      Berlin  Europe  Western Europe    81770900    357114.00  28.3
Hungary                       HU        HUN    Budapest  Europe  Eastern Europe     9823000     93028.00  31.2
Liechtenstein                 LI        LIE       Vaduz  Europe  Western Europe       37623       160.00   NaN
Luxembourg                    LU        LUX  Luxembourg  Europe  Western Europe      576200      2586.00  30.8
Moldova (Republic of)         MD        MDA    Chișinău  Europe  Eastern Europe     3553100     33846.00  33.0
Monaco                        MC        MCO      Monaco  Europe  Western Europe       38400         2.02   NaN
Netherlands                   NL        NLD   Amsterdam  Europe  Western Europe    17019800     41850.00  30.9
Poland                        PL        POL      Warsaw  Europe  Eastern Europe    38437239    312679.00  34.1
Republic of Kosovo            XK        KOS    Pristina  Europe  Eastern Europe     1733842     10908.00   NaN
Romania                       RO        ROU   Bucharest  Europe  Eastern Europe    19861408    238391.00  30.0
Russian Federation            RU        RUS      Moscow  Europe  Eastern Europe   146599183  17124442.00  40.1
Slovakia                      SK        SVK  Bratislava  Europe  Eastern Europe     5426252     49037.00  26.0
Switzerland                   CH        CHE        Bern  Europe  Western Europe     8341600     41284.00  33.7
Ukraine                       UA        UKR        Kiev  Europe  Eastern Europe    42692393    603700.00  26.4
```

## Cvičení
::exc[excs>ceska-jmena-2]
