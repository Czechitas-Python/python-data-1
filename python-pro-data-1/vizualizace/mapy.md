## Čtení na doma: Mapové vizualizace

Pokud tvá data mají nějaký geografický prvek (např. vztahují se k nějakým státům nebo regionům), můžeš je zobrazit s využitím map. My si ukážeme použití knihovny `geopandas` a zobrazíme si mapu Evropy. Tu si musíš nejprve nainstalovat pomocí příkazu

```shell
pip install geopandas
```

nebo 

```shell
pip3 install geopandas
```

### Předem definované barvy

Dále si stáhni soubor [eu.csv](assets/eu.csv) a překopíruj ho do svého adresáře ve VS code. Soubor rozděluje státy světa na státy v eurozóně, státy EU mimo eurozónu a státy mimo EU. Nakonec si z webu [Natural Earth](https://www.naturalearthdata.com/) stáhni data o státech světa z [této stránky](https://www.naturalearthdata.com/downloads/10m-cultural-vectors/10m-admin-0-countries/) pomocí odkazu `Download data`. Data jsou zabalená v zip archivu, který si rozbal a celou rozbalenou složku přesuň do VS Code. Po přesunutí složky bys měl(a) ve VS Code vidět podobný výsledek jako na obrázku níže.

Nyní se můžeme pustit do psaní programu. Začneme importem a načtením dat. `geopandas` je rozšířením `pandas` pro geografická data, takže zde můžeme uplatnit své znalosti pro práci s tabulkami. Vytvoříme tedy novou tabulku `world_merged`, která propojí geografická data s daty o členství v EU.

```py
import geopandas as gpd
import matplotlib.pyplot as plt
import pandas as pd

shapefile_path = 'ne_10m_admin_0_countries/ne_10m_admin_0_countries.shp'
world = gpd.read_file(shapefile_path)

data = pd.read_csv("eu.csv")
world_merged = pd.merge(world, data, on="NAME")
```

Dále si přidáme sloupec `color`, který bude obsahovat barvu, pomocí které stát v mapě vykreslíme. K určení barvy můžeme zvolit metodu `map`, se kterou jsme již pracovali. (Její jméno je zde trochu nešťastné, název `map` se nijak nevztahuje k mapám, ale k tomu, že "mapujeme" jednu hodnotu na jinou hodnotu.) Státe v eurozóně (hodnota `eurozone`) vykreslíme zlatou barvou, státy EU mimo eurozónu (`other eu members`) modrou barvou a ostatní státy (`other countries`) šedou.

```py
world_merged["color"] = world_merged["EU"].map({"eurozone": "gold", "other eu members": "blue", "other countries": "grey"})
```

Protože výchozí velikost grafu je pro mapu příliš malá, použijeme funkci `plt.subplots()` pro vytvoření grafu a osy pro graf a parametrem `figsize` nastavíme velikost obrázku na 15x10 palců. Dále použijeme metodu `plot` s tím, že pro parametr `color` použijeme sérii `color` z tabulky `world_merged` a přidáme parametr `edgecolor`, kterým určíme barvu pro hranice států. Nakonec použijeme metody `set_xlim()` a `set_ylim`, kterými omezíme graf pouze na Evropu. Použitá čísla představují minimální a maximální zeměpisnou délku a šířku, která bude v grafu viditelná.

```py
fig, ax = plt.subplots(1, 1, figsize=(15, 10))
world_merged.plot(ax=ax, color=world_merged["color"], edgecolor='black')

ax.set_xlim([-25, 40])
ax.set_ylim([34, 71])
```

::fig[Mapa Evropy]{src=assets/eu.png}

### Barevná škála

Číselné hodnoty jsou na mapách obvykle prezentované pomocí barevné škály. My si zobrazíme tzv. [Index ekonomické svobody](https://www.heritage.org/index/), data z něj najdeš v souboru [economic_freedom_index.csv](assets/economic_freedom_index.csv). Data si načteme a propojíme s tabulkou `world`, jako jsme to udělali v předchozím případě. Rozdíl je pouze v tom, že v tabulce `freedom` je název státu ve sloupci `Country`.

```py
freedom = pd.read_csv("economic_freedom_index.csv")
world_merged = pd.merge(world, freedom, left_on="NAME", right_on="Country")
```

Dále vybereme berevnou škálu. Jejich přehled najdeš [v dokumentaci](https://matplotlib.org/stable/users/explain/colors/colormaps.html). Využijeme škálu, kde jsou nízké hodnoty červené a vysoké hodnoty zelené. Škálu si uložíme do proměnné `cmap`.

```py
cmap = plt.cm.RdYlGn
```

Poté vytvoříme pole `color`. Zatímco v předchozím případě jsme měli slovní pojmenování barev, které bylo možné vložit jako sérii do tabulky, nyní potřebujeme vícerozměrný datový typ, protože pro každou barvu potřebujeme tři čísla (množství červené, zelené a modré, které vytvoří správnou barvu). Pole je pro nás nový datový typ a jde o "jednodušší" obdobu tabulky. Pole je dvourozměrné, ale neobsahuje názvy sloupečků a indexy, které známe z tabulek. Pro vytvoření pole použijeme barevnou škálu `cmap`. Barevná škála očekává čísla v intervalu 0 až 1, vydělíme tedy hodnotu indexu číslem 100.

```py
color = cmap(world_merged["Overall Score"] / 100)
```

Mapu vytvoříme obdobným způsobem jako v předchozím případě. Tentokrát si namísto Evropy zobrazíme latinskou Ameriku.

```py
fig, ax = plt.subplots(1, 1, figsize=(15, 10))
world_merged.plot(ax=ax, color=color, edgecolor='black')
ax.set_xlim([-120, -30])
ax.set_ylim([-60, 30])
```

Srozumitelnost naší mapy výrazně zvýší přidání barevné škály, díky které bude jasné, jaká barva znamená jakou hodnotu indexu. K přidání použijeme `ScalarMappable`. `ScalarMappable` je objekt, který převádí číselné hodnoty na barvy vybraného barevného schématu, v našem případě `RdYlGn`, což nastavíme pomocí parametru `cmap`.

```py
import matplotlib as mpl

sm = mpl.cm.ScalarMappable(cmap=cmap)
```

Barevnou legendu přidáme pomocí funkce `colorbar`. Parametr `sm` určuje `ScalarMappable`, který bude použit k vytvoření barevné škály. Svislou orientaci škály zaručuje `orientation='vertical'`. A nakonec `fraction=0.025` nastavuje velikost relativně k velikosti grafu. Pomocí metody `set_ticklabels()` nastavíme popisky osy barevné čáry. Výchozí počet popisek je 6, využijeme tedy funkci `range()`, která vytvoří čísla od 0 do 100 s velikostí kroku 20. Pokud bychom chtěli upravit i počet hodnot, můžeme využít metodu `set_ticks()`.

```py
cbar = fig.colorbar(sm, ax=ax, orientation='vertical', fraction=0.025)
cbar.set_label('Overall Score')
cbar.set_ticklabels(range(0, 101, 20))
```

::fig[Mapa latinské Ameriky]{src=assets/latinska_amerika.png}

### Cvičení

::exc[excs/volebni-mapa]
