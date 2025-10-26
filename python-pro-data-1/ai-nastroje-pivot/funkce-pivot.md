## Čtení na doma: Funkce pivot

Začneme tím, že si načteme data ze souboru `food_nutrient.csv` do tabulky `food_nutrient`.

```py
import pandas as pd
food_nutrient = pd.read_csv("food_nutrient.csv")
```

Pokračujme nyní funkcí `pivot()`. Tato funkce slouží k "přeskládání" tabulky. Výsledná tabulka nebude mít samostatný řádek pro každou kombinaci potraviny a výživné látky. Tabulku přeskládáme tak, aby každá potravina měla pouze jeden řádek a jednotlivé výživné látky budou uloženy ve sloupcích. Namísto sloupce `name` s názvem výživné látky budeme mít názvy výživných látek přímo ve sloupcích. 

Funkci `pivot` určíme čtyři parametry, kromě prvního parametru `data` musíme všechny psát jako pojmenované:

- Prvním parametrem (`data`) určujeme tabulku, se kterou bude funkce pracovat.
- Parametr `index` slouží jako popisek řádků. V našem případě zvolíme sloupeček `fdc_id`, tj. unikátní číslo potraviny.
- Parametr `columns` bude použit k vytvoření nových sloupečků. Každá unikátní hodnota v tomto sloupci bude znamenat nový sloupeček ve výsledné tabulce. Sem doplníme sloupec `name`.
- Parametr `value` označuje sloupec, ze kterého budou čteny hodnoty. V našem případě půjde o sloupec `amount`. Funkce `pivot` se pro každý řádek původní tabulky "podívá" na sloupce `fdc_id` a `name`, tj. na číslo potraviny a název výživné látky. Hodnotu, která je ve sloupci `amount`, pak doplní do nové tabulky na řádek se stejným `fdc_id` a do sloupce pro příslušnou výživnou látku.

Pozor na to, že kombinace každé dvojice sloupců `index` a `columns` může být v původní tabulce pouze jednou, jinak by totiž nebylo možné data "přeskládat" - do jednoho políčka tabulky bychom museli nacpat více než jednu hodnotu. Funkce na takovou situaci reaguje chybou. Pokud máme v tabulce malé množství duplicitních hodnot, můžeme je odstranit s využitím metody `drop_duplicates()`. Použijeme metodu pro naše data. Využijeme parametr `subset` a do něj vložíme názvy sloupců, podle kterých má být duplicita rozpoznána.

```py
food_nutrient = food_nutrient.drop_duplicates(subset=["fdc_id", "name"])
```

U funkce `pivot()` je důležité, abychom pro každou kombinaci `fdc_id` a `name` měli pouze jeden řádek. Funkce totiž neprovádí žádnou agregaci. Pokud bychom agregaci potřebovali provést, musíme použít některou z funkcí, které si ukážeme dále.

Níže je použití funkce `pivot`.

```py
food_nutrient_pivot = pd.pivot(food_nutrient, index="fdc_id", columns="name", values="amount")
```

K tabulce `food_nutrient_pivot` můžeme připojit další informace o potravinách, se kterými jsme pracovali v předchozích lekcích. Budeme tedy vědět, kterých potravin se každý řádek týká.

```py
branded_food = pd.read_csv("branded_food.csv")
food = pd.concat([pd.read_csv("food_sample_100.csv"), pd.read_csv("food_other.csv")], ignore_index=True)
food_brands = pd.merge(food, branded_food, on="fdc_id")
food_nutrient_pivot = pd.merge(food_nutrient_pivot, food_brands, on="fdc_id")
```

Po úpravě dat s využitím funkce `pivot()` můžeme snadno vytvořit další typ grafu, a to je :term{cs="bodový graf" en="scatter plot"}. Bodový graf bude mít na svislé i vodorovné ose množství některé z výživných látek. Pomocí bodového grafu uvidíme, jestli může být mezi množstvím výživných látek nějaká souvislost. Vraťme se opět k výzkumu o kardiovaskulárním systému. Pro něj mohou být škodlivé nasycené kyseliny a cholesterol. Pomocí bodového grafu se můžeme podívat, jestli mají sýry s větším množstvím nasycených kyselin i větší množství cholesterolu. Graf doplníme popiskem, musíme pro něj využít mírně odlišnou syntaxi.

```py
import seaborn as sns

cheese = food_nutrient_pivot[food_nutrient_pivot["branded_food_category"] == "Cheese"]
g = sns.JointGrid(cheese, x="Fatty acids, total saturated", y="Cholesterol")
g.plot_joint(sns.scatterplot)
g.fig.suptitle("Porovnání množství výživných látek v sýrech")
g.ax_joint.set_xlabel("Nasycené kyseliny")
g.ax_joint.set_ylabel("Cholesterol")
```

Výsledný graf je na obrázku níže.

::fig[HTML značka]{src=assets/scatterplot.png size=60}

## Cvičení

::exc[excs/dve-kriteria]
