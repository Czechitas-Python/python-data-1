## Web scraping v Pythonu

Abychom mohli s obsahem webových stránek pracovat přímo v Pythonu, potřebujeme nainstalovat modul, který umí číst HTML značky a pomocí těchto značek v HTML souborech vyhledávat. Takových modulů pro jazyk Python existuje vícero. My použijeme balíček zvaný `requests-html`, který je určený přímo pro web scraping. Nainstalovat jej můžeme příkazem

```shell
pip3 install requests-html
```

Popř. na Windows

```shell
pip install requests-html
```

Abychom mohli v našem programu scrapovat, musíme nejdřív webovou stránku otevřít. Vzhledem k tomu, že obsah textového souboru už do proměnné načíst umíme, stačí tedy jen použít náš nový modul, aby si tento obsah přečetl a umožnil v něm vyhledávat.

Ve Visual Studiu ve složce s naší ukázkovou stránkou si vytvořte program `scrape.py` s tímto obsahem

```py
from requests_html import HTML
with open('sample.html', encoding='utf-8') as soubor:
    obsah = soubor.read()
html = HTML(html=obsah)
```

Proměnná html, nyní obsahuje naši HTML stránku ve formátu, který můžeme použít k vyhledávání.

HTML značky můžeme vyhledávat podle jména. Takto například najdeme všechny odstavce a vypíšeme jejich text každý na nový řádek.

```py
for odstavec in html.find('p'):
    print(odstavec.text)
```

Velmi časté je také vyhledávání podle třídy (atribut `class`). Třídy se vyhledávají tak, že jejich název začneme tečkou.

```py
html.find('.sekce1')
```

Snadno také můžeme přistupovat k atributům nalezených značek. Takto můžeme například najít adresy všech odkazů na naší stránce.

```py
for odkaz in html.find('a'):
    print(odkaz.attrs['href'])
```

### Složitější pravidla vyhledávání

Vyhledávací řetězce v metodě `find()` mohou být složitější, než jak jsme viděli doposud.

Můžeme vyhledávat podle více značek najednou. Například najít všechny nadpisy první i druhé úrovně.

```py
html.find('h1, h2')
```

Můžeme vyhledávat podle atributů. Například najít všechny seznamy, kde atribut `type` je roven `a`.

```py
html.find('ol[type="a"]')
```

Můžeme vyhledávat podle zanoření. Například najít všechny odstavce, které jsou uvnitř značky s třídou `sekce1`.

```py
html.find('.sekce1 p')
```

Mezera ve vyhledávacím řetězci znamená libovolně hluboké zanoření. Pokud bychom chtěli pouze odstavce, které jsou _přímým_ potomkem značky s třídou `sekce1`, použijeme symbol zobáčku.

```py
html.find('.sekce1 > p')
```

Pokud tyto techniky zkombinujeme, můžeme například najít všechny položky ve všech seznamech, jejichž atribut `type` je roven `a`.

```py
html.find('ol[type="a"] li')
```

## Scraping přes internet

Zatím jsme scrapovali pouze stránku, kterou jsme měli uloženou na disku. Pomocí modulu `requests-html` můžeme však také snadno otevřít stránku přímo na internetu. Na adrese <https://apps.kodim.cz/python-data/scrape> najdete naši malou ukázkovou stránku z úvodu. Na adrese <https://apps.kodim.cz/python-data/dhmo> najdete také finální verzi stránky šířící poplach ohledně DHMO.

Načteme v Pythonu první z odkazů a stejně jako prve vypíšeme texty všech odstavců.

```py
from requests_html import HTMLSession
session = HTMLSession()
stranka = session.get('https://apps.kodim.cz/python-data/scrape')
for odstavec in stranka.html.find('p'):
    print(odstavec.text)
```

Dále můžeme postupovat úplně stejně jako když jsme zpracovávali stránky z disku. Pokud chcete vidět celý stažený zdrojový kód stránky jako text, napište

```py
print(stranka.html.html)
```

## Cvičení: Scraping
::exc[excs>scraping-dhmo]
::exc[excs>scraping-kodim.cz]

## Web scraping vs JavaScript

Web scraping je velmi mocná technika. Její úspěšnost však závisí na tom, jakým způsobem jsou webové stránky napsány. Pokud jsou napsány prasácky a nekonzistentně, tak si web scrapingem můžeme snadno způsobit velký bolehlav.

Jeden z velkých problémů pro web scraping však představují stránky, které jsou vytvořené celé v JavaScriptu. Velkým trendem v dnešní době je nepsat HTML kód stránky přímo, jako jsme to viděli výše. Místo toho se použije jazyk JavaScript, který kód stránky sám vygeneruje. Tím může být stránka mnohem flexibilnější a interaktivnější, což je hezké pro uživatele. Pro nás to však znamená, že když stránku stahujeme v Pythonu, neobdržíme značky HTML, ale JavaScriptový program, který nejdříve musíme v Pythonu spustit a nechat si výsledné HTML vygenerovat.

Podívejte se například na [tuto stránku](https://react-shopping-cart-67954.firebaseapp.com/), která je psána přesně tímto způsobem. Pokud chceme takovou stránku scrapovat, musíme použít takovýto kód.

```py
from requests_html import HTMLSession
session = HTMLSession()
stranka = session.get('https://react-shopping-cart-67954.firebaseapp.com/')
stranka.html.render(sleep=5)
```

## Doporučené úložky na doma
::exc[excs>kava-na-mall.cz]

## Čtení na doma

Webscraping je velmi široká oblast a těžko se člověk do jejích tajů dostane během jedné lekce. Obzvláště u komplikovanějších stránek je často nutné zkoušet různé techniky a přístupy, umět si poradit v různých situacích, nenechat se snadno odradit ošklivě napsaným HTML kódem a vůbec být mazaný jako liška.
