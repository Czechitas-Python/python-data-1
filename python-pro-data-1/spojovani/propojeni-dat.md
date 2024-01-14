## Propojení dat

`pandas` však umí `DataFrame` také propojit, což odpovídá SQL příkazu `JOIN`. Vyvětleme si propojení na našich datech. V tabulce `food_nutrient` máme výživné látky v potravinách, nevíme ale názvy potravin, kterých se hodnoty týkají. V tabulce `food` máme názvy potravin. Propojení dat nám umožní ke každé informaci o výživných látkách připojit název potraviny.

Jak ale `pandas` pozná, která informace o výživné láce patří jaké potravině? K tomu využijeme sloupec s identifikačním číslem potraviny. Při propojování tabulek vždy potřebujeme nějaký sloupeček (nebo sloupečky), pomocí kterého propojení provedeme. Pokud se sloupce jmenují v obou tabulkách stejně, můžeme použít parametr `on`. Pokud se jmenují různě, je třeba využít parametry `left_on` (název v tabulce, kterou zapisujeme jako první) a `right_on` (název v tabulce, kterou zapisujeme jako druhou). Propojovat tabulky můžeme i pomocí více sloupců, v takovém případě je třeba zapsat názvy jako seznam.

Dále si položme otázku, co se stane, pokud bude nějaká potravina zastoupena pouze v jedné tabulce. Je například možné, že nějakou potravinu máme v tabulce `food`, ale nemáme k ní žádné údaje o výživných látkách. Co v takovém případě máme dělat? A co v případě "anonymních" potravin, o kterých víme výživné látky, ale nemáme je uvedené v tabulce `food`?

Na to neexistuje univerzální odpověď, vždy záleží na konkrétní situaci. Můžeme si tedy vybrat jednu z několika variant provedení operace `merge`. Vybranou variantu zadáváme jako parametr `how`.

- `inner` zachová pouze řádky, které mají svůj "protějšek" ve druhé tabulce. Pokud bychom tedy k nějaké potravině nevěděli žádnou výživnou látku a spojili tabulky pomocí `inner`, tato potravina ve výsledné tabulce nebude. Tato varianta je výchozí, tj. je použita, když parametr `how` nezadáme.
- `left` a `right` jsou varianty, kdy tabulky mají **nerovnocenné postavení**. Při této variantě zachováme kompletní jednu z tabulek, ale tu druhou ne. Je určitou zvyklostí využívat variantu `left`, která zachová celou tu tabulku, kterou zapíšeme jako první.
- `outer` zachová všechny řádky z obou tabulek.

```py
food_merged = pd.merge(food, food_nutrient, on="fdc_id")
```

U tabulky `food_merged` je dobré zkontrolovat počet řádků a porovnat ho s původními tabulkami.
