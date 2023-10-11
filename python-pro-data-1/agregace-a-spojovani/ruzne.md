## Počítané sloupce

Občas je užitečné přidat nový sloupec, který obsahuje hodnotu vypočtenou na základě hodnot ostatních sloupců. Vraťme se například k naší tabulce s údaji o státech ve světě. Máme informaci o rozloze a počtu obyvatel, mohli bychom tedy přidal sloupec s hodnotou hustoty zalidnění (počet obyvatel na 1 km čtvereční), který získáme vydělením počtu obyvatel rozlohou země.

Pokud nemáme načtený soubor s daty, načteme si ho. Data si můžeme [stáhnout zde](assets/staty.json). Opět platí, že si je dáme do adresáře, kde máme právě otevřený terminál!

```py
staty = pandas.read_json("staty.json")
staty = staty.set_index("name")
```

Přidání nového sloupce je poměrně jednoduché. Před znaménko `=` vložíme proměnnou s `DataFrame` a do hranatých závorek vložíme název nového sloupce. Na pravou stranu umístíme výpočet. Ve výpočtu pracujeme s jednotlivými sloupci, v našem konkrétním případě vydělíme sloupec `population` sloupcem `area`.

```py
staty["population_density"] = staty["population"] / staty["area"]
```

**Poznámka:** `pandas` nás neupozorní, pokud sloupec již existuje, musíme si tedy dát pozor, abychom nepřepsali nějaký existující sloupec.

## Řazení

Data řadíme poměrně často. U běžeckého závodu nás zajímají ti nejrychlejší běžci, u položek v e-shopu ty nejlépe hodnocené, u projektu zase chceme vidět úkoly s nejbližším deadline. Abychom tyto hodnoty získali, musíme data seřadit. Ve světě databází pro to používáme klíčová slova `ORDER BY`, v `pandas` nám poslouží metoda `sort_values`. Jako její první parametr zadáváme sloupec (nebo seznam sloupců), podle kterého (kterých) řadíme.

```py
staty.sort_values(by="population")
```

Metoda `sort_values` standardně řadí vzestupně. Chceme-li řadit sestupně, zadáme jí parametr `ascending` a nastavíme ho na `False`.

```py
staty.sort_values(by="population", ascending=False)
```
