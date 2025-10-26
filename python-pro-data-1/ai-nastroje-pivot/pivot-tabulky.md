## Skupiny a pivot tabulky

Pivot tabulka (často se též používá termín kontingenční tabulka) je nástroj, který vám umožní rychle a efektivně shrnovat, analyzovat, prozkoumávat a prezentovat souhrnná data.

Jak bychom pivot tabulky mohli využít pro naše data? Pivot tabulky dokážou zobrazit vztah mezi dvěma sloupci tabulky (dvěma proměnnými), v našem případě můžeme například sledovat vztah mezi dvěma výživnými látkami. Mohli bychom tedy zkoumat, jestli při růstu množství jedné výživné látky roste (nebo naopak klesá) množství jiné výživné látky. Dále nám tabulka může usnadnit hledání vhodných potravin na základě více než jednoho kritéria. Pokud bychom například hledali potravinu, která má hodně vlákniny a současně málo nasycených tuků, museli bychom napsat poměrně složitou podmínku. Pivot tabulka nám situaci zjednoduší. Poslední příklad je zkoumání průměrného množství výživných látek v jednotlivých kategoriích potravin, kontingenční tabulky tedy mohou být alternativou k vizualizacím, které jsme si ukázali v minulé lekci.

### Skupiny

Na základě nějaké číselné hodnoty můžeme data rozdělit i do skupin. Každá skupina potřebuje dvě věci:

- číselný interval, který udává rozsah pro zařazení do skupiny,
- označení skupiny.

Uvažujme následující skupiny.

| Obsah cholesterolu     | Kategorie           |
|------------------------|---------------------|
| 0-20                   | Low Cholesterol     |
| 20-100                 | Moderate Cholesterol|
| 100-inf                | High Cholesterol    |

Každé potravině můžeme přiřadit popisek, která nám usnadní psaní dotazů. Data máme připravená v souboru [pivot_cholesterol.csv](assets/pivot_cholesterol.csv).

```py
import pandas as pd
import seaborn as sns

pivot_cholesterol = pd.read_csv("pivot_cholesterol.csv")

bins = [0, 20, 100, float('inf')]
labels = ['Nízký cholesterol', 'Střední cholesterol', 'Vysoký cholesterol']

pivot_cholesterol['cholesterol_category'] = pd.cut(pivot_cholesterol['amount'], bins=bins, labels=labels)
pivot_cholesterol
```

Pokud bychom chtěli zkoumat množství cholesterolu v různých typech potravin, můžeme použít vizualizaci `countplot`, kterou už jsme si ukazovali v minulé lekci. V grafu pak uvidíme, kolik je v každé skupině potravin s nízkým obsahem cholesterolu, se středním a vysokým.

```py
sns.countplot(pivot_cholesterol, y="branded_food_category", hue="cholesterol_category")
```

### Pivot tabulka

Pivot tabulka umí zobrazit podobné informace jako `countplot`, má oproti grafu několik výhod. Vidíme v ní přesná čísla, můžeme použít i jiné typy agregací než počet a můžeme s ní pracovat v dalších výpočtech (např. pomocí dotazů). Pivot tabulku můžeme vytvořit několika způsoby, jednou z nich je funkce `crosstab()`. Pokud chceme v tabulce vidět počet potravin v jednotlivých kombinacích sloupců `branded_food_category` a `cholesterol_category`, stačí nám vložit tyto dva sloupce jako série.

```py
pd.crosstab(pivot_cholesterol["branded_food_category"], pivot_cholesterol["cholesterol_category"])
```

Pro zobrazení součtu řádků a sloupců můžeme využít parametr `margins`.

```py
pd.crosstab(pivot_cholesterol["branded_food_category"], pivot_cholesterol["cholesterol_category"], margins=True)
```

Pokud bychom chtěli hodnoty převést na procenta, můžeme využít parametr `normalize`. Hodnota `index` zobrazí procentuální podíl jednotlivých skupin z pohledu množství cholesterolu v jednotlivých kategoriích.

```py
pd.crosstab(pivot_cholesterol["branded_food_category"], pivot_cholesterol["cholesterol_category"], normalize="index")
```

Uvažujme nyní, že bychom chtěli použít nějakou agregační funkci. Můžeme například spočítat průměrné množství výživné látky. V datech máme nyní pouze cholesterol, výsledná pivot tabulka by měla jen jeden sloupec a tím pádem by šlo o obdobu klasické agregace. Využijme tedy soubor [pivot_cholesterol_fiber.csv](assets/pivot_cholesterol_fiber.csv), který vedle dat o cholesterolu obsahuje data o vláknině. Vyšší množství vlákniny totiž může částečně pomoci eliminovat nezdravý efekt cholesterolu. Nezdravé potraviny tedy budou mít vysoké množství cholesterolu a nízké množství vlákniny.

```py
import numpy as np

pivot_cholesterol_fiber = pd.read_csv("pivot_cholesterol_fiber.csv")
pd.crosstab(pivot_cholesterol_fiber["branded_food_category"], pivot_cholesterol_fiber["nutrient_name"], pivot_cholesterol_fiber["amount"], aggfunc=np.mean)
```

### Cvičení

::exc[excs/booking]

### Bonus

::exc[excs/titanic]
