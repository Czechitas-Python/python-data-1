## Sjednocení dat

V předchozí lekci jsme si ukázali, jak v `pandas` načteme data do tabulky (`DataFrame`) a jak z něj můžeme vybírat data pomocí různých způsobů dotazování. Nyní se posuneme o kus dále a ukážeme si, jak můžeme s `DataFrame` dělat složitější operace jako je filtrování chybějících hodnot, spojování a agregace.

### Práce s chybějícími hodnotami

V praxi se poměrně často setkáme s tím, že v datovém setu některé hodnoty chybí. Můžeme si například všimnout, že v tabulce `food_nutrient.csv` jsou chybějící hodnoty. Ty jsou při výpisu zobrazovány jako `NaN`.

Při práci s daty je možné se s chybějícími daty vypořádat různými způsoby. 

Nejprve bychom si měli uvědomit, zda jsou pro nějaké sloupec chybějící hodnoty vlastně problém. Typický příklad sloupečku, kde to nevadí, je `footnote`. To, že k nějaké hodnotě není poznámka pod čarou, nevadí, zkrátka k měření žádná doplňující poznámka není. Naopak u sloupce `amount` to vadit může. U nějaké konkrétní výživné látky nevíme její obsah v nějaké konkrétní potravině. 

Co v takovém případě udělat? Nejlepší je vždy ověření, proč údaje chybí (např. u poskytovatele dat, v metodice sběru, v programu na export dat) a pokud je to možné, zajistit jejich doplnění. Při hledání příčiny může pomoci i uložení těchto dat do samostatné tabulky, protože v ní můžeme vidět nějaké společné znaky, které tyto neúplné řádky mají. Zkusme si to na našich datech.

Pokud nemáš načtená data, použij stejný příkaz jako v minulé lekci.

```py
import pandas as pd
food_nutrient = pd.read_csv('food_nutrient.csv')
```

Zkusíme si uložit řádky bez množství výživné látky do tabulky `food_nutrient_incomplete`. Použijeme metodu `isna()`, která pro každý řádek vrátí pravdivostní hodnotu (tj. hodnotu `False` pro řádek s hodnotou nebo `True` pro prázdný řádek). Poté můžeme použít dotaz, který jsme si ukázali už v minulé lekci.

```py
food_nutrient_incomplete = food_nutrient[food_nutrient["amount"].isna()]
food_nutrient_incomplete.head()
```

Metoda `.notna()` funguje obráceně (tj. vrací hodnotu `True` pro řádek s hodnotou nebo `False` pro prázdný řádek).

```py
food_nutrient_complete = food_nutrient[food_nutrient["amount"].notna()]
food_nutrient_complete.head()
```

V některých případech může být vhodné nahradit chybějící hodnoty jinými hodnotami. Uvažujme třeba výkaz práce, kde je chybějící hodnota u nějakého zaměstnance a projektu. V programu, který data generuje, zjistíme, že to znamená, že daný zaměstnanec na projektu ten měsíc nepracoval. Prázdnou hodnotu tedy můžeme nahradit nulou.

Poslední možností je odstranit všechny řádky, které nejsou úplné. Musíme pak ale pamatovat na to, že již nepracujeme s kompletními daty. Vyřazením některých řádků může dojít ke zkreslení výsledků analýzy.

V našem případě použijeme poslední možnost, tj. odstraníme všechny řádky, kde chybí množství výživné látky. K tomu je možné použít metodu `.notna()`, kterou jsme si už ukazovali. Ukážeme si ale ještě jednu metodu, a to `dropna()`. Pokud chceme, aby se metoda řídila pouze některými sloupci, použijeme parametr `subset` (podmnožina). V opačném případě metoda smaže všechny řádky, kde je chybějící hodnota alespoň v jednom sloupci.

Jako hodnotu můžeme dát název jednoho sloupce jako řetězec nebo seznam názvů sloupců.

```py
food_nutrient = food_nutrient.dropna(subset="amount")
```

### Sjednocení dat

Nyní bychom chtěli všechny tři naše tabulky spojit do jedné. Nejprve si ukážeme, jak spojit tabulky **pod sebe**. Jaké budou rozměry výsledné tabulky?

- Počet **sloupců** je ve výsledné tabulce stejný jako u spojovaných tabulek.
- Počet **řádků** odpovídá součtu řádků spojovaných tabulek.

V SQL používáme pro danou operaci klíčové slovo `UNION`, `pandas` používáme funkci `concat()`.

My funkci využijeme, abychom si vytvořili větší tabulku s názvy potravin. V naší tabulce `food_sample_100.csv` máme pouze 100 vybraných potravin. My si k nim přidáme data z tabulky [food_other.csv](assets/food_other.csv).

Pozor na to, že v takto vzniklém DataFrame se nám **rozbije index**, protože se spojí za sebe indexy jednotlivých tabulek. Pokud chceme, aby `pandas` při spojování index přepočítal, musíme nastavit hodnotu parametru `ignore_index` na `True`.

```py
food_sample_100 = pd.read_csv("food_sample_100.csv")
food_other = pd.read_csv("food_other.csv")
food = pd.concat([food_sample_100, food_other], ignore_index=True)
```

K čemu je spojování vlastně dobré? Některé programy například ukládají data za každý den do samostatného souboru, takže pokud potřebujeme data za jeden týden, stačí nám stáhnout a spojit 7 souborů a ostatní stovky souborů a gigabajty dat můžeme ignorovat.

Různé typy operací merge si můžeš procvičit na [této stránce](https://pesikj.github.io/Visual-JOIN/).
