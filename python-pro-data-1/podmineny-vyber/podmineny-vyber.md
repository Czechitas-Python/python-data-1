## Podmíněný výběr

Nyní si vyzkoušíme jeden z hlavních nástrojů zpracování dat, a to je psaní :term{cs="dotazů" en="query"}. Logika psaní dotazů je v různých prostředích stejná, liší se pouze to, jak ji provádíme. Podívejme se na konkrétní příklady.

Budeme se dále věnovat potravinám, tentokrát jejich výživovým hodnotám. Data jsou uložena v souboru [food_nutrient.csv](assets/food_nutrient.csv). Níže je stručný popis sloupců tabulky.

- `id`: Identifikační číslo záznamu.
- `fdc_id`: Identifikační číslo potraviny, ke které se vztahuje živina.
- `nutrient_id`: Identifikační číslo živiny.
- `amount`: Množství živiny v potravině.
- `data_points`: Počet datových bodů použitých pro výpočet průměru.
- `derivation_id`: Identifikační číslo metody, kterou byla hodnota živiny odvozena.
- `standard_error`: Standardní chyba měření množství živiny.
- `min`: Minimální hodnota množství živiny nalezená v potravině.
- `max`: Maximální hodnota množství živiny nalezená v potravině.
- `median`: Medián hodnot množství živiny v potravině.
- `footnote`: Poznámka nebo dodatečné informace o živině.
- `name`: Název živiny.
- `unit_name`: Název jednotky, ve které se měří živina.
- `nutrient_nbr`: Unikátní číslo identifikující živinu nebo potravinovou složku​.

Všimni si, že nevíme název potraviny, pouze její identifikační číslo. To je stejné jako v předchozí tabulce. Abychom ve výstupu viděli i názvy potravin, je potřeba obě tabulky propojit, což si ukážeme v další lekci.

### Co vlastně umí série

Již jsme si říkali, že ne vždy nás zajímají všechna data. Zopakujme si, že pokud chceme vybrat jen sloupec a zachovat všechny řádky, zpravidla použijeme výběr sloupců pomocí hranatých závorek.

```py
import pandas as pd
food_nutrient = pd.read_csv("food_nutrient.csv")
food_nutrient.head()
```

Podívejme se na sloupec `name`, který obsahuje názvy jednotlivých výživných látek.

```py
food_nutrient["name"]
```

Nás by mohlo zajímat, jaké všechny výživné látky v datech jsou. K tomu nám stačí vidět každý název jenom jednou. Jinak řečeno, potřebujeme unikátní hodnoty. K tomu můžeme využít metodu `.unique()`.

```py
food_nutrient["name"].unique()
```

Též může být zajímavé, kolikrát máme o některé z výživných látek informaci. K tomu můžeme použít metodu `.value_counts()`. Ta nám pro každou výživnou hodnotu zobrazí, kolikrát se v tabulce vyskytuje.

```py
food_nutrient["name"].value_counts()
```

### Podmíněný výběr

Při zpracovávání dat podmínkám rozhodně neutečeš. Podmínky jsou velmi užitečné, protože bez nich bychom museli pracovat se všemi daty, co jsme dostali, což není vždy žádoucí.

- Data často obsahují chyby, které vzniknou třeba špatným nastavením stroje nebo překlepem pracovníka, který je zadával. Pokud bychom chyby nechali v datech a dále s nimi pracovali, udělaly by nám tam pěknou paseku.
- Často chceme zpracovat jen část dat. Například u výživných látek nás můžou zajímat jen ty, které jsou zdraví prospěšné (chceme jich konzumovat nějaké doporučené množství), nebo naopak ty zdraví škodlivé (chceme jich konzumovat co nejméně).

V jazyce SQL píšeme podmínky za klíčové slovo `WHERE`, v Excelu můžeme použít funkce Filtr atd. V `pandas` můžeme použít metodu `query` nebo zápis s využitím hranatých závorek.

Uvažujme například, že nám jde o obsah hořčíku (`Magnesium`), protože naším úkolem je doporučit potraviny lidem s nedostatkem hořčíku. Nejprve potřebujeme formulovat podmínku. Ta bude vypadat takto `food_nutrient["Magnesium"] == "Magnesium, Mg"`. V podmínce máme sloupec, na který se ptáme, a porovnání s řetězcem. Používáme operátor na kontrolu rovnosti (`==`). Zkusme si zadat samotnou podmínku a podívejme se na výsledek.

Název zadáváme včetně chemické značky, protože tak je to v původních datech.

```py
food_nutrient["name"] == "Magnesium, Mg"
```

Pokud si vzpomeneš na hodnoty typu `bool`, víš, že můžou nabývat pouze dvou hodnot: `True` (pravda) a `False` (nepravda). Při použití operátorů pro porovnávání vždy získáme hodnotu typu `bool`. Nyní použijeme poměrně svérázný zápis pomocí hranatých závorek. Podmínku, kterou jsme formulovali v předchozím kroku, vložíme do hranatých závorek, před kterou vložíme název tabulky `food_nutrient`. Tento zápis provede následující:

- Z původní tabulky `food_nutrient` vybere ty řádky, které vyhovují podmínce, tj. ty, které měly při předchozím zápisu hodnotu `True`.
- Výsledek uloží do nové tabulky `magnesium`. Výsledná tabulka bude obsahovat všechny sloupce z původní tabulky, ale pouze ty řádky, které vyhovují podmínce.

```py
magnesium = food_nutrient[food_nutrient["name"] == "Magnesium, Mg"]
```

### Popisná statistika

Pokud pracujeme s číselnými ukazateli, je dobré podívat se na ukazatele popisné statistiky. Ač to může znít hrozivě, jsou to hodnoty, se kterými se setkáváme poměrně běžně. Zjistíme je pomocí metody `describe()`. Metodu je možné použít pro více sloupců, ale ne vždy dávají smysl. Například pro sloupec `fdc_id` nemá smysl hodnoty počítat, protože to jsou číslá označení ("pojmenování") jednotlivých potravin.

Též by nemělo smysl použít metodu `.describe()` pro všechny řádky najednou, protože by došlo k promíchání dat o různých živinách, navíc měřených v odlišných jednotkách.

```py
magnesium["amount"].describe()
```

V datech se zobrazují tyto hodnoty: 

- `count`: Počet hodnot.
- `mean`: Aritmetický průměr hodnot.
- `std`: :term{cs="Směrodatná odchylka" en="standard deviation"}. Pomocí ní měříme :term{cs="různorodost" en="variability"} dat.
- `min`: Nejmenší hodnota.
- `25%`: Toto číslo rozděluje data na 25 % menších hodnot a 75 % větších hodnot.
- `50%`: Medián. Jde o číslo, které by leželo přesně uprostřed seřazených hodnot, tj. rozděluje data na 50 % menších hodnot a 50 % větších hodnot.
- `75%`: Toto číslo rozděluje data na 75 % menších hodnot a 25 % větších hodnot.
- `max`: Největší hodnota.


Naším úkolem je vybrat potraviny, které mají vyšší množství hořčíku. K tomu opět využijeme dotaz. Uvažujeme, že nás zajímají potraviny, které mají více než 100 gramů hořčíku.

```py
magnesium_limit = magnesium[magnesium["amount"] > 100]
```

### Spojení více podmínek

Nyní uvažujme, že chceme spojit více podmínek dohromady. U některých živin může být například vhodné nekonzumovat jich příliš malé, ale ani příliš velké množství. Uvažujme například vápník. Nejprve si vytvoříme tabulku, která bude obsahovat data pouze o vápníku. Postup je stejný jako v předchozí části.

```py
calcium = food_nutrient[food_nutrient["name"] == "Calcium, Ca"]
```

Naším úkolem bude vybrat potraviny, které mají mezi 30 a 500 mg vápníku. Vepíšeme do hranatých závorek obě podmínky. Každou z podmínek vložíme do kulatých závorek. V našem případě chceme, aby byly splněné obě podmínky, proto mezi ně vložíme symbol `&`.

```py
calcium_limit = calcium[(calcium["amount"] > 30) & (calcium["amount"] < 500)]
```

Podmínek můžeme zkombinovat i více, například tři. Předchozí dva kroky můžeme díky tomu spojit do jednoho, tj. z původní tabulky `food_nutrient` vybereme řádky, které:

- mají ve sloupci `name` hodnotu ` "Calcium, Ca"`,
- mají ve sloupci `amount` hodnotu větší než 30,
- mají ve sloupci `amount` hodnotu menší než 500.

Mezi každou dvojici podmínek vložíme symbol `&`, tento symbol tedy použijeme dvakrát.

```py
calcium_limit = food_nutrient[(food_nutrient["name"] == "Calcium, Ca") & (food_nutrient["amount"] > 30) & (food_nutrient["amount"] < 500)]
```

Pokud chceme, aby stačilo splnění jedné podmínky, použijeme symbol `|`.


## Cvičení

::exc[excs/vlaknina]
::exc[excs/hlaseni_chyb]

## Bonusy

::exc[excs/ceska-jmena]
::exc[excs/dve-kriteria]
