---
title: Místenky
demand: 3
---

Připravujeme systém pro správu místenek ve vlaku. Vlak má 2 vozy a každý vůz má 20 míst. Chceme vytvořit tabulku, kde bude pro každý vůz a každé místo jeden řádek se sloupcem pro jméno cestujícího.

Nejprve si vytvoř dvě tabulky - jednu se seznamem vozů a druhou se seznamem míst. V kódu níže využíváme funkci `range()`, která vygeneruje posloupnost celých čísel. Funkce přijímá dva parametry - začátek (včetně) a konec (bez něj). Například `range(1, 3)` vygeneruje čísla 1, 2.

Tabulku vytváříme pomocí `pd.DataFrame()`, kterému předáme slovník. Klíč slovníku se stane názvem sloupce a hodnota (v našem případě výsledek funkce `range()`) se stane obsahem sloupce.

```py
import pandas as pd

vozy = pd.DataFrame({"vuz": range(1, 3)})
mista = pd.DataFrame({"misto": range(1, 21)})
```

Pomocí cross join (`how="cross"`) vytvoř tabulku `mistenky`, která bude obsahovat všechny kombinace vozů a míst.

Přidej do tabulky sloupec `jmeno` s prázdnou hodnotou (např. prázdný řetězec `""`).

Nastav sloupce `vuz` a `misto` jako index tabulky pomocí metody `set_index()`. Nastavení dvou sloupců jako indexu funguje stejně jako u jednoho sloupce, jen názvy sloupců zadáme jako seznam.

```py
tabulka = tabulka.set_index(["sloupec_1", "sloupec_2"])
```

Nakonec do prvního vozu na místo 13 zapiš své jméno a vypiš si tabulku.

:::solution
```py
import pandas as pd

vozy = pd.DataFrame({"vuz": range(1, 3)})
mista = pd.DataFrame({"misto": range(1, 21)})

mistenky = pd.merge(vozy, mista, how="cross")
mistenky["jmeno"] = ""
mistenky = mistenky.set_index(["vuz", "misto"])

mistenky.at[(1, 13), "jmeno"] = "Jana Nováková"
mistenky
```
:::
