---
title: Booking
demand: 1
---

Stáhni si data ze souboru o rezervacích hotelů ze serveru Booking.com. Data jsou uložená v souboru [hotel_bookings.csv](assets/hotel_bookings.csv). U rezervací evidujeme, jestli byly zrušené, to najdeme ve sloupci `is_canceled` (1 pro zrušené rezervace a 0 pro nezrušené). Vytvoř kontingenční tabulku, která porovná počet zrušených rezervací podle typu hotelu (sloupec `hotel`). Je více rezervací zrušeno pro městské hotely nebo pro hotely v rezortech?

Dále zkus rezervace rozdělit do skupin podle toho, v jakém předstihu byly rezervace provedeny. Zaměř se pouze na rezervace v městských hotelech, tj. vytvoř tabulku, která bude obsahovat pouze data, která mají ve sloupci `hotel` hodnotu `City Hotel`. Využij sloupec `lead_time`. Níže máš skupiny, podle kterých můžeš data rozdělit. Vytvoř si pivot tabulku, která zobrazuje počty rezervací v jednotlivých kategoriích v závislosti na tom, jestli byly zrušeny nebo ne. Pro které kategorie je více zrušených rezervací a pro které naopak více nezrušených? A v jaké skupině je celkově nejvíce rezervací?

| Lead Time              | Reservation Category    |
|------------------------|-------------------------|
| 0-7                    | Last-minute             |
| 8-30                   | Short-term              |
| 31-180                 | Medium-term             |
| 180-inf                | Long-term               |


:::solution
```py
import pandas as pd

bookings = pd.read_csv("hotel_bookings.csv")

# Kontingenční tabulka s počty zrušených rezervací podle typu hotelu
pd.crosstab(bookings["hotel"], bookings["is_canceled"], margins=True)

# Filtrujeme pouze městské hotely
city_hotels = bookings[bookings["hotel"] == "City Hotel"]

# Vytvoříme kategorie podle lead_time
bins = [0, 7, 30, 180, float('inf')]
labels = ['Last-minute', 'Short-term', 'Medium-term', 'Long-term']
city_hotels['reservation_category'] = pd.cut(city_hotels['lead_time'], bins=bins, labels=labels)

# Pivot tabulka s počty rezervací podle kategorií a toho, jestli byly zrušeny
pd.crosstab(city_hotels["reservation_category"], city_hotels["is_canceled"], margins=True)
```
:::
