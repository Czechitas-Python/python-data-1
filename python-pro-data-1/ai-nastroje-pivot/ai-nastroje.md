## Generativní AI

Generativní AI je část umělé inteligence zaměřená na vytváření nového obsahu, jako jsou texty, obrázky, zvuky a videa, prostřednictvím učení se z existujících dat. Na rozdíl od tradiční AI, která analyzuje a interpretuje data, generativní AI aktivně generuje nové výstupy, často s originálními a kreativními výsledky.

Jedním z nejpoužívanějších nástrojů postavených na generativní AI je ChatGPT. Vedle něj existuje řada dalších (Claude, Google Gemini, Meta Llama atd.). Nástroje fungují na principu konverzace, tj. uchovává si informace, které byly řečeny v předchozích zprávách (postupně ale informace zapomíná). Ideální je vložení příkladu. Prompty je možné zadávat v různých jazycích. Jazyk promptu nemusí být shodný s jazykem výstupu.

Platí dvě obecná pravidla:

* Generativní AI může poskytovat nepravdivé nebo zavádějící informace (ačkoli se jeho výstupy zdají velmi důvěrychodné).
* Pokud do generativní AI něco vložíme, může to zobrazit někomu jinému nebo to obecně využít k trénování svého modelu (existuje *Temporary chat*, která dotazy do trénovacích dat nevkládá).

Do ChatGPT můžeme vložit reálná data, uměle vytvořená data (např. náhodně vygenerovaná) nebo mu pouze vysvětlíme strukturu našich dat.

V rámci ChatGPT můžeme použít *Custom GPTs*, což jsou předpřipravené instrukce zaměřené na řešení úloh v nějaké konkrétní oblasti. Vlastní GPT si můžeme vytvořit i sami a sdílet ho s ostatními. Do vlastního GPT můžeme nahrát i data.

::fig[Vlastní GPTs]{src=assets/custom_gpts.png}

### Příklady použití

#### Napsání kódu

ChatGPT můžeme požádat o napsání kódu pro řešení nějaké konkrétní úlohy. Můžeme ho požádat i o vysvětlení, zeptat se na alternativní způsob řešení atd.

Pozor na následující:

* kód může obsahovat chyby, může být zbytečně složitý nebo obsahovat věci, které neznáme,
* abychom se posunuli svými znalostmi dál, je potřeba kódu porozumět.

Příklad konverzace, kde žádáme o pomoc s napsáním kódu, je [zde](https://chatgpt.com/share/674cd60b-e210-800d-b259-209fd924831c).

Můžeme požádat i o vylepšení kódu, který jsme vytvořili, protože v něm vidíme určité nedostatky. Příklad konverzace je [zde](https://chatgpt.com/share/674cda14-c3b0-800d-931f-39b3c415b603).

Můžeme požádat i o přepis kódu do jiného jazyka, který známe lépe, např. do SQL. Příklad konverzace je [zde](https://chatgpt.com/share/674dc519-90c4-800d-97e5-88e0dd3bd7c6).

#### Vysvětlení a oprava kódu a doplnění komentářů

Často se stává, že dlouho editujeme nějaký kód a nevidíme v něm chybu, která by byla někomu jinému jasné. AI nástroj může posloužit jako "druhý pár očí", který nám může pomoci odhalit chybu. Příklad konverzace je [zde](https://chatgpt.com/share/674ddf18-ea18-800d-918a-43b3835c8ced).

Pokud máme nějaký kód (např. najdeme ho na internetu, ve firemním projektu atd.), kterému nerozumíme, můžeme požádat ChatGPT o jeho vysvětlení. ChatGPT umí doplnit do kódu komentáře (obvykle dopíše vysvětlení ke každému řádku) nebo vypíše souvislý text, který vysvětluje, co kód dělá. Příklad konverzace je [zde](https://chatgpt.com/share/674de060-d878-800d-bffa-ddba6f06eac9).

Pozor na následující:

* pokud do ChatGPT vložíme nějaký kód, ChatGPT ho vloží do trénovacích dat a může ho zobrazit někomu jinému,
* pokud je kód součástí nějakého rozsáhlého projektu, nemusí ho ChatGPT interpretovat správně (např. může používat knihovny, které nejsou veřejně dostupné).

#### Nápady na analýzu

Příklad konverzace je [zde](https://chatgpt.com/share/674cd895-7824-800d-810e-21dc51300ba5).

### Cvičení

::exc[excs/speedating]

### Bonusy

::exc[excs/speedating-1]
::exc[excs/speedating-2]
