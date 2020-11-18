V předchozí lekci jsme si ukázali, jak v `pandas` vytváříme DataFrame a jak z něj můžeme vybírat data pomocí různých způsobů dotazování. Nyní se posuneme o kus dále a ukážeme si, jak můžeme s `DataFrame` dělat složitější operace jako je filtrování chybějících hodnot, spojování a agregace.

## Maturita

Abychom měli nějaký praktický příklad k procvičování, použijeme fiktivní data z výsledků maturitních zkoušek během jednoho týdne na nějakém menším gymnáziu. Maturita se odehrává ve třech místnostech: U202, U203 a U302. Máme tedy tři tabulky dat, z každé místnosti jednu. Níže si můžete prohlédnout příklad tabulky z místnosti U202. Všechny tabulky jsou ke stažení zde: [u202.csv](assets/u202.csv), [u203.csv](assets/u203.csv), [u302.csv](assets/u302.csv).

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

Tajemná zkratka `ID` (zkratka od identifier) v prvním sloupci obsahuje číslo studenta.

## Práce s chybějícími hodnotami

V praxi se poměrně často setkáme s tím, že v datovém setu některé hodnoty chybí. Můžeme si například všimnout, že v tabulce U202 dvěma studentům chybí známka. To může znamenat, že se doma hrůzou zhroutili a na maturitu ani nedorazili. Na takové případy je třeba být připraven.

V `pandas`, ale i obecně v datové analýze, je možné se s chybějícími daty vypořádat různými způsoby:

1. Nahradit je za jiné výchozí hodnoty.
1. Odstranit všechny řádky s chybějícími daty z datového setu.
1. Vyčlenit je do separátního datasetu a zpracovat je zvlášť.

Načtěme si naši první tabulku jako DataFrame.

```pycon
>>> import pandas
>>> u202 = pandas.read_csv('u202.csv')
```

Pokud Pandas narazí na prázdnou buňku, vloží místo ní do tabulky speciální hodnotu `NaN`, se kterou už jsme se setkali.

Série obsahují metodu `isnull()`, která vrátí pravdivostní sérii s hodnotou `True` všude tam, kde v původní sérii chybí hodnota. Metoda `notnull()` pracuje přesně opačně. Vrátí pravdivostní sérii s hodnotami `True` všude tam, kde v původní sérii hodnota nechybí.

```pycon
>>> u202['znamka'].isnull() 
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

Tyto metody můžeme využít například k tomu, abychom získali všechna data, kde chybí hodnota ve sloupečku *známka*.

```pycon
>>> u202[u202['znamka'].isnull()]      
   cisloStudenta  predmet  znamka den
0              1   Chemie     NaN  pá
9              9  Dějepis     NaN  pá
```

Další užitečné metody na práci s chybějícími hodnotami najdeme na DataFrame.

1. `dropna()` \- vrátí datový set očištěn od chybějících dat
1. `dropna(ax`is=1) - odstraní všechny sloupce, které obsahují chybějící data
1. `fillna(x)` \- nahradí všechna chybějící data a hodnoty hodnotou x

## Spojení dat

Nyní bychom chtěli všechny tři naše tabulky spojit do jedné. Nejprve si ukážeme, jak spojit tabulky pod sebe. Výsledná tabulka tedy bude mít stále tři sloupce a počet řádků bude odpovídat součtu počtu řádků všech tří tabulek. V SQL používáme pro danou operaci klíčové slovo `UNION`.

Začneme s tím, že každou tabulku uložíme do `DataFrame` s tím, že vyhodíme studenty, kteří na maturitu nedorazili.

```pycon
>>> u202 = pandas.read_csv('u202.csv').dropna()
>>> u203 = pandas.read_csv('u203.csv').dropna()
>>> u302 = pandas.read_csv('u302.csv').dropna()
```

Pokud chceme tyto tři DataFrame spojit do jednoho, můžeme použít funkci `concat`

```pycon
>>> maturita = pandas.concat([u202, u203, u302])
```

Pozor ale na to, že v takto vzniklém DataFrame se nám **rozbije index**, protože se prostě spojí za sebe indexy jednotlivých tabulek. Pokud chceme, aby Pandas při spojování index přepočítal, musíme nastavit hodnotu parametru `ignore_index` na `True`.

```pycon
>>> maturita = pandas.concat([u202, u203, u302], ignore_index=True)
```

To už je lepší. Stále nám však zůstává jeden problém. Po spojení tabulek do jedné už nevíme, kdo maturoval v jaké místnosti. Tuto informaci si proto doplníme do původních tří tabulek jako nový sloupeček. Až poté tabulky spojíme do jedné.

```pycon
>>> u202['mistnost'] = 'u202'
>>> u203['mistnost'] = 'u203'
>>> u302['mistnost'] = 'u302'
>>> maturita = pandas.concat([u202, u203, u302], ignore_index=True)
```

Takto už nám vznikla pěkná vyčištěná tabulka. Uložme si ji do CSV, ať ji nemusíme vyrábět pořád znova. Nebudeme ukládat index, protože ten si vždycky necháme vyrobit automaticky.

```pycon
>>> maturita.to_csv('maturita.csv', index=False)
```

Výslednou tabulku si můžete stáhnout jako soubor [maturita.csv](assets/maturita.csv).

## Joinování dat

Pandas však umí `DataFrame` také mergovat, což odpovídá SQL příkazu JOIN. Nyní si ukážeme, jak na to.

Naše výsledky byly anonymní. Pokud bychom ale chtěli vytisknout maturitní vysvědčení, potřebujeme k číslům studenta zjistit jejich jména. Jména najdeme v samostatné tabulce [jmena.csv](assets/jmena.csv). Načtěme si jej jako `DataFrame`.

```pycon
>>> jmena = pandas.read_csv('jmena.csv')
>>> jmena.head()
   cisloStudenta             jméno
0              1    Jana Zbořilová
1              2      Lukáš Jurčík
2              3       Pavel Horák
3              4     Pavel Kysilka
4              5  Kateřina Novotná
```

U operace JOIN jsou důležité dvě věci:

- Podle jakého sloupce (nebo jakých sloupců) dvě různé tabulky spojujeme.
- Co udělat v případě, že pro jeden řádek nemám ve druhé tabulce odpovídající hodnotu.

Join tabulek se v Pandase dělá pomocí funkce `merge` (dokumentaci k ní je [zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)). Ve výchozím nastavení funkce `merge` provádí spojení podle sloupců, které mají shodný název. V našem případě mají oba `DataFrame` sloupec `cisloStudenta`, je tedy použit tento sloupec, což je přesně ten sloupec, podle kterého bychom je chtěli spojit.

Ve výchozím nastavení funkce `merge()` ponechá pouze řádky, které mají záznamy v obou tabulkách. V SQL bychom tuto operaci označili jako INNER JOIN. 

```pycon
>>> propojenyDF = pandas.merge(u202, jmena)
>>> propojenyDF.head()
   cisloStudenta           predmet  znamka den           jmeno
0              1            Chemie     NaN  pá  Jana Zbořilová
1              2           Dějepis     3.0  pá    Lukáš Jurčík
2              2  Společenské vědy     2.0  pá    Lukáš Jurčík
3              3        Matematika     2.0  út     Pavel Horák
4              3            Chemie     5.0  út     Pavel Horák
```

Pokud by například nějaký student nebyl uvedený v tabulce se studenty, jeho maturitní výsledek by zmizel. U nového `DataFrame` bychom tedy měli zkontrolovat, zda má `spojenyDF` stejný počet řádků jako `u202`.

```pycon
>>> u202.shape              
(15, 4)
>>> spojenyDF.shape
(15, 5)
```

Zde vidíme, že data jsou zřejmě v pořádku.

Dále připojíme tabulku [predsedajici.csv](assets/predsedajici.csv), kde máme vypsané předsedy maturitních komusí. Tu si opět načteme jako `DataFrame`.

```pycon
>>> preds = pandas.read_csv('predsedajici.csv')
```

Zkusme tabulky spojit jako předtím.

```pycon
>>> novyPropojenyDF = pandas.merge(propojenyDF, preds)
>>> novyPropojenyDF.head()
Empty DataFrame
Columns: [den, datum, jmeno, cisloStudenta, predmet, znamka]
Index: []
```

Tentokrát jsme příliš neuspěli, spojený `DataFrame` je prázdný. Proč tomu tak je? Protože v obou `DataFrame` máme sloupec `jmeno`, v jednom případě však jde o jméno studenta a ve druhém o jméno předsedy komise. To ale `pandas` samozřejmě neví. Proto mu musíme říct, že chceme data spojit pouze podle sloupce `den`.

```pycon
>>> novyPropojenyDF = pandas.merge(preds, spojenyDF, on=['den'])
>>> novyPropojenyDF.head()
       datum           jmeno_x den  cisloStudenta     predmet  znamka          jmeno_y
0  21.5.2019  Marie Zuzaňáková  út              3  Matematika     2.0      Pavel Horák
1  21.5.2019  Marie Zuzaňáková  út              3      Chemie     5.0      Pavel Horák
2  22.5.2019     Petr Ortinský  st             10      Chemie     2.0  Miroslav Bednář
3  22.5.2019     Petr Ortinský  st             10     Dějepis     5.0  Miroslav Bednář
4  22.5.2019     Petr Ortinský  st             11  Matematika     1.0  Ivana Dvořáková
```

Zatím to vypadá dobře. Pokud se ovšem podíváme na `shape`, něco nám tady nehraje.

```pycon
>>> novyPropojenyDF.shape   
(7, 7)
```

Najednou máme v tabulce pouze 12 řádků, některé tedy zmizely. To znamená, že funkce `merge()` nenašla pro všechna zkoušení odpovídajícího předsedu. Jak je to možné? Zkusme nyní říct funkci `merge()`, aby nám zachovala v prvním `DataFrame` ty řádky, pro které nenajde odpovídající záznam. Této operaci se v jazyce SQL říká LEFT OUTER JOIN. My ho provede tak, že funkci `merge()` jako parametr `how` zadáme hodnotu `left`.

```pycon
>>> novyPropojenyDF = pandas.merge(spojenyDF, preds, on=['den'], how="outer") 
>>> novyPropojenyDF.shape
(17, 7)
```

Tentokrát jsme již o data nepřišli, ale kde se stala chyba? Zkusme si zobrazit ty řádky, které se nepodařilo propojit. Poznáme je tak, že mají prázdný sloupec `datum`.

```pycon
>>> novyPropojenyDF[novyPropojenyDF["datum"].isnull()]
   cisloStudenta     predmet  znamka den           jmeno_x datum jmeno_y
7            5.0     Dějepis     1.0  po  Kateřina Novotná   NaN     NaN
8            7.0     Dějepis     4.0  po       Vasil Lácha   NaN     NaN
9            8.0  Matematika     2.0  po    Alexey Opatrný   NaN     NaN
```

Nyní jsme již na stopě problému. Z nějakého důvodu nám nefunguje propojení v případě, že ve sloupci `den` je hodnota `po`. Po chvíli zkoumání zjistíme, že za chybu může nenápadná mezera, která je ve sloupci `den` za hodnotou `po` v souboru `prednasejici.csv`. Ať už vznikla chyba překlepem nebo nějakou jinou chybou, takové věci se bohužel stávají a proto při práci s daty musíme neustále kontrolovat, zda jsme nějako operací o část dat nepřišli.

Poslední nepříjemností, na kterou se podíváme, je to, že sloupce `jmeno` se automaticky přejmenovaly, aby neměly v tabulce stejný název. Zde můžeme použít metodu `rename`, abychom sloupečky přejmenovali na něco smysluplného.

```pycon
>>> novyPropojenyDF = novyPropojenyDF.rename(columns={'jméno_x': 'jméno', 'jméno_y': 'předs'})
```
## Grupování

Z databází známe kromě UNION a JOIN také operaci GROUP BY. V Pandase ji provedeme tak, že pomocí metody `groupby` vyrobíme z `DataFrame` speciální objekt `DataFrameGroupBy`. Dejme tomu, že chceme grupovat podle sloupečku `mistnost`.

```pycon
>>> maturita2.groupby('mistnost')
<pandas.core.groupby.generic.DataFrameGroupBy object at 0x7f96153a1cf8>
```

Na tomto speciálním objektu pak můžeme používat různé agregační funkce. Nejjednodušší je funkce `count`

```pycon
>>> maturita2.groupby('místnost').count()
          jméno  předmět  známka  den  datum  předs
místnost
u202         13       13      13   13     13     13
u203         13       13      13   13     13     13
u302         12       12      12   12     12     12
```

Další užitečné agregační funkce jsou například

- `sum` \- součet hodnot
- `max` \- maximální hodnota
- `min` \- minimální hodnota
- `first` \- první hodnota
- `last` \- poslední hodnota
- `mean` \- průměr z hodnot
- `median` \- medián z hodnot
- `var` \- rozptyl hodnot
- `std` \- standardní odchylka hodnot
- `all` \- `True`, pokud jsou všechny hodnoty `True`
- `any` \- `True`, pokud je alespoň jedna z hodnot `True`

Nemusíme samozřejmě grupovat přes všechny sloupečky. Vybereme si pouze ty, které nás zajímají. Zkusme například spočítat průměrnou známku z jednotlivých předmětů.

```pycon
>>> maturita2.groupby('predmet')['znamka'].mean()
```

Všimněte si, že takto obdržíme sérii, nikoliv `DataFrameU. Pozornější z vás možná tuší, že abychom získali DataFrame, musíme psát

```pycon
>>> maturita2.groupby('předmět')[['známka']].mean()
```

Pomocí agregací můžeme vyřešit i náš problém s nákupy. Pokud máme stále načtený `Data Frame` `nakupy`, můžeme použít funkci `groupby` podle jména a následně spočítat sumu nákupů pomocí `.sum()`.

```pycon
>>> nakupy.groupby("Jméno")["Částka v korunách"].sum()
Jméno
Libor    124
Míša     160
Ondra    500
Pavla     50
Petr     539
Zuzka     80
Name: Částka v korunách, dtype: int64
```