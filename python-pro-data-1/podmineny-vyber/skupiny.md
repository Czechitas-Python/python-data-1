## Čtení na doma: Skupiny

Uvažujme, že bychom chtěli potraviny rozdělit do skupin podle obsahu některé výživné látky, např. podle obsahu soli. Takové rozdělení můžeme najít na obalech některých potravin a umožňuje spotřebitelům získat základní přehled o zdravotních aspektech konzumace potraviny. Uvažujme například následující skupiny.

| Množství soli              | Název skupiny           |
|----------------------------|-------------------------|
| 0 až 140                   | Low Sodium              |
| 140 až 400                 | Moderately Low Sodium   |
| 400 až 1000                | Moderately High Sodium  |
| 1000 a více                | High Sodium             |

K rozdělení můžeme použít funkci `cut`. Té předáme krajní hodnoty skupin a jejich názvy jako seznamy. Vytvoříme tedy krajní hodnoty jednotlivých intervalů jako seznam `bins` a jejich názvy jako seznam `labels`. Platí, že seznam `bins` by měl být o 1 delší než seznam `labels`, protože krajních hodnot budeme mít o 1 více než samotných skupin.

```py
bins = [-1, 140, 400, 1000, float('inf')]
labels = ['Low Sodium', 'Moderately Low Sodium', 'Moderately High Sodium', 'High Sodium']
```

Funkce `cut()` má parametr `right`, který říká, co se stane s krajními hodnotami, v našem případě např. s hodnotami 0, 140, 400 a 1000. Výchozí hodnota parametru `right` je `True`, což znamená, že do každé skupiny patří pravá krajní hodnota. V našem případě tedy potraviny, u kterých je množství soli přesně 140, budou patřit do skupiny "Low Sodium", potraviny s množstvím přesně 400 do skupiny "Moderately Low Sodium" atd.

Musíme ale myslet na to, co s potravinami, které neobsahují žádnou sůl. Pokud je množství soli 0 a my bychom zvolili 0 jako krajní hodnotu, nebudou takové potraviny zařazené do skupiny "Low Sodium", ale nebudou zařazeny do žádné skupiny. Abychom tyto potraviny zařadili do skupiny "Low Sodium", zvolíme jako první krajní hodnotu -1. Jako poslední krajní hodnotu můžeme zvolit např. :term{cs="nekonečno" en="infinity"}, protože nemáme dáno, jaká je maximální hodnota množství soli v potravinách. Nekonečno zapíšeme pomocí řetězce "inf", který převedeme na číslo pomocí funkce `float`.

```py
food_nutrient = food_nutrient[food_nutrient["name"] == "Sodium, Na"]
food_nutrient['Sodium Group'] = pd.cut(food_nutrient["amount"], bins, labels=labels)
```
