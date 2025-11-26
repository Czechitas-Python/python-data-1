---
title: Titanic a další pivot tabulky
demand: 1
---

K tomuto cvičení využij data o cestujících na Titanicu ze souboru [titanic.csv](assets/titanic.csv).

Vytvoř kontingenční tabulku, která porovná závislost mezi třídou (sloupec `Pclass`), ve které cestoval pasažér, a tím, jestli přežil. Zkus spočítat počty přeživších z každé třídy. Dále zkus vypočítat relativní počet přeživších pro jednotlivé třídy a vytvořit tabulku s relativním počtem přeživších v závislosti na pohlaví (sloupec `Sex`).

:::solution

## Řešení

```py
import pandas as pd

titanic = pd.read_csv("titanic.csv")

# Kontingenční tabulka s počty přeživších podle třídy
pd.crosstab(titanic["Pclass"], titanic["Survived"])

# Pro lepší přehlednost můžeme přidat součty:
pd.crosstab(titanic["Pclass"], titanic["Survived"], margins=True)


# Relativní počet přeživších pro jednotlivé třídy získáme pomocí parametru `normalize="index"`:
pd.crosstab(titanic["Pclass"], titanic["Survived"], normalize="index")


# Tabulka s relativním počtem přeživších v závislosti na pohlaví:
pd.crosstab(titanic["Sex"], titanic["Survived"], normalize="index")
```
