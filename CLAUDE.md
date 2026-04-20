# INSTRUKCE PRO GENEROVÁNÍ TEXTU VE STEJNÉM STYLU

## 1. Charakter stylu

- Styl je **přátelský a konverzační**, ale věcný — text mluví se čtenářem přímo, jako zkušený kolega u počítače.
- Každý nový koncept je **nejprve motivován konkrétním životním příkladem** (výlet, bankovní účet, vysvědčení, kosmická mise), teprve poté přichází abstrakce.
- Abstraktní definice se **nikdy nepíší před konkrétním příkladem** — pořadí je vždy: situace → kód → vysvětlení.
- Text je **iterativní**: ukázka nejprve pracuje, pak se rozšiřuje, pak zobecňuje.
- Kód a text jsou **rovnocenné poloviny** — žádná část není jen ozdobou druhé.
- Varování a záludnosti se uvádějí explicitně, tučným textem (`**POZOR!**`) nebo jako pojmenovaná `**Poznámka:**`.
- Klíčové je **propojení s tím, co čtenář již zná** — nový pojem se téměř vždy uvede větou, která odkazuje na dříve naučené.
- Terminologie se zavádí v *kurzívě* s českým i anglickým názvem, nebo alespoň s anglickým výrazem v závorce.
- Text nikdy nepůsobí akademicky ani jako dokumentace — cílem je, aby čtenář **pochopil „proč"**, ne jen „co".
- Délka výukové stránky je typicky **300–600 slov + kód**.

---

## 2. Logická struktura a práce s kontextem

**Propojování nových pojmů s dříve vysvětlenými:**
- Každý nový oddíl začíná větou, která explicitně navazuje: „Už jsme viděli...", „Zatím jsme poznali...", „Vzpomeňme si, že...".
- Nový pojem se zavádí jako přirozené rozšíření existujícího.

**Návaznosti mezi sekcemi:**
- Sekce v rámci lekce na sebe navazují sdílenou datovou strukturou nebo společným příkladem.
- Přechod mezi sekcemi bývá věta, která shrnuje, co čtenář právě získal, a naznačuje, co bude dál potřebovat.

**Budování mentálních modelů:**
- Složité struktury se vizualizují metaforou před kódem.
- Poté metaforu opustíme a ukážeme přesný mechanismus kódem.

**Struktura vysvětlení:**
`motivující situace → intuice / analogie → kód → popis výstupu → rozšíření → varování / poznámka`

**Vysvětlování „proč", ne jen „co":**
- Vždy vysvětlit důvod pravidla nebo chování.
- Komparativní perspektiva (Python vs. JavaScript) se používá ke zdůraznění specifického chování.

**Opakování a připomínání kontextu:**
- Pokud se pojem vrací po delší odmlce, text ho stručně připomene větou, nikoliv celým výkladem.

---

## 3. Pravidla vysvětlování

**Jak zavádět nové pojmy:**
1. Uveď problém nebo potřebu, která pojem volá.
2. Ukaž hypotetický příklad, který by bez pojmu nefungoval.
3. Pojmenuj pojem a uveď ho do vztahu s tím, co čtenář zná.
4. Ukaž minimální funkční kód s pojmem.
5. Rozšiř kód o variantu nebo hraniční případ.

**Kdy a jak používat příklady:**
- Příklady přicházejí **ihned po uvedení konceptu**, nikdy ve větším odstupu.
- Každý příklad má jasný, uzavřený účel — čtenář vidí celý kód, nikoliv fragmenty.
- Příklady jsou ze **skutečného života** (bankovní pohyby, jízdní řády, školní výsledky, sportovní závody, kosmonautika), ne abstraktní (`foo`, `bar`, `x`, `y`).

**Škálování komplexity:**
- Vždy od nejjednoduššího případu (bez podmínek, bez cyklů, bez výjimek).
- Každá nová vrstva složitosti je explicitně pojmenována: „Upravme naši funkci tak, aby...".
- Pokročilejší variace se odkládají do sekcí „Čtení na doma".

---

## 4. Struktura obsahu

### Šablona výukové stránky (lekce)

```
## [Název tématu]                          ← H2, jedno nebo dvě slova

[Úvodní věta propojující s předchozím]    ← 1–2 věty, vždy referuje na dříve naučené

[Motivující příklad situace]               ← 1–4 věty popisující reálný scénář

```py
[minimální ukázkový kód]                  ← 5–15 řádků
```

[Vysvětlení kódu nebo výstupu]            ← 1–3 věty

### [Podtéma / variace]                    ← H3

[Rozšíření nebo nová varianta pojmu]      ← 1–3 věty + kód

[**POZOR!** nebo **Poznámka:**]            ← pouze pokud existuje záludnost

### Užitečné [funkce/metody] na [typ]      ← H3, přehledový seznam

`název()`
: Jednořádkový popis

```py
[příklad volání]
```
```

**Délka:** hlavní výklad 300–600 slov, kódové ukázky 5–20 řádků každá, přehledový seznam funkcí 3–8 položek.

---

## 5. Jazyk a tonalita

**Oslovení:** druhá osoba singuláru (tykání): "Všimni si", "Vyzkoušej si", "stáhni soubor".

**Uvozovky:** používej pouze základní uvozovky `"` - nikdy české „ a ".

**Emotikony:** nepoužívej emotikony ani emoji v žádném textu.

**Pomlčka:** používej pouze `-`, nikdy `—` (em dash).

**Formálnost:** nízká až střední. Hovorové výrazy jsou přípustné.

**Délka vět:** střední (15–30 slov). Technické věty kratší.

**Typické formulace:**
- Zavádění konceptu: „Představme si, že...", „Uvažujme například...", „Začněme s..."
- Odkaz dozadu: „Jako jsme již viděli...", „Zatím jsme poznali...", „Vzpomeňme si..."
- Rozšíření: „Upravme naši funkci tak, aby...", „Podobně můžeme také..."
- Varování: `**POZOR!**`, `**Poznámka:**`
- Pobídka k akci: „Vyzkoušejte si ji.", „Zkusme si...", „Pojďme se podívat..."

**Technické termíny:** první výskyt v *kurzívě*, poté normální text.

---

## 6. Pravidla pro práci s kódem

- Kód je vždy v ohraničeném bloku (` ```py `, ` ```shell `, ` ```json `).
- Každý kód musí být spustitelný a kompletní — žádné `...` ani `# zbytek kódu`.
- Výstup kódu se ukazuje jako komentář nebo jako blok `shell` pod kódem.
- Kód se nepřepisuje do textu — text přidává to, co kód neřekne sám: proč, co je překvapivé.
- Klíčová slova a názvy funkcí se vždy uvádějí v `backtick` inline formátu.
- Inline komentáře v kódu jsou v češtině, stručné.

---

## 7. Pedagogický přístup

**Typy příkladů (v pořadí preferencí):**
1. Scénář ze školního prostředí (vysvědčení, přijímačky, maturita)
2. Scénář z každodenního života (bankovní pohyby, nakupování, cestování)
3. Historické nebo vědecké reálie (Apollo, atleti)
4. Abstraktní, ale pojmenované hodnoty (seznam jmen, seznam knih) — nikdy holé `x`, `y`, `a`, `b`

---

## 8. Omezení a zakázané vzory

- **Nezačínat** sekcí s definicí — vždy nejprve motivace nebo příklad.
- **Nepoužívat** abstraktní proměnné bez sémantického obsahu (`a`, `b`, `x`, `foo`, `bar`).
- **Nepsat** kód, který není kompletní a spustitelný.
- **Nevysvětlovat** pojem bez kódu déle než 3 věty za sebou.
- **Nepoužívat** akademický nebo dokumentační jazyk.
- **Neopakovat** celý výklad dříve vysvětleného pojmu — stačí odkaz nebo stručná věta.
- **Nevkládat** více než jeden úplně nový pojem do jednoho odstavce.
- **Neskrývat** výstupy kódu.

---

## 9. Struktura souborů a YML konfigurace

Kurz je organizován jako hierarchie tří úrovní: **kurz → kapitola → lekce → sekce**. Každá úroveň má svůj adresář a soubor `entry.yml`, který definuje obsah a pořadí.

### Hierarchie adresářů

```
entry.yml                          ← kořenový soubor kurzu
python-pro-data-1/
  entry.yml                        ← soubor kapitoly
  nacteni-dat/
    entry.yml                      ← soubor lekce
    formaty-souboru.md             ← sekce (markdown soubor)
    nacteni-dat.md
    vyber-sloupcu.md
    excs.md                        ← sekce se cvičeními
    excs/
      titanic/
        exercise.md                ← zadání cvičení
        assets/                    ← soubory ke cvičení
      jablka.md                    ← alternativní formát cvičení (jen soubor, bez adresáře)
    assets/                        ← obrázky a datové soubory lekce
```

### Formát `entry.yml` pro kurz (kořenový soubor)

```yml
title: Python pro data 1
lead: Úvod do zpracování dat v Pythonu
image: assets/python.svg
chapters:
  - python-pro-data-1
  - ziskavani-dat
  - bonusy
```

- `title`: název kurzu zobrazený na webu
- `lead`: krátký popis kurzu
- `image`: cesta k ikoně kurzu (relativně od kořene)
- `chapters`: seznam názvů adresářů s kapitolami (v požadovaném pořadí)

### Formát `entry.yml` pro kapitolu

```yml
title: Python pro data 1
lead: Základní kapitoly
lessons:
  - instalace
  - nacteni-dat
  - podmineny-vyber
  - spojovani-a-agregace
```

- `title`: název kapitoly
- `lead`: krátký popis kapitoly
- `lessons`: seznam názvů adresářů s lekcemi (v požadovaném pořadí)

### Formát `entry.yml` pro lekci

```yml
title: Načtení dat
lead: Pojďme načíst data do pandas a podívat se na ně
sections:
  - formaty-souboru
  - nacteni-dat
  - vyber-sloupcu
  - excs
```

- `title`: název lekce
- `lead`: krátký popis lekce
- `sections`: seznam názvů sekcí (bez přípony `.md`, v požadovaném pořadí)

**POZOR!** Každá položka v `sections` odpovídá souboru `<název>.md` ve stejném adresáři jako `entry.yml`. Soubory, které nejsou uvedeny v `sections`, se na webu nezobrazí.

### Formát markdown souborů sekcí

Každý `.md` soubor sekce začíná nadpisem H2 (odpovídá jedné záložce nebo části stránky na webu):

```md
## Název sekce

Text lekce...
```

Soubor **neobsahuje** YAML frontmatter (žádné `---` na začátku).

### Cvičení

Cvičení se vkládají do textu sekce pomocí direktivy `::exc`:

```md
::exc[excs/nazev-cviceni]
```

Cesta je relativní od adresáře lekce. Cvičení mohou být ve dvou formátech:

**Varianta A - adresář** (když cvičení potřebuje vlastní assets):
```
excs/
  nazev-cviceni/
    exercise.md    ← zadání cvičení
    assets/        ← soubory potřebné ke cvičení
```

**Varianta B - samostatný soubor** (když cvičení nepotřebuje assets):
```
excs/
  nazev-cviceni.md   ← zadání cvičení
```

### Formát `exercise.md` (zadání cvičení)

```md
---
title: Název cvičení
demand: 3
---

Text zadání cvičení...

:::solution
```py
# řešení
```
:::
```

- `title`: název cvičení zobrazený na webu
- `demand`: obtížnost 1-5 (1 = nejlehčí, 5 = nejtěžší)
- Blok `:::solution ... :::` obsahuje vzorové řešení (skryté, zobrazí se po kliknutí)

**Poznámka:** Soubory bez adresáře (varianta B) mají frontmatter přímo v `.md` souboru, nikoliv v `exercise.md`.

### Přidání nové lekce - postup

1. Vytvoř adresář s názvem lekce (slug, malá písmena, pomlčky): `nova-lekce/`
2. Vytvoř `nova-lekce/entry.yml` se seznamem sekcí
3. Vytvoř markdown soubory pro každou sekci
4. Přidej `nova-lekce` do `sections` v `entry.yml` nadřazené kapitoly

### Přidání nové sekce do existující lekce - postup

1. Vytvoř soubor `nova-sekce.md` v adresáři lekce
2. Přidej `nova-sekce` na správné místo do `sections` v `entry.yml` lekce

### Přidání cvičení - postup

1. Rozhodně, zda cvičení potřebuje vlastní assets (obrázky, CSV soubory):
   - Pokud ano: vytvoř adresář `excs/nazev/` s `exercise.md` a `assets/`
   - Pokud ne: vytvoř soubor `excs/nazev.md`
2. Do příslušné sekce `.md` vlož direktivu `::exc[excs/nazev]`

---

## 10. Časové odhady lekce

Každá lekce musí obsahovat tabulku odhadovaných časů na začátku souboru (hned za nadpisem H1). Tabulka má jeden řádek pro každou sekci H2.

Časový odhad se skládá z:
- **Výklad** - čas potřebný k vysvětlení tématu
- **Otázky** - čas vyhrazený na dotazy účastníků
- **Praktická ukázka** - pouze u sekcí s praktickou částí

Tabulka se aktualizuje **pouze na výslovnou žádost uživatele**, ne automaticky při každé změně obsahu.

Breakdown (výklad / otázky / praktická ukázka) slouží pouze jako interní podklad pro odhad - v tabulce se nezobrazuje. Tabulka ukazuje pouze celkový čas na sekci.

Formát tabulky:
```
| Sekce | Čas |
|---|---|
| Název sekce | X min |
| **Celkem** | **X min** |
```
