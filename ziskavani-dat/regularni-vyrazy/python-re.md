## Regulární výrazy v Pythonu

V Pythonu máme řadu funkcí, které můžeme použít pro práci s regulárními výrazy. Projdeme si ty základní. Funkce jsou v modulu `re`, který je součástí Pythonu a můžeš ho importovat pomocí příkazu `import re` na začátku programu.

### Ověření formátu

Často potřebujeme ověřit, jestli máme zadaná data ve správném formátu. Např. telefonní čísla, rodná čísla, ISBN u knih, poštovní směrovací čísla, e-maily nebo čísla bankovního účtu mají jasně definovaný formát.

Zkusme si nejprve zadat rodné číslo. Víme, že rodné číslo se skládá ze 6 číslic, které kódují datum narození, a tří nebo čtyř číslic, které identifikují konkrétního člověka. Regulární výraz, který by číslo ověřil, je `\d{9,10}`.

Regulární výraz můžeme vytvořit pomocí funkce `compile()` z modulu `re`. Před řetězec s regulárními výrazy píšeme `r`, abychom Pythonu dali vědět, co je daný řetězec zač.

```py
import re

regularni_vyraz = re.compile(r"\d{9,10}")

rezetec = "9511121234"
print(regularni_vyraz.match(rezetec))
rezetec = "ahoj"
print(regularni_vyraz.match(rezetec))
```

Pokud funkce `match` došla k závěru, že se řetězec shoduje s regulárním výrazem, vrátí objekt  `Match`. S ním později budeme pracovat. Pokud by však funkce došla k závěru, že se řetězce s regulárním výrazem neshoduje, vrátí hodnotu označovanou jako `None`, tj. prázdnou hodnotu.

**Otázka:** Často je rodné číslo zapisováno ve formátu s podtržítkem, které odděluje datum narození od zbytku. Jak upravíme regulární výraz, aby akceptoval oba formáty, tj. formát s podtržítkem i bez podtržítka?

### Přísnější ověření formátu

Pokud chceš ověřit, jestli řetězec odpovídá zadanému výrazu a není tam nic navíc, můžeš použít funkci `fullmatch`, která funguje stejně jako funkce `match()`.

```py
import re

regularni_vyraz = re.compile(r"\d{9,10}")

rezetec = "9511121234"
print(regularni_vyraz.match(rezetec))
rezetec = "9511121234$ je moje rodné číslo"
print(regularni_vyraz.fullmatch(rezetec))
```

### Zapojení podmínky

Pojďme nyní zapojit do akce podmínku. Můžeme třeba uživateli vypsat, jestli jím zadaná hodnota je správná. Výsledek volání funkce `match()` můžeme vložit přímo do podmínky, protože podmínka, které nevložíme operátor na porovnávání (např. `==`) funguje takto:

* Pokud podmínce vložíme nějaký smysluplný výraz, vyhodnotí ho jako **pravda**.
* Pokud podmínce vložíme prázdnou hodnotu `None`, vyhodnotí ji jako **nepravda**.

```py
import re

regularni_vyraz = re.compile(r"\d{9,10}")
vstup = input("Zadej rodné číslo: ")
hledani = regularni_vyraz.fullmatch(vstup)
if hledani:
    print("Rodné číslo je v pořádku!")
else:
    print("Nesprávné rodné číslo!")
```

### E-maily

Pokud např. dostaneme e-mail `info@czechitas.cz`, víme, že je v pořádku. E-mail `info@czechitascz` by ale v pořádku nebyl, protože "koncovka" `"cz"` (v řeči počítačů doména prvního řádu) musí být oddělena tečkou.

```py
import re

regularni_vyraz = re.compile(r"\w+@\w+\.cz")
email = input("Zadej e-mail: ")
hledani = regularni_vyraz.fullmatch(email)
if hledani:
    print("E-mail je v pořádku!")
else:
    print("Nesprávný e-mail!")
```

### Vyhledávání

Kromě ověřování správného formátu můžeme použít regulární výrazy i k vyhledávání. Například funkce `findall` vrátí ze zadaného řetězce všechny podřetězce, které odpovídají danému regulárnímu výrazu, jako seznam.

Následující program například z deníku lékaře vyhledá rodná čísla všech pacientů, které lékař zmínil.

```py
import re

zapis = """
Zápisy o provedených vyšetřeních:
Pacient 6407156800 trpěl bolestí zad a byl poslán na vyšetření. 
Pacientka 8655057477 přišla na kontrolu po zranění kotníku.
Do ordinace telefonovala pacientka 7752126712, které byl elektronicky vydán recept na Paralen. 
"""

regularni_vyraz = re.compile(r"\d{9,10}")
vysledky = regularni_vyraz.findall(zapis)
for vysledek in vysledky:
    print(vysledek)
```

<!-- TODO: rozpracovaný text
Nyní máme zpracovat program, připraví informaci o pokutě pro majitele vozidla, který projel měřeným úsekem příliš rychle. V šabloně máme

```py
zapis = """
Vážený majiteli vozidla,
náš rychlostní radar dne ${offenseDate} v ${offenseTime} hodin, který je umístěný na silnici ${road} ve směru jízdy ${direction}, 
změřil, že  motorové vozidlo registrační značky ${lp} překročilo maximální povolenou rychlost v daném místě a pohybovalo se rychlostí ${speed}. ¨
Za tento přestupek Vás vyzýváme k zaplacení pokuty ${fine} Kč. 
Vyřizuje ${firstname} ${lastname}
"""
```
-->

### Nahrazování

Uvažujme, že máme nějakém textu provést anonymizaci, tj. vymazat všechny osobní údaje. K tomu můžeme využít funkci `sub()`, která nahradí všechny podřetězce, které odpovídají regulárnímu výrazu, námi zadanou hodnotou.

```py
import re

zapis = """
Zápisy o provedených vyšetřeních:
Pacient 6407156800 trpěl bolestí zad a byl poslán na vyšetření. 
Pacientka 8655057477 přišla na kontrolu po zranění kotníku.
Do ordinace telefonovala pacientka 7752126712, které byl elektronicky vydán recept na Paralen. 
"""

regularni_vyraz = re.compile(r"\d{9,10}")
anonymniZapis = regularni_vyraz.sub("X" * 9, zapis)
print(anonymniZapis)
```

## Cvičení: Regulární výrazy v Pythonu
::exc[excs/uzivatelske-jmeno]
::exc[excs/email-s-teckou]
::exc[excs/zaznamy]
::exc[excs/adresy-stranek]
::exc[excs/ip-adresy]
::exc[excs/prace-s-kodem]
