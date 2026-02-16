---
title: Hlasy
demand: 3
---

V souboru [votes.csv](assets/votes.csv) jsou data o výsledcích voleb ve Spojených státech amerických v období 1976 až 2020. Důležité pro nás budou následující sloupce:

- `year` \- rok,
- `state` \- stát,
- `party_detailed` \- strana, za kterou kandidát(ka) kandidoval(a),
- `candidatevotes` \- počet hlasů pro kandidáta (kandidátku) v daném roce a státě.

Vytvoř tabulky, které budou zobrazovat následující data.

1. Celkový počet odevzdaných hlasů v jednotlivých letech.
2. Celkový počet odevzdaných hlasů pro kandidáty a kandidátky jednotlivých stran pro volby v roce 2020.
3. Celkový počet odevzdaných hlasů pro kandidáty dvou hlavních stran (`DEMOCRAT` a `REPUBLICAN`) pro všechny volby od roku 1980.
4. Celkový počet odevzdaných hlasů pro stát `MONTANA` pro všechny roky. 

:::solution
```py
import pandas as pd

data = pd.read_csv('votes.csv')

#Celkový počet odevzdaných hlasů v jednotlivých letech.
data.groupby('year')['candidatevotes'].sum()

#Celkový počet odevzdaných hlasů pro kandidáty a kandidátky jednotlivých stran pro volby v roce 2020.
data[data['year']==2020].groupby('party_detailed')['candidatevotes'].sum().sort_values(ascending=False)

# Celkový počet odevzdaných hlasů pro kandidáty dvou hlavních stran (DEMOCRAT a REPUBLICAN) pro všechny volby od roku 1980.
# actully for now data has only year 1980 (for condition  od roku 1980)
year = 1980
filter = (data['party_detailed'].isin(['DEMOCRAT','REPUBLICAN'])) &  (data['year']>=year)
data[filter].groupby('year')['candidatevotes'].sum().sort_values(ascending=False)

#Celkový počet odevzdaných hlasů pro stát MONTANA pro všechny roky.
data[data['state']=='MONTANA']['candidatevotes'].sum()
```
:::
