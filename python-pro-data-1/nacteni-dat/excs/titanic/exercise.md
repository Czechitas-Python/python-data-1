---
title: Titanic data set
demand: 1
---

Každý tutoriál datové analýzy začíná zpracováváním data setu pasažérů lodi Titanic. Nebude tomu jinak ani v našem případě. Stáhni si soubor [titanic.csv](assets/titanic.csv).

1. Načti data do `DataFrame`, který si pojmenuj `titanic`.
1. Nech si zobrazit názvy sloupců, které jsou v souboru uloženy.
1. Podívej se, kolik má soubor řádků.
1. Zjisti, v jakých sloupcích nějaké hodnoty chybí.

:::solution

```py
import pandas as pd

titanic = pd.read_csv("titanic.csv")

titanic.columns
titanic.shape
titanic.info()
```

:::
