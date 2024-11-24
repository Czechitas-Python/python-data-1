---
title: Podgrafy
demand: 3
---

`matplotlib`, na kterém je `seaborn` postavený, umí vytvářet :term{cs="podgrafy" en="subplot"}, což znamená vložení více grafů do jednoho obrázku. Můžeme například ke grafu s průměrným množstvím proteinů v kategoriích potravin přidat graf s průměrným množstvím karbohydrátů.

Na začátku je potřeba přidat import `matplotlib`, abychom ho mohli využít ke tvorbě podgrafů.

```py
import matplotlib.pyplot as plt
```

Použijeme funkci `subplots`. První parametr znamená, kolik podgrafů chceme vytvořit na výšku, a druhý parametr, kolik na šířku. Pokud bychom chtěli dva grafy vedle sebe, použijeme hodnoty 1 a 2. Nakonec přidáme parametr `sharey`, aby oba grafy sdílely svislou osu grafu a neopakovaly se nám zbytečně názvy kategorií u obou grafů.

Funkce vrací několik hodnot. `fig` reprezentuje celý obrázek, `ax1` a `ax2` reprezentuje tzv. :term{cs="osy" en="axis"} grafu. Každá z podgrafů má svoji osu, tj. pokud máme dva podgrafy, budeme mít dvě osy.

```py
fig, (ax1, ax2) = plt.subplots(1, 2, sharey=True)
```

Data z tabulky `food_merged_brands_protein` vložíme do jednoho z podgrafů. Zápis je stejný jako během lekce, pouze do prvního řádku přidáme parametr `ax` a k němu hodnotu `ax1`, čímž zařídíme, že se tento graf vloží do prvního podgrafu. Popisek vodorovné osy nastavíme s využitím metody `set_xlabel` pro osu `ax1`.

```py
sns.barplot(food_merged_brands_protein, y="branded_food_category", x="amount", ax=ax1)
ax1.set_xlabel("Množství proteinů (g)")
```

Vytvoř tabulku `food_merged_brands_carb`, která z tabulky `food_merged_brands` vybere řádky, kde je ve sloupci `nutrient_name` hodnota `Carbohydrate, by difference`. Přidej graf, který zobrazuje průměrné množství karbohydrátů v jednotlivých kategoriích, jako druhý podgraf. Kód bude stejný jako v případě prvního podgrafu, pouze vyměň tabulku a použij osu `ax2`.

::fig[Přiklad výsledku]{src=assets/output.png}

Pokud ti připadá, že graf data zkresluje tím, že každý podgraf má svůj rozsah vodorovné osy, vyzkoušej parametr `sharex` u funkce `subplots()`.
