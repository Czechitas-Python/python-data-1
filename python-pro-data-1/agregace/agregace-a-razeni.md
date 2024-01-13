## Agregace a řazení

V této části si ukážeme další dvě důležité metody pro práci s daty - agregaci a řazení.

### Agregace

Agregace je operace, která "sloučí" více řádků tabulky do jednoho. U agregace většinou slučujeme řádky podle nějakého konkrétního sloupce. V našem případě by nás například mohlo zajímat, jak se liší obsah různých výživných látek pro různé kategorie potravin. 

Opět nás bude zajímat jedna výživná látka. Vyberme si například protein (`Protein`). (V tabulce je použitý název sloupce `nutrient_name`, takto jsme sloupec přejmenovali z původního názvu `name` ve cvičení v předchozí lekci.)

```py
food_merged_brands_protein = food_merged_brands[food_merged_brands["nutrient_name"] == "Protein"]
```

Při agregaci se musíme nejprve rozhodnout, podle jakého sloupce chceme řádky sloučit. V našem případě to bude `branded_food_category`. Poté vybereme sloupec, jehož hodnoty mají být sloučeny, a početní operaci, která k tomu bude použita. Vybereme si sloupec `amount` (množství výživné látky) a operaci výpočtu aritmetického průměru (`.mean()`). Zápis je uvedený v řádku níže.

```py
food_merged_brands_protein.groupby("branded_food_category")["amount"].mean()
```

Ve výsledné tabulce vidíme průměrné množství proteintů v jednotlivých kategoriích potravin. 

Níže je přehled všech funkcí, které lze pro agregaci použít.

- `sum` \- součet hodnot,
- `size` \- počet řádků,
- `count` \- počet hodnot,
- `nunique` \- počet unikátních hodnot,
- `max` \- maximální hodnota,
- `min` \- minimální hodnota,
- `first` \- první hodnota,
- `last` \- poslední hodnota,
- `mean` \- průměr z hodnot,
- `median` \- medián z hodnot,
- `var` \- rozptyl hodnot,
- `std` \- standardní odchylka hodnot,
- `all` \- `True`, pokud jsou všechny hodnoty `True`,
- `any` \- `True`, pokud je alespoň jedna z hodnot `True`.

Dále se pojďme podívat, které kategorie potravin obsahuje v průměru nejvíce proteinů. K tomu použijeme řazení. Data můžeme řadit s využitím metody `sort_values()`. U tabulky, která vznikla agregací a máme v ní jen jeden sloupec (název kategorie je index a mezi sloupce se nepočítá), nemusíme zadávat, podle jakého sloupce má být řazení provedeno. Výchozí způsob řazení je vzestupný, tj. nejmenší hodnoty jsou nahoře. Abychom viděli nahoře nejvyšší hodnoty, použijeme parametr `ascending` a nastavíme mu hodnotu `False`. Nakonec použijeme metodu `head()` a zobrazíme si prvních 10 záznamů.

### Řazení

```py
food_merged_brands_protein_agg.sort_values(ascending=False).head(10)
```

Při řazení dat v původní tabulce je třeba uvést, podle jakého sloupečku chceme data seřadit. Název sloupce nebo sloupců zadáváme jako první parametr. Pokud chceme řadit podle více sloupců, vložíme jejich názvy do seznamu.

```py
food_merged_brands_protein_agg.sort_values("amount", ascending=False)
```
