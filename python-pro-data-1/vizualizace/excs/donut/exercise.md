---
title: Prstencový graf
demand: 3
---

Nyní si zkus vytvořit :term{cs="prstencový graf" en="donut chart"}. Tento graf je "příbuzným" :term{cs="koláčového grafu" en="pie chart"}, ale s tím rozdílem, že méně zkresluje hodnoty, které jsou v něm obsaženy. `seaborn` tento typ grafu nemá naprogramovaný, vyzkoušíme tedy `matplotlib`. Proto do svého notebooku přidej řádek s jeho importem.

```py
import matplotlib.pyplot as plt
```

Vyzkoušíme si pomocí prstencového grafu zobrazit poměr výživných látek, a to konkrétně pro čokoládu s `fdc_id` 885174. Vytvoř tedy tabulku `data_pie_plot` (použij prosím tento název, aby ti fungovalo doplnění do kódu níže), do které ulož z tabulky `food_merged_brands` řádky z `fdc_id` rovné 885174 (tmavá čokoláda s pistáciemi). Dále ponech v tabulce pouze řádky, které ve sloupci `nutrient_name` mají hodnotu z následujícího seznamu. Jinak totiž hrozí, že bychom některé výživné látky měli započítané dvakrát (např. protože vláknina a cukr patří mezi karbohydráty).

```py
["Carbohydrate, by difference", "Total lipid (fat)", "Protein", "Potassium, K", "Iron, Fe", "Calcium, Ca"]
```

Aby graf nebyl zkreslený, je potřeba převést všechna data na stejné jednotky, např. na grafy. To uděláme ve třech krocích.

Využijeme strukturu označovanou jako :term{cs="slovník" en="dict"}, do které vložíme dvojice hodnot: označení jednotky a číslo, kterou bychom každou jednotku rádi násobili. Abychom převedli jednotky na grafy, budeme miligramy násobit číslem 0.001. Poté použijeme pro sloupec `unit_name` metodu `map`. Tato metody na základě hodnoty ve sloupci `unit_name` vybere hodnotu ze slovníku, např. pokud je ve sloupci `unit_name` hodnota `MG`, metoda vybere hodnotu 0.001. Nakonec vynásobíme mezi sebou sloupci `amount` a `coefficient`, takže ve sloupci `amount` máme hodnotu v gramech.

```py
# Definujeme převodní koeficienty pro jednotky hmotnosti (z miligramů na gramy)
coefficient = {"G": 1, "MG": 0.001}

# Vytvoříme nový sloupec "coefficient" s koeficienty na základě jednotky ve sloupci "unit_name"
data_pie_plot["coefficient"] = data_pie_plot["unit_name"].map(coefficient)

# Přepočítáme množství živin na gramy pomocí koeficientů
data_pie_plot["amount"] = data_pie_plot["amount"] * data_pie_plot["coefficient"]
```

Nyní pokračujme s tvorbou grafu. Při práci s `matplotlib` často začínáme tím, že vytvoříme grafickou plochu a osu pro kreslení. Do osy totiž budeme chtít vložit graf a legendu a chceme, aby byly pohromadě.


```py
fig, ax = plt.subplots()
```

Vykreslíme koláčový graf s množstvím jednotlivých živin. Jako první parametr vlož sloupec (sérii) `amount` z tabulky `data_pie_plot`. Parametr `wedgeprops` je opět slovník, pomocí kterého určujeme "šířku" prstence. Nyní již můžeš graf vykreslit, zkus tedy experimentovat s různými hodnotami šířky prstence, tj. místo 0.3 zkus 0.2 nebo 0.1.

```py
ax.pie(__________, wedgeprops={"width": 0.3})
```

Nyní přidáme legendu s názvy živin, umístěnou vlevo od středu grafu. Jako první parametr, tj. texty legendy, vlož sloupec `nutrient_name` z tabulky `data_pie_plot`. Parametr `loc` určuje, kde se bude legendy nacházet. Parametry `loc="center left"` a `bbox_to_anchor=(1, 0.5)` umístí legendu vlevo od středu a mimo osu grafu.

```py
ax.legend(__________, loc="center left", bbox_to_anchor=(1, 0.5))
```

Nakonec nastav nadpis grafu. Pomocí metody `ax.set_title` nastav nadpis, do volání metody vlož nadpis, který by se ti pro graf líbil. 

::fig[Přiklad výsledku]{src=assets/output.png}

:::solution

```py
import matplotlib.pyplot as plt

nutrient_list = ["Carbohydrate, by difference", "Total lipid (fat)", "Protein", "Potassium, K", "Iron, Fe", "Calcium, Ca"]
data_pie_plot = food_merged_brands[(food_merged_brands["fdc_id"] == 885174) & food_merged_brands["nutrient_name"].isin(nutrient_list)]

# Definujeme převodní koeficienty pro jednotky hmotnosti (z miligramů na gramy)
coefficient = {"G": 1, "MG": 0.001}

# Vytvoříme nový sloupec "coefficient" s koeficienty na základě jednotky ve sloupci "unit_name"
data_pie_plot["coefficient"] = data_pie_plot["unit_name"].map(coefficient)

# Přepočítáme množství živin na gramy pomocí koeficientů
data_pie_plot["amount"] = data_pie_plot["amount"] * data_pie_plot["coefficient"]

fig, ax = plt.subplots()
ax.pie(data_pie_plot["amount"], wedgeprops={"width": 0.3})
ax.legend(data_pie_plot["nutrient_name"], loc="center left", bbox_to_anchor=(1, 0.5))
```

:::
