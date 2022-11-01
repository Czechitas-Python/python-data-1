## Agregace a spojování

V předchozí lekci jsme si ukázali, jak v `pandas` vytváříme `DataFrame` a jak z něj můžeme vybírat data pomocí různých způsobů dotazování. Nyní se posuneme o kus dále a ukážeme si, jak můžeme s `DataFrame` dělat složitější operace jako je filtrování chybějících hodnot, spojování a agregace.

### Maturita

Abychom měli nějaký praktický příklad k procvičování, použijeme fiktivní data z výsledků maturitních zkoušek během jednoho týdne na nějakém menším gymnáziu. Maturita se odehrává ve třech místnostech: U202, U203 a U302. Máme tedy tři tabulky dat, z každé místnosti jednu. Níže si můžete prohlédnout příklad tabulky z místnosti U202. Všechny tabulky jsou ke stažení zde: [u202.csv](assets/u202.csv), [u203.csv](assets/u203.csv), [u302.csv](assets/u302.csv).

Funkce `read_csv()` knihovny `pandas` umí stáhnout CSV soubor rovnou z internetu.

```py
import pandas

u202 = pandas.read_csv("https://kodim.cz/cms/assets/kurzy/python-data-1/python-pro-data-1/agregace-a-spojovani/u202.csv")
u203 = pandas.read_csv("https://kodim.cz/cms/assets/kurzy/python-data-1/python-pro-data-1/agregace-a-spojovani/u203.csv")
u302 = pandas.read_csv("https://kodim.cz/cms/assets/kurzy/python-data-1/python-pro-data-1/agregace-a-spojovani/u302.csv")
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

### Propojení dat

`pandas` však umí `DataFrame` také propojit, což odpovídá SQL příkazu `JOIN`. Nyní si ukážeme, jak na to. U výsledné tabulky je důležité, že bude mít **více sloupců**, počet řádků závisí na konkrétním typu operace a na samotných datech, jak ještě uvidíme.

Naše výsledky byly anonymní. Pokud bychom ale chtěli vytisknout maturitní vysvědčení, potřebujeme k číslům studenta zjistit jejich jména. Jména najdeme v samostatné tabulce [studenti.csv](assets/studenti.csv). Načtěme si jej jako `DataFrame`.

```py
studenti = pandas.read_csv('https://kodim.cz/cms/assets/kurzy/python-data-1/python-pro-data-1/agregace-a-spojovani/studenti.csv')
print(studenti.head())
```

```shell
   cisloStudenta             jméno
0              1    Jana Zbořilová
1              2      Lukáš Jurčík
2              3       Pavel Horák
3              4     Pavel Kysilka
4              5  Kateřina Novotná
```

U operace `JOIN` jsou důležité dvě věci:

- **Podle jakého sloupce** (nebo jakých sloupců) dvě různé tabulky propojujeme.
- Co udělat v případě, že pro nějaké řádky **nemám ve druhé tabulce odpovídající hodnotu**.

Propojení tabulek se v `pandas` dělá pomocí funkce `merge` (dokumentaci k ní je [zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)). Ve výchozím nastavení funkce `merge` provádí spojení podle sloupců, které mají shodný název. V našem případě mají oba `DataFrame` sloupec `cisloStudenta`, je tedy použit tento sloupec. Je to přesně ten sloupec, podle kterého bychom je chtěli spojit.

Ve výchozím nastavení funkce `merge()` ponechá pouze řádky, které mají záznamy v obou tabulkách. V SQL bychom tuto operaci označili jako `INNER JOIN`.

```py
propojeny_df = pandas.merge(u202, studenti)
print(propojeny_df.head())
```

```shell
   cisloStudenta           predmet  znamka den           jmeno
0              1            Chemie     NaN  pá  Jana Zbořilová
1              2           Dějepis     3.0  pá    Lukáš Jurčík
2              2  Společenské vědy     2.0  pá    Lukáš Jurčík
3              3        Matematika     2.0  út     Pavel Horák
4              3            Chemie     5.0  út     Pavel Horák
```

Pokud by například nějaký student nebyl uvedený v tabulce se studenty, jeho maturitní výsledek by zmizel. U nového `DataFrame` bychom tedy měli zkontrolovat, zda má `spojenyDF` stejný počet řádků jako `u202`.

```py
print(u202.shape)
```

```shell
(15, 4)
```

```py
print(propojeny_df.shape)
```

```shell
(15, 5)
```

Zde vidíme, že data jsou zřejmě v pořádku.

Dále připojíme tabulku [predsedajici.csv](assets/predsedajici.csv), kde máme vypsané předsedy maturitních komisí. Tu si opět načteme jako `DataFrame`.

```py
preds = pandas.read_csv('https://kodim.cz/cms/assets/kurzy/python-data-1/python-pro-data-1/agregace-a-spojovani/predsedajici.csv')
```

Zkusme tabulky spojit jako předtím.

```py
novy_propojeny_df = pandas.merge(propojeny_df, preds)
print(novy_propojeny_df.head())
```

```shell
Empty DataFrame
Columns: [den, datum, jmeno, cisloStudenta, predmet, znamka]
Index: []
```

Tentokrát jsme příliš neuspěli, výsledný `DataFrame` je prázdný. Proč tomu tak je? Protože v obou `DataFrame` máme sloupec `jmeno`, v jednom případě však jde o jméno studenta a ve druhém o jméno předsedy komise. To ale `pandas` samozřejmě neví. Proto mu musíme říct, že chceme data spojit pouze podle sloupce `den`.

```py
novy_propojeny_df = pandas.merge(propojeny_df, preds, on=['den'])
print(novy_propojeny_df.head())
```

```shell
       datum           jmeno_x den  cisloStudenta     predmet  znamka mistnost          jmeno_y
0  21.5.2019  Marie Zuzaňáková  út              3  Matematika     2.0     u202      Pavel Horák
1  21.5.2019  Marie Zuzaňáková  út              3      Chemie     5.0     u202      Pavel Horák
2  22.5.2019     Petr Ortinský  st             10      Chemie     2.0     u202  Miroslav Bednář
3  22.5.2019     Petr Ortinský  st             10     Dějepis     5.0     u202  Miroslav Bednář
4  22.5.2019     Petr Ortinský  st             11  Matematika     1.0     u202  Ivana Dvořáková
```

Zatím to vypadá dobře. Pokud se ovšem podíváme na `shape`, něco nám tady nehraje.

```py
print(novy_propojeny_df.shape)
```

```shell
(10, 8)
```

Najednou máme v tabulce pouze 12 řádků, některé tedy zmizely. To znamená, že funkce `merge()` nenašla pro všechna zkoušení odpovídajícího předsedu. Jak je to možné? Zkusme nyní říct funkci `merge()`, aby nám zachovala v prvním `DataFrame` ty řádky, pro které nenajde odpovídající záznam. Této operaci se v jazyce SQL říká LEFT OUTER JOIN. My ho provede tak, že funkci `merge()` jako parametr `how` zadáme hodnotu `left`.

```py
novy_propojeny_df = pandas.merge(propojeny_df, preds, on=['den'], how="outer")
print(novy_propojeny_df.shape)
```

```shell
(14, 8)
```

Tentokrát jsme již o data nepřišli, ale kde se stala chyba? Zkusme si zobrazit ty řádky, které se nepodařilo propojit. Poznáme je tak, že mají prázdný sloupec `datum`.

```py
print(novy_propojeny_df[novy_propojeny_df["datum"].isnull()])
```

```shell
   cisloStudenta     predmet  znamka den mistnost           jmeno_x datum jmeno_y
5            5.0     Dějepis     1.0  po     u202  Kateřina Novotná   NaN     NaN
6            7.0     Dějepis     4.0  po     u202       Vasil Lácha   NaN     NaN
7            8.0  Matematika     2.0  po     u202    Alexey Opatrný   NaN     NaN
```

Nyní jsme již na stopě problému. Z nějakého důvodu nám nefunguje propojení v případě, že ve sloupci `den` je hodnota `po`. Po chvíli zkoumání zjistíme, že za chybu může nenápadná mezera, která je ve sloupci `den` za hodnotou `po` v souboru `prednasejici.csv`. Ať už vznikla chyba překlepem nebo nějakou jinou chybou, takové věci se bohužel stávají a proto při práci s daty musíme neustále kontrolovat, zda jsme nějako operací o část dat nepřišli.

Pokud nemáme možnost vstupní data opravit, můžeme použít funkci `strip()`, která z řetězce odstraní mezery (a další bílé znaky) na začátku a na konci. Tyto mezery jsou v drtivé většině případů způsobeny chybou a proto jejich odstraněním nic nezkazíme.

```py
preds["den"] = preds["den"].str.strip()
novy_propojeny_df = pandas.merge(propojeny_df, preds, on=['den'], how="outer")
print(novy_propojeny_df.shape)
```

```shell
(13, 8)
```

Poslední nepříjemností, na kterou se podíváme, je to, že sloupce `jmeno` se automaticky přejmenovaly, aby neměly v tabulce stejný název. Zde můžeme použít metodu `rename`, abychom sloupečky přejmenovali na něco smysluplného.

```py
novy_propojeny_df = novy_propojeny_df.rename(columns={'jmeno_x': 'jmeno', 'jmeno_y': 'predseda'})
```
### Agregace

Z databází známe kromě UNION a JOIN také operaci GROUP BY. V `pandas` ji provedeme tak, že pomocí metody `groupby` vyrobíme z `DataFrame` speciální objekt `DataFrameGroupBy`. Dejme tomu, že chceme grupovat podle sloupečku `mistnost`.

```py
maturita.groupby('mistnost')
```

Na tomto speciálním objektu pak můžeme používat různé agregační funkce. Nejjednodušší je funkce `count`

```py
print(maturita.groupby('mistnost').count())
```

```shell
          jméno  předmět  známka  den  datum  předs
místnost
u202         13       13      13   13     13     13
u203         13       13      13   13     13     13
u302         12       12      12   12     12     12
```

Další užitečné agregační funkce jsou například:

- `sum` \- součet hodnot,
- `max` \- maximální hodnota,
- `min` \- minimální hodnota,
- `first` \- první hodnota,
- `last` \- poslední hodnota,
- `mean` \- průměr z hodnot,
- `median` \- medián z hodnot,
- `var` \- rozptyl hodnot,
- `std` \- standardní odchylka hodnot,
- `all` \- `True`, pokud jsou všechny hodnoty `True`,
- `any` \- `True`, pokud je alespoň jedna z hodnot `True`.

Nemusíme samozřejmě grupovat přes všechny sloupečky. Vybereme si pouze ty, které nás zajímají. Zkusme například spočítat průměrnou známku z jednotlivých předmětů.

```py
print(maturita.groupby('predmet')['znamka'].mean())
```

Pomocí agregací můžeme vyřešit i náš problém s nákupy. Pokud máme stále načtený `Data Frame` `nakupy`, můžeme použít funkci `groupby` podle jména a následně spočítat sumu nákupů pomocí `.sum()`.

```py
nakupy = pandas.read_csv('nakupy.csv')
nakupy_celkem = nakupy.groupby("Jméno")["Částka v korunách"].sum()
print(nakupy_celkem)
```

```shell
Jméno
Libor    124
Míša     160
Ondra    500
Pavla     50
Petr     539
Zuzka     80
Name: Částka v korunách, dtype: int64
```

### Počítané sloupce

Občas je užitečné přidat nový sloupec, který obsahuje hodnotu vypočtenou na základě hodnot ostatních sloupců. Vraťme se například k naší tabulce s údaji o státech ve světě. Máme informaci o rozloze a počtu obyvatel, mohli bychom tedy přidal sloupec s hodnotou hustoty zalidnění (počet obyvatel na 1 km čtvereční), který získáme vydělením počtu obyvatel rozlohou země.

Pokud nemáme načtený soubor s daty, načteme si ho.

```py
staty = pandas.read_json("staty.json")
staty = staty.set_index("name")
```

Přidání nového sloupce je poměrně jednoduché. Před znaménko `=` vložíme proměnnou s `DataFrame` a do hranatých závorek vložíme název nového sloupce. Na pravou stranu umístíme výpočet. Ve výpočtu pracujeme s jednotlivými sloupci, v našem konkrétním případě vydělíme sloupec `population` sloupcem `area`.

```py
staty["population_density"] = staty["population"] / staty["area"]
```

**Poznámka:** `pandas` nás neupozorní, pokud sloupec již existuje, musíme si tedy dát pozor, abychom nepřepsali nějaký existující sloupec.

### Řazení

Data řadíme poměrně často. U běžeckého závodu nás zajímají ti nejrychlejší běžci, u položek v e-shopu ty nejlépe hodnocené, u projektu zase chceme vidět úkoly s nejbližším deadline. Abychom tyto hodnoty získali, musíme data seřadit. Ve světě databází pro to používáme klíčová slova `ORDER BY`, v `pandas` nám poslouží metoda `sort_values`. Jako její první parametr zadáváme sloupec (nebo seznam sloupců), podle kterého (kterých) řadíme.

```py
staty.sort_values(by="population")
```

Metoda `sort_values` standardně řadí vzestupně. Chceme-li řadit sestupně, zadáme jí parametr `ascending` a nastavíme ho na `False`.

```py
staty.sort_values(by="population", ascending=False)
```
