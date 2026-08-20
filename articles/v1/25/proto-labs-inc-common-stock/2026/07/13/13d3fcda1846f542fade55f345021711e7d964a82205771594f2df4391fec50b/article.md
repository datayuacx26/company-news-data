---
schema_version: "1.0.0"
document_id: "13d3fcda1846f542fade55f345021711e7d964a82205771594f2df4391fec50b"
company_key: "proto-labs-inc-common-stock"
company: "Proto Labs Inc."
source_id: "proto-labs-inc-common-stock-news-import-0f8bb12cc68e"
canonical_url: "https://www.protolabs.com/de-de/ressourcen/blog/uberlegungen-zur-ausrichtung-von-3d-gedruckten-teilen/"
published_at: null
first_seen_at: "2026-07-23T22:35:08.710119+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:84cbc255ae17e7bce3785b118d4cd579247bcc5d2c86806e1ede588a002d66a6"
---

# Ausrichtung von 3D-gedruckten Teilen: Auswirkungen auf Festigkeit, Genauigkeit und Oberflächenqualität

Es mag auf den ersten Blick einleuchtend erscheinen, sich für die Ausrichtung zu entscheiden, die die geringste Z-Höhe und damit die kürzeste Bauzeit bietet. Allerdings ist es nicht immer sinnvoll, sich nur auf die Z-Höhe zu konzentrieren, wenn es um die Anforderungen an Ihre Teile geht. Es gibt noch einige andere technische Faktoren, die bei der Ausrichtung von Teilen für den 3D-Druck eine Rolle spielen. Wenn Sie über die Ausrichtung von Teilen für den 3D-Druck - insbesondere für die Stereolithografie (SLA) - nachdenken, denken Sie an einen Bleistift. Was wäre nötig, um diesen Bleistift Schicht für Schicht horizontal oder vertikal zu drucken? Der horizontale Druck würde weniger Zeit in Anspruch nehmen, da er flacher ist, wenn er auf der Seite liegt, während der vertikale Druck wesentlich länger dauern würde, da er aufrecht steht und mehr gezeichnete Schichten benötigt, die nacheinander übereinander gestapelt werden. Außerdem würde der horizontale Druck mehr Stützstrukturen erfordern als der vertikale Druck, und das Entfernen dieser Strukturen würde zusätzliche Zeit erfordern, um ein fertiges Teil zu erhalten. Die Wahl der Orientierung kann daher von den Anforderungen an die Festigkeit oder die Oberflächenbeschaffenheit des Bleistifts abhängen, und es kann sogar gewünscht sein, das Teil in einem Winkel auszurichten. Bei der Wahl der Ausrichtung für die[Stereolithographie](https://www.protolabs.com/de-de/services/3d-druck/stereolithographie/) gibt es eine Reihe von Überlegungen. Jetzt geht's ans Eingemachte!


### Ausrichtung von 3D-gedruckten Teilen für Festigkeit


Schauen wir uns zunächst eine kurze Illustration an, die die allgemeinen Auswirkungen des Schichtaufbaus beim 3D-Druck und der externen Kräfte auf die Teile erklärt. Wenn die Zugkräfte normal oder senkrecht zu den Schichten verlaufen, sind die Teile eher schwach. Verlaufen die Zugkräfte parallel oder in einer Linie zu den Schichten, ist das Teil stärker. Wenn die Kräfte senkrecht zu den Schichten verlaufen, ist das Teil im Allgemeinen in der Lage, einem höheren Druck standzuhalten, als wenn die Kräfte in einer Linie mit den Schichten verlaufen. Daraus ergeben sich zwei Schlussfolgerungen:


1. Drucken Sie so, dass die Schichten mit der Achse ausgerichtet sind, in der die Zugkräfte am größten sind.
2. Drucken Sie so, dass die Schichten die Achse schneiden, in der die Druckkräfte am größten sind.


Im theoretischen Fall des Bleistifts ist es wahrscheinlich sinnvoll, die Konstruktion horizontal auszurichten, um die Zug- und Biegekräfte zu berücksichtigen, denen der Bleistift während des Gebrauchs ausgesetzt ist. Es ist zu beachten, dass diese Richtlinien immer für Fused Deposition Modelin (FDM)-Teile gelten. SLA-Teile weisen eine wesentlich geringere Anisotropie auf als FDM-Teile, da die Schichten chemisch verbunden werden und die mit SLA erreichbare Auflösung wesentlich höher ist als mit FDM. Bei duroplastischen Werkstoffen kann es jedoch erforderlich sein, die Ausrichtung der Teile zu berücksichtigen, um die Festigkeit kritischer Teilemerkmale zu maximieren.


---


Bei einem 3D-gedruckten Teil, das wie oben gezeigt ausgerichtet wurde, ist eine unterschiedliche Schwindung zu beobachten. Der schnelle Übergang von einem kleinen zu einem großen Querschnitt hat bei diesem Teil zu einer Schrumpfung geführt.


### Ausrichtung zur Vermeidung unterschiedlicher Schwindung


Im Allgemeinen gilt: Je ungleichmäßiger das Material in einem Teil verteilt ist, desto anfälliger ist es für ein Phänomen, das als differentielle Schwindung bezeichnet wird. Bestimmte Teilegeometrien und -größen sind von Natur aus anfälliger für Schwindung, da sie nicht ausreichend von den vorhergehenden Materialschichten gestützt werden. Zur Veranschaulichung kann man sich den Großbuchstaben "H" vorstellen. Wenn der Querschnitt in der Mitte geschichtet ist, neigt er dazu, die stützenden Schenkel nach innen zu ziehen, was zu einer Wölbung des Teils im unteren Bereich führt. Es gibt mehrere Möglichkeiten, die unterschiedliche Schwindung zu verhindern. Häufig werden SLA-Teile in einem Winkel gebaut, um die Oberfläche und damit die Gesamtbelastung für jede Schicht zu verringern. Dies trägt auch dazu bei, die Maßhaltigkeit und Toleranzgenauigkeit der Teile zu verbessern. Außerdem bauen wir die Teile liegend auf, so dass jede gebildete Schicht die gleiche Oberfläche wie die darunter liegende Schicht hat und es keine großen Querschnittsschwankungen gibt. Wenn maßkritische oder hochauflösende Teilemerkmale erforderlich sind, empfehlen wir, die Teile so auszurichten, dass sich diese Merkmale auf der nach oben weisenden Oberfläche des Teils befinden. Es ist zu beachten, dass die schräge Herstellung von Teilen zu einer nicht idealen Oberflächenbeschaffenheit führen kann, wenn es um kosmetische Aspekte geht.


Eine unterschiedliche Schrumpfung kann vermieden werden, indem das Teil so ausgerichtet wird, dass kleine bis große Querschnittsveränderungen vermieden werden (siehe oben), oder indem das Teil in einem Winkel ausgerichtet wird (siehe rechts).


Wenn glatte, ästhetische Merkmale das Ziel sind, sollten die Teile senkrecht gebaut werden. Eine schräge Bauweise führt zu sichtbaren Schichtlinien, wie der abgewinkelte Zylinder zeigt.


---


### Ausrichten für die gewünschte Oberflächenqualität


Wenn es wichtig ist, dass Ihr Teil ein kosmetisches Finish für den Einsatz auf einer Messe oder Ausstellung hat, werden wir das Teil eher flach als schräg bauen. Alle kritischen Merkmale sollten nach oben zeigen, da die Oberseiten glatt und anfälliger für Maßhaltigkeit sind. Wenn das Teil mit der Oberseite nach unten gebaut wird, würden die Stützstrukturen, die entfernt und gestrahlt werden müssen, jeden Glanzeffekt verhindern.


Der Glanzeffekt ist auf dem liegenden "H" zu sehen. Das schräg gebaute "H" zeigt die Schichtlinien und die visuellen Effekte, die sie auf Text haben können.


Um Ihnen das qualitativ beste Teil zu liefern, kann unser Team in der Regel eine andere Ausrichtung wählen. Ein wichtiger Punkt, den Sie beachten sollten, ist, dass wir, wenn Sie keine gewünschte Ausrichtung des Teils (horizontal oder vertikal) angeben, die Ausrichtung wählen, die wir für den besten Kompromiss zwischen schnellstmöglicher Bauzeit und guter Optik halten. Wenn Sie uns mitteilen, welche Aspekte für Ihre 3D-gedruckten Teile am wichtigsten sind, können wir bestimmen, wie wir Ihr Teil während des Bauprozesses am besten ausrichten. Wenn Sie besondere Anforderungen an ein 3D-gedrucktes Teil haben, seien es kritische Eigenschaften, Kostenüberlegungen oder eine bestimmte gewünschte Oberflächenbeschaffenheit, sollten Sie unser Ingenieurteam im Voraus darüber informieren, damit wir sicherstellen können, dass Ihr Teil Ihre spezifischen Ziele erfüllt.
