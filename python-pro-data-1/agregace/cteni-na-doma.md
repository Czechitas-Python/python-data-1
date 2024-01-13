## Čtení na doma

Pokud chceme provést více různých agregací, použijeme metodu `agg`. Metodě `agg` vložíme jako parametr jako slovním. S tí už jsme se seznámili při přejmenování sloupce. Do slovníku vložíme jako první prvek dvojice název sloupce a jako druhý prvek seznam agregací, které chceme provést. Seznam zapisujeme do hranatých závorek, jak už jsme zvyklí.

V našem případě budeme chtít vedle průměrného množství proteinů vědět i počet potravin, ze kterého je průměr spočítaný. Pokud by byl vypočítaný například pouze z jedné nebo dvou potravin, není to příliš reprezentativní vzorek. V takovém případě by bylo lepší například skupinu z tabulky výsledků vyřadit, získat pro danou kategorii více údajů atd.

```py
food_merged_brands_protein_agg = food_merged_brands_protein.groupby("branded_food_category").agg({"amount": ["mean", "count"]})
```

Nyní již máme v tabulce (kromě indexu) dva sloupce, při řazení bude potřeba uvést, podle kterého chceme řadit. Podívejme se na vlastnost `.columns`. Sloupce, které máme v tabulce, jsou specifické, protože jejich názvy se skládají ze dvou řetězců - názvu sloupce, ze kterého jsou hodnoty počítány, a názvu agregace. Pokud bychom chtěli například řadit podle průměrného množství, musíme zadat název sloupce jako dva řetězce v kulatých závorkách. Kulaté závorky ve skutečnosti znamenají, že vytváříme typ hodnoty označovaný jako n-tice (*tuple*).

```py
food_merged_brands_protein_agg = food_merged_brands_protein_agg.sort_values(by=("amount", "mean"), ascending=False)
```

Podobně můžeme používat tento zápis názvu sloupce i při psaní dotazu. Zkusme třeba ponechat kategorii v datech pouze v případě, že k ní máme alespoň 10 záznamů.

```py
food_merged_brands_protein_agg = food_merged_brands_protein_agg[food_merged_brands_protein_agg[("amount", "count")] > 10]
```

Nyní máme v tabulce `food_merged_brands_protein_agg` průměrné hodnoty pouze pro kategorie, pro které máme více než 10 údajů o množství proteinu.
