## Odkud data bereme

Data se nejčastěji nachází v databázích nebo v souborech.

Data můžeme v základu rozdělit na textová data (včetně dat, která můžeme na texty převést), videa, obrázky a zvuky atd. My se budeme zabývat textovými daty, protože ty jsou pro zpracování nejjednodušší.

### Zápisy textových dat

Podívejme se nyní na základní formáty, jak zapisovat data. Pro nás nejznámější formou je tabulka.

| Jméno |       Věc        | Částka v korunách |
| :----- | :-------------- | ----------------: |
| Petr  |   Prací prášek   |               399 |
| Ondra |       Savo       |                80 |
| Petr  |  Toaletní papír  |                65 |
| Libor |       Pivo       |               124 |
| Petr  | Pytel na odpadky |                75 |
| Míša  | Utěrky na nádobí |               130 |
| Ondra |  Toaletní papír  |               120 |
| Míša  |   Pečící papír   |                30 |
| Zuzka |       Savo       |                80 |
| Pavla |      Máslo       |                50 |
| Ondra |       Káva       |               300 |

S tabulkami pracujeme v software Microsoft Excel (soubory mají příponu `.xlsx`), případně v alternativách jako Google Spreadsheet, Libre Office Calc atd. Python umí pracovat přímo se soubory XLSX, slouží k tomu modul `openpyxl` (můžete ho stáhnout [zde](https://openpyxl.readthedocs.io/en/stable/)), případně s nimi lze pracovat i v `pandas`. Práce s nimi je ale poměrně složitá, proto budeme používat soubory CSV.

Soubor CSV obsahuje data v textové podobě ve struktuře podobné tabulce. Jednotlivé buňky jsou odděleny **středníky** nebo **čárkami**. Většinou závisí na nastavení našeho systému.

```
Jméno,Věc,Částka v korunách
Petr,Prací prášek,399
Ondra,Savo,80
Petr,Toaletní papír,65
Libor,Pivo,124
Petr,Pytel na odpadky,75
Míša,Utěrky na nádobí,130
Ondra,Toaletní papír,120
Míša,Pečící papír,30
Zuzka,Savo,80
Pavla,Máslo,50
Ondra,Káva,300
```

Kromě CVS používáme další dva důležité formáty: JSON (JavaScript Object Notation), XML (Extensible Markup Language).

Formát JSON ti bude povědomý, pokud už jsi v Pythonu pracoval(a) se slovníky (`dict`). Na první pohled vypadají téměř stejně. Python ti navíc jednoduše umožní data ve formátu JSON převést na slovníky a seznamy. K tomu slouží modul příhodně pojmenovaný `json`. S tímto formátem si ale hravě poradí i `pandas`.

```json
[
  {
    "Jméno": "Petr",
    "Věc": "Prací prášek",
    "Částka v korunách": 399
  },
  {
    "Jméno": "Ondra",
    "Věc": "Savo",
    "Částka v korunách": 80
  },
  {
    "Jméno": "Petr",
    "Věc": "Toaletní papír",
    "Částka v korunách": 65
  },
  {
    "Jméno": "Libor",
    "Věc": "Pivo",
    "Částka v korunách": 124
  },
  {
    "Jméno": "Petr",
    "Věc": "Pytel na odpadky",
    "Částka v korunách": 75
  },
  {
    "Jméno": "Míša",
    "Věc": "Utěrky na nádobí",
    "Částka v korunách": 130
  },
  {
    "Jméno": "Ondra",
    "Věc": "Toaletní papír",
    "Částka v korunách": 120
  },
  {
    "Jméno": "Míša",
    "Věc": "Pečící papír",
    "Částka v korunách": 30
  },
  {
    "Jméno": "Zuzka",
    "Věc": "Savo",
    "Částka v korunách": 80
  },
  {
    "Jméno": "Pavla",
    "Věc": "Máslo",
    "Částka v korunách": 50
  },
  {
    "Jméno": "Ondra",
    "Věc": "Káva",
    "Částka v korunách": 300
  }
]
```

Dalším používaným formátem je XML. XML je velmi podobné HTML, tedy jazyku, kterým určujeme, jak má vypadat webová stránka.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<nákupy>
   <nákup jméno="Petr" věc="Prací prášek">399</nákup>
   <nákup jméno="Ondra" věc="Savo">80</nákup>
   <nákup jméno="Petr" věc="Toaletní papír">65</nákup>
   <nákup jméno="Libor" věc="Pivo">124</nákup>
   <nákup jméno="Petr" věc="Pytel na odpadky">75</nákup>
   <nákup jméno="Míša" věc="Utěrky na nádobí">130</nákup>
   <nákup jméno="Ondra" věc="Toaletní papír">120</nákup>
   <nákup jméno="Míša" věc="Pečící papír">30</nákup>
   <nákup jméno="Zuzka" věc="Savo">80</nákup>
   <nákup jméno="Pavla" věc="Máslo">50</nákup>
   <nákup jméno="Ondra" věc="Káva">300</nákup>
</nákupy>
```

XML (a HTML) je založeno na párových značkách (tag). V naší ukázce máme například párovou značku `nákupy`, v níž leží veškerá data. Dále máme značku `nákup`, která symbolizuje řádek tabulky. Každá dvojice se skládá ze zahajovací a ukončovací značky. Ukončovací značku poznáme podle symbolu lomítka (`/`).

Každá značka může mít atributy (attribute), v našem případě máme atributy `jméno` a `věc`. Vždy píšeme název atributu, symbol `=` a hodnotu atributu v uvozovkách (podobně jako v Pythonu můžeme používat apostrofy `'` i složené uvozovky `"`).

Tag může mít i hodnotu, kterou píšeme mezi zahajovací a ukončovací značku.

Protože data zapisujeme jako hodnoty a atributy, můžeme jednu tabulku zapsat více způsoby.

U obou formátů musíme dodržovat základní pravidla, jinak bude soubor pro počítač nečitelný.

### Čtení na doma - formát YAML

Nejnovějším z formátů je YAML (YAML Ain't Markup Language), který vznikl v roce 2011. Byl vyvinut s ohledem pro snadnou čtenost člověkem.

```yaml
- Jméno: Petr
  Věc: Prací prášek
  Částka v korunách: 399
- Jméno: Ondra
  Věc: Savo
  Částka v korunách: 80
- Jméno: Petr
  Věc: Toaletní papír
  Částka v korunách: 65
- Jméno: Libor
  Věc: Pivo
  Částka v korunách: 124
- Jméno: Petr
  Věc: Pytel na odpadky
  Částka v korunách: 75
- Jméno: Míša
  Věc: Utěrky na nádobí
  Částka v korunách: 130
- Jméno: Ondra
  Věc: Toaletní papír
  Částka v korunách: 120
- Jméno: Míša
  Věc: Pečící papír
  Částka v korunách: 30
- Jméno: Zuzka
  Věc: Savo
  Částka v korunách: 80
- Jméno: Pavla
  Věc: Máslo
  Částka v korunách: 50
- Jméno: Ondra
  Věc: Káva
  Částka v korunách: 300
```

Používá se především pro zapisování konfigurace programů.
