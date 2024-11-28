---
title: Odstín
demand: 3
---

V rámci lekce jsme porovnávali množství proteinů.

Vytvoř tabulku `food_merged_brands_protein_lipid`, která bude obsahovat data o množství proteinů (`Protein`) a lipidů (`Total lipid (fat)`), tj. z tabulky `food_merged_brands` vyber řádky, které obsahují buď `Protein` nebo `Total lipid (fat)` ve sloupci `nutrient_name`. K vytvoření je nejlepší metoda `isin()`. Pokud si na metodu nepamatuješ, můžeš se inspirovat řádkem níže.

```py
food_merged_brands_protein_lipid = food_merged_brands[food_merged_brands["nutrient_name"].isin("sem_vloz_seznam_s_nazvy_filtrovanych_vyzivnych_latek")]
```

Nyní zkus pomocí histogramu porovnat, jestli je v potravinách více proteinů nebo lipidů. Nejprve zkus vytvořit histogram stejným způsobem, jako jsme to dělali v lekci. Dostaneš obrázek podobný tomu níže.

::fig[První histogram]{src=assets/output_1.png}

Tento obrázek nám ale neříká, kolik je v potravinách tuků a kolik lipidů, protože obě výživné látky jsou smíchané dohromady. Abys dokázal(a) rozlišit mezi oběma výživnými látkami, použij parametr `hue`, kterému zadáš jako hodnotu `nutrient_name`. Díky tomu bude mít každá výživná látka samostatný sloupec se svojí barvou.

::fig[Příklad výsledku]{src=assets/output_2.png}

:::solution

```py
food_merged_brands_protein_lipid = food_merged_brands[
    food_merged_brands["nutrient_name"].isin(["Protein", "Total lipid (fat)"])]
# Šířka intervalu je nastavená na 5, ale můžeš zvolit libovolnou jinou hodnotu nebo parametr bins úplně vynechat
sns.histplot(food_merged_brands_protein_lipid, x="amount", hue="nutrient_name", bins=range(0, 105, 5))
```

:::
