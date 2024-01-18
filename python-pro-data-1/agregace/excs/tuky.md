---
title: Lipidy a tuky
demand: 3
---

Podívej se nyní na to, které kategorie potravin obsahují nejvíce lipidů (tuků). Nejprve pomocí dotazu vytvořit novou tabulku `food_merged_brands_lipid`, do které pomocí dotazu vlož pouze řádky, které mají jako název výživné látky hodnotu `Total lipid (fat)`. Poté proveď agregaci podle návu kategorie a seřaď výslednou tabulku tak, aby nahoře byly vidět kategorie s největším počtem tuků. Porovnej si výslednou tabulku s tabulkou `food_merged_brands_protein_agg`, kterou jsme vytvořili v rámci lekce. Podívej se, zda se některé kategorie objevují v obou tabulkách.


```py
import seaborn as sns
data = food_nutrient_pivot[food_nutrient_pivot["branded_food_category"] == "Cheese"]
g = sns.JointGrid(data=data, x="Protein", y="Energy")
g.plot_joint(sns.scatterplot, color="g")
```

```py
g.plot_marginals(sns.rugplot, height=0.5, color="g")
```
