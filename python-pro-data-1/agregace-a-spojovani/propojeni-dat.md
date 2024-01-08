## Propojení dat

`pandas` však umí `DataFrame` také propojit, což odpovídá SQL příkazu `JOIN`. Vyvětleme si propojení na našich datech. V tabulce `food_nutrient` máme výživné látky v potravinách, nevíme ale názvy potravin, kterých se hodnoty týkají. V tabulce `food` máme názvy potravin. Propojení dat nám umožní ke každé informaci o výživných látkách připojit název potraviny.

Jak ale `pandas` pozná, která informace o výživné láce patří jaké potravině? K tomu využijeme sloupec s identifikačním číslem potraviny. Při propojování tabulek vždy potřebujeme nějaký sloupeček (nebo sloupečky), pomocí kterého propojení provedeme.



U operace `JOIN` jsou důležité dvě věci:

- **Podle jakého sloupce** (nebo jakých sloupců) dvě různé tabulky propojujeme.
- Co udělat v případě, že pro nějaké řádky **nemám ve druhé tabulce odpovídající hodnotu**.

Propojení tabulek se v `pandas` dělá pomocí funkce `merge` (dokumentaci k ní je [zde](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.merge.html)). Ve výchozím nastavení funkce `merge` provádí spojení podle sloupců, které mají shodný název. V našem případě mají oba `DataFrame` sloupec `cisloStudenta`, je tedy použit tento sloupec. Je to přesně ten sloupec, podle kterého bychom je chtěli spojit.

Ve výchozím nastavení funkce `merge()` ponechá pouze řádky, které mají záznamy v obou tabulkách. V SQL bychom tuto operaci označili jako `INNER JOIN`.

```py
propojeny_df = pandas.merge(u202, studenti)
print(propojeny_df.head())
```

```shell
   cisloStudenta           predmet  znamka den           jmeno
0              1            Chemie     NaN  pá  Jana Zbořilová
1              2           Dějepis     3.0  pá    Lukáš Jurčík
2              2  Společenské vědy     2.0  pá    Lukáš Jurčík
3              3        Matematika     2.0  út     Pavel Horák
4              3            Chemie     5.0  út     Pavel Horák
```

Pokud by například nějaký student nebyl uvedený v tabulce se studenty, jeho maturitní výsledek by zmizel. U nového `DataFrame` bychom tedy měli zkontrolovat, zda má `propojeny_df` stejný počet řádků jako `u202`.

```py
print(u202.shape)
```

```py
print(propojeny_df.shape)
```

Zde vidíme, že data jsou zřejmě v pořádku.

Dále připojíme tabulku [predsedajici.csv](assets/predsedajici.csv), kde máme vypsané předsedy maturitních komisí. Tu si opět načteme jako `DataFrame`.

```py
preds = pandas.read_csv('https://kodim.cz/cms/assets/analyza-dat/python-data-1/python-pro-data-1/agregace-a-spojovani/propojeni-dat/predsedajici.csv')
```

Zkusme tabulky spojit jako předtím.

```py
novy_propojeny_df = pandas.merge(propojeny_df, preds)
print(novy_propojeny_df.head())
```

Tentokrát jsme příliš neuspěli, výsledný `DataFrame` je prázdný. Proč tomu tak je? Protože v obou `DataFrame` máme sloupec `jmeno`, v jednom případě však jde o jméno studenta a ve druhém o jméno předsedy komise. To ale `pandas` samozřejmě neví. Proto mu musíme říct, že chceme data spojit pouze podle sloupce `den`.

```py
novy_propojeny_df = pandas.merge(propojeny_df, preds, on=['den'])
print(novy_propojeny_df.head())
```

```shell
       datum           jmeno_x den  cisloStudenta     predmet  znamka mistnost          jmeno_y
0  21.5.2019  Marie Zuzaňáková  út              3  Matematika     2.0     u202      Pavel Horák
1  21.5.2019  Marie Zuzaňáková  út              3      Chemie     5.0     u202      Pavel Horák
2  22.5.2019     Petr Ortinský  st             10      Chemie     2.0     u202  Miroslav Bednář
3  22.5.2019     Petr Ortinský  st             10     Dějepis     5.0     u202  Miroslav Bednář
4  22.5.2019     Petr Ortinský  st             11  Matematika     1.0     u202  Ivana Dvořáková
```

Zatím to vypadá dobře. Pokud se ovšem podíváme na `shape`, něco nám tady nehraje.

```py
print(novy_propojeny_df.shape)
```

Najednou máme v tabulce pouze 12 řádků, některé tedy zmizely. To znamená, že funkce `merge()` nenašla pro všechna zkoušení odpovídajícího předsedu. Jak je to možné? Zkusme nyní říct funkci `merge()`, aby nám zachovala v prvním `DataFrame` ty řádky, pro které nenajde odpovídající záznam. Této operaci se v jazyce SQL říká LEFT OUTER JOIN. My ho provede tak, že funkci `merge()` jako parametr `how` zadáme hodnotu `left`.

```py
novy_propojeny_df = pandas.merge(propojeny_df, preds, on=['den'], how="outer")
print(novy_propojeny_df.shape)
```

Tentokrát jsme již o data nepřišli, ale kde se stala chyba? Zkusme si zobrazit ty řádky, které se nepodařilo propojit. Poznáme je tak, že mají prázdný sloupec `datum`.

```py
print(novy_propojeny_df[novy_propojeny_df["datum"].isnull()])
```

```shell
   cisloStudenta     predmet  znamka den mistnost           jmeno_x datum jmeno_y
5            5.0     Dějepis     1.0  po     u202  Kateřina Novotná   NaN     NaN
6            7.0     Dějepis     4.0  po     u202       Vasil Lácha   NaN     NaN
7            8.0  Matematika     2.0  po     u202    Alexey Opatrný   NaN     NaN
```

Nyní jsme již na stopě problému. Z nějakého důvodu nám nefunguje propojení v případě, že ve sloupci `den` je hodnota `po`. Po chvíli zkoumání zjistíme, že za chybu může nenápadná mezera, která je ve sloupci `den` za hodnotou `po` v souboru `prednasejici.csv`. Ať už vznikla chyba překlepem nebo nějakou jinou chybou, takové věci se bohužel stávají a proto při práci s daty musíme neustále kontrolovat, zda jsme nějako operací o část dat nepřišli.

Pokud nemáme možnost vstupní data opravit, můžeme použít funkci `strip()`, která z řetězce odstraní mezery (a další bílé znaky) na začátku a na konci. Tyto mezery jsou v drtivé většině případů způsobeny chybou a proto jejich odstraněním nic nezkazíme.

```py
preds["den"] = preds["den"].str.strip()
novy_propojeny_df = pandas.merge(propojeny_df, preds, on=['den'], how="outer")
print(novy_propojeny_df.shape)
```

Poslední nepříjemností, na kterou se podíváme, je to, že sloupce `jmeno` se automaticky přejmenovaly, aby neměly v tabulce stejný název. Zde můžeme použít metodu `rename`, abychom sloupečky přejmenovali na něco smysluplného.

```py
novy_propojeny_df = novy_propojeny_df.rename(columns={'jmeno_x': 'jmeno', 'jmeno_y': 'predseda'})
```
