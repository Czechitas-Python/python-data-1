---
title: Čtečka čárových kódů
demand: 3
---

Pokračuj v práci s daty ze čtečky čárových kódů ze souboru [barcode_reader.csv](assets/barcode_reader.csv). Zjisti, který uživatel provedl nejvíce operací bez přihlášení, tj. operací, kde v původním sloupci `user_name` chybí hodnota.

Nápověda: Využij metodu `value_counts()`.

:::solution
```py
import pandas as pd

data = pd.read_csv("barcode_reader.csv")
data['user_name_filled'] = data['user_name'].fillna(method="ffill")
data_missing_user = data[data["user_name"].isna()]
data_missing_user["user_name_filled"].value_counts()
```
:::
