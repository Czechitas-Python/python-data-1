Mnoho webových stránek na internetu obsahuje velmi zajímavá a užitečná data. Takových dat může být velký objem a mohou být rozházená po různých stránkách pod mnoha odkazy a není v našich silách je ručně ze stránek vykopírovat. Příkladem budiž například historická data o naměřených teplotách ze stránek [Českého hydrometeorologického ústavu](http://portal.chmi.cz/historicka-data/pocasi/uzemni-teploty). Tato data jsou dostupná pouze skrze tabulky vložené přímo do webových stránek. Pokud chceme takovéto množství dat ze stránek dostat do našeho programu, musíme použít takzvaný _web scraping_.

## HTML

Web scraping je technika pomocí které můžeme strojově číst obsah webových stránek na internetu. Webové stránky často vypadají velmi komplikovaně a sofistikovaně, ale nakonec jsou to pouhopouhé textové soubory psané ve speciálním jazyce zvaném HTML (HyperText Markup Language). Naštěstí pro nás není HTML jazyk programovací, nýbrž takzvaně _značkovací_. Není tedy zdaleka tak složitý jako například jazyk Python. Jazyk HTML má relativně jednoduchou strukturu a ani pro úplného začátečníka není těžké se v něm zorientovat. Pomocí HTML tvůrci webů definují samotný obsah stránek, tedy texty, obrázky, odkazy apod. Samotný vzhled stránky (barvičky, styl písma, rozmístění prvků na stránce apod.) se vytváří v jazyce zvaném CSS, který ale v tuto chvíli můžeme nechat být, neboť z hlediska zpracování dat nás vzhled stránek nezajímá.

### HTML značky (tagy)

V následující ukázce vidíte HTML kód celé webové stránky tak, jak by si ji stáhl prohlížeč odněkud ze serveru.

```html
<html>
<head>
  <meta charset="UTF-8">
  <title>Ukázka</title>
</head>
<body>
  <h1>Nadpis první úrovně</h1>
  <p>
    Text nějakého odstavce, který obsahuje
    <em>zvýrazněný text</em> a také <strong>
    důležitý text.</strong>
  </p>

  <h2>Nadpis druhé úrovně</h2>
  <div class="sekce1">
    <p>
      Druhý odstavec je v takzvaném divu, což je
      značka, která nemá sama o sobě žádný význam.
      Také zde máme
      <a href=&quothttp;://www.czechitas.cz"> odkaz na
      stránky Czechitas</a>.
    </p>

    <ol type="a">
      <li>První položka seznamu</li>
      <li>Druhá položka seznamu</li>
      <li>Třetí položka seznamu</li>
    </ol>
  </div>
</body>
</html>
```

Stránka se poté v prohlížeči zobrazí nějak takto. Zatím nevypadá příliš vábně, protože není nastylovaná. Styly nás však v této lekci nezajímají, protože pro webscraping nejsou důležité.

::fig[Ukázka HTML]{src=assets/ukazka-html.png size=80}

Vytvořte si ve Visual Studiu soubor `ukazka.html`, zkopírujte do něj výše uvedený kód a soubor uložte. Poté tento soubor najděte v průzkumníku a dvojklikem by se vám měl otevřít ve vašem oblíbeném prohlížeči. Můžete tak zkontrolovat, že prohlížeč vaši stránku skutečně zobrazí tak, jak je uvedeno na obrázku výše.

V naší první webové stránce jsme viděli takzvané :term{cs="HTML značky" en="HTML tags"}. Značky se píší do špičatých závorek a většina značek má otevírací a zavírací část. Například značka `em` pro zvýraznění textu vypadá takto

::fig[HTML značka]{src=assets/html-znacka.png size=60}

Značky mohou mít takzvané atributy, které dále specifikují, co značka bude přesně zobrazovat. Například značka `ol` představuje seznam položek a má atribut zvaný `type`, který určuje, jestli se číslování položek děje pomocí písmen nebo čísel.

::fig[HTML atribut]{src=assets/html-atribut.png size=60}

Zajímavá a téměř nejpoužívanější je značka `div`, která sama o sobě nemá žádný vizuální význam. Slouží totiž k členění stránky na menší části. Všimněte si, že naší ukázkové stránka značku `div` také používá. Navíc u ní najdeme atribut `class`. Ten se běžně používá k stylování stránky a často podle něj můžeme při webscrapingu odlišit důležité části stránky.

Všech HTML značek je kolem stovky a mnoho z nich má spoustu možných atributů. Rozumět všem těmto značkám je prací webových vývojářů. Nám bude stačit získat nějaké malé povědomí alespoň o pár základních.

Pokud by vás zajímalo, co vše je v HTML možné, přehled všech používaných značek [najdete zde (anglicky)](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

## Cvičení
::exc[excs/porozumeni-html]
