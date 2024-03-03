## Metoda shift

Uvažujme nyní, že chceme vypočítat délku nějakého výpadku. Určitou komplikací je, že začátek a konec jednoho výpadku je na různých řádcích, nemůžeme tedy postupovat jako při běžném výpočtu. Můžeme ale použít metodu `shift()`, která umí data v jednom sloupci posunout nahoru nebo dolů.

```py
import pandas as pd

signal_monitoring = pd.read_csv("signal_monitoring.csv")
signal_monitoring["event_date_time"] = pd.to_datetime(signal_monitoring["event_date_time"])
```

Nyní použijeme metodu `shift()` na sloupec `event_date_time`. Pomocí metody pak přidáme k tabulce nový sloupec. Nejdůležitějším parametrem metody je parametr periods, který může mít kladnou nebo zápornou hodnotu.

- Kladná hodnota parametru periods znamená, že hodnoty budou posunuty směrem dolů.
- Záporná hodnota parametru periods znamená, že hodnoty budou posunuty směrem nahoru.

Pro náš případ bude ideální, pokud posuneme hodnoty sloupce `event_date_time` o jeden řádek směrem nahoru. Tím zajistíme, že pokud má sloupec `event_type` hodnotu _signal lost_, uvidíme v jednom řádku začátek i konec výpadku. Tím pádem bude stačit tyto hodnoty od sebe odečíst. Pro `event_type` _signal restored_ nebude mít tato hodnota smysl, ale to nevadí, tyto řádky můžeme pomocí dotazu z tabulky odfiltrovat.

```py
signal_monitoring["event_end_date_time"] = signal_monitoring["event_date_time"].shift(periods=-1)
```

Opět v datech ponecháme pouze řádky, které mají ve sloupci `event_type` hodnotu _signal lost_.

```py
signal_monitoring_signal_lost = signal_monitoring[signal_monitoring["event_type"] == "signal lost"]
```

Nyné můžeme spočítat rozdíl mezi začátkem výpadku a koncem výpadku, který udává jeho délku.

```py
signal_monitoring["outage_length"] = signal_monitoring["event_end_date_time"] - signal_monitoring["event_date_time"]
```

Typ hodnoty ve sloupci `outage_length` je označovaný jako `timedelta`. Tento typ hodnoty označuje rozdíl mezi dvěma hodnotami typu `datetime`.

```py
signal_monitoring_signal_lost.groupby("date")["outage_length"].sum()
```

Příklad řešení s využitím ChatGPT je [zde](https://chat.openai.com/share/eb92296b-1968-4387-9cc4-592023d4d104).
