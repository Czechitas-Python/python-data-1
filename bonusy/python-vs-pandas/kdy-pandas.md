## Kdy sáhnout po Pandas

Vzpomeňme si, co jsme o Pandas zjistili v předchozích lekcích - umí načítat soubory, filtrovat řádky, spojovat tabulky a počítat agregace. Tahle sekce ukazuje, kde Pandas skutečně svítí a proč by ruční Python byl zbytečně pracný.

### Větší soubory ve standardním formátu

Máš CSV s výsledky maturitních zkoušek za celý kraj - třicet tisíc řádků, osm sloupců. Chceš vědět průměrné skóre podle školy. V čistém Pythonu bys musel(a) ručně parsovat CSV, skupinovat záznamy do slovníku a počítat průměry. Pandas to zvládne ve třech řádcích. S rostoucím počtem řádků roste i výhoda Pandas - operace jsou napsané v jazyce C a jsou výrazně rychlejší než ekvivalentní Python cykly.

### Spojování více souborů

Uvažujme, že dostaneš výsledky školního kola olympiády jako tři samostatné soubory - jeden za každý ročník. Chceš je sloučit do jedné tabulky a přidat sloupec s ročníkem. Funkce `concat` sloučí tabulky pod sebe (přidá řádky za sebou, jako SQL UNION), `merge` pak propojí tabulky přes společný klíč - tedy jako SQL JOIN, kdy každý řádek z jedné tabulky najde odpovídající řádek v druhé. Ruční implementace těchto operací v čistém Pythonu by byla náchylná k chybám a zbytečně dlouhá.

### Klasické analytické úlohy

Čistění dat, přejmenování sloupců, doplňování chybějících hodnot, přetypování - to jsou úlohy, pro které Pandas vznikl. Představ si tabulku výdajů domácnosti, kde chybí část hodnot a datum je ve špatném formátu.

```py
vydaje = pandas.read_csv("vydaje.csv")

vydaje["castka"] = vydaje["castka"].fillna(0)
vydaje["datum"] = pandas.to_datetime(vydaje["datum"], format="%d.%m.%Y")
vydaje = vydaje.rename(columns={"castka": "kc", "popis": "kategorie"})

vydaje.dtypes
```

```shell
datum        datetime64[ns]
kategorie            object
kc                  float64
dtype: object
```

Každá z těchto operací by se v čistém Pythonu musela psát jako cyklus s podmínkami.

### Časové řady

Pandas umí pracovat s daty v čase zvláštním způsobem - dokáže například vzorkovat záznamy po týdnech, počítat klouzavý průměr nebo najít meziroční změnu. K tomu slouží funkce `resample` (agregace do časových oken - den, týden, měsíc) a `rolling` (statistika přes klouzající okno). Taková analýza by v čistém Pythonu vyžadovala netriviální kód.

### Statistika, strojové učení a vizualizace

Pokud chceš udělat lineární regresi, natrénovat klasifikátor nebo nakreslit graf, pravděpodobně použiješ knihovny jako `scikit-learn`, `statsmodels` nebo `matplotlib`. Všechny tyto knihovny umí přijmout Pandas DataFrame přímo jako vstup. Kdybys data měl(a) v čistém Pythonu (jako seznam seznamů), musel(a) bys je nejdřív převést.
