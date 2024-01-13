## Agregace

Agregace je operace, která "sloučí" více řádků tabulky do jednoho. U agregace většinou slučujeme řádky podle nějakého konkrétního sloupce. V našem případě by nás například mohlo zajímat, jak se liší obsah různých výživných látek pro různé kategorie potravin. Opět nás bude zajímat jedna výživná látka.

```py
food_merged_brands_protein.groupby("branded_food_category")["amount"].mean()
```

Další užitečné agregační funkce jsou například:

- `sum` \- součet hodnot,
- `size` \- počet řádků,
- `count` \- počet hodnot v jednotlivých sloupcích,
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

Nemusíme samozřejmě grupovat přes všechny sloupečky. Vybereme si pouze ty, které nás zajímají. Zkusme například spočítat průměrnou známku z jednotlivých předmětů.

```py
print(maturita.groupby('predmet')['znamka'].mean())
```

Pomocí agregací můžeme vyřešit i náš problém s nákupy. Pokud máme stále načtený `Data Frame` `nakupy`, můžeme použít funkci `groupby` podle jména a následně spočítat sumu nákupů pomocí `.sum()`.

```py
nakupy = pandas.read_csv('nakupy.csv')
nakupy_celkem = nakupy.groupby("Jméno")["Částka v korunách"].sum()
print(nakupy_celkem)
```

```shell
Jméno
Libor    124
Míša     160
Ondra    500
Pavla     50
Petr     539
Zuzka     80
Name: Částka v korunách, dtype: int64
```
