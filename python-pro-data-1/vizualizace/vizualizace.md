## Vizualizace

V této lekci si ukážeme, jak zobrazovat různé druhy grafů. V Pythonu můžeme využívat spoustu různých knihoven na generování vizualizací. Velmi oblíbenou je knihovna `matplotlib`. Nad knihovnou `matplotlib` byla vytvořena knihovna `seaborn`, která zjednodušuje tvorbu některých typů vizualizací.

Vyzkoušíme si nyní vytvořit několik základních vizualizací. Začněme s grafem `countplot`, pomocí kterého zobrazíme sloupcový graf s počtem hodnot v jednotlivých kategoriích. Protože kategorií máme v datech opravdu hodně a do jednoho grafu by se nevešly, vytvoříme si seznam 12 kategorií s největším počtem potravin a zobrazíme výsledky pouze pro ně. Použijeme metodu `isin()`, kterou jsme si ukazovali v části o podmíněném výběru.

```py
import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

food_nutrient = pd.read_csv("food_nutrient.csv")
branded_food = pd.read_csv("branded_food.csv")
food = pd.concat([food_sample_100, food_other])
food_brands = pd.merge(food, branded_food, on="fdc_id")

top_cat_list = ['Candy', 'Popcorn, Peanuts, Seeds & Related Snacks', 'Cheese', 'Ice Cream & Frozen Yogurt', 'Chips, Pretzels & Snacks', 'Cookies & Biscuits', 'Pickles, Olives, Peppers & Relishes', 'Breads & Buns', 'Fruit & Vegetable Juice, Nectars & Fruit Drinks', 'Snack, Energy & Granola Bars', 'Chocolate', 'Other Snacks']
food_top_cat = food_brands[food_brands["branded_food_category"].isin(top_cat_list)]
```

Graf vytvoříme pomocí funkce `countplot`. Jako první hodnotu zadáme název tabulku s daty a jako parametr `x` název sloupce, podle kterého se vygenerují sloupce grafu. V předchozí lekci to odpovídalo sloupci, který jsme zadávali do metody `groupby().` Výsledek uložíme do proměnné `ax`. Jde o zkratku slova axis, která obsahuje odkaz na vytvořený graf. Pomocí metody `tick_params()` otočíme popisky osy *x* o 90 stupňů, protože jinak by se popisy vzájemně překrývaly.

```py
ax = sns.countplot(food_top_cat, x="branded_food_category")
ax.tick_params(axis='x', rotation=90)
```

Pokud píšeme program jako skript, je nutné ještě přidat řádke `plt.show()`. Ten zajistí, že se graf zobrazí v samostatném okně. Pozor ale na to, že program se pozastaví, dokud okno s grafem neuzavřeme. Pokud používáme Jupyter notebook, tento řádek přidávat nemusíme.

Vygenerovaný graf je poměrně špatně čitelný. Můžeme ale zkusit názvy kategorií zkrátit. V rámci toho rovnou provedeme překlad do češtiny. K přejmenování použijeme metodu `.replace()`. Hodnoty můžeme vložit jako slovník. Vložíme do něj původní hodnotu a jejích náhradu, obojí opět oddělíme dvojtečkou. Protože chceme přejmenovat více hodnot, vložíme více dvojic, které oddělíme čárkou.

```py
food_brands["branded_food_category"] = food_brands["branded_food_category"].replace({
    "Candy": "Cukrovinky",
    "Popcorn, Peanuts, Seeds & Related Snacks": "Slané snacky",
    "Cheese": "Sýry",
    "Ice Cream & Frozen Yogurt": "Zmrzlina",
    "Chips, Pretzels & Snacks": "Chipsy",
    "Cookies & Biscuits": "Sušenky",
    "Pickles, Olives, Peppers & Relishes": "Nakl. zelenina",
    "Breads & Buns": "Pečivo",
    "Fruit & Vegetable Juice, Nectars & Fruit Drinks": "Džusy",
    "Snack, Energy & Granola Bars": "En. tyčinky",
    "Chocolate": "Čokoláda",
    "Other Snacks": "Další snacky"
})
food_list = ["Cukrovinky", "Slané snacky", "Sýry", "Zmrzlina", "Chipsy", "Sušenky", "Nakl. zelenina", "Pečivo", "Džusy", "En. tyčinky", "Čokoláda", "Další snacky"]
food_top_cat = food_brands[food_brands["branded_food_category"].isin(top_cat_list)]
```

Po přejmenování kategorií stačí otočit popisky o 45 stupňů, takže budou lépe čitelné.

```py
ax = sns.countplot(food_top_cat, x="branded_food_category")
ax.tick_params(axis='x', rotation=45)
```

Pokud bychom chtěli graf zveřejnit například v nějakém článku, je vhodné jej doplnit o popisky. K tomu využijeme metodu `set`, které nastavíme následující parametry:

- `xlabel` nastaví popisek osy *x* (vodorovné osy),
- `ylabel` nastaví popisek osy *y* (svislé osy),
- `title` nastaví titulek grafu.

```py
ax = sns.countplot(food_top_cat, x="branded_food_category")
ax.tick_params(axis='x', rotation=45)
ax.set(xlabel="Kategorie", ylabel="Počet potravin", title="Počty potravin ve 12 nejpočetnějších kategoriích")
```

Další z oblíbených grafů je histogram. Uvažujme, že se chceme blíže podívat, jak je to s množstvím proteinů v potravinách. Histogram nám umožní komplexnější pohled než průměr. Na vodorovné ose histogramu vidíme množství proteinu a na svislé ose množství potravin, které takovou hodnotu mají. Z histogramu tedy vidíme, že většina potravin má uvedené velmi nízké množství proteinu. Pouze malé množství potravin má více než 10 gramů proteinu.

```py
food_merged_brands = pd.merge(food_brands, food_nutrient, on="fdc_id")
food_merged_brands = food_merged_brands.rename(columns={"name": "nutrient_name"})
food_merged_brands_protein = food_merged_brands[food_merged_brands["nutrient_name"] == "Protein"]
ax = sns.histplot(food_merged_brands_protein, x="amount")
ax.set(xlabel="Množství proteinu (g)", ylabel="Počet potravin", title="Množství proteinů v potravinách")
```

U histogramu můžeme použít parametr `bins`, kterým nastavíme, jaké mají být hranice jednotlivých intervalů. K nastavení hranic můžeme použít například funkci `range()`, která nám vygeneruje seznam čísel. Funkci zadáváme tři parametry:

1. první číslo seznamu,
1. číslo, které ukončuje seznam (pozor, jde o první číslo, které v seznamu **nebude**),
1. rozdíl mezi dvěma čísly v seznamu.

Funkce `range(0, 110, 10)` tedy vygeneruje řadu čísel 0, 10, 20 až 100.

```py
ax = sns.histplot(food_merged_brands_protein, x="amount", bins=range(0, 110, 10))
ax.set(xlabel="Množství proteinu (g)", ylabel="Počet potravin", title="Množství proteinů v potravinách")
```

Dále se můžeme podívat, jak se liší průměrné množství proteinu pro jednotlivé kategorie potravit. K tomu sloučí `barplot`. Ten vypočte průměrné hodnoty dle sloupce, který zadáme jako parametr `x`. Parametr `y` udává, ze kterého sloupce se vypočítá průměr, který udává výšku sloupců. Černá čára v grafu je označovaná jako `errorbar`. Vychází z předpokladu, že v datech máme vždy jen vzorek dat, například v našich datech je jen část potravin, které jsou na trhu k dostání. Sloupec, který udává výšku sloupce, je tedy v podstatě jen odhadem hodnoty, kterou bychom zjistili, pokud bychom analyzovali všechny dostupné postraviny na trhu. Černá čára pak udává tzv. interval spolehlivosti, tedy interval, ve kterém by se ten průměr nacházel s pravděpodobností 95 %.

```py
food_brands_nut = pd.merge(food_brands, food_nutrient, on="fdc_id")
ax = sns.barplot(food_brands_nut, x="branded_food_category", y="amount")
ax.tick_params(axis='x', rotation=45)
ax.set(xlabel="Kategorie", ylabel="Množství proteinu (g)", title="Průměrné množství proteinů v potravinách")
```

Další z oblíbených grafů je je krabicový graf :term{cs="krabicový graf" en="box plot"}. Zkusme pomocí krabicového grafu například porovnat množství proteinu a lipidů (tuků) v energetických tyčinkách. Jak tento graf interpretovat?

- Černá čára uprostřed udává průměr. Průměrná hodnota pro obě látky je tedy přibližně stejná.
- Modré obdélníky udávají rozsah, ve kterém se nachází 50 % hodnot. Dolní hrana obdélníku odděluje 25 % nejmenších hodnot od zbývajících 75 %. Horní hrana obdélníku odděluje 75 % nejmenších hodnot od zbývajících 25%. Tento obdélník ukazuje různorost dat. Na našem příkladu vidíme, že z pohledu množství proteinů jsou jednotlivé energetické tyčinky více různorodé než z pohledu množství lipidů (tuků).
- Černé čáry jsou označované jaké *whisker* (kočičí vousy). V našem případě fungují podobně jako obdelník, ale oddělují vždy 5 % nejmenších a největších hodnot od zbývajících 90 %.
- Zbývajících 10 % hodnot je vykresleno pomocí černých teček.

Aby byl graf celý v jednom jazyce, pojďme nejprve přejmenovat názvy výživných látek do češtiny

```py
food_merged_brands["nutrient_name"] = food_merged_brands["nutrient_name"].replace({
    "Total lipid (fat)": "Lipid (tuk)", 
    "Protein": "Protein"
    })
```

```py
food_merged_brands_box = food_merged_brands[(food_merged_brands["nutrient_name"].isin(["Proteiny", "Lipidy (tuky)"])) & (food_merged_brands["branded_food_category"] == "Snack, Energy & Granola Bars")]
ax = sns.boxplot(food_merged_brands_box, x="nutrient_name", y="amount", whis=[5, 95])
ax.set(xlabel="Kategorie", ylabel="Množství v gramech", title="Množství proteinů a lipidů (tuků) v potravinách")
```
