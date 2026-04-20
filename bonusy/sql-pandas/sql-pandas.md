## Podmínky v SQL a Pandas

Zatím jsme pracovali s podmíněným výběrem v pandas pomocí hranatých závorek a logických operátorů. Databázoví analytici dělají totéž v SQL pomocí klauzule `WHERE`. Podívejme se, jak si tyto dva přístupy odpovídají - každý úkol vyřešíme oběma způsoby.

### Dataset: bezpečnostní logy

Představme si, že pracujeme v týmu kybernetické bezpečnosti a máme tabulku zaznamenaných přihlášení do firemního systému za poslední týden. Každý řádek je jeden pokus o přihlášení. Data si [stáhni zde](assets/logy.csv) a načti příkazem:

```py
import pandas

logy = pandas.read_csv("logy.csv")
```

Sloupce jsou:
- `uzivatel` - uživatelské jméno
- `zeme` - kód země, ze které přišel pokus
- `pocet_pokusu` - kolik pokusů přihlášení proběhlo v dané relaci
- `uspesne` - zda bylo přihlášení nakonec úspěšné

### Jedna podmínka

Chceme zjistit, která přihlášení přišla ze zahraničí - tedy ze zemí mimo Českou republiku. To může být první signál k dalšímu prošetření.

V SQL bychom napsali:

```sql
SELECT *
FROM logy
WHERE zeme != 'CZ';
```

V pandas zapíšeme totéž takto:

```py
zahranicni = logy[logy["zeme"] != "CZ"]
zahranicni
```

```shell
  uzivatel zeme  pocet_pokusu  uspesne
3  hackerx   RU            47    False
4    novak   DE             1     True
6  bot2024   CN           120    False
8  bot2024   CN            98    False
```

Podmínka `logy["zeme"] != "CZ"` vrátí sérii hodnot `True`/`False` pro každý řádek, a hranatá závorka pak vybere jen ty řádky, kde je hodnota `True`.

### Podmínka AND

Zahraniční přihlášení nás zajímají, ale ne všechna. Skutečně podezřelé je to, kde navíc proběhlo hodně neúspěšných pokusů - to může být útok hrubou silou (*brute force attack*). Chceme tedy záznamy, kde přihlášení přišlo ze zahraničí **a zároveň** bylo neúspěšné.

V SQL spojíme podmínky klíčovým slovem `AND`:

```sql
SELECT *
FROM logy
WHERE zeme != 'CZ'
  AND uspesne = FALSE;
```

V pandas použijeme operátor `&`. Každou podmínku musíme uzavřít do závorek:

```py
podezrele = logy[(logy["zeme"] != "CZ") & (logy["uspesne"] == False)]
podezrele
```

```shell
  uzivatel zeme  pocet_pokusu  uspesne
3  hackerx   RU            47    False
6  bot2024   CN           120    False
8  bot2024   CN            98    False
```

**POZOR!** V pandas nikdy nepíš `and` (pythonové klíčové slovo), ale `&` (bitový operátor). Pandas potřebuje porovnat každý řádek zvlášť, a `and` to neumí - hodí chybu.

### Podmínka OR

Bezpečnostní tým chce také upozornění na všechny záznamy, které jsou buď podezřelé z důvodu množství pokusů, nebo pocházejí z rizikové země. Nevyžadujeme obojí najednou - stačí splnit alespoň jednu podmínku.

V SQL použijeme `OR`:

```sql
SELECT *
FROM logy
WHERE pocet_pokusu > 10
   OR zeme = 'CN';
```

V pandas operátor `|`:

```py
rizikove = logy[(logy["pocet_pokusu"] > 10) | (logy["zeme"] == "CN")]
rizikove
```

```shell
   uzivatel zeme  pocet_pokusu  uspesne
3   hackerx   RU            47    False
6   bot2024   CN           120    False
8   bot2024   CN            98    False
10  svoboda   CZ            15    False
```

**Poznámka:** Závorky kolem každé dílčí podmínky jsou povinné. Bez nich by Python špatně vyhodnotil prioritu operátorů a výsledek by byl chybný.

### Podmínka IN a NOT IN

Zatím jsme porovnávali hodnotu vždy s jednou konkrétní hodnotou. Pokud chceme porovnat s více hodnotami najednou, v SQL použijeme `IN` se seznamem:

```sql
SELECT *
FROM logy
WHERE zeme IN ('CZ', 'SK', 'DE');
```

V pandas k tomu slouží metoda `isin()`:

```py
domaci = logy[logy["zeme"].isin(["CZ", "SK", "DE"])]
domaci
```

```shell
   uzivatel zeme  pocet_pokusu  uspesne
0     novak   CZ             1     True
1     novak   CZ             1     True
2   svoboda   CZ             3    False
4     novak   DE             1     True
5    kratky   CZ             2     True
7     mares   CZ             1     True
9    martin   DE             2     True
10  svoboda   CZ            15    False
11 novakova   SK             1     True
```

Opačný případ - `NOT IN` - zapíšeme přidáním operátoru `~`, který celou sérii `True`/`False` obrátí:

```sql
SELECT *
FROM logy
WHERE zeme NOT IN ('CZ', 'SK', 'DE');
```

```py
bezpecne_zeme = ["CZ", "SK", "DE"]
zahranicni = logy[~logy["zeme"].isin(bezpecne_zeme)]
zahranicni
```

```shell
   uzivatel zeme  pocet_pokusu  uspesne
3   hackerx   RU            47    False
6   bot2024   CN           120    False
8   bot2024   CN            98    False
12  neznamy  NaN             3    False
13    kpbot   KP            88    False
```

### Chybějící hodnoty: IS NULL a IS NOT NULL

V datech se občas stane, že hodnota chybí úplně - třeba systém nezaznamenal, ze které země přihlášení přišlo. V SQL takové řádky najdeme přes `IS NULL`:

```sql
SELECT *
FROM logy
WHERE zeme IS NULL;
```

V pandas použijeme metodu `isna()`:

```py
neznama_zeme = logy[logy["zeme"].isna()]
neznama_zeme
```

```shell
   uzivatel zeme  pocet_pokusu  uspesne
12  neznamy  NaN             3    False
```

Opačný dotaz - záznamy, kde země je vyplněná - zapíšeme pomocí `notna()`, což odpovídá SQL `IS NOT NULL`:

```py
zname_zeme = logy[logy["zeme"].notna()]
```

### Přehled odpovídajících zápisů

| Operace | SQL | pandas |
|---|---|---|
| rovná se | `= 'CZ'` | `== "CZ"` |
| nerovná se | `!= 'CZ'` nebo `<> 'CZ'` | `!= "CZ"` |
| větší než | `> 10` | `> 10` |
| a zároveň | `AND` | `&` |
| nebo | `OR` | `\|` |
| je v seznamu | `IN (...)` | `df["s"].isin([...])` |
| není v seznamu | `NOT IN (...)` | `~df["s"].isin([...])` |
| hodnota chybí | `IS NULL` | `df["s"].isna()` nebo `df["s"].isnull()` |
| hodnota nechybí | `IS NOT NULL` | `df["s"].notna()` nebo `df["s"].notnull()` |
