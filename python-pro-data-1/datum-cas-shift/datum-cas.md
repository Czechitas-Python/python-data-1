## Datum a čas

Mnoho datových sad obsahuje nějaké časové údaje. Např. :term{cs="časové razítko" en="time stamp"} je údaj, které indikuje, kdy byla data uložena nebo kdy se nějaká událost stala. Tato informace je klíčová pro řadu analýz, jako je sledování trendů v čase, sezónní analýza, předpovídání a časové řady. Pandas umožňuje řadu operací, jak manipulovat s datem a časem. Při načtení ze souboru je ale nutné na začátku provést převod série na typ `datetime`, aby bylo jasné, že se jedná o časový údaj a nikoli o řetězec.

Uvažujme data o monitorování příjmu nějakého (např. televizního) signálu. Máme data o tom, kdy došlo ke ztrátě signálu (začátek výpadku) a kdy byl signál obnoven (konec výpadku). Data jsou uložena v souboru [signal_monitoring.csv](assets/signal_monitoring.csv)

```py
import pandas as pd

signal_monitoring = pd.read_csv("signal_monitoring.csv")
signal_monitoring["event_date_time"] = pd.to_datetime(signal_monitoring["event_date_time"])
```

Typ `datetime` má sadu vlastností, pomocí kterých můžeme získat určitou část z data a času. Například vlastnost `dt.date` obsahuje pouze datum, tj. "ořeže" údaj o čas. Pomocí této vlastnosti pak můžeme například vypočítat počet výpadků za jednotlivé dny. Budeme pracovat pouze se řádky, které evidují ztrátu signálu, tj. sloupec `event_type` obsahuje hodnotu `signal lost`. Nakonec můžeme použít metodu `.value_counts()`.

```py
signal_monitoring_signal_lost = signal_monitoring[signal_monitoring["event_type"] == "signal lost"]
signal_monitoring_signal_lost = signal_monitoring_signal_lost.reset_index()
signal_monitoring_signal_lost["date"] = signal_monitoring_signal_lost["event_date_time"].dt.date
signal_monitoring_signal_lost["date"].value_counts()
```

Dále můžeme například pomocí vlastnosti `dt.dayofweek` den v týdnu (jako číslo, kde 0 označuje pondělí a 6 neděli), `dt.month` vrátí měsíc jako číslo (stejné jako v kalendáři, tj. leden je 1), `dt.hour` naopak vrací hodinu atd. Můžeme tedy sledovat, jak často se výpadky vyskytují v rámci denní doby, měsíce, dne v týdnu atd.
