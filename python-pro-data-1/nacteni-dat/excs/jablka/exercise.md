---
title: Jablka
demand: 1
---

Stáhni si soubor [aapl_daily_2y.csv](assets/aapl_daily_2y.csv), který obsahuje informace o vývoji ceny akcie firmy Apple. Soubor si načti do tabulky `data_aapl`, jako index použij sloupec `date`.

* Zjisti, kolik má soubor řádek a kolik sloupců. Jaké jsou názvy jednotlivých sloupců?
* Použij metody `head()` a `tail()`. Jaký časový rozsah soubor pokrývá? Jsou akcie na konci období draží než na jeho začátku?
* Počet řádků ulož do proměnné `pocet_radku` jako číslo.
* Ulož si do proměnné cenu akcie ze sloupce `close` pro první dostupné datum. Do další proměnné si ulož cenu akcie pro poslední dostupné datum. Nyní uvažuj, že jsi na začátku období nakoupila akcie společnosti v hodnotě 9 tisíc dolarů. Spočítej, kolik akcií si za tuto částku můžeš koupit. Dále spočítej, jaká by byla hodnota těchto akcií na konci období, pro které máme data.
* Pokud bys akcie prodal(a) na konci tohoto období, jaký bude zisk v procentech?

:::solution

```py
import pandas as pd

data_aapl = pd.read_csv("aapl_daily_2y.csv")
data_aapl = data_aapl.set_index("date")

data_aapl.shape[0]  # počet řádků v tabulce
data_aapl.shape[1]  # počet sloupců v tabulce
data_aapl.columns  # názvy všech sloupců

start_date = data_aapl.index[0]  # datum začátku sledovaného období
end_date = data_aapl.index[-1]  # datum konce sledovaného období

data_aapl.head()
data_aapl.tail()

start_close = data_aapl["close"].iat[0]  # zavírací cena na začátku období
end_close = data_aapl["close"].iat[-1]  # zavírací cena na konci období

initial_investment = 9000  # výchozí investice v USD
share_count = initial_investment / start_close  # počet nakoupených akcií
final_value = share_count * end_close  # hodnota investice na konci období

profit_percent = (final_value - initial_investment) / initial_investment * 100  # zisk v procentech
```

:::
