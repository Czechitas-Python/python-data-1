## Ověření instalace

Instalaci si můžeš **ověřit** tím, že zkusíš moduly `pandas` a `matplotlib` importovat. Otevři Python terminál (**Terminal → New Terminal** a pak příkaz `python` ve Windows a `python3` v Linuxu nebo MacOS), napiš `import pandas` a stiskni Enter. V ideálním případě Python nevypíše nic. To znamená, že modul importoval a můžeš ho začít používat.

Pokud Python reaguje chybou `ModuleNotFoundError` (viz níže), pak se instalace nepodařila.

```
>>> import pandas
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ModuleNotFoundError: No module named 'pandas'
```

Následně pomocí `import matplotlib` vyzkoušej, zda se úspěšně nainstaloval i modul `matplotlib`.
