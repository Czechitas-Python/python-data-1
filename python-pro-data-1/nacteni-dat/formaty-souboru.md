## Odkud data bereme

Data se nejčastěji nachází v databázích nebo v souborech. V rámci tohoto kurzu budeme pracovat se soubory, proto si o nich řekneme něco víc. Budeme zabývat textovými daty, protože ty jsou pro zpracování nejjednodušší.

V rámci kurzu budeme používat data o potravinách, která [zveřejňuje americké ministerstvo zemědělství](https://fdc.nal.usda.gov/download-datasets.html).

### Zápisy textových dat

Podívejme se nyní na základní formáty, jak zapisovat data. Pro nás nejznámější formou je tabulka. Níže máme tabulku s příkladem několika potravin, které jsou v datové sadě k dispozici.

| fdc_id  | data_type        | description       | food_category_id | publication_date |
|---------:|------------------|-------------------|------------------:|------------------:|
| 2644829 | sub_sample_food  | lentils, dry      | 16.0             | 2023-10-19       |
| 2347263 | sub_sample_food  | heavy cream       | 1.0              | 2022-10-28       |
| 2261954 | sub_sample_food  | Flour, potato     | 11.0             | 2022-04-28       |
| 321470  | sub_sample_food  | Salt, Iodized     | 2.0              | 2019-04-01       |
| 322951  | sub_sample_food  | Hot dogs beef     | 7.0              | 2019-04-01       |

Význam sloupců je následující:

- `fdc_id`: Jedinečný identifikační kód potraviny.
- `data_type`: Typ dat, který popisuje kategorii potraviny.
- `description`: Popis potraviny. Obsahuje název potraviny a může zahrnovat další specifické informace, jako je forma potraviny (např. sušené, vařené atd.).
- `food_category_id`: Identifikační číslo kategorie potravin, které pomáhá klasifikovat potravinu do určité skupiny nebo typu.
- `publication_date`: Datum publikace nebo záznamu dat. Udává, kdy byly informace o potravině zaznamenány nebo aktualizovány.

S tabulkami pracujeme v software Microsoft Excel (soubory mají příponu `.xlsx`), případně v alternativách jako Google Spreadsheet, Libre Office Calc atd. Python umí pracovat přímo se soubory XLSX, slouží k tomu modul `openpyxl` (můžete ho stáhnout [zde](https://openpyxl.readthedocs.io/en/stable/)), případně s nimi lze pracovat i v `pandas`. Práce s nimi je ale poměrně komplexní, proto budeme používat soubory CSV.

Soubor CSV obsahuje data v textové podobě ve struktuře podobné tabulce. Jednotlivé buňky jsou odděleny **středníky** nebo **čárkami**. V rámci České republiky se častěji setkáváme se středníkem, protože čárky používáme pro zápis desetinných míst. Celosvětově je oblíbenější spíše čárka.

```
fdc_id,data_type,description,food_category_id,publication_date
2644829,sub_sample_food,"lentils, dry",16.0,2023-10-19
2347263,sub_sample_food,heavy cream,1.0,2022-10-28
2261954,sub_sample_food,"Flour, potato",11.0,2022-04-28
321470,sub_sample_food,"Salt, Iodized",2.0,2019-04-01
322951,sub_sample_food,Hot dogs beef,7.0,2019-04-01
```


Formát JSON ti bude povědomý, pokud už jsi v Pythonu pracoval(a) se slovníky (`dict`). Na první pohled vypadají téměř stejně. Python ti navíc jednoduše umožní data ve formátu JSON převést na slovníky a seznamy. K tomu slouží modul příhodně pojmenovaný `json`. S tímto formátem si ale hravě poradí i `pandas`.

```json
[
    {
        "fdc_id": 2644829,
        "data_type": "sub_sample_food",
        "description": "lentils, dry",
        "food_category_id": 16.0,
        "publication_date": "2023-10-19"
    },
    {
        "fdc_id": 2347263,
        "data_type": "sub_sample_food",
        "description": "heavy cream",
        "food_category_id": 1.0,
        "publication_date": "2022-10-28"
    },
    {
        "fdc_id": 2261954,
        "data_type": "sub_sample_food",
        "description": "Flour, potato",
        "food_category_id": 11.0,
        "publication_date": "2022-04-28"
    },
    {
        "fdc_id": 321470,
        "data_type": "sub_sample_food",
        "description": "Salt, Iodized",
        "food_category_id": 2.0,
        "publication_date": "2019-04-01"
    },
    {
        "fdc_id": 322951,
        "data_type": "sub_sample_food",
        "description": "Hot dogs beef",
        "food_category_id": 7.0,
        "publication_date": "2019-04-01"
    }
]
```

Dalším používaným formátem je XML. XML je velmi podobné HTML, tedy jazyku, kterým určujeme, jak má vypadat webová stránka.

```xml
<?xml version='1.0' encoding='utf-8'?>
<data>
  <row fdc_id="2644829" data_type="sub_sample_food" food_category_id="16.0"
    publication_date="2023-10-19">
    <description>lentils, dry</description>
  </row>
  <row fdc_id="2347263" data_type="sub_sample_food" food_category_id="1.0"
    publication_date="2022-10-28">
    <description>heavy cream</description>
  </row>
  <row fdc_id="2261954" data_type="sub_sample_food" food_category_id="11.0"
    publication_date="2022-04-28">
    <description>Flour, potato</description>
  </row>
  <row fdc_id="321470" data_type="sub_sample_food" food_category_id="2.0"
    publication_date="2019-04-01">
    <description>Salt, Iodized</description>
  </row>
  <row fdc_id="322951" data_type="sub_sample_food" food_category_id="7.0"
    publication_date="2019-04-01">
    <description>Hot dogs beef</description>
  </row>
</data>
```

XML (a HTML) je založeno na párových značkách (tag). V naší ukázce máme například párovou značku `data`, v níž leží veškerá data. Dále máme značku `row`, která symbolizuje řádek tabulky. Každá dvojice se skládá ze zahajovací a ukončovací značky. Ukončovací značku poznáme podle symbolu lomítka (`/`).

Každá značka může mít atributy (attribute), v našem případě máme atributy `fdc_id`, `data_type`, `food_category_id` a `publication_date`. Vždy píšeme název atributu, symbol `=` a hodnotu atributu v uvozovkách.

Tag může mít i hodnotu, kterou píšeme mezi zahajovací a ukončovací značku.

Protože data zapisujeme jako hodnoty a atributy, můžeme jednu tabulku zapsat více způsoby.

U obou formátů musíme dodržovat základní pravidla, jinak bude soubor pro počítač nečitelný.

### Čtení na doma - formát YAML

Nejnovějším z formátů je YAML (YAML Ain't Markup Language), který vznikl v roce 2011. Byl vyvinut s ohledem pro snadnou čtenost člověkem.

```yaml
- fdc_id: 2644829
  data_type: sub_sample_food
  description: "lentils, dry"
  food_category_id: 16.0
  publication_date: 2023-10-19
- fdc_id: 2347263
  data_type: sub_sample_food
  description: heavy cream
  food_category_id: 1.0
  publication_date: 2022-10-28
- fdc_id: 2261954
  data_type: sub_sample_food
  description: "Flour, potato"
  food_category_id: 11.0
  publication_date: 2022-04-28
- fdc_id: 321470
  data_type: sub_sample_food
  description: "Salt, Iodized"
  food_category_id: 2.0
  publication_date: 2019-04-01
- fdc_id: 322951
  data_type: sub_sample_food
  description: Hot dogs beef
  food_category_id: 7.0
  publication_date: 2019-04-01
```

Používá se především pro zapisování konfigurace programů.
