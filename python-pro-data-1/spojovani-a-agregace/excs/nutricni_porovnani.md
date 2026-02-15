---
title: Nutriční porovnání kategorií
demand: 4
---

Metoda `groupby` umožňuje agregaci podle více sloupců. V takovém případě vložíme názvy sloupců do seznamu.

```py
tabulka.groupby(["sloupec_1", "sloupec_2"])["sloupec_hodnot"].mean()
```

Výsledkem je série s **víceúrovňovým** indexem, kde každá kombinace hodnot ze sloupců `sloupec_1` a `sloupec_2` tvoří jeden řádek.

V tomto příkladu se podíváme na průměrné obsahy výživných látek v jednotlivých kategoriích. Současně i zjistíme, kolik máme pro každou kombinaci kategorie a výživné látky záznamů, abychom neměli naše závěry zkreslené příliš malým množstvím dat.

Nejprve si pomocí dotazu vytvoř tabulku `food_merged_brands_nutrients`, která bude obsahovat pouze řádky s výživnými látkami `Protein`, `Sugars, total including NLEA` a `Fiber, total dietary`. Využij metodu `isin()`.

Poté proveď agregaci podle sloupců `nutrient_name` a `branded_food_category` s využitím metody `agg`. Pro sloupec `amount` vypočítej průměr (`mean`) a počet (`count`). Tak zjistíš průměrný obsah výživných látek v jednotlivých kategoriích a zároveň kolik záznamů pro jednotlivé výživné látky v jednotlivých kategoriích máme.

1. Která kategorie má nejvyšší průměrný obsah cukru?
1. Podívej se na výsledky. Není některý z nich zkreslený příliš nízkým počtem záznamů?
1. Zkus zadat sloupce v metodě `groupby` v opačném pořadí, tj. nejprve `branded_food_category` a poté `nutrient_name`. Porovnej výsledky. Jak se liší struktura výsledné tabulky? Na zamyšlení: Uvažuj dvě různá zadání. První: "Zjisti, která kategorie má nejvyšší průměrné množství jednotlivých výživných látek." Druhé: "Zjisti, která výživná látka má nejvyšší průměrné zastoupení v jednotlivých kategoriích." Pro které zadání by bylo vhodnější které pořadí sloupců?

:::solution
```py
import pandas as pd

food_merged_brands = pd.read_csv("food_merged_brands.csv")

nutrients = ["Protein", "Sugars, total including NLEA", "Fiber, total dietary"]
food_merged_brands_nutrients = food_merged_brands[food_merged_brands["nutrient_name"].isin(nutrients)]

nutrient_agg = food_merged_brands_nutrients.groupby(["nutrient_name", "branded_food_category"]).agg({"amount": ["mean", "count"]})

nutrient_agg_reverse = food_merged_brands_nutrients.groupby(["branded_food_category", "nutrient_name"]).agg({"amount": ["mean", "count"]})
```
:::
