---
title: Házení kostkami
demand: 2
---

Mějme dvě hrací kostky, kterými vždy hodíme najednou a zaznamenáme součet bodů. Stáhněte si textový soubor [kostky.csv](assets/kostky.csv), který obsahuje 1000 takových nezávislých hodů.

Načtěte tato data do tabulky a zobrazte histogram hodů. Zvolte vhodné rozložení přihrádek a zodpovězte následující dotazy:

1. Jaká je nejčastější hodnota, která na dvou kostkách padne?
1. Je větší šance, že padne hodnota 12 než že padne hodnota 2?

```py
import pandas

kostky = pandas.read_csv("https://kodim.cz/cms/assets/analyza-dat/python-data-1/python-pro-data-1/vizualizace/excs/hazeni-kostkami/kostky.csv")
```
