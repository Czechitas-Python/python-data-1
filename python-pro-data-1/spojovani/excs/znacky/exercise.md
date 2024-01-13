---
title: Značky a výrobci
demand: 3
---

Stáhni si data z tabulky [branded_food.csv](assets/branded_food.csv), která obsahuje data o konkrétních potravinách od výrobců. Tabulku načti do `pandas` tabulky `branded_food`.

Pro tabulku `branded_food` splň následující úkoly.

1. Zobraz si prvních několik řádků tabulky a podívej se na to, jaké jsou v ní sloupce a jaké jsou v nich hodnoty.
1. Ve sloupci `brand_owner` jsou názvy výrobců potravin. Zjisti tři výrobce s největším počtem potravin v tabulce.
1. Ve sloupci `branded_food_category` jsou kategorie potravin. Zjisti pět kategorií s největších počtem potravin v tabulce.

V tabulce je sloupec `fdc_id`, pomocí kterého ji můžeš propojit s tabulkou `food_merged`. Protože názvy jsou v obou tabulkách stejné, takže by bylo možné použít parametr `on`. Vyzkoušej si ale místo toho parametry `left_on` a `right_on`, kterým dáš stejnou hodnotu, tj. název sloupce `fdc_id`. Výsledek ulož do tabulky `food_merged_brands`.

Pro tabulku `food_merged_brands` splň následující úkoly.

1. U výsledné tabulky `food_merged_brands` zkontroluj počet řádků a srovnej ho s původní tabulkou `food_merged`. Ubyly nějaké řádky? A čím to je?
1. Nyní proveď operaci `merge` znovu, ale s parametrem `how` nastaveným na hodnotu `left`. Zkontroluj počet řádků a porovnej ho s počtem řádků tabulky `food_merged_brands`. Proč se počet liší?
