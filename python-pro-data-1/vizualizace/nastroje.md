## Čtení na doma: Využití dalších nástrojů pro vizualizace

Tableau a PowerBI představují přední nástroje pro business intelligence a interaktivní vizualizaci dat. Oba fungují na principu drag-and-drop, umožňují rychlé vytváření dashboardů bez nutnosti programování a nabízejí sdílení výstupů prostřednictvím cloudových platforem. [Tableau](https://www.tableau.com/) vyniká pokročilými vizualizačními možnostmi, zatímco [PowerBI](https://www.microsoft.com/en-us/power-platform/products/power-bi) těží z integrace do ekosystému Microsoftu a dostupné desktopové verze zdarma. Mezi další podobné nástroje patří Looker (součást Google Cloud), [Qlik Sense](https://www.qlik.com/us), [Metabase](https://www.metabase.com/) (open-source alternativa) nebo [Apache Superset](https://superset.apache.org/). Pokud chceme interaktivní dashboard vytvořit přímo v Pythonu, můžeme využít knihovnu [Dash](https://dash.plotly.com/) ve spojení s [Plotly](https://plotly.com/python/), která nabízí interaktivní grafy a umožňuje dashboard spustit jako webovou aplikaci.

### Kdy zvolit seaborn nebo matplotlib

- Potřebujeme plnou kontrolu nad podobou grafu, například pro vědecký článek nebo publikaci.
- Vizualizace jsou součástí automatizovaného skriptu nebo pipeline a nevyžadují interakci uživatele.
- Chceme sdílet analýzu jako Jupyter notebook s viditelným kódem i výsledky (např. na GitHubu).

### Kdy zvolit PowerBI nebo Tableau

- Dashboardy a reporty budou vytvářet uživatelé bez znalosti programování.
- Chceme rychle prototypovat vizualizace pomocí drag-and-drop rozhraní.
- Firma již využívá placené nástroje Microsoftu (Office 365, Azure).

### Kdy zvolit Plotly a Dash

- Chceme interaktivní dashboard (filtry, rozbalovací seznamy, aktualizace grafů), ale zároveň chceme mít vše pod kontrolou v Pythonu.
- Dashboard je součástí většího pythonového projektu nebo pipeline a nechceme zavádět externí nástroj.
- Potřebujeme sdílet výsledky jako webovou aplikaci, ale bez nutnosti licence pro PowerBI nebo Tableau.


::fig[Příklad dashboardu vytvořeného v Pythonu]{src=assets/dashboard.png}


### Cvičení

::exc[excs/dashboard]
