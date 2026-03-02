---
title: Nábytek
demand: 3
---

Využij dataset [s informacemi o prodejích nábytku](https://www.kaggle.com/datasets/tanayatipre/store-sales-forecasting-dataset/data).

Při práci se ti mohou hodit významy jednotlivých sloupců.

- **Row ID** - Sequential identifier for each row.
- **Order ID** - Unique identifier for each sales order.
- **Order Date** - Date of the sales order.
- **Ship Date** - Date of shipment for the order.
- **Ship Mode** - Mode of shipment for the order.
- **Customer ID** - Unique identifier for each customer.
- **Customer Name** - Name of the customer.
- **Segment** - Segment classification of the customer.
- **Country** - Country where the sale occurred.
- **City** - City where the sale occurred.
- **State** - State where the sale occurred.
- **Postal Code** - Postal code where the sale occurred.
- **Region** - Geographical region where the sale occurred.
- **Product ID** - Unique identifier for each product.
- **Category** - Category classification of the product.
- **Sub-Category** - Sub-category classification of the product.
- **Product Name** - Name of the product.
- **Sales** - Total sales amount for the order.
- **Quantity** - Quantity of products sold in the order.
- **Discount** - Discount applied to the order.
- **Profit** - Profit generated from the order.

Nech si zobrazit základní popisné statistiky:

- jak se vyvíjely tržby v čase (v jednotlivých letech a měsících),
- jaká je průměrná doba bezi objednávkou a odesláním zboží (uvažuj pouze pracovní dny),
- kolik procent tvoří firemní zákazníci a kolik procent domácnosti,
- do jakých měst firma nejčastěji nábytek prodává a jaké jsou nejdůležitější kategorie produktů,
- kolik procent zboží bylo prodáno se slevou a jaká je průměrná velikost slevy.

Dále proveď analýzu věrnosti zákazníků. Kolik procent zákazníků provedlo více než jednu objednávku. Vytvoř histogram počtu objednávek pro jednotlivé zákazníky. Nakupují vracející se zákazníci spíše zboží ve slevě? Je nějaká kategorie, která se prodává ve slevě výrazně častěji než ostatní? Jaká je průměrná sleva pro jednotlivé kategorie. A jaký je průměrný zisk?

Požádej AI, aby ti nabídl metody, pomocí kterých je možné predikovat budoucí prodeje. Vyber si jednu z metod a požádej o bližší vysvětlení. Dále ho nech vygenerovat kód, pomocí kterého predikci provede, a aby výsledek zobrazil pomocí grafu.

Na základě dat vytvoř návrh obchodní strategie pro prodejce. Doporuč kategorie a podkategorie produktů, na které by se firma měla zaměřit. Vybírej kategorie jak podle zisku, tak podle tržeb.