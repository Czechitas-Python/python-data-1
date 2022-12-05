Regulární výrazy jsou v podstatě malí "kouzelníci", kteří nám umožní vytáhnout z textu důležitá data. Často například dostaneme celou adresu jako jeden řetězec, například jako "Václavské nám. 837/11, 110 00 Nové Město" a my potřebujeme z adresy vytáhnout poštovní směrovací číslo, abychom zhruba věděli, kde se dané místo nachází.

O poštovním směrovacím čísle víme, že je složeno z pěti číslic a většinou (ale ne vždy) ho zapisujeme s mezerou mezi třetím a čtvrtém čísle. Potřebujeme tedy nějak obecně říct, že máme z textu vybrat pětici čísel, která může mít mezi třetím a čtvrtým číslem mezeru. K řešení takových úloh slouží regulární výrazy.

## Jak fungují regulární výrazy

Regulární výrazy umožňují používání symbolů, které mohou zastoupit nějaký znak (nebo více znaků). Fungují podobně jako třeba žolík v karetní hře nebo hvězdička ve vyhledávání souborů, umožňují ale přesněji specifikovat, co vlastně zastupují. Práci s regulárními výrazy si můžeme pohodlně trénovat například v aplikaci [Regular Expressions 101](https://regex101.com/). Do pole "Regular Expression" zadáváme regulární výraz a do pole "Text String" řetězec, se kterým pracujeme. Aplikace nám interaktivně podbarvuje části textu, které odpovídají našemu výrazu. Protože regulární výrazy se v různých programovacích jazycích mírně liší, měli bychom si vlevo v části `Flavor` **přepnout na Python**.

Oněm magickým znakům říkáme *metaznaky*.

### Žolík

Zkusme si to na příkladu tečky `.`. Tečka zastupuje **právě jeden** libovolný znak, přesně tedy odpovídá právě "žolíku". Pokud budeme pracovat s řetězcem `"A23456789JQKA"` a zadáme regulární výraz `"78.J"`, podbarví se nám část řetězce od `7` do `J`.

Vyzkoušejme si nyní upravit program, který bude sledovat vývoj kurzu měn ve Směnárně na Růžku, aby nám například poslal upozornění ve chvíli, kdy má nějaká měna výhodný kurz. Náš program zatím umí stáhnout informace do následující řetězce.

```
Vítejte ve Směnárně na Růžku!
Kurzy měn pro 19. 12. 2020 jsou:

1 €   = 26.35 Kč
1 $   = 21.76 Kč
1 £   = 28.78 Kč
1 DKK = 3.54 Kč

Neúčtujeme žádné poplatky.
```

Podívejme se nejprve na řádek, kde máme kurz Eura. Mohli bychom napsat pouze dolar, ale abychom označili i jedničku před znakem měny, napíšeme `1 €`.

### Skupiny znaků

Pokud chceme, aby náš metaznak zastupoval jeden ze skupiny znaků, vložíme tyto znaky do hranatých závorek `[ ]`. Například pokud chceme v kurzovním lístku vyhledat řádky, které mají v sobě znak dolaru nebo eura, napíšeme `1 [€$]`.

### Kvantifikátory

Další významnou skupinou metaznaků jsou kvantifikátory. Kvantifikátorů máme několik, začneme se složenými závorkami `{ }`. Ty nám říkají, kolikrát se znak před kvantifikátorem může opakovat. Pokud vložíme do závorek jedno číslo `{n}`, znamená to opakování právě *n*-krát. Pokud dvě čísla `{m,n}`, znamená to opakování minimálně *m*-krát a maximálně *n*-krát a pokud `{n,}`, znamená to opakování minimálně *n*-krát a maximální počet opakování není omezený. Platí, že regulární výrazy jsou **žravé**, tedy zaberou vždy maximální možný počet znaků.

Pokud například chceme označit celou část našeho řádku s kurzem měn před symbolem `=`, napíšeme `1 [€$] {3}`. Mezera před složenými závorkami je důležitá, protože právě ona se má opakovat.

Někdy se ale náš kurzovní lístek může "nafouknout", proto můžeme využít i výraz `1 [€$] {3,}`.

```
Vítejte ve Směnárně na Růžku!
Kurzy měn pro 19. 12. 2020 jsou:

1 €     = 26.35 Kč
1 $     = 21.76 Kč
1 £     = 28.78 Kč
1 DKK   =  3.54 Kč
100 INR = 29.43 Kč

Neúčtujeme žádné poplatky.
```

Morseova abeceda sloužila dřív k předávání zpráv. Každé písmeno mělo svoji reprezentaci pomocí krátkých a dlouhých signálů (např. telegrafem, rádiem nebo světlem baterky). Podívejme se na následující zprávu, zda v ní není skryto volání o pomoc. O pomoc voláme pomocí mezinárodní zkratky SOS, S kódujeme pomocí tří teček a O pomocí tří čárek.

```
.--- .- -.-. .... -.-. .. -.. --- -- ..- -.-.-- ... --- ... -.-.-- -. ..- -.. .. -- ... . -.-.--
```

K ověření, zda odesílatel volá o pomoc, napíšeme `\.{3} -{3} \.{3}`. Důležitá jsou **zpětná lomítka** před tečkami. Zpětná lomítka totiž říkají, že nechceme použít tečku jako takovou (která, jak už víme, ve světe regulárních výrazů zastupuje libovolný symbol), ale skutečnou tečku. Jak bychom našli písmeno J, které kódujeme jako tečku a tři čárky?

**Pozn.** Poznámku o zpětném lomítku si zapamatuj, protože to je často používaná technika v celém IT světě, nikoli pouze v Pythonu.

Kromě složených závorek existují i tři speciální kvantifikátory.

* `?` znamená výskyt minimálně 0-krát, maximálně 1-krát.
* `*` znamená výskyt minimálně 0-krát, maximální počet není omezen.
* `+` znamená výskyt minimálně 1-krát, maximální počet není omezen.

Pro náš případ s výběrem řádků můžeme použít např. `1 [€$] +`.

### Skupiny znaků

Ve většině případů potřebujeme pracovat nejen s konkrétním znakem nebo konkrétními znaky, ale skupinami znaků. Abychom si práci usnadnili a nemuseli používat příliš často hranaté závorky, existují předem definované skupiny znaků. Skupiny jsou celkem tři a ke každé z nich existuje i její "opak". Např. skupina `\d` označuje čísla a skupina `\D` označuje vše kromě čísel, tj. písmena, mezery, speciální znaky jako `€`, `$` atd. Přehled skupin najdeš zde:

* `\d` zahrnuje číslice 0-9.
* `\D` zahrnuje jakýkoliv znak kromě číslic 0-9
* `\w` zahrnuje alfanumerické znaky, tj. všechna čísla, malá i velká písmena a podtržítko.
* `\W` zahrnuje jakýkoliv znak kromě alfanumerických znaků.
* `\s` zahrnuje "bílé" znaky (mezera, tabulátor, znaky pro zalomení řádků).
* `\S` zahrnuje jakýkoliv znak kromě "bílých" znaků.

Níže máme různorodou skupinu textových dat, se kterými se často setkáváme. Vyzkoušejme si na nich, jak co vše zachytí skupiny znaků.

```
hadi_notace_ma_podtrzitka
9A55423
9A5 5423
+420 735 123 456
Václavské nám. 837/11, 110 00 Nové Město
19. prosince 2020
frantisek.novak@ocelove-mesto.cz
80-902734-1-6
```

### Několik příkladů

Zkusme nyní propojit naše znalosti dohromady.

Víme, že v britské angličtině používáme pro barvu výraz "colour" a v americké "color". Chceme-li spočítat, kolikrát je v textu zmíněna barva, použijeme `colou?r`.

Podobně můžeme využít regulární výraz k "license" "licence", napíšeme `licen[cs]e`.

Otazníky nám pomůžou vypořádat se s nepovinnou mezerou, kterou píšeme například u data. U nás často používáme zápisy data 19. 12. 2020 nebo 19.12.2020. Pojďme sestavit regulární výraz:

* Na začátku je číslo dne. Uvažujme, že tam může být jedno nebo dvě čísla. Použijeme množinu `\d` a kvantifikátor `{1,2}`.
* Následuje tečka, kterou musíme "zkrášlit" zpětným lomítkem.
* Následuje nepovinná mezera. Zde využijeme kvantifikátor `?`.
* Pak už opakujeme tu samou myšlenku, abychom sestavili celý výraz: `\d{1,2}\. ?\d{1,2}\. ?\d{4}`.

Často chceme označit několik slov do jednoho bloku. Skupina `\w` nezahrnuje mezery, musíme ji tedy rozšířit na `[\w ]*`. Například adresu `Václavské náměstí 11, 110 00 Nové Město` tak rozdělíme na dva samostatné bloky.

Nyní už umíme sestavit výraz, kterým vybereme celý řádek s kurzem dolaru nebo eura: `1 [€$] += +\d+.\d+ Kč`.

Číslo bankovního účtu má v Česku tvar:

* předčíslí (nepovinné, 0 až 6 číslic), zakončené je pomlčkou,
* číslo účtu (max. 10 číslic),
* lomítko,
* kód banky (právě 4 číslice).

Pokud bychom neuvažovali předčíslí, stačí nám regulární výraz `\d{6,10}/\d{4}`, který by měl pasovat např. na číslo účtu 2300117015/2010. Nesmíme zapomenout na zpětné lomítko před lomítkem.

Uvažujme, že máme program, do kterého nějaký programátor vložil proměnnou `magicka_konstanta`. Víme, že proměnná je desetinné číslo, ale potřebujeme vědět, kde je zadána její hodnota. Napiš regulární výraz, který najde řádek, kde programátor zadává hodnotu proměnné `magicka_konstanta`.

```
polomer = input("Zadej poloměr koule: ")
polomer = int(polomer)
magicka_konstanta = 3.1415
objem = 4/3 * magicka_konstanta * polomer ** 3
povrch = 4 * magicka_konstanta * r ** 2
```

Zkus si program zkopírovat do Visual Studia a vyzkoušej si vyhledávání přepnout na regulární výrazy. Najde regulární výraz `magicka_konstanta = \d+\.\d*` správný řádek?

### Rozmezí

Kromě výpisu znaků a předdefinovaných skupin můžeme ještě vybrat znaky pomocí rozmezí. K tomu použijeme pomlčku, kterou vepíšeme do hranatých závorek. Například čísla od 1 do 5 napíšeme jako `[1-5]`, malá písmena od a do e jako `[a-e]` a všechna velká písmena jako `[A-Z]`.

Pokud například víme, že se na nějaké střední školy vyskytují třídy označené od A do M, regulární výraz pasující na všechna jména tříd je `[1-4][A-M]`.

Pokud potřebujeme zajistit opakování určité sekvence znaků (ne jen jednoho), můžeme sekvenci znaků uzavřít do kulatých závorek `( )` a za pravou závorku umístit kvantifikátor. Pokud máme variant více, můžeme k jejich oddělení použít znak `|`. Například pokud chceme vybrat oba víkendové dny, napíšeme `(sobota|neděle)`.

### Další příklady

Podívejme se nyní na pár příkladů. Níže máme tabulku s kurzy Czechitas.

* Chceme jít na kurz programování v Pythonu nebo v JavaScriptu. Kurz musí být pro začátečníky. Řádky, které nás zajímají, vyhledáme pomocí `Úvod do programování 1 - (JavaScript|Python)`. Co kdyby nám nevadil ani navazující kurz?
* Uvažujme, že nás zajímají pouze kurzy o víkendu. Vyzkoušíme si výraz `(sobota|neděle)`. Můžeme k povoleným dnům přidat ještě úterý?
* Protože se nám o víkendu nechce příliš brzy vstávat, chceme víkendové kurzy, které začínají nejdříve v 8:30. Napíšeme `(sobota|neděle) [89]:30`. Co kdybychom naopak chtěli kurzy, které začínají nejpozději v 8:30
* Napíšeme si regulární výraz, který označí všechna data ve formátu, jaký je v tabulce. Můžeme například použít výraz `\d{1,2}\. (led|úno). 2021`. Do závorky bychom pro rozvrh na celý rok potřebovali přidat zkratky všech měsíců.

```
9. led. 2021 sobota 9:30 - 16:30 Úvod do programování 1 - Java
16. led. 2021 sobota 7:30 - 15:30 Úvod do programování 1 - JavaScript
16. led. 2021 sobota 8:30 - 17:30 Úvod do programování 2 - Python
18. led. 2021 úterý 9:30 - 17:30 Úvod do programování 1 - JavaScript
23. led. 2021 sobota 9:30 - 16:30 Úvod do programování 2 - Java
27. led. 2021 středa 9:30 - 17:30 Úvod do HTML a CSS
7. úno. 2021 neděle 8:30 - 17:30 Úvod do programování 1 - Python ONLINE
14. úno. 2021 neděle 8:30 - 17:30 Úvod do programování 1 - Python ONLINE
20. úno. 2021 sobota 9:30 - 17:30 Testuju Úvod do testování - manuální
```

* Poštovní směrovací číslo označíme výrazem `\d{3} ?\d{2}`.
* Regulární výraz, který rozpozná všechny paragrafy, které mají maximálně tři čísla, je `§\d{0,3}`.

**Tip:** Pokud chceme získat z webové stránky data jako texty, bez jakéhokoli formátování, můžeme použít jednu z on-line služeb, například [html2txt](https://www.w3.org/services/html2txt).


## Cvičení: Regulární výrazy
::exc[excs>cislo-uctu-1]
::exc[excs>cislo-uctu-2]
::exc[excs>registracni-znacka]
::exc[excs>telefonni-cislo]
::exc[excs>ministerstva]
::exc[excs>napravy]
::exc[excs>slavny-soude]
::exc[excs>ave-caesar]
