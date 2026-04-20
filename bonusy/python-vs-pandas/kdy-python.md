## Kdy sáhnout po klasickém Pythonu

Pandas je mocný nástroj, ale není vždy nejlepší volba. Podívejme se, kdy se vyplatí zůstat u čistého Pythonu.

### Nestandardní struktura dat

Typický příklad jsou data bez tabulkové struktury - třeba konfigurační soubor ve formátu JSON, kde každý záznam má jiné klíče. Čistý Python tu bude přirozenější volbou, protože se slovníky a vnořenými strukturami pracuje přímo.

Představme si soubor s nastavením více různých služeb:

```py
konfigurace = [
    {"sluzba": "api", "timeout": 30, "ssl": True},
    {"sluzba": "databaze", "host": "localhost", "port": 5432, "credentials": {"user": "admin"}},
    {"sluzba": "cache", "max_items": 1000},
]
```

Zpracování v čistém Pythonu tu dává větší smysl - kód pracuje přímo se slovníky, takže je dobře čitelný a přehledný. Zkus si představit, jak by tahle data vypadala v Pandas. Musel(a) bys je nejdřív "narovnat" do tabulkové podoby, ale protože každý řádek má jiné sloupce, vznikly by všude hodnoty `NaN`. Výsledný kód by byl složitější, ne jednodušší.

### Malý dataset, jednoduchá transformace

Pandas exceluje při práci s tisíci nebo miliony řádků. Ale co když máš jen patnáct výsledků závodu a chceš z nich zjistit, kdo skončil "na bedně"?

```py
vysledky = [
    [3, "Novák"],
    [1, "Procházka"],
    [5, "Dvořák"],
    [2, "Krejčí"],
    [4, "Horák"],
]

bedna = []
for zavodnik in vysledky:
    if zavodnik[0] <= 3:
        bedna.append(zavodnik)
bedna.sort()

for zavodnik in bedna:
    print(zavodnik[0], "-", zavodnik[1])
```

```shell
1 - Procházka
2 - Krejčí
3 - Novák
```

Protože pořadí je první prvek v ntici, `sort()` řadí podle něj automaticky - není potřeba nic přidávat. Tohle je osm řádků Pythonu, které každý programátor okamžitě přečte. Zavolat `import pandas`, načíst data do DataFrame, filtrovat a seřadit by zabralo víc kódu i víc přemýšlení. Obecně platí, že pro malé datasety, kde jde o jednu nebo dvě transformace, je čistý Python rychlejší na napsání i na spuštění. Navíc pokud skript spouštíš v jiném prostředí - třeba na serveru nebo u kolegy - odpadá starost s instalací Pandas.

### Extrémně velké soubory

Na druhé straně extrémního spektra jsou soubory, které se do paměti prostě nevejdou - třeba logové soubory o velikosti desítek gigabajtů. Pandas při načtení souboru funkcí `read_csv` celý soubor drží v paměti najednou. Python ale umožňuje soubor číst řádek po řádku, aniž by ho celý načetl.

```py
pocet_chyb = 0

with open("server.log", encoding="utf-8") as soubor:
    for radek in soubor:
        if "ERROR" in radek:
            pocet_chyb += 1

print("Počet chyb v logu:", pocet_chyb)
```

Tento kód může zpracovat soubor libovolné velikosti, protože v paměti má vždy jen jeden řádek. Pandas by si v takovém případě stěžoval na nedostatek paměti.

### Vícerozměrná data

Pandas pracuje přirozeně s dvourozměrnými tabulkami - řádky a sloupce. Někdy ale potřebujeme pracovat s daty, která mají více dimenzí - například výsledky studentů v různých předmětech za více školních let (student × předmět × rok). V Pandas to jde, ale práce s více dimenzemi může být nepřehledná. V takovém případě jsou vhodnějšími nástroji specializované knihovny jako `numpy` (pro numerická data) nebo čistě slovníky a seznamy.
