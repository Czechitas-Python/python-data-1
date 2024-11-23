## Agregace a řazení

V této části si ukážeme další dvě důležité metody pro práci s daty - agregaci a řazení. Začneme přípravou dat. V rámci této lekce budeme uvažovat úpravy, které jsme provedli v předchozích lekcích.

```py
# Načteme tabulky o potravinách
food_sample_100 = pd.read_csv("food_sample_100.csv")
food_other = pd.read_csv("food_other.csv")
# Sjednotíme tabulky o potravinách do jedné tabulky food
food = pd.concat([food_sample_100, food_other], ignore_index=True)
# Načteme data o výživných látkách
food_nutrient = pd.read_csv("food_nutrient.csv")
# Propojíme tabulky o potravinách a výživných látkách
food_merged = pd.merge(food, food_nutrient, on="fdc_id")
# Načteme tabulku o značkách potravin
branded_food = pd.read_csv("branded_food.csv")
# Propojíme tabulku o značkách potravin a tabulkou o potravinách a výživných látkách
food_merged_brands = pd.merge(food_merged, branded_food, right_on="fdc_id", left_on="fdc_id", how="left")
# Přejmenujeme sloupec name na nutrient_name
food_merged_brands = food_merged_brands.rename(columns={"name": "nutrient_name"})
# NEPOVINNÝ KROK: Odstraníme nepotřebné sloupce footnote a min_year_acquired
food_merged_brands = food_merged_brands.drop(columns=["footnote", "min_year_acquired"])
```

### Agregace

Agregace je operace, která "sloučí" více řádků tabulky do jednoho. U agregace většinou slučujeme řádky podle nějakého konkrétního sloupce. V našem případě by nás například mohlo zajímat, jak se liší obsah různých výživných látek pro různé kategorie potravin. 

Opět nás bude zajímat jedna výživná látka. Vyberme si například protein (`Protein`). (V tabulce je použitý název sloupce `nutrient_name`, takto jsme sloupec přejmenovali z původního názvu `name` ve cvičení v předchozí lekci.)

```py
food_merged_brands_protein = food_merged_brands[food_merged_brands["nutrient_name"] == "Protein"]
```

Při agregaci se musíme nejprve rozhodnout, podle jakého sloupce chceme řádky sloučit. V našem případě to bude `branded_food_category`. Poté vybereme sloupec, jehož hodnoty mají být sloučeny, a početní operaci, která k tomu bude použita. Vybereme si sloupec `amount` (množství výživné látky) a operaci výpočtu aritmetického průměru (`.mean()`). Zápis je uvedený v řádku níže.

```py
food_merged_brands_protein_agg = food_merged_brands_protein.groupby("branded_food_category")["amount"].mean()
```

Výsledkem není tabulka, ale série. V ní vidíme průměrné množství proteintů v jednotlivých kategoriích potravin. 

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

Při řazení dat v původní tabulce je třeba uvést, podle jakého sloupečku chceme data seřadit. Název sloupce nebo sloupců zadáváme jako první parametr. Pokud chceme řadit podle více sloupců, vložíme jejich názvy do seznamu.

Uvažujme, že chceme najít konkrétní potravinu s největším obsahem proteinu. K tomu využijeme metodu `sort_values()`. Výchozí řazení je vzestupné, chceme-li tedy řadit sestupně, použijeme parametr `ascending` a nastavíme mu hodnotu `False`.

```py
food_merged_brands_protein.sort_values("amount", ascending=False)
```

Nyní chceme znát skupinu potravin s největším obsahem proteinu, protože je např. jednodušší doporučovat skupinu potravin než konkrétní proudukt, který nemusí být v každém obchodě k dispozici. V případě série `food_merged_brands_protein_agg`, kterou jsme vytvořili s využitím metody `groupby()`, není potřeba zadávat název sloupce, to řešíme pouze u tabulek. Využijeme ale parametr `ascending`, který funguje stejně jako v předchozím případě.

```py
food_merged_brands_protein_agg.sort_values(ascending=False)
```

V řadě případů je práce se sérií jednodušší než s tabulkou, jsou tam ale určitá omezení. Např. do tabulky je možné přidat další sloupec. Sérii na tabulku převedeme pomocí metody `.to_frame()`. Výsledek je tabulka, u které můžeme používat všechny metody, které pro tabulky už známe.

```py
food_merged_brands_protein_agg = food_merged_brands_protein_agg.to_frame()
```
