## Čtení na doma

`DataFrame` nejsou uzavřeným světem, ale umožňují snadný převod na seznamy, případně můžeme naopak převést seznam na `DataFrame`.

### Převod DataFrame na seznam

K takovému převodu na seznam nám poslouží kombinace funkcí `to_numpy` a `tolist`. Převod totiž neprovádíme přímo, ale jako mezikrok jej převedeme na pole modulu `numpy`.

```py
staty_list = staty.to_numpy().tolist()
print(staty_list[0])
```

```shell
['Kabul', 'Asia', 'Southern Asia', 27657145, 652230.0, 27.8]
```

Ve výsledných seznamech nám chybí názvy států. Potíž je v tom, že index se v Pandas nebere jako součást dat. Pokud chceme index vrátit do původního stavu a mít ho jako automaticky generovaná čísla řádků, můžeme použít metodu `reset_index`. S její pomocí pak už dokážeme dostat z DataFramu čistá data takto

```py
staty_list = staty.reset_index().to_numpy().tolist()
print(staty_list[0])
```

```shell
['Afghanistan', 'Kabul', 'Asia', 'Southern Asia', 27657145, 652230.0, 27.8]
```

### Vytvoření DataFrame ze seznamu

Zkusme si nyní opačný postup, převedeme si seznam seznamů (což je jiný zápis dvourozměrné tabulky) na `DataFrame`. Jistě si vzpomínáš na příklad se známkami z testu, kterým jsme měli na prvním workshopu.

```py
znamky = [
    ['Petr', 2],
    ['Roman', 1],
    ['Jitka', 3],
    ['Zuzana', 5],
    ['Ondřej', 2],
    ['Julie', 2],
    ['Karel', 4],
    ['Anna', 1],
    ['Eva', 1]
]
```

Naším úkolem bylo spočítat průměrnou známku. K tomu jsme použili cyklus.

```py
soucet = 0
for radek in znamky:
    soucet = soucet + radek[1]
prumer = soucet / len(znamky)
```

Průměrnou známku ale můžeme spočítat i pomocí `pandas`. Pomocí funkce `DataFrame` převedeme proměnnou `znamky` na `DataFrame`. Abychom měli v `DataFrame` správné názvy sloupců, definujeme je jako parametr `columns`.

Následně vybereme data ve sloupci `znamka`. Protože jsme vybrali jeden sloupec, získáme sérii. Průměrnou hodnotu v sérii pak spočítáme pomocí funkce `mean`.

```py
znamky = pandas.DataFrame(znamky, columns=['student', 'znamka'])
prumer = znamky["znamka"].mean()
```

Přehled všech funkcí, které pro sérii můžeš použít, opět najdeš [v dokumentaci](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.html).

### Vytvoření DataFrame ze seznamu slovníků

Pokud jsi absolvovala Úvod do programování v Pythonu 2, znáš již též slovníky. I pole složené ze slovníků můžeme převést na `DataFrame`.

```py
nakupy = [
    {"person": "Petr", "item": "Prací prášek", "value": 399},
    {"person": "Ondra", "item": "Savo", "value": 80},
    {"person": "Petr", "item": "Toaletní papír", "value": 65},
    {"person": "Libor", "item": "Pivo", "value": 124},
    {"person": "Petr", "item": "Pytel na odpadky", "value": 75},
    {"person": "Míša", "item": "Utěrky na nádobí", "value": 130},
    {"person": "Ondra", "item": "Toaletní papír", "value": 120},
    {"person": "Míša", "item": "Pečící papír", "value": 30},
    {"person": "Zuzka", "item": "Savo", "value": 80},
    {"person": "Pavla", "item": "Máslo", "value": 50},
    {"person": "Ondra", "item": "Káva", "value": 300}
]
```

Výhodou je, že nyní nemusíme přidávat názvy sloupců, protože ty už funkce `DataFrame` získá z klíčů slovníků.

```py
nakupy = pandas.DataFrame(nakupy)
nakupy.info()
```
```shell
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 11 entries, 0 to 10
Data columns (total 3 columns):
 #   Column  Non-Null Count  Dtype
---  ------  --------------  -----
 0   person  11 non-null     object
 1   item    11 non-null     object
 2   value   11 non-null     int64
dtypes: int64(1), object(2)
memory usage: 392.0+ bytes
```
