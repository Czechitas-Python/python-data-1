---
title: Dashboard
demand: 3
---

V Pythonu je možné vytvořit i dashboard, který si ostatní mohou otevřít v prohlížeči. Můžeme k tomu využít například knihovnu `dash`. Ta nejlépe pracuje s knihovnou `plotly`. Začni tím, že si obojí nainstaluješ. Instalace probíhá stejným způsobem, jako instalace `pandas` a dalších knihoven, tj. příkazy

```
pip install dash
pip install plotly
```

nebo

```
pip3 install dash
pip3 install plotly
```

Níže je příklad vytvoření dashboardu. Projdeme si ho postupně po částech a poté do něj doplníme chybějící kód.

### Importy a načtení dat

```py
from dash import Dash, dcc, html, Input, Output
import plotly.express as px
import pandas as pd
```

Na rozdíl od `seaborn` a `matplotlib` importujeme z `dash` rovnou konkrétní části, které budeme potřebovat. `dcc` (Dash Core Components) obsahuje interaktivní prvky jako rozbalovací seznam nebo graf. `html` umožňuje vkládat HTML prvky jako nadpisy nebo popisky textu. `Input` a `Output` použijeme k propojení interaktivních prvků s grafem. Z `plotly` importujeme `plotly.express`, což je zjednodušené rozhraní pro tvorbu grafů.

### Vytvoření aplikace a rozložení stránky

```py
food_merged_brands = pd.read_csv("food_merged_brands_viz.csv")

DEFAULT_NUTRIENT = "Energy"

app = Dash(__name__)

app.layout = html.Div([
    html.H1("Výživné látky v kategoriích potravin"),

    html.Label("Vyber výživnou látku:"),
    dcc.Dropdown(
        id="nutrient-dropdown",
        options=food_merged_brands["nutrient_name"].unique(),
        value=DEFAULT_NUTRIENT,
        clearable=False,
    ),
    dcc.Graph(id="bar-chart")
])
```

Nejprve načteme data a uložíme výchozí výživnou látku do proměnné `DEFAULT_NUTRIENT`. Poté vytvoříme samotnou aplikaci příkazem `Dash(__name__)`.

Rozložení stránky definujeme pomocí `app.layout`. Stránka se skládá z několika prvků, které zapisujeme podobně jako slovník z předchozích lekcí:

- `html.H1` vytvoří nadpis stránky.
- `html.Label` vytvoří popisek nad rozbalovacím seznamem.
- `dcc.Dropdown` vytvoří rozbalovací seznam. Parametr `id` je jedinečný název, pomocí kterého na seznam budeme odkazovat. Parametr `options` určuje seznam možností — použijeme unikátní hodnoty ze sloupce `nutrient_name`. Parametr `value` nastavuje výchozí vybranou hodnotu. Parametr `clearable=False` zabrání uživateli výběr smazat.
- `dcc.Graph` vytvoří místo pro graf. Opět mu dáme `id`, pomocí kterého na něj budeme odkazovat.

### Callback — propojení výběru s grafem

```py
@app.callback(
    Output("bar-chart", "figure"),
    Input("nutrient-dropdown", "value")
)
def update_chart(selected_nutrient):
    # SEM VLOŽ KÓD

    fig = px.bar(
        avg_per_category,
        x="branded_food_category",
        y="amount",
        title=f"Průměrné množství výživné látky '{selected_nutrient}' v kategoriích potravin",
        labels={
            "branded_food_category": "Kategorie",
            "amount": "Průměrné množství"
        }
    )

    return fig
```

Klíčová část dashboardu je tzv. :term{cs="zpětné volání" en="callback"}. Jde o funkci, která se automaticky spustí pokaždé, když uživatel změní výběr v rozbalovacím seznamu.

Dekorátor `@app.callback` nad funkcí říká aplikaci `Dash`, co má funkce na vstupu a výstupu:

- `Input("nutrient-dropdown", "value")` znamená: sleduj hodnotu (`value`) rozbalovacího seznamu s `id="nutrient-dropdown"` a při každé její změně funkci spusť.
- `Output("bar-chart", "figure")` znamená: výsledek funkce vlož jako graf (`figure`) do prvku s `id="bar-chart"`.

Funkce `update_chart` přijme jako parametr aktuálně vybranou výživnou látku (`selected_nutrient`). Uvnitř funkce připravíme data a vytvoříme graf pomocí `px.bar`. Ten funguje podobně jako `sns.barplot`, který už známe. Funkce musí na konci graf vrátit pomocí `return fig`.

### Spuštění aplikace

```py
if __name__ == "__main__":
    app.run(debug=True)
```

Poslední část spustí webový server. Podmínka `if __name__ == "__main__"` zajistí, že se server spustí pouze tehdy, když soubor spustíme přímo (ne když ho někdo jiný importuje jako knihovnu). Parametr `debug=True` zapne vývojový režim, ve kterém se aplikace automaticky restartuje při každé změně kódu. Po spuštění skriptu se v terminálu zobrazí adresa, na které dashboard běží — obvykle `http://127.0.0.1:8050`. Tuto adresu otevři v prohlížeči.

### Tvůj úkol

Doplň do funkce `update_chart` na místo komentáře `# SEM VLOŽ KÓD` kód, který:

1. z tabulky `food_merged_brands` vybere pouze řádky, kde je ve sloupci `nutrient_name` hodnota `selected_nutrient`,
2. provede agregaci podle sloupce `branded_food_category` a vypočítá průměrné množství (`amount`),
3. výsledek převede na tabulku a seřadí sestupně.

Výsledek ulož do proměnné `avg_per_category`, kterou pak použije již připravený kód pro vytvoření grafu. Takto by měl výsledný dashboard vypadat:

::fig[Příklad výsledku]{src=assets/dashboard.png}

```py
from dash import Dash, dcc, html, Input, Output
import plotly.express as px
import pandas as pd

food_merged_brands = pd.read_csv("food_merged_brands_viz.csv")

DEFAULT_NUTRIENT = "Energy"

app = Dash(__name__)

app.layout = html.Div([
    html.H1("Výživné látky v kategoriích potravin"),

    html.Label("Vyber výživnou látku:"),
    dcc.Dropdown(
        id="nutrient-dropdown",
        options=food_merged_brands["nutrient_name"].unique(),
        value=DEFAULT_NUTRIENT,
        clearable=False,
    ),
    dcc.Graph(id="bar-chart")
])


@app.callback(
    Output("bar-chart", "figure"),
    Input("nutrient-dropdown", "value")
)
def update_chart(selected_nutrient):
    # SEM VLOŽ KÓD pro vytvoření avg_per_category

    fig = px.bar(
        avg_per_category,
        x="branded_food_category",
        y="amount",
        title=f"Průměrné množství výživné látky '{selected_nutrient}' v kategoriích potravin",
        labels={
            "branded_food_category": "Kategorie",
            "amount": "Průměrné množství"
        }
    )

    return fig


if __name__ == "__main__":
    app.run(debug=True)
```


:::solution

```py
from dash import Dash, dcc, html, Input, Output
import plotly.express as px
import pandas as pd

food_merged_brands = pd.read_csv("food_merged_brands_viz.csv")

DEFAULT_NUTRIENT = "Energy"

app = Dash(__name__)

app.layout = html.Div([
    html.H1("Výživné látky v kategoriích potravin"),

    html.Label("Vyber výživnou látku:"),
    dcc.Dropdown(
        id="nutrient-dropdown",
        options=food_merged_brands["nutrient_name"].unique(),
        value=DEFAULT_NUTRIENT,
        clearable=False,
    ),
    dcc.Graph(id="bar-chart")
])


@app.callback(
    Output("bar-chart", "figure"),
    Input("nutrient-dropdown", "value")
)
def update_chart(selected_nutrient):
    food_merged_brands_filtered = food_merged_brands[food_merged_brands["nutrient_name"] == selected_nutrient]

    avg_per_category = food_merged_brands_filtered.groupby("branded_food_category")["amount"].mean()
    avg_per_category = avg_per_category.reset_index()
    avg_per_category = avg_per_category.sort_values("amount", ascending=False)

    fig = px.bar(
        avg_per_category,
        x="branded_food_category",
        y="amount",
        title=f"Průměrné množství výživné látky '{selected_nutrient}' v kategoriích potravin",
        labels={
            "branded_food_category": "Kategorie",
            "amount": "Průměrné množství"
        }
    )

    return fig


if __name__ == "__main__":
    app.run(debug=True)

```

:::
