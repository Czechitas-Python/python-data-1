## Čtení na doma

### Více různých agregací

Pokud chceme provést více různých agregací, použijeme metodu `agg`. Metodě `agg` vložíme jako parametr slovník, kde klíčem je název sloupce, pro který počítáme agregaci, a hodnotou je řetězec nebo seznam řetězců se jmény agregací, které chceme provést. Například u maturity chceme zjistit, jestli student prospěl, prospěl s vyznamenáním nebo neprospěl. K tomu potřebujeme funkci `max()` (pětka znamená, že student neuspěl a trojka znamená, že nemůže získat vyznamenání) a funkci `mean()` (abychom zjistili, zda je průměr známek menší než 1.5).

```py
maturita.groupby("cisloStudenta").agg({"znamka": ["max", "mean"]})
```

K určení výsledku studenta bychom ještě potřebovali nový sloupec, jehož hodnota bude určena na základě podmínky, což si ukážeme níže.


### Podmíněný sloupec

Občas chceme do výpočtu zapracovat i podmínku. Ve skutečnosti je podmínka to poslední, co nám chybělo k vyřešení našeho problému s finančním vypořádání spolubydlících pomocí `pandas`. Náš výpočet se skládá z pěti kroků.

1. Provedeme agregaci hodnot nákupů podle jmen. Tím zjistíme sumu, kolik každý utratil.
1. Zjistíme si průměrnou útratu za osobu. K tomu použijeme funkci `mean()`.
1. Přidáme sloupec s podmínkou. V podmínce porovnáváme, zda spolubydlící utratil více nebo méně, než je průměr. K tomu použijeme funkci `where`, která je součástí modulu `numpy`. Nejprve provedeme import modulu `numpy` a následně z modulu zavoláme funkci `where()`. Jako první parametr zadáme podmínku (porovnání hodnot), jako druhý hodnotu vloženou v případě splnění podmínky (text "má dáti") a jako poslední hodnotu vloženou v případě nesplnění podmínky (text "dostane"). Jako předposlední krok si určíme částku potřebnou k vypořádání - rozdíl mezi součtem pro danou osobu a průměrnou útratou. Poslední krok je pak jen výpisem hodnoty.

```py
import numpy

nakupy = pandas.read_csv('nakupy.csv')
nakupy_celkem = nakupy.groupby("Jméno")[["Částka v korunách"]].sum()
prumerna_hodnota = nakupy_celkem["Částka v korunách"].mean()

nakupy_celkem["Operace"] = numpy.where(nakupy_celkem["Částka v korunách"] > prumerna_hodnota, "má dáti", "dostane")
nakupy_celkem["Kolik"] = abs(nakupy_celkem["Částka v korunách"] - prumerna_hodnota)
print(nakupy_celkem[["Operace", "Kolik"]])
```

```shell
       Operace       Kolik
Jméno
Libor  dostane  118.166667
Míša   dostane   82.166667
Ondra  má dáti  257.833333
Pavla  dostane  192.166667
Petr   má dáti  296.833333
Zuzka  dostane  162.166667
```

Srovnej si toto řešení s tím, které jsme si ukazovali na úvodním workshopu. Zdá se ti jednodušší?
