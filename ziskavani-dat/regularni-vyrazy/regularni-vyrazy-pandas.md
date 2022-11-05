## Regulární výrazy v Pandas

Regulární výrazy můžeme využívat i v modulu Pandas. Uvažujme, že máme lékařské zprávy uložené v souboru [zpravy.csv](assets/zpravy.csv). Zkusme si soubor stáhnout a nahrát jako tabulku v Pandas.

```py
import pandas

df = pandas.read_csv("zpravy.csv", sep=";")
```

Jako první vyzkoušíme metodu `str.contains()`, která ověří, zda je ve sloupci skupina znaků, která odpovídá regulárnímu výrazu. Můžeme tedy například zkontrolovat, zda je ve sloupci `zapis` skupina znaku, která odpovídá rodnému číslu.

```py
df["obsahuje_rodne_cislo"] = df["zapis"].str.contains(r"\d{9,10}")
```

Komplexnější informací je počet rodných čísel, která jsou v zápise zmíněna. K jeho získání můžeme použít metodu `str.count()`, která vrátí počet skupin znaků, které odpovídají regunárnímu výrazu, jako celé číslo.

```py
df["pocet_rodnych_cisel"] = df["zapis"].str.count(r"\d{9,10}")
```

Pokud chceme rodná čísla vyhledat, můžeme použít metodu `findall()`. Ta vrátí všechny skupiny znaků, které odpovídají regulárnímu výrazu. Protože takových skupin může být více, vloží metoda jednotlivé nalezené řetězce do seznamu.

```py
df["rodna_cisla"] = df["zapis"].str.findall(r"\d{9,10}")
```

Zápisy můžeme též anonymizovat, k tomu využijeme metodu `str.replace()`.

```py
df["anonymni_zapis"] = df["zapis"].str.replace(r"\d{9,10}", "XXX")
```

A provádět můžeme i kontrolu formátu, k tomu slouží dvojice metod `str.match()` a `str.fullmatch()`. Rozdíl v nich je stejný jako u stejnojmenných funkcí modulu `re` - první stačí, že regulární výraz se shoduje s začátkem řetězce, druhá kontroluje celý řetězec a nesmí tam tedy být nic navíc.

```py
df["datum_ok"] = df["datum"].str.match(r"\d{4}-\d{2}-\d{2}")
```
