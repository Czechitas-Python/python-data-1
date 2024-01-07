## Výběr sloupců

V některých případech nás jako první při práci s daty napadne nějak si data zjednodušit. Například budeme chtít v DataFrame vybrat pouze některé sloupce, a to co nás nezajímá, můžeme zahodit.

K tomu použijeme výběr sloupců pomocí hranatých závorek. Zápis připomíná práci se seznamy - hranatou závorku napíšeme přímo za název proměnné, kde máme uložený `DataFrame`, a do ní vepíšeme název sloupce, který nás zajímá.

```py
food['description']
```

```shell
0                                             lentils, dry
1                                              heavy cream
2                                            Flour, potato
3                                            Salt, Iodized
4                                            Hot dogs beef
                               ...                        
38914                            Yogurt, whole milk, plain
58027                  chicken, breast, boneless, skinless
58591                                 pork, loin, boneless
46593             Oats, whole grain, rolled, old fashioned
7858     Cheese, cheddar, mild, block/chunk, store bran...
Name: description, Length: 100, dtype: object
```

Zde je důležité říct, že pokud vybíráme pouze jeden sloupec, vrátí se nám takzvaná **série** (`Series`), což je jiný datový typ než tabulka (`DataFrame`). Sérii si představme jako **jednorozměrnou strukturu**. Nelze do ní například přidávat další sloupce.

Pro výběr více sloupců musíme do indexace DataFrame vložit seznam s názvy sloupců.

```py
food[["fdc_id", "description"]]
```

```shell
        fdc_id                                        description
0      2644829                                       lentils, dry
1      2347263                                        heavy cream
2      2261954                                      Flour, potato
3       321470                                      Salt, Iodized
4       322951                                      Hot dogs beef
...        ...                                                ...
38914  2260615                          Yogurt, whole milk, plain
58027  2646468                chicken, breast, boneless, skinless
58591  2647032                               pork, loin, boneless
46593  2349564           Oats, whole grain, rolled, old fashioned
7858    328565  Cheese, cheddar, mild, block/chunk, store bran...

[100 rows x 2 columns]
```

Tady se nám již vrátil datový typ DataFrame. Tohoto triku můžeme využít, když chceme získat pouze jeden sloupec, ale chceme ho získat jako DataFrame.

```py
food[['description']]
```

```shell
                                             description
0                                           lentils, dry
1                                            heavy cream
2                                          Flour, potato
3                                          Salt, Iodized
4                                          Hot dogs beef
...                                                  ...
38914                          Yogurt, whole milk, plain
58027                chicken, breast, boneless, skinless
58591                               pork, loin, boneless
46593           Oats, whole grain, rolled, old fashioned
7858   Cheese, cheddar, mild, block/chunk, store bran...

[100 rows x 1 columns]
```
