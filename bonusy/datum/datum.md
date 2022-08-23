## Datum a čas

Data patří k základním datovým typům a provázejí nás celý život. Každý máme své datum narození, datum, kdy jsme šli poprvé do školy atd. Data jsou však záludná v tom, že je můžeme zapsat v různých formátech. Pojďme se podívat, jak data v různých formátech zpracovat a jak je naopak vypsat.

### Vytvoření data

K práci s daty potřebujeme modul `datetime`, který je základní součástí Pythonu, takže jej nemusíme instalovat. Stačí jej importovat.

```pycon
>>> from datetime import datetime, timedelta
```

Občas je matoucí, že modul `datetime` se dále člení. Obsahuje typ pro uložení samotného data (bez času) `date`, typ pro uložení samotného času (bez data) `time`, typ pro uložení data i času `datetime`, typ pro práci s intervaly `timedelta` a typ pro práci s časovými zónami `tzinfo`. Proto jsme napsali poněkud legrační zápis, ve kterém je dvakrát `datetime`.

První datum, které nás napadne, je aktuální.

```pycon
>>> datetime.now()
datetime.datetime(2020, 11, 21, 20, 26, 26, 472567)
```

Nejjednodušší způsob, jak datum vytvořit, je pomocí funkce `datetime`. Té zadáváme hodnoty postupně: nejprve rok, poté měsíc, den, hodiny, minuty, sekundy a mikrosekundy. Pouze první tři parametry jsou povinné, další vypisovat nemusíme a Python si za ně případně dosadí nuly.

Zkusme si vytvořit proměnnou, která bude reprezentovat start Apolla 11.

```pycon
>>> apollo_start = datetime(1969, 7, 16, 14, 32)
>>> print(apollo_start)
```

Pokud by nás zajímalo, jaký den v týdnu Apollo startovalo, můžeme použít funkci `weekday()` nebo `isoweekday()`. Pozor, je mezi nimi rozdíl. Obě číslují od pondělí, funkce `weekday()` však čísluje od 0 a funkce `isoweekday()` od 1.

```pycon
>>> apollo_start.weekday()
2
>>> apollo_start.isoweekday() 
3
```

#### Formátování

Hodnotu aktuální proměnné můžeme vypsat na obrazovku pomocí funkce `print()`. Ta vypíše datum v tzv. ISO formátu (jako oddělovač data a času je použita mezera).

```pycon
>>> print(apollo_start)
1969-07-16 14:32:00
```

Standardně je jako oddělovač použit symbol `T`. Stoprocentně autentický zápis v ISO formátu získáme pomocí funkce `isoformat()`.

```pycon
>>> apollo_start.isoformat()
'1969-07-16T14:32:00'
```

Často ale chceme data vypsat v jiném formátu. Ve střední Evropě jsme zvyklí psát na začátek číslo dne, pak měsíc atd. a jako oddělovač používáme tečku. Pokud chceme výpis v tomto formátu, musíme to Pythonu říct. 

Pokud chceme datum vypsat ve vlastním formátu, použijeme funkci `strftime()`. Ta používá tzv. direktivy, což jsou vlastně značky, které reprezentují nějaký konkrétní časový údaj. Tyto značky poskládáme do řetězce a ten pak tvoří instrukce pro Python, jak má zpracovat datum. Základní direktivy jsou v tabulce níže.

| Direktiva  | Význam |
|:---| :---|
| `%d`  | den  |
| `%m`  | měsíc |
| `%Y`  | rok (nezkrácený) |
| `%H`  | hodina (rozsah 0-23) |
| `%M`  | minuta |
| `%S`  | sekunda |

 Kompletní tabulku s direktivami najdeš [zde](https://docs.python.org/3/library/datetime.html#strftime-and-strptime-format-codes). 

 Zkusme si třeba vypsat datum startu Apolla 11 v našem středoevropském formátu.

```pycon
>>> apollo_start.strftime("%d. %m. %Y, %H:%M")            
'16. 07. 1969, 14:32'
```

#### Čtení data z výstupu

Bohužel data často získáváme jako řetězce (např. z CSV souborů, ze vstupů od uživatele atd.). Abychom s ním mohli pracovat, musíme si ho převést na typ `datetime`. 

Pokud jsou data v ISO formátu, máme vyhráno. Je možné použít funkci `fromisoformat()`, které stačí zadat řetězec a ona se již o vše postará.

```pycon
>>> apollo_pristani = datetime.fromisoformat("1969-07-21T18:54:00")
```

Takové štěstí ale často nemáme, protože řada programů ukládá datum ve formátu, který má nastavený aktuální uživatel. K načtení pak použijeme funkci `strptime()`, které zadáme formát data a času, se kterým máme tu čest.

```pycon
>>> apollo_pristani = datetime.strptime("21. 7. 1969, 18:54", "%d. %m. %Y, %H:%M")
```

#### Počítání s daty

Často s daty potřebujeme počítat. Pokud například víme, kdy závodník proběhl startem a cílem, můžeme spočítat, kolik času strávil na trati. Dvě data od sebe můžeme jednoduše odečíst. Zkusme si spočítat, jak dlouho trvala mise Apollo.

```pycon
>>> delka_mise = apollo_pristani - apollo_start
>>> print(delka_mise)
5 days, 4:22:00
```

Výsledek je hodnota typu `timedelta`.

## Cvičení: Datum a čas
::exc[excs>prevod-casu]
::exc[excs>cas-od-startu]
::exc[excs>doprava-vecere]
