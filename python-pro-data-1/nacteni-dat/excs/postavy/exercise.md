---
title: Postavy
demand: 1
---

Stáhni si soubor [character-deaths.csv](assets/character-deaths.csv), který obsahuje informace o smrti některých postav z prvních pěti knih románové série Píseň ohně a ledu (*A Song of Ice and Fire*). Načti si data do tabulky a jako index použij sloupec `Name`.

### Jak pracovat s `.loc`

`.loc` vybírá řádky a sloupce podle **názvů**. Základní tvar vypadá takto:

```py
data.loc["Jméno řádku (index)", "Jméno sloupce"]
```

- první parametr se týká řádků – může to být jedna hodnota, seznam hodnot nebo řetězec ve formátu `"první řádek":"poslední řádek"`;
- druhý parametr se týká sloupců – funguje úplně stejně (jedna hodnota, seznam, rozsah).

Pár příkladů:

```py
# konkrétní řádek a sloupec – vrátí jednu hodnotu
data.loc["Goady", "Allegiances"]

# vybíráme všechny řádky od řádku Jeren do řádku Jodge (všechny sluopce)
data.loc["Jeren":"Jodge"]

# pro řádek Mordane vypisujeme sloupce GoT až SoS
data.loc["Mordane", "GoT":"SoS"]
```

Pokud druhý parametr vynecháš, dostaneš všechny sloupce. Stejně tak můžeš vynechat první parametr a vybírat pouze sloupce.

* Načti soubor do tabulky (DataFrame) a nastav sloupec `Name` jako index.
* Zobraz si sloupce, které tabulka má. Posledních pět sloupců tvoří zkratky názvů knih a informace o tom, jestli se v knize postava vyskytuje.
* Použij `loc` ke zjištění informací o smrti postavy jménem "Hali".
* Použij `loc` k zobrazení řádků mezi "Gevin Harlaw" a "Gillam".
* Použij `loc` k zobrazení řádků mezi "Gevin Harlaw" a "Gillam" a sloupce `Death Year`.
* Použij `loc` k zobrazení řádků mezi "Gevin Harlaw" a "Gillam" a informace o tom, v jakých knihách se postava vyskytuje, tj. vypiš všechny sloupce mezi `GoT` a `DwD`.
