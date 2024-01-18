## Pivot tabulky

Pivot tabulka (často se též používá termín kontingenční tabulka) je nástroj, který vám umožní rychle a efektivně shrnovat, analyzovat, prozkoumávat a prezentovat souhrnná data.

Jak bychom pivot tabulky mohli využít pro naše data? Pivot tabulky dokážou zobrazit vztah mezi dvěma sloupci tabulky (dvěma proměnnými), v našem případě můžeme například sledovat vztah mezi dvěma výživnými látkami. Mohli bychom tedy zkoumat, jestli při růstu množství jedné výživné látky roste (nebo naopak klesá) množství jiné výživné látky. Dále nám tabulka může usnadnit hledání vhodných potravin na základě více než jednoho kritéria. Pokud bychom například hledali potravinu, která má hodně vlákniny a současně málu nasycených tuků, museli bychom napsat poměrně složitou podmínku. Pivot tabulka nám situaci zjednodušší. Poslední příklad je zkoumání průměrného množství výživných látek v jednotlivých kategoriích potravin, kontingenční tabulky tedy mohou být alternativou k vizualizacím, které jsme i ukázali v minulé lekci.

Začneme tím, že si načteme data ze souboru `food_nutrient.csv` do tabulky `food_nutrient`.

```py
import pandas as pd
food_nutrient = pd.read_csv("food_nutrient.csv")
```

V `pandas` existuje několik funkcí. My začneme z funkcí `pivot()`. Tato funkce slouží k "přeskládání" tabulky. Výsledná tabulka nebude mít samostatný řádek pro každou kombinaci potraviny a výživné látky. Tabulku přeskládáme tak, aby každá potravina měla pouze jeden řádek a jednotlivé výživné látky budou uloženy ve sloupcích. Namísto sloupce `name` s názvem výživné látky budeme mít názvy výživných látek přímo ve sloupcích. 

Funkci `pivot` určíme tři parametry:

- Parametr `index` slouží jako popisek řádků. V našem případě zvolíme sloupeček `fdc_id`, tj. unikátní číslo potraviny.
- Parametr `columns` bude použit k vytvoření nových sloupečků. Každá unikátní hodnota v tomto sloupci bude znamenat nový sloupeček ve výsledné tabulce. Sem doplníme sloupec `name`.
- Parametr `value` označuje sloupec, ze kterého budou členy hodnoty. V našem případě půjde o sloupec `amount`. Funkce `pivot` se pro každý řádek původní tabulky "podívá" na sloupce `fdc_id` a `name`, tj. na číslo potraviny a název výživné látky. Hodnotu, která je ve sloupci `amount`, pak doplní do nové tabulky na řádek se stejným `fdc_id` a do sloupce pro příslušnou výživnou látku.

U funkce `pivot()` je důležité, abychom pro každou kombinaci `fdc_id` a `name` měli pouze jeden řádek. Funkce totiž neprovádí žádnou agregaci. Pokud bychom agregaci potřebovali provést, musíme použít některou z funkcí, které si ukážeme dále.

Níže je použití funkce `pivot`.

```py
food_nutrient_pivot = pd.pivot(food_nutrient, index="fdc_id", columns="name", values="amount")
```
