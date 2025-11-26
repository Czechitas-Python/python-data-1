## Čtení na doma: Využití dalších nástrojů pro vizualizace

Tableau a PowerBI představují přední nástroje pro business intelligence a interaktivní vizualizaci dat. Oba fungují na principu drag-and-drop, umožňují rychlé vytváření dashboardů bez nutnosti programování a nabízejí sdílení výstupů prostřednictvím cloudových platforem. [Tableau](https://www.tableau.com/) vyniká pokročilými vizualizačními možnostmi, zatímco [PowerBI](https://www.microsoft.com/en-us/power-platform/products/power-bi) těží z integrace do ekosystému Microsoftu a dostupné desktopové verze zdarma. Mezi další podobné nástroje patří Looker (součást Google Cloud), [Qlik Sense](https://www.qlik.com/us), [Metabase](https://www.metabase.com/) (open-source alternativa) nebo [Apache Superset](https://superset.apache.org/).

### Kdy zvolit PowerBI nebo Tableau

- Tyto nástroje jsou ideální pro pravidelný reporting a dashboardy. Umožňují snadno vytvářet měsíční, týdenní či denní přehledy pro management nebo obchodní týmy.
- Koncoví uživatelé mohou interaktivně prozkoumávat data bez znalosti programování. K dispozici mají filtrování, drill-down analýzu a ad-hoc dotazování.
- Vizualizace lze jednoduše sdílet v rámci organizace. Dashboardy se publikují na interní portály s možností řízení přístupových práv.
- BI nástroje umožňují napojení na živé datové zdroje. Dashboardy se tak automaticky aktualizují při změně zdrojových dat.
- Díky okamžité zpětné vazbě jsou vhodné pro rychlé prototypování vizualizací. Uživatel může snadno zkoušet různé typy grafů a rozložení.
- Nástroje podporují spolupráci netechnických týmů. Obchodníci, marketéři nebo finanční analytici mohou samostatně vytvářet a upravovat reporty bez pomoci IT oddělení.

### Kdy zvolit seaborn nebo matplotlib

- Tyto knihovny jsou vhodné pro vytvoření přesně definované vizualizace určené pro vědecký článek, blog nebo publikaci. Nabízejí plnou kontrolu nad každým vizuálním prvkem včetně fontů, rozměrů a exportu do vektorových formátů (SVG, PDF) vyžadovaných akademickými časopisy.
- Analýzu je možné sdílet prostřednictvím Jupyter notebooku. Platformy jako [Kaggle](https://www.kaggle.com/) nebo [GitHub](https://github.com/) umožňují prezentovat kód i vizualizace pohromadě, což podporuje reprodukovatelnost a transparentnost analytického procesu.
- Vizualizace lze integrovat do automatizovaných pipeline. Grafy se generují jako součást ETL procesů, CI/CD nebo naplánovaných úloh bez nutnosti grafického rozhraní.
- Knihovny umožňují vytvářet nestandardní nebo vysoce specializované grafy. Jde o vizualizace, které PowerBI ani Tableau nativně nepodporují, například specifické statistické grafy, vlastní anotace nebo kombinované osy.
- Pro kód, který vytváří vizualizace, je možné použít verzování v Gitu. To umožňuje procházet změnami a provádět review stejně jako u jakéhokoli jiného software.
- Knihovny představují řešení pro omezený rozpočet nebo prostředí bez přístupu k licencovanému software. Jako open-source nástroje nevyžadují žádné licenční poplatky.
