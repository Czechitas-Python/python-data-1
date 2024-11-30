---
title: Vítězové
demand: 3
---

Pokračuj v práci se souborem `votes.csv` z předchozího cvičení. Ve volbách ve Spojených státech je důležité pořadí kandidáta/kandidátky v každém ze států, protože tento vítěz(ka) získá hlasy tzv. volitelů pro daný stát. 

Pořadí na základě číselného sloupce je možné vypočítat pomocí metody `rank()`. Metoda `rank()` má parametr `ascending`, který určuje, jestli pořadí 1 dostane nejnižší nebo nejvyšší číslo. Pokud napíšeme `rank(ascending=False)`, získá pořadí 1 nejvyšší hodnota ze sloupce, pořadí 2 druhá nejvyšší atd.

My ale chceme pořadí určit pro každý stát a rok samostatně, aby číslo 1 měl(a) vždy vítěz(ka) v daném státu a daném roce. Pokud bychom chtěli určit pořadí pro každý stát a rok samostatně, je možné využít metodu `rank()` v kombinaci s metodou `groupby()`. Jako `"sloupec_poradi"` zadáme sloupec, ze kterého chceme pořadí spočítat. Níže vidíš příklad, jak metodu použít.

```py
data["jmeno_noveho_sloupce"] = data.groupby(["sloupec_1", "sloupec_2"])["sloupec_poradi"].rank()
```

Následně vytvoř tabulku, která bude obsahovat vítěznou stranu v jednotlivých státech ve volbách v roce 2020.


:::solution
```py
import pandas as pd
data = pd.read_csv('votes.csv')
data["rank"] = data.groupby(["state", "year"])["candidatevotes"].rank(ascending=False)
winners_by_state_2020 = data[(data1["rank"]==1) & (data["year"]==2020)][["state", "party_detailed"]]
```
:::

