---
title: Call centrum
demand: 2
---


V souboru [callcentrum.csv](assets/callcentrum.csv) najdete několik tisíc záznamů pro call centrum, které udávají časy mezi jednotlivými příchozími hovory v minutách a vteřinách. Načtěte tato data do série v Pythonu. Časy převeďte na vteřiny a zobrazte jejich histogram a boxplot. Co lze z těchto dvou grafů vyčíst?

K převodu na vteřiny můžeš použít metodu `str.split()`. Pomocí ní rozdělíš hodnoty minut a vteřin do samostatných sloupců. Pomocí metody `astype(int)` převedeš hodnoty na čísla. Poté pomocí počítaných sloupců můžeš spočítat celkový počet vteřin.

```py
import pandas

callcentrum = pandas.read_csv("https://kodim.cz/cms/assets/kurzy/python-data-1/python-pro-data-1/vizualizace/excs/excs>call-centrum/callcentrum.csv")
callcentrum = callcentrum["hodnota"].str.split(':', expand=True).astype(int)
```
