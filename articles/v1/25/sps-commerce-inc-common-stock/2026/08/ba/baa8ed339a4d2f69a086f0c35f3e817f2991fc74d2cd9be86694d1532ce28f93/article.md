---
schema_version: "1.0.0"
document_id: "baa8ed339a4d2f69a086f0c35f3e817f2991fc74d2cd9be86694d1532ce28f93"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/edi-business-central-der-einstieg-ohne-umwege"
published_at: "2026-08-04T00:00:00+00:00"
first_seen_at: "2026-08-04T21:23:14.881903+00:00"
fetched_at: "2026-08-04T22:08:55.728008+00:00"
content_hash: "sha256:28feda06f86dcd773a67318f995b9c33e55cdd553ab4c8700530d8a4bd195dc0"
---

# EDI Business Central: Der Einstieg ohne Umwege

# EDI Business Central: Der Einstieg ohne Umwege


By[Aleksandra Vovchenko](https://www.spscommerce.com/community/profile/aleksandra-vovchenko) , *Senior Marketing Coordinator*


Last Updated August 4, 2026


6 min read


# EDI Business Central: Der Einstieg ohne Umwege


Wer als Lieferant, Hersteller oder Distributor mit Microsoft Dynamics 365 Business Central arbeitet, stößt früher oder später auf dieselbe Frage: Wie kommt EDI eigentlich in mein System? Business Central ist ein ERP-System für kleine und mittlere Unternehmen und deckt Finanzen, Lager, Einkauf und Verkauf ab. Elektronischer Datenaustausch mit Handelspartnern gehört serienmäßig nicht dazu. Genau hier setzt dieser Beitrag an: Was EDI in Business Central bedeutet, wie E-Invoicing über EDI funktioniert und wie eine Anbindung an Handelspartner ohne wachsenden internen Wartungsaufwand gelingt.


## Was ist EDI in Business Central?


Kurz gesagt: EDI ist keine eingebaute Funktion von Business Central, sondern eine Ergänzung, die von außen angebunden wird.


EDI (Electronic Data Interchange) bezeichnet den automatisierten Austausch von Geschäftsdokumenten wie Bestellungen, Lieferavisen und Rechnungen direkt zwischen den Systemen zweier Handelspartner, ohne manuelle Eingabe. Business Central selbst bringt dafür keine native Übersetzungsfunktion mit. Damit ein eingehender Auftrag eines Einzelhändlers oder eine ausgehende Rechnung an einen Lieferanten korrekt verarbeitet werden kann, braucht es eine zusätzliche Komponente, die zwischen den unterschiedlichen Formaten der Handelspartner (zum Beispiel EDIFACT) und der internen Struktur von Business Central übersetzt.


Diese Komponente kann auf zwei Wegen bereitgestellt werden:


- **Als App oder Add-on innerhalb von Business Central** , meist über den Microsoft AppSource Marketplace, das grundlegende Mapping-Funktionen mitbringt.


- **Als webbasierte, cloudgehostete Lösung eines spezialisierten Anbieters** , die außerhalb von Business Central läuft und Dokumente aufbereitet an das ERP übergibt.


In beiden Fällen gilt: Ohne zusätzliche Software oder einen Dienstleister, der diese Übersetzung übernimmt, lässt sich EDI mit Business Central nicht betreiben. Die zentrale Entscheidung ist also nicht *ob* , sondern *wie* diese Anbindung erfolgt, insbesondere mit Blick darauf, wer die Handelspartnerregeln pflegt, wenn sich Formate oder Anforderungen ändern.


### EDI Cloudlösung Business Central: Add-on oder Managed Service?


Bei einer App aus dem Marketplace liegt die laufende Pflege der Mapping-Regeln meist beim eigenen Team. Bei einer EDI Cloudlösung für Business Central, die von einem spezialisierten Anbieter betrieben wird, übernimmt dieser Anbieter Einrichtung, Aktualisierung und Support der Anbindung, während Business Central selbst nur die bereits aufbereiteten Daten empfängt.


## E-Invoicing EDI: Elektronische Rechnungsstellung mit Business Central


Ein Bereich, in dem diese Frage besonders sichtbar wird, ist die elektronische Rechnungsstellung. Seit dem 1. Januar 2025 müssen alle inländischen B2B-Unternehmen in Deutschland E-Rechnungen empfangen können; die Pflicht zur Ausstellung folgt gestaffelt ab 2027. Für Unternehmen, die bereits EDI-Rechnungen austauschen, ändert sich dadurch nicht der Kanal, wohl aber die Anforderung an das Format und an die korrekte Extraktion der steuerlich relevanten Daten.


In der Praxis zeigt sich ein wiederkehrendes Muster: Eingehende EDI-Belege treffen in unterschiedlichen Formaten ein, je nachdem, welches System der jeweilige Handelspartner nutzt. Damit Business Central diese Belege verarbeiten kann, müssen sie zunächst in ein einheitliches XML-Format konvertiert werden. Viele Unternehmen lösen das über zusätzliche Konvertierungssoftware, die zwischen den Handelspartner-Formaten und dem ERP vermittelt. Bei einem Volumen von mehreren Hunderttausend Belegen im Jahr und mehreren Dutzend Lieferantenanbindungen bedeutet das: jede neue Anbindung, jede Formatänderung und jede regulatorische Anpassung (etwa durch die deutsche E-Rechnungspflicht) muss in dieser Konvertierungsschicht separat gepflegt werden.


Das ist technisch lösbar, bindet aber kontinuierlich interne Kapazität, insbesondere wenn die Zahl der Handelspartner wächst oder sich Formatanforderungen ändern. Die Frage, die sich viele IT-Verantwortliche irgendwann stellen, ist deshalb weniger „Können wir das selbst bauen und pflegen?", sondern „Wie viel unserer Zeit soll dauerhaft in die Pflege von Formatregeln fließen, statt in Aufgaben, die unser Geschäft direkt voranbringen?"


## EDI Dienstleister Business Central: Wie SPS Commerce die Anbindung an Handelspartner unterstützt


Genau an diesem Punkt setzt SPS Commerce an. Mit Fulfillment EDI für Microsoft Dynamics 365 Business Central übernimmt SPS die Übersetzung, Anreicherung und Standardisierung von Auftrags-, Versand- und Rechnungsdaten zwischen Ihren Handelspartnern und Ihrem Business-Central-System. Statt für jede Handelspartnerverbindung eigene Formatregeln aufzubauen und zu warten, nutzen Sie eine einzige Verbindung, die bereits auf die Anforderungen von tausenden Einzelhändlern, Lieferanten, Distributoren und 3PLs abgestimmt ist, einschließlich komplexer Auftragsabwicklungsmodelle wie Ship-to-DC, Ship-to-Store oder Drop-Shipping.


Als Microsoft-zertifizierter Partner seit 2004 hat SPS mehr als 5.000 Systemintegrationen in Microsoft-ERP-Umgebungen begleitet. Ein dediziertes Team von über 300 Microsoft-Spezialisten übernimmt dabei nicht nur die Einrichtung, sondern auch den laufenden Betrieb: von der Konfiguration der Geschäftsregeln über die Datenstandardisierung bis zum 24/7-Support, wenn sich Anforderungen ändern. Für den Bereich E-Invoicing kommt hinzu, dass SPS Commerce die gesetzlichen Anforderungen in über 50 Ländern im Blick behält, einschließlich der aktuellen Entwicklungen zur E-Rechnungspflicht in Deutschland, sodass neue Formatvorgaben nicht bei jedem Handelspartner einzeln nachgepflegt werden müssen.


Für Unternehmen, die Business Central als führendes System nutzen, bedeutet das: Die Anzahl der Lieferanten- und Retaileranbindungen kann wachsen, ohne dass die interne Mapping- und Wartungsarbeit im gleichen Maß mitwächst.


## Häufig gestellte Fragen zu EDI und Business Central


**Was bedeutet EDI im Zusammenhang mit der Rechnungsstellung?** Im Rechnungskontext bezeichnet EDI den Austausch von Rechnungsdaten in einem strukturierten, standardisierten Format direkt zwischen den Systemen zweier Handelspartner, anstelle einer papierbasierten oder als PDF versendeten Rechnung. Die Rechnungsdaten werden dabei automatisch aus dem ERP-System des Rechnungsstellers erzeugt, in das vereinbarte Format übersetzt und direkt in das System des Empfängers eingespielt, etwa in Business Central. Das reduziert manuelle Erfassung und ist zugleich die Grundlage dafür, dass EDI-Rechnungen auch die aktuellen gesetzlichen Anforderungen an elektronische Rechnungen erfüllen können, sofern die notwendigen Pflichtangaben korrekt abgebildet werden.


**Was ist der Unterschied zwischen EDI und einer API?**
EDI und APIs dienen beide dem Datenaustausch zwischen Systemen, unterscheiden sich aber in Aufbau und Einsatzzweck. EDI basiert auf feststehenden, oft branchenweit genormten Nachrichtenformaten (etwa EDIFACT) und eignet sich besonders für wiederkehrende, standardisierte Dokumente wie Bestellungen oder Rechnungen im Handelspartnernetzwerk. APIs ermöglichen einen flexibleren, meist direkteren Datenaustausch, erfordern aber in der Regel eine individuelle Anbindung je Partner. In der Praxis werden beide Ansätze häufig kombiniert.


**Brauche ich für EDI in Business Central zusätzliche Software?**
Ja. Business Central bringt keine eingebaute EDI-Funktion mit. Für den Austausch mit Handelspartnern ist entweder eine App aus dem Microsoft-Marketplace oder eine webbasierte Lösung eines spezialisierten Anbieters wie SPS Commerce notwendig, die die Formatübersetzung und die laufende Pflege der Handelspartnerregeln übernimmt.


## Der nächste Schritt


EDI in Business Central einzuführen, bedeutet nicht, ein komplett neues System zu lernen. Es bedeutet, die Übersetzungsebene zwischen Ihren Handelspartnern und Ihrem ERP so aufzusetzen, dass sie mit wachsender Anbindungszahl nicht zum internen Dauerprojekt wird. Wenn Sie herausfinden möchten, wie eine EDI- und E-Invoicing-Anbindung für Ihr Business-Central-System konkret aussehen könnte, sprechen Sie mit dem[Microsoft-Team von SPS Commerce](https://www.spscommerce.com/de/produkte/fulfillment/integrierte-fulfillment/systemintegration/microsoft-dynamics-business-central/) oder informieren Sie sich über die[E-Invoicing-Lösung von SPS Commerce](https://www.spscommerce.com/de/produkte/e-invoicing/) .
