---
title: Česká jména
demand: 3
---

Stáhni si soubor [jmena.csv](assets/jmena.csv), která obsahuje nejpoužívanější česká jména.

1. Vypiš všechny řádky se jmény, jejichž nositelé mají průměrný věk vyšší než 60 (hodnota ve sloupci `prumerny_vek` je větší než 60).
1. Vypiš pouze jména z těch řádků, kde četnost je mezi 80 000 a 100 000.
1. Vypiš jména a četnost pro jména se slovanským nebo hebrejským původem. Kolik takových jmen je?

Pro poslední úkol můžeš využít operátor `|`. Alternativně si můžeš vyzkoušet metodu `.isin()`, která zápis zkrátí. Jako parametr vkládáme seznam hodnot, které vyhovují podmínce. Níže je příklad použití metody. Z tabulky `tabulka` chceme vybrat řádky, které ve sloupci `sloupec` mají hodnotu `hodnota_1` nebo `hodnota_2`. Pokud jsi vytvořil(a) i verzi s operátorem `|`, můžeš obě verze porovnat a rozhodnout se, která verze se ti líbí více.

```python
tabulka = tabulka[tabulka["sloupec"].isin(["hodnota_1", "hodnota_2"])]
```
