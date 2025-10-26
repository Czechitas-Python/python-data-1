---
title: Rapid7
demand: 1
---

Stáhni si soubor [rpd_daily_2y.csv](assets/rpd_daily_2y.csv), který obsahuje informace o vývoji ceny akcie firmy [Rapid7](https://www.rapid7.com/). Soubor si načti do tabulky `data_rpd`, jako index použij sloupec `date`.

* Zjisti, kolik má soubor řádek a kolik sloupců. Jaké jsou názvy jednotlivých sloupců?
* Zjisti, jaký časový rozsah soubor pokrývá. Jsou akcie na konci období draží než na jeho začátku?
* Počet řádků ulož do proměnné `pocet_radku` jako číslo.
* Ulož si do proměnné cenu akcie ze sloupce `close` pro první dostupné datum. Do další proměnné si ulož cenu akcie pro poslední dostupné datum. Nyní uvažuj, že jsi na začátku období nakoupila akcie společnosti v hodnotě 5 tisíc dolarů. Jaká by byla hodnota těchto akcií na konci období?
