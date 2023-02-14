## Agregace

Z databází známe kromě UNION a JOIN také operaci GROUP BY. V `pandas` ji provedeme tak, že pomocí metody `groupby` vyrobíme z `DataFrame` speciální objekt `DataFrameGroupBy`. Dejme tomu, že chceme grupovat podle sloupečku `mistnost`.

```py
maturita.groupby('mistnost')
```

Na tomto speciálním objektu pak můžeme používat různé agregační funkce. Nejjednodušší je funkce `count`

```py
print(maturita.groupby('mistnost').count())
```

```shell
          jméno  předmět  známka  den  datum  předs
místnost
u202         13       13      13   13     13     13
u203         13       13      13   13     13     13
u302         12       12      12   12     12     12
```

Další užitečné agregační funkce jsou například:

- `sum` \- součet hodnot,
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
