## Pokročilé úpravy

V předchozí lekci jsme si ukázali, jak v `pandas` vytváříme `DataFrame` a jak z něj můžeme vybírat data pomocí různých způsobů dotazování. Nyní se posuneme o kus dále a ukážeme si, jak můžeme s `DataFrame` dělat složitější operace jako je filtrování chybějících hodnot, spojování a agregace.

### Maturita

Abychom měli nějaký praktický příklad k procvičování, použijeme fiktivní data z výsledků maturitních zkoušek během jednoho týdne na nějakém menším gymnáziu. Maturita se odehrává ve třech místnostech: U202, U203 a U302. Máme tedy tři tabulky dat, z každé místnosti jednu. Níže si můžete prohlédnout příklad tabulky z místnosti U202. Všechny tabulky jsou ke stažení zde: [u202.csv](assets/u202.csv), [u203.csv](assets/u203.csv), [u302.csv](assets/u302.csv).

Funkce `read_csv()` knihovny `pandas` umí stáhnout CSV soubor rovnou z internetu.

```py
import pandas

u202 = pandas.read_csv("https://kodim.cz/cms/assets/analyza-dat/python-data-1/python-pro-data-1/agregace-a-spojovani/pokrocile-upravy/u202.csv")
u203 = pandas.read_csv("https://kodim.cz/cms/assets/analyza-dat/python-data-1/python-pro-data-1/agregace-a-spojovani/pokrocile-upravy/u203.csv")
u302 = pandas.read_csv("https://kodim.cz/cms/assets/analyza-dat/python-data-1/python-pro-data-1/agregace-a-spojovani/pokrocile-upravy/u302.csv")
```

|cisloStudenta |predmet         |znamka|den|
|---:|:----------------|------:|:---|
|1  |Chemie          |      |pá |
|2  |Dějepis         |3     |pá |
|3  |Matematika      |2     |út |
|2  |Společenské vědy|2     |pá |
|4  |Biologie        |1     |pá |
|5  |Dějepis         |1     |po |
|6  |Fyzika          |2     |čt |
|7  |Dějepis         |4     |po |
|8  |Matematika      |2     |po |
|9  |Dějepis         |      |pá |
|10 |Chemie          |2     |st |
|3  |Chemie          |5     |út |
|11 |Matematika      |1     |st |
|12 |Biologie        |4     |st |
|10 |Dějepis         |5     |st |

### Práce s chybějícími hodnotami

V praxi se poměrně často setkáme s tím, že v datovém setu některé hodnoty chybí. Můžeme si například všimnout, že v tabulce U202 dvěma studentům chybí známka. To znamená, že se studenti k maturitě nedostavili. Na takové případy je třeba být připraven.

V `pandas`, ale i obecně v datové analýze, je možné se s chybějícími daty vypořádat různými způsoby:

1. Nejlepší je vždy ověření, proč údaje chybí (např. u poskytovatele dat) a pokud je to možné, zajistit jejich doplnění.
1. Nahradit chybějící hodnoty jinými hodnotami.
1. Odstranit všechny řádky s chybějícími daty z datového setu.
1. Vyčlenit je do separátního datasetu a zpracovat je zvlášť.

Důležité je mít na paměti, že vyřazením některých řádků může dojít ke zkreslení výsledků analýzy!

### Odstranění neúplných řádků

Předpokládejme, že jsme si ověřili, že data chybí skutečně pouze u studentů, kteří z daného předmětu nematurovali. Protože nás budou zajímat především statistiky jednotlivých předmětů, můžeme prázdné řádky vynechat, protože označují zkoušky, které ve skutečnosti neproběhly.

Pokud jsme tak ještě neučinili, načteme si naši první tabulku jako DataFrame.

```py
import pandas
u202 = pandas.read_csv('u202.csv')
```

Pokud `pandas` narazí na prázdnou buňku, vloží místo ní do tabulky speciální hodnotu `NaN`, se kterou už jsme se setkali.

Série obsahují metodu `isnull()`, která vrátí pravdivostní sérii s hodnotou `True` všude tam, kde v původní sérii chybí hodnota. Metoda `notnull()` pracuje přesně opačně. Vrátí pravdivostní sérii s hodnotami `True` všude tam, kde v původní sérii hodnota nechybí.

```py
print(u202['znamka'].isnull())
```

```shell
0      True
1     False
2     False
3     False
4     False
5     False
6     False
7     False
8     False
9      True
10    False
11    False
12    False
13    False
14    False
Name: znamka, dtype: bool
```

Tyto metody můžeme využít například k tomu, abychom získali všechna data, kde chybí hodnota ve sloupečku `znamka`.

```py
print(u202[u202['znamka'].isnull()])
```

```shell
   cisloStudenta  predmet  znamka den
0              1   Chemie     NaN  pá
9              9  Dějepis     NaN  pá
```

Další užitečné metody na práci s chybějícími hodnotami najdeme na DataFrame.

1. `dropna()` vrátí datový set očištěn od chybějících dat.
1. `dropna(axis=1)` odstraní všechny sloupce, které obsahují chybějící data.
1. `fillna(x)` nahradí všechna chybějící data a hodnoty hodnotou x.

### Spojení dat

Nyní bychom chtěli všechny tři naše tabulky spojit do jedné. Nejprve si ukážeme, jak spojit tabulky **pod sebe**. Výsledná tabulka tedy bude mít stále **tři sloupce** a **počet řádků bude odpovídat součtu počtu řádků všech tří tabulek**. V SQL používáme pro danou operaci klíčové slovo `UNION`.

Začneme s tím, že každou tabulku uložíme do `DataFrame` s tím, že vyhodíme studenty, kteří na maturitu nedorazili.

```py
u202 = pandas.read_csv('u202.csv').dropna()
u203 = pandas.read_csv('u203.csv').dropna()
u302 = pandas.read_csv('u302.csv').dropna()
```

Pokud chceme tyto tři DataFrame spojit do jednoho, můžeme použít funkci `concat`.

```py
maturita = pandas.concat([u202, u203, u302])
```

Pozor ale na to, že v takto vzniklém DataFrame se nám **rozbije index**, protože se prostě spojí za sebe indexy jednotlivých tabulek. Pokud chceme, aby `pandas` při spojování index přepočítal, musíme nastavit hodnotu parametru `ignore_index` na `True`.

```py
maturita = pandas.concat([u202, u203, u302], ignore_index=True)
```

To už je lepší. Stále nám však zůstává jeden problém. Po spojení tabulek do jedné už nevíme, kdo maturoval v jaké místnosti. Tuto informaci si proto doplníme do původních tří tabulek jako nový sloupeček. Až poté tabulky spojíme do jedné.

```py
u202['mistnost'] = 'u202'
u203['mistnost'] = 'u203'
u302['mistnost'] = 'u302'
maturita = pandas.concat([u202, u203, u302], ignore_index=True)
```

Takto už nám vznikla pěkná vyčištěná tabulka. Uložme si ji do CSV, ať ji nemusíme vyrábět pořád znova. Nebudeme ukládat index, protože ten si vždycky necháme vyrobit automaticky.

```py
maturita.to_csv('maturita.csv', index=False)
```

Výslednou tabulku si můžete stáhnout jako soubor [maturita.csv](assets/maturita.csv).
