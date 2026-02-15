## Čtení na doma: Doplnění chybějících hodnot

V některých případech je možné prázdné hodnoty doplnit. Při doplňování chybějících hodnot musíme být vždy velmi opatrní, protože doplněním nesprávné hodnoty dojde ke zkreslení výsledků analýz, které s daty provádíme. Před doplněním prázdné hodnoty je dobré vždy vědět, proč hodnoty chybí, to nám pomůže rozhodnout se, zda a jak chybějící hodnoty doplnit.

K doplnění můžeme využít metodu `fillna()`. Tato metoda se nejčastěji používá k doplnění konstantní hodnoty, umí si ale poradit i s dalšími věcmi. Uvažujme například, že máme data ze čtečky čárových kódů, která jsou v souboru [barcode_reader.csv](assets/barcode_reader.csv). Čtečka ukládá informace o činnosti uživatelů a uživatelek, např. přihlášení, přijetí položky, uskladnění položky, zabalení položky atd. V programu pro zápis je ale chyba, zapisuje uživatelské jméno pouze při přihlášení. Víme, že všechny následující akce provedl přihlášený uživatel, a to až do přihlášení dalšího uživatele nebo uživatelky. Využijeme tedy metodu `fillna()` a parametrem `method="ffill"`. Metoda pak do prázdných řádků doplní nejbližší předcházející neprázdnou hodnotu.

```py
import pandas as pd

data = pd.read_csv("barcode_reader.csv")
data['user_name'] = data['user_name'].fillna(method="ffill")
```

Výsledek metody `fillna()` nemusíme vždy ukládat do stejného sloupce. Můžeme ho uložit do nového sloupce, pokud chceme zachovat původní data pro porovnání. Například:

```py
data['user_name_filled'] = data['user_name'].fillna(method="ffill")
```

Tak máme v tabulce oba sloupce - původní `user_name` s prázdnými hodnotami i nový `user_name_filled` s doplněnými hodnotami.

### Cvičení

::exc[excs/ctecka]
