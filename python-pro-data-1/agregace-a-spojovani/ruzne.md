## Řazení

Data řadíme poměrně často. U běžeckého závodu nás zajímají ti nejrychlejší běžci, u položek v e-shopu ty nejlépe hodnocené, u projektu zase chceme vidět úkoly s nejbližším deadline. Abychom tyto hodnoty získali, musíme data seřadit. Ve světě databází pro to používáme klíčová slova `ORDER BY`, v `pandas` nám poslouží metoda `sort_values`. Jako její první parametr zadáváme sloupec (nebo seznam sloupců), podle kterého (kterých) řadíme.

```py
staty.sort_values(by="population")
```

Metoda `sort_values` standardně řadí vzestupně. Chceme-li řadit sestupně, zadáme jí parametr `ascending` a nastavíme ho na `False`.

```py
staty.sort_values(by="population", ascending=False)
```
