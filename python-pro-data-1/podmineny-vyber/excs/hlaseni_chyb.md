---
title: Hlášení chyb
demand: 3
---

V datech se bohužel můžou vyskytnout i chyby. V případě výživných látek jsou uváděné hodnoty vždy přepočtené na 100 gramů potraviny. Chybou by tedy například bylo, pokud by bylo nějaké výživné látky v potravině více než 100 gramů, tj. výživné látky by bylo více než samotné potraviny. Podobně to platí i pro miligramy. Více než 100 000 miligramů výživné látky též nedává smysl (1 gram = 1 000 miligramů).

Vyhledej všechny řádky, kde jsou hodnoty v miligramech (ve sloupci `unit_name` je hodnota `MG`) a množství látky (sloupec `amount`) má větší hodnotu než 100 000. Ulož tato data do souboru `error_reports.csv`. K tomu využij metodu `.to_csv()`, které zadáš jako parametr název souboru. Níže je příklad jejího použití.

```py
tabulky.to_csv("nazev_souboru.csv")
```

Takto vytvořený soubor bychom mohli poslat poskytovateli dat a požádat ho o opravu.

:::solution
```py
errors = food_nutrient[(food_nutrient["unit_name"] == "MG") & (food_nutrient["amount"] > 100_000)]
errors.to_csv("error_reports.csv")
```
:::
