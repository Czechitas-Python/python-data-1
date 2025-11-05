---
title: Vláknina
demand: 3
---

Problémem dnešních potravin je často nedostatek vlákniny. Konzumace potravin bohatých na vlákninu může být pro řadu lidí zdravotně prospěná. Vyhledej v tabulce `food_nutrient` potraviny, které obsahují alespoň 10 gramů vlákniny. Vláknina je uložená pod názvem `Fiber, total dietary`. Napiš dotaz jako jeden příkaz a využij operátor `&`.

:::solution
```py
food_nutrient[
    (food_nutrient["name"] == "Fiber, total dietary")
      & (food_nutrient["amount"] >= 10)
      ]
```
:::
