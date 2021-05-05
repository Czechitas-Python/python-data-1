Pojďme se nyní podívat na dotazy, které nám umožní pracovat s daty předtím, než je zobrazíme uživatelům.

## První dotaz

Uvažujme nyní, že bychom chtěli do aplikace přidat pohled na všechny přihlášky pro konkrétní kurz. Už bychom zvládli vytvořit pohled na všechny přihlášky pro všechny kurzy, to by ale bylo pro uživatele nepřehledné, protože uživatel chce často vidět přihlášky jen pro jeden konkrétní kurz.

Začněme se šablonou. Opět využijeme cyklus, protože chceme vypsat více záznamů. Opět využijeme proměnnou `object_list`, která bude obsahovat všechny záznamy, se kterými chceme pracovat.

```html
{% extends "base.html" %}
{% block content %}
<h3>Seznam přihlášek pro kurz {{ kurz.nazev }}</h3>
<ul>
    {% for prihlaska in object_list %}
    <li>{{ prihlaska.prijmeni }} {{ prihlaska.jmeno }}</li>
    {% endfor %}
</ul>
{% endblock %}
```

Dále vytvoříme pohled. Opět využijeme `ListView` jako mateřskou třídu a nastavíme atributy `model` a `template_name`. S tím ale tentokrát nekončíme, protože potřebujeme upravit další dvě metody.

Jednou z nich je metoda `get_queryset`, která určuje, co bude k dispozici v proměnné `object_list` v naší šabloně. Chtěli bychom využít stejný trik, jako v minulé kapitole, tj. chtěli bychom `id` našeho kurzu umístit do URL adresy. Opět tedy použijeme atribut `kwargs`, abychom vytvořili proměnnou `id_kurzu`. Následně napíšeme první dotaz, který nám vrátí všechny přihlášky, které jsou svázané s konkrétním kurzem. Pro náš dotaz využijeme metodu `filter`. Metodu zavoláme s parametrem `kurz`, kterému nastavíme hodnotu `id_kurzu`. Filtrem tak projdou pouze přihlášky pro konkrétní kurz. Výsledek poté nastavíme jako návratovou hodnotu a ostatní metody se postarají o to, aby byl na správném místě.

Dále bychom chtěli na stráce vidět název kurzu, ke kterému se přihlášky vztahují. K tomu potřebujeme kurz vložit do proměnné, která bude k dispozici v šabloně. Množinu proměnných, které jsou k dispozici v šabloně, označujeme jako kontext (`context`). Kontext je objekt, se kterým ale můžeme pracovat jako se slovníkem. Pokud tedy chceme mít v šabloně novou proměnnou, jednoduše ji přidáme do kontextu.

Pro úpravu kontextu vytvoříme metodu `get_context_data()`. Protože my kontext pouze doplňujeme, chceme mít k dispozici vše, co připraví mateřská třída `ListView`. Protože tedy na začátku zavoláme metodu `get_context_data()` mateřské třídy `ListView`, a to tradičně pomocí funkce `super()`. Výsledek uložíme do proměnné `context` a následně přidáme informace o kurzu, jako bychom vkládali novou hodnotu do slovníku.

```python
class SeznamPrihlasekView(ListView):
    model = models.Prihlaska
    template_name = "prihlaska/prihlasky_list.html"

    def get_queryset(self):
        id_kurzu = self.kwargs['pk']
        query_set = models.Prihlaska.objects.filter(kurz=id_kurzu)
        return query_set

    def get_context_data(self, **kwargs):
        context = super().get_context_data(**kwargs)
        context["kurz"] = models.Kurzy.objects.get(pk=self.kwargs['pk'])
        return context
```

Aby byl pohled dostupný, musíme mu opět přiřadit URL adresu, do seznamu `urlpatterns` tedy přidáme následující hodnotu

```python
path('<int:pk>/prihlasky', views.SeznamPrihlasekView.as_view(), name='seznam_prihlasek'),
```

Nakonec přidáme tlačítko s odkazem na nově vytvořenou stránku.

```python
{% url 'seznam_prihlasek' object.pk as target_url %}
{% bootstrap_button "Seznam přihlášek" href=target_url button_type="link" button_class="btn-info" %}
```