## Cvičení

Níže je souhrn kódu z lekce s tabulkami, které můžeš použít jako výchozí pro řešení cvičení.

```py

import matplotlib.pyplot as plt
import pandas as pd
import seaborn as sns

food_nutrient = pd.read_csv("food_nutrient.csv")
branded_food = pd.read_csv("branded_food.csv")
food_sample_100 = pd.read_csv("food_sample_100.csv")
food_other = pd.read_csv("food_other.csv")
food = pd.concat([food_sample_100, food_other])
food_brands = pd.merge(food, branded_food, on="fdc_id")

top_cat_list = ['Candy', 'Popcorn, Peanuts, Seeds & Related Snacks', 'Cheese', 'Ice Cream & Frozen Yogurt', 'Chips, Pretzels & Snacks', 'Cookies & Biscuits', 'Pickles, Olives, Peppers & Relishes', 'Breads & Buns', 'Fruit & Vegetable Juice, Nectars & Fruit Drinks', 'Snack, Energy & Granola Bars', 'Chocolate', 'Other Snacks']
food_brands = food_brands[food_brands["branded_food_category"].isin(top_cat_list)]
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

food_merged_brands = pd.merge(food_brands, food_nutrient, on="fdc_id")
food_merged_brands = food_merged_brands.rename(columns={"name": "nutrient_name"})

```

::exc[excs/odstin]
::exc[excs/catplot]

### Bonusy

::exc[excs/podgrafy]
::exc[excs/donut]
