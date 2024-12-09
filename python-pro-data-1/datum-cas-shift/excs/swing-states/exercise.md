---
title: Swing states
demand: 3
---

V případě amerických prezidentských voleb platí, že ve většině států dlouhodobě vítězí kandidáti jedné politické strany. Například v Kalifornii tradičně vítězí kandidáti Demokratické strany, zatímco v Texasu jsou úspěšní kandidáti Republikánské strany. Státy, kde se vítězné strany střídají, se označují jako _swing states_ (kolísavé státy). Tvým úkolem je spočítat pro jeden konkrétní stát, kolikrát se v něm změnila vítězná politická strana. Pokud jsi již řešil(a) příklad *Vítězové* z lekce o agregacích, můžeš použít výslednou tabulku. Pokud ne, můžeš se k příkladu vrátit nebo využít data v souboru [election-data.csv](assets/election-data.csv).

Soubor obsahuje následující důležité sloupce:

- `year`: rok voleb,  
- `state`: stát,  
- `party_simplified`: zjednodušené označení politické strany,  
- `rank`: pořadí kandidáta v rámci státu a roku

**Kroky k řešení:**

**Filtrace dat na vítězné kandidáty**:  
Vyber pouze řádky, kde hodnota ve sloupci `rank` je rovna `1`, tj. data o vítězích.

**Výběr dat pro stát PENNSYLVANIA**:  
Vytvoř tabulku obsahující pouze data za stát PENNSYLVANIA. Tabulku pojmenuj  `data_pen`, abys zachoval(a) původní data s vítězi ve všech státech.

**Přidání sloupce s vítězem předchozích voleb**:  
Vytvoř nový sloupec `party_simplified_previous_election`, který obsahuje politickou stranu vítěze z předchozích voleb. Tento sloupec lze vytvořit pomocí metody `shift()`:

```py
data_pen["party_simplified_previous_election"] = data_pen["party_simplified"].shift(1)
```

**Určení změn vítězné strany**:  
Přidej sloupec `swing`, který bude obsahovat hodnotu `True`, pokud se vítězná strana oproti minulým volbám změnila, a `False`, pokud zůstala stejná. Použij k tomu následující příkaz:

```py
data_pen["swing"] = data_pen["party_simplified"] != data_pen["party_simplified_previous_election"]
```

**Spočítání změn**:  
Počet změn vítězné strany zjistíš spočítáním řádků, kde je hodnota ve sloupci `swing` rovna `True`. Nezapomeň ignorovat prázdné hodnoty ve sloupci `party_simplified_previous_election` (např. pomocí metody `dropna()`), které se objeví u prvních voleb v datasetu.

Pro stát `PENNSYLVANIA` by mělo vyjít, že ke změně vítězné strany došlo celkem čtyřikrát: v letech 1980, 1992, 2016 a 2020. Tento výsledek ověř pomocí dotazu a metody `.sum()` na sloupci `swing`.

**Obecná analýza změn ve všech státech**:  
Pokud ti analýza funguje pro Pensylvanii, pokračuj výpočtem pro všechny státy. Přidej sloupec `party_simplified_previous_election` do původní tabulky dat, tentokrát s použitím metody `groupby()`, aby se hodnoty posunuly v rámci každého státu zvlášť:

```py
data["party_simplified_previous_election"] = data.groupby("state")["party_simplified"].shift(1)
```

**Spočítání změn ve všech státech**:  
Vytvoř pomocný sloupec `swing` podobným způsobem jako v předchozí části. Pomocí agregace spočítej, kolikrát se v jednotlivých státech změnila vítězná politická strana.

**Tip:** Porovnej rozdíl v použití metody `shift()` s a bez metody `groupby()` a ujisti se, že výsledky odpovídají očekávání. Nezapomeň opět odstranit řádky s neznámými hodnotami vítěze předchozích voleb.

:::solution
```py
import pandas as pd

data = pd.read_csv("election-data.csv")
data = data[data["rank"] == 1]

data_pen = data[data["state"] == "PENNSYLVANIA"]
data_pen["party_simplified_previous_election"] = data_pen["party_simplified"].shift()
data_pen = data_pen.dropna()
data_pen["swing"] = data_pen["party_simplified"] != data_pen["party_simplified_previous_election"]
data_pen[data_pen["swing"] == 1]

data_pen["swing"].sum()

data["party_simplified_previous_election"] = data.groupby("state")["party_simplified"].shift(1)
data["party_simplified_previous_election"] = data.groupby("state")["party_simplified"].shift(1)
data = data.dropna()
data["swing"] = data["party_simplified"] != data["party_simplified_previous_election"]
data.groupby("state")["swing"].sum().sort_values(ascending=False)
```
:::

