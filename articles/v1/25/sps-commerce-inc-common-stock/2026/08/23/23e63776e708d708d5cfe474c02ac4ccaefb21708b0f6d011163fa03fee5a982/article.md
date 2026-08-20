---
schema_version: "1.0.0"
document_id: "23e63776e708d708d5cfe474c02ac4ccaefb21708b0f6d011163fa03fee5a982"
company_key: "sps-commerce-inc-common-stock"
company: "SPS Commerce Inc."
source_id: "sps-commerce-inc-common-stock-news-import-ac2626a08ad7"
canonical_url: "https://www.spscommerce.com/community/articles/sicherheitsbestand-berechnen-formeln-und-methoden-die-zu-ihren-daten-passen"
published_at: null
first_seen_at: "2026-08-13T00:02:35.836751+00:00"
fetched_at: "2026-08-13T00:02:37.360473+00:00"
content_hash: "sha256:d6f93d9904ee328d32f1e99785c315339d96f9753d743088f577c688898cee38"
---

# Sicherheitsbestand berechnen: Formeln und Methoden, die zu Ihren Daten passen

In diesem Artikel erfahren Sie:


• Wovor der Sicherheitsbestand schützt und worin er sich vom Umlaufbestand (Cycle Stock) und vom Meldebestand (Reorder Point) unterscheidet


• Wie die Durchschnitt-Maximum-Methode funktioniert, warum sie der schnellste Einstieg ist und wo sie zu hohe Puffer erzeugt


• Wie die Z-Wert-Formel für Bedarfsschwankungen aufgebaut ist und wie Ihr Servicegrad die Puffergröße bestimmt


---


Die meisten Werte für den Sicherheitsbestand beginnen als fundierte Schätzung. Eine Planerin oder ein Planer legt eine Reichweite von ein paar Wochen fest, rundet der Sicherheit halber auf und macht weiter. Das funktioniert so lange, bis ein Fehlbestand oder ein Audit die Frage aufwirft, woher diese Zahl eigentlich stammt.


Der Sicherheitsbestand ist eine berechenbare Größe, die von drei Faktoren abhängt:


• Wie stark Ihr Bedarf schwankt


• Wie stark Ihre Wiederbeschaffungszeit schwankt


• Welchen Servicegrad Sie sich leisten wollen


Sie brauchen nicht immer die anspruchsvollste Formel, aber Sie brauchen eine, die zu Ihrer Datenlage passt. Dieser Leitfaden geht die Methoden von der einfachsten bis zur komplexesten durch, damit Sie den für Ihr Unternehmen richtigen Ansatz wählen können.


## Worin unterscheiden sich Sicherheitsbestand und Meldebestand?


Der Sicherheitsbestand ist der Bestand, den Sie über den erwarteten Bedarf hinaus vorhalten – und zwar gezielt, um genau die Schwankungen abzufedern, die eine Prognose nicht vorhersagen kann. Er ist strikt vom Umlaufbestand zu trennen, also von dem Bestand, den Sie in einem normalen Wiederbeschaffungszyklus planmäßig verkaufen oder verbrauchen.


Am häufigsten werden Sicherheitsbestand und Meldebestand miteinander verwechselt. Beide hängen zusammen, sind aber nicht dasselbe:


**Meldebestand = Erwarteter Bedarf während der Wiederbeschaffungszeit + Sicherheitsbestand**


Anders gesagt: Der Meldebestand sagt Ihnen, **wann** Sie bestellen müssen. Der Sicherheitsbestand ist das Polster, das verhindert, dass Sie leerlaufen, bevor diese Bestellung eintrifft.


## Wie berechnet man den Sicherheitsbestand am einfachsten?


Welche Berechnung die richtige ist, hängt davon ab, wie planbar Ihr Bedarf und Ihre Wiederbeschaffungszeiten sind. Für viele Unternehmen ist die Durchschnitt-Maximum-Methode ein guter Einstieg, weil sie einfach zu rechnen ist und nur grundlegende historische Daten voraussetzt.


### Die Durchschnitt-Maximum-Methode


Wenn Ihnen nur wenige historische Daten vorliegen, liefert die Durchschnitt-Maximum-Methode schnell einen belastbaren Wert:


**Sicherheitsbestand = (Maximaler Tagesbedarf × Maximale Wiederbeschaffungszeit) − (Durchschnittlicher Tagesbedarf × Durchschnittliche Wiederbeschaffungszeit)**


Angenommen, von einem Teil gehen in Spitzenzeiten 25 Stück pro Tag ab, im Durchschnitt 15 Stück pro Tag, und die Wiederbeschaffungszeit beträgt maximal 14 Tage bei einem Mittelwert von 10 Tagen. Die Rechnung lautet dann (25 × 14) minus (15 × 10), also 350 minus 150, und ergibt einen Sicherheitsbestand von 200 Stück.


Der Haken: Diese Methode unterstellt, dass alles gleichzeitig schiefgeht – der Bedarf erreicht seinen Höchststand, Ihr Lieferant kommt zu spät, und beides trifft im selben Wiederbeschaffungszyklus zusammen. Das kann passieren, ist aber selten. Wer den Sicherheitsbestand auf ein Szenario auslegt, das vielleicht einmal in zehn Jahren eintritt, trägt diesen Puffer das ganze Jahr über mit – und genau dort versickert Working Capital.


### Wie lautet die Formel für den Sicherheitsbestand bei schwankendem Bedarf?


Sobald Sie über verlässliche Bedarfshistorien verfügen, können Sie sich von Worst-Case-Schätzungen lösen. Die folgende Formel bemisst den Sicherheitsbestand danach, wie stark der Bedarf im Zeitverlauf tatsächlich schwankt und welchen Servicegrad Sie erreichen wollen.


**Sicherheitsbestand = Z × σd × √L**


Die Formel wirkt einschüchternd, doch jeder Bestandteil hat eine einfache Aufgabe:


• **Z** ist der Wert, der zu Ihrem angestrebten Servicegrad gehört (siehe Tabelle unten)


• **σd** ist die Standardabweichung des Bedarfs, also ein Maß dafür, wie stark der Tagesbedarf um seinen Mittelwert schwankt


• **L** ist die Wiederbeschaffungszeit, angegeben in derselben Zeiteinheit wie Ihre Bedarfsdaten


Jede Erhöhung des Servicegrads erfordert einen größeren Bestandspuffer. Und dieser Zuwachs verläuft nicht proportional. Der Sprung von 90 % auf 99 % Servicegrad bedeutet nicht ein bisschen mehr Bestand – er kann den Sicherheitsbestand nahezu verdoppeln.


### Was ist ein Servicegrad?


Der Servicegrad ist der Prozentsatz des Kundenbedarfs, den ein Unternehmen innerhalb eines Wiederbeschaffungszyklus ohne Fehlbestand bedienen will. Anders formuliert: Er beschreibt, wie häufig Sie bereit sind, einen Fehlbestand zu riskieren.


Höhere Servicegrade senken das Risiko, dass Ihnen die Ware ausgeht, erfordern aber auch einen höheren Sicherheitsbestand. Der richtige Zielwert ergibt sich aus dem Vergleich der Kosten eines Fehlbestands mit den Kosten der zusätzlichen Lagerhaltung.


Bei einer kritischen Produktionskomponente oder einem Topseller kann ein Servicegrad von 99 % gerechtfertigt sein, bei einem Langsamdreher dagegen nicht.


Die folgende Tabelle zeigt die Z-Werte für die gängigsten Servicegrad-Ziele.


### Servicegrad und zugehöriger Z-Wert


**Servicegrad**


**Z-Wert**


90%


1,28


95%


1,65


97,5%


1,96


99%


2,33


Ein Detail führt häufiger zu Fehlern als fast alles andere: Ihre Einheiten müssen zusammenpassen. Wird die Bedarfsschwankung pro Tag gemessen, muss auch die Wiederbeschaffungszeit in Tagen angegeben werden. Wer Tagesbedarf mit monatlicher Wiederbeschaffungszeit kombiniert, bekommt keine Fehlermeldung – sondern einfach ein falsches Ergebnis.


## Wie berücksichtigen Sie zusätzlich schwankende Wiederbeschaffungszeiten?


Der Bedarf ist nicht die einzige Unsicherheitsquelle. Lieferanten halten zugesagte Termine nicht ein, die Produktion verzögert sich, Wetterlagen bremsen den Transport und Häfen sind überlastet. Wenn auch die Wiederbeschaffungszeiten schwanken, muss Ihre Berechnung des Sicherheitsbestands dieses Risiko abbilden. Dafür gibt es zwei weitere Formeln.


#### Wenn die Wiederbeschaffungszeit schwankt, der Bedarf aber stabil ist


**Sicherheitsbestand = Z × σL × Davg**


Auch hier ist Z der Wert, der zu Ihrem angestrebten Servicegrad gehört. σL ist die Standardabweichung der Wiederbeschaffungszeit und Davg der durchschnittliche Bedarf. Nutzen Sie diese Formel, wenn Ihr Bedarf gut planbar ist, das Lieferfenster Ihres Lieferanten dagegen nicht.


#### Wenn Bedarf und Wiederbeschaffungszeit schwanken


Das ist für die meisten SKUs das realistischste Bild – und zugleich die datenhungrigste Variante:


**Sicherheitsbestand = Z × √(L̄ × σd² + D̄² × σL²)**


Hier werden die Varianzen von Bedarf und Wiederbeschaffungszeit unter einer gemeinsamen Wurzel zusammengeführt, statt zwei separate Sicherheitsbestände zu addieren. So wird das Risiko nicht doppelt gezählt. Die Formel lohnt sich nur mit belastbaren historischen Daten zu beiden Größen, bildet dafür aber am besten ab, wie reale Lieferketten tatsächlich funktionieren.


### Welche Methode sollten Sie wählen?


Beginnen Sie mit der einfachsten Methode, die Ihre Datenlage hergibt. Sobald sich Ihre Prognosen und Ihre Datenhistorie verbessern, können Sie zu anspruchsvolleren Formeln übergehen. Bessere Daten führen in der Regel zu einem besseren Sicherheitsbestand als der bloße Griff zur komplizierteren Gleichung.


**Methode**


**Formel**


**Geeignet für**


**Wesentlicher Nachteil**


Durchschnitt-Maximum


(Max. Bedarf × Max. Wiederbeschaffungszeit) −
(Durchschn. Bedarf × Durchschn. Wiederbeschaffungszeit)


Schnelle Schätzungen, wenig Historie


Unterstellt, dass Worst-Case-Bedarf und Worst-Case-Wiederbeschaffungszeit zusammentreffen; puffert tendenziell zu hoch


Bedarfsschwankung (Z-Wert)


Z × σd × √L


Stabile Wiederbeschaffungszeit, schwankender Bedarf


Blendet das Risiko schwankender
Wiederbeschaffungszeiten vollständig aus


Schwankende Wiederbeschaffungszeit


Z × σL × Davg


Stabiler Bedarf, schwankende
Wiederbeschaffungszeit


Blendet das Bedarfsrisiko vollständig aus


Kombiniert (doppelte Schwankung)


Z × √(L̄ × σd² + D̄² × σL²)


Bedarf und Wiederbeschaffungszeit schwanken beide


Am genauesten, erfordert aber die
umfangreichsten historischen Daten


#### Rechenbeispiel: Eine SKU bei drei Servicegraden


Nehmen Sie eine SKU mit einem durchschnittlichen Tagesbedarf von 50 Stück, einer Standardabweichung des Bedarfs von 12 Stück und einer Wiederbeschaffungszeit von 7 Tagen. Mit der Formel für Bedarfsschwankungen ergibt sich bei drei Servicegraden:


• **90% Servicegrad (Z = 1,28):** 1,28 × 12 × √7 ≈ 41 Stück


• **95% Servicegrad (Z = 1,65):** 1,65 × 12 × √7 ≈ 52 Stück


• **99% Servicegrad (Z = 2,33):** 2,33 × 12 × √7 ≈ 74 Stück


Beachten Sie, was sich verändert hat: Der Bedarf ist exakt gleich geblieben, die Wiederbeschaffungszeit ebenfalls. Verändert hat sich einzig der Servicegrad. Genau deshalb ist die Festlegung des Servicegrads ebenso sehr eine unternehmerische wie eine mathematische Entscheidung.


#### Wie dimensionieren Sie den Sicherheitsbestand je SKU, statt pauschal zu überpuffern?


Alle SKUs gleich zu behandeln ist einer der schnellsten Wege, unnötig Kapital im Bestand zu binden. Bei manchen Produkten lässt sich ein sehr hoher Servicegrad rechtfertigen, weil ein Fehlbestand teuer ist. Bei anderen schlicht nicht.


Genau hier heben viele Unternehmen verstecktes Working Capital. Laut Arda lässt sich durch die Optimierung des Sicherheitsbestands anhand tatsächlicher Bedarfsschwankungen statt anhand von Worst-Case-Annahmen der Überbestand so weit senken, dass[15–25 % des im Pufferbestand gebundenen Working Capital freigesetzt werden.](https://www.arda.cards/post/7-strategies-to-reduce-inventory-without-stockouts)


Ein praxistauglicher Einstieg:


1. **Klassifizieren Sie Ihre SKUs** nach Umsatz- oder Margenbeitrag und danach, wie störend ein Fehlbestand wäre


2. **Ordnen Sie den Klassen Servicegrade** zu und reservieren Sie die höchsten Werte für die SKUs, bei denen ein Fehlbestand wirklich wehtut


3. **Rechnen Sie regelmäßig nach** , denn Bedarfs- und Wiederbeschaffungsmuster verschieben sich, und ein einmal jährlich festgelegter Sicherheitsbestand veraltet


### Häufig gestellte Fragen


##### Sind Sicherheitsbestand und Pufferbestand dasselbe?


Ja. Beide Begriffe bezeichnen zusätzlichen Bestand, der Unsicherheiten bei Bedarf und Wiederbeschaffungszeit abfedert – getrennt vom Umlaufbestand, den Sie in einer normalen Wiederbeschaffungsperiode planmäßig verbrauchen.


##### Welche Formel für den Sicherheitsbestand sollten die meisten Unternehmen verwenden?


Das hängt von der Qualität Ihrer Daten ab. Wenn Sie gerade erst beginnen, reicht die Durchschnitt-Maximum-Methode in der Regel aus. Liegen verlässliche historische Bedarfsdaten vor, liefert die Formel für Bedarfsschwankungen (Z-Wert) meist ein ausgewogeneres Ergebnis. Die kombinierte Formel sollten Sie nur einsetzen, wenn Sie belastbare Daten zu Bedarfs- und Wiederbeschaffungsschwankungen haben.


##### Wie oft sollte ich den Sicherheitsbestand neu berechnen?


Überprüfen Sie ihn immer dann, wenn sich Bedarfsmuster oder Wiederbeschaffungszeiten spürbar verändern – mindestens jedoch quartalsweise. Ein einmal berechneter und nie wieder hinterfragter Wert bildet irgendwann nicht mehr ab, wie sich Ihre Lieferanten und Kunden tatsächlich verhalten.


##### Was tun, wenn ich nicht genügend Daten habe?


Fangen Sie einfach an. Wenn Ihnen nur grundlegende historische Daten vorliegen, schaffen Sie mit der Durchschnitt-Maximum-Methode eine Ausgangsbasis. Sobald Sie mehr Bedarfshistorie gesammelt haben, können Sie auf die statistischen Formeln umsteigen und den Sicherheitsbestand präziser berechnen.


##### Braucht jede SKU denselben Servicegrad?


Nein. Legen Sie Servicegrade je SKU-Klasse danach fest, wie teuer ein Fehlbestand wäre – und nicht als eine einzige Vorgabe für das gesamte Sortiment.


##### Sie möchten bessere Eingangsdaten für Ihre Sicherheitsbestandsrechnung?


Jede Formel für den Sicherheitsbestand steht und fällt mit einem Punkt: verlässlichen Daten. Ist die Bedarfshistorie lückenhaft oder sind die Wiederbeschaffungszeiten der Lieferanten veraltet, liefert auch die beste Gleichung das falsche Ergebnis.


Hersteller, die[Manufacturing Supply Chain von SPS Commerce](https://www.spscommerce.com/products/manufacturing-supply-chain/) nutzen, erhalten standardisierte Echtzeit-Transparenz über die Leistung ihrer Lieferanten und über eingehendes Material – und damit verlässlichere Eingangsdaten für ihre Bestandsentscheidungen.
