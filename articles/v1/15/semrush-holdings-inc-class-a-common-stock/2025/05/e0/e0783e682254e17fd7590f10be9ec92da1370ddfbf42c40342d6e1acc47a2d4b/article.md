---
schema_version: "1.0.0"
document_id: "e0783e682254e17fd7590f10be9ec92da1370ddfbf42c40342d6e1acc47a2d4b"
company_key: "semrush-holdings-inc-class-a-common-stock"
company: "SEMrush Holdings Inc."
source_id: "semrush-holdings-inc-class-a-common-stock-news-import-32e03f4bdca1"
canonical_url: "https://de.semrush.com/blog/llms-txt/"
published_at: "2025-05-13T13:08:03+00:00"
first_seen_at: "2026-07-22T16:49:43.743641+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:8deb99a81c59ad594460fc320f65700c31ef44806027b635697e9e0254f93c99"
---

# Was ist LLMs.txt und sollte man es verwenden?

Wir haben diesen Artikel aus dem Englischen ins Deutsche übertragen.[Klicke hier](https://www.semrush.com/blog/llms-txt/) , um den Originalartikel zu lesen. Wenn dir am Inhalt Probleme auffallen, schicke uns gerne eine Nachricht anreport-osteam@semrush.com .


## Was ist LLMs.txt?


Die Datei llms.txt ist ein vorgeschlagener Standard, der großen Sprachmodellen (LLMs) helfen soll, Inhalte von Websites besser zu verstehen und zu nutzen.


Die Idee ist ziemlich einfach: Anstatt KI-Crawler auf Ihrer Website umherstreifen zu lassen, geben Sie ihnen eine kuratierte Liste Ihrer wichtigsten Inhalte. Um der KI mitzuteilen, auf welche Inhalte Ihrer Website sie tatsächlich achten soll.


Wir haben bereits Standards wie[robots.txt](https://www.semrush.com/blog/beginners-guide-robots-txt/) und[Sitemaps](https://www.semrush.com/blog/xml-sitemap/) , um Suchmaschinen zu helfen, Webseiten effizienter zu navigieren. Das Besondere an llms.txt ist, dass es speziell für KI-Modelle entwickelt wurde, die Ihre Inhalte nutzen könnten, um Fragen zu beantworten oder Antworten für Benutzer zu generieren.


Es wird auch spekuliert, dass die Implementierung von llms.txt Websites mehr Sichtbarkeit in KI-generierten Antworten verschaffen und potenziell mehr Referral-Traffic generieren könnte.


Bevor wir uns mit llms.txt befassen und entscheiden, ob sich eine Implementierung lohnt, wollen wir verstehen, warum überhaupt die Notwendigkeit besteht, einen weiteren Webstandard zu schaffen.


## Welches Problem versucht LLMs.txt zu lösen?


Die Datei llms.txt wurde entwickelt, um KI-Crawlern ein effektiveres Durchsuchen von Websites zu ermöglichen. Aktuell stehen diese Raupenfahrzeuge vor zwei großen Herausforderungen:


- **Moderne Websites sind schwer lesbar.** Die meisten KI-Crawler können nur den grundlegenden HTML-Code Ihrer Seiten lesen – nicht die Inhalte, die durch JavaScript geladen werden. Das bedeutet, dass llms.txt ein klares, strukturiertes Format bietet, das KI-Crawlern hilft, die Informationen schnell zu verarbeiten.
- **Die meisten Websites enthalten zu viele Informationen.** Wenn KI-Crawler Ihre Website besuchen, wissen sie nicht unbedingt, was wichtig ist. Wenn sie Zeit damit verbringen, Seiten auszulesen, die nicht nützlich sind (wie ältere Blogbeiträge), generieren sie möglicherweise Antworten auf der Grundlage suboptimaler Informationen – llms.txt hilft, dieses Problem zu lösen.


Die Datei llms.txt kann auch die Ineffizienzen beim Training großer Sprachmodelle reduzieren.


Die Ausbildung von LLM-Absolventen ist mit einem enormen Rechenaufwand verbunden. Mit Hilfe der llms.txt-Anleitung verschwenden LLMs weniger Ressourcen für irrelevante Inhalte.


## Wie sind LLMs.txt-Dateien strukturiert?


Dem vorgeschlagenen Standard zufolge sollten llms.txt-Dateien in Markdown strukturiert und formatiert sein.


Markdown ist eine leichtgewichtige Auszeichnungssprache, die eine einfache Textformatierungssyntax verwendet, um strukturierte Dokumente zu erstellen. (Es handelt sich um dasselbe Format, das Entwickler in GitHub-README-Dateien verwenden, und es ist von KI-Systemen leicht auswertbar.)


Einige gängige Markdown-Elemente, die Sie in Ihrer llms.txt-Datei verwenden werden, sind:


- # für die H1-Überschrift, ## für H2-Überschriften, ### für H3-Überschriften usw.
- > für Blockzitate, um wichtige Beschreibungen hervorzuheben
- - oder * für Aufzählungspunkte in ungeordneten Listen
- \[text\](url) für Hyperlinks zu Ihren Inhalten
- : zum Hinzufügen von Beschreibungen neben Links, um zu erklären, wohin diese führen
- \`\`\` für Codeblöcke beim Teilen technischer Beispiele


Die offizielle llms.txt-Spezifikation enthält ein sehr einfaches Beispiel dafür, wie Ihre Datei aussehen könnte. Wenn Ihre Website jedoch groß oder komplex ist, sollten Sie sie stärker strukturieren – beispielsweise durch die Verwendung von H3- und H4-Überschriften zur Erstellung von Unterabschnitten, durch das Einfügen von Tabellen zur Datenorganisation oder durch das Hinzufügen von Code-Snippets zur Veranschaulichung der API-Nutzung.


Daran ist nichts auszusetzen. Markdown-Dateien sind für KI-Crawler vollständig lesbar, solange Sie eine gültige Syntax verwenden, sind Sie also auf der sicheren Seite. Die zusätzliche Struktur könnte KI-Crawlern tatsächlich mehr Kontext bieten.


Hier ein einfaches Beispiel gemäß der grundlegenden Spezifikation:


` # Firmenname
>Kurze Beschreibung der Tätigkeit Ihres Unternehmens


## Produkte
- \[Produkt 1\](https://example.com/product-1): Beschreibung dieses Produkts
- \[Produkt 2\](https://example.com/product-2): Beschreibung dieses Produkts


## Dokumentation
- \[Erste Schritte\](https://example.com/docs/getting-started): Einführung in unsere Plattform
- \[API-Referenz\](https://example.com/api): Vollständige API-Dokumentation`


## Nutzen Marken den LLMs.txt-Standard?


Ja, einige SaaS- und entwicklerorientierte Unternehmen verwenden bereits llms.txt-Dateien auf ihren Websites.


Die allgemeine Akzeptanz ist jedoch eher nischig. Laut NerdyData hatten bis Juli 2025 nur[951 Domains](https://www.nerdydata.com/reports/llms.txt/b012b7e8-c50d-45e3-8719-0d72f097c3db) (ein winziger Bruchteil des Webs) eine llms.txt-Datei veröffentlicht.


Hier sind einige Beispielunternehmen:


**Marke**


**Worauf sich die Datei konzentriert**


**Die Gesamtstruktur**


[Umarmendes Gesicht](https://huggingface-projects-docs-llms-txt.hf.space/diffusers/llms.txt)


Entwicklerdokumentation


Verwendet mehrere Überschriftenebenen (#, ##, ###, ####), um den Inhalt in separate Abschnitte zu unterteilen. Es enthält außerdem vollständige Codebeispiele, zahlreiche Links und hilfreiche Hinweise. Insgesamt wirkt es wie eine umfassende Wissensdatenbank.


[Vercel](https://ai-sdk.dev/llms.txt)


Entwicklerdokumentation


Beginnt mit beschreibenden Zeilen am Anfang, wie z. B. title:, description: und tags:, um einen Eindruck von der jeweiligen Dokumentation zu vermitteln, die folgt. Und es verwendet eindeutige Überschriften (#, ##, ###), um den Inhalt in logische Abschnitte zu gliedern. Unter jedem Abschnitt finden Sie Schritt-für-Schritt-Anleitungen und praktische Codebeispiele.


[Zapier](https://docs.zapier.com/llms.txt)


Entwicklerdokumentation


Verwendet nur wenige Überschriften und schafft eine sehr einfache Struktur. Sie besteht größtenteils aus einer langen Liste von Links und daneben stehenden Beschreibungen, die erläutern, wohin diese führen.


[Cal.com](https://cal.com/docs/llms.txt)


Entwicklerdokumentation


Verwendet Überschriften am Anfang (#, ##) und springt dann direkt in eine sehr lange Liste von Links. Die Links sind nicht in Abschnitte gruppiert, und es gibt keine Unterüberschriften, Zusammenfassungen oder Beschreibungen.


Beachten Sie, wie unterschiedlich die einzelnen Unternehmen mit ihrer llms.txt-Datei umgehen. Sie verwenden jeweils eine andere Struktur.


Daran ist nichts auszusetzen. Solange gültiges Markdown verwendet wird, ist die Datei maschinenlesbar und kann problemlos von KI-Systemen verarbeitet werden.


Außerdem verfügt keines dieser Unternehmen über eine Datei, die sich auf die gesamte Website konzentriert. Das war eine persönliche Entscheidung. Sie können eine Datei erstellen, die sich auf Ihre gesamte Website oder nur auf einen bestimmten Abschnitt konzentriert.


## Sollten Sie LLMs.txt auf Ihrer Website verwenden?


Die Verwendung von llms.txt lohnt sich im Moment wahrscheinlich nicht, es sei denn, Sie sind einfach neugierig und möchten experimentieren.


llms.txt ist derzeit nur ein vorgeschlagener Standard und wird nicht bereits von den großen KI-Unternehmen verwendet.


Keines der LLM-Unternehmen wie OpenAI, Google oder Anthropic hat offiziell bestätigt, dass sie diese Dateien beim Crawlen von Websites berücksichtigen.


Googles John Mueller bestätigte dies ebenfalls auf Bluesky:


Allerdings gibt es einige interessante Signale.


Anthropic hat beispielsweise eine llms.txt-Datei auf ihrer eigenen Website veröffentlicht. Das heißt nicht, dass ihr KI-Crawler diese Dateien tatsächlich verwendet – aber es lässt vermuten, dass sie der Idee zumindest aufgeschlossen gegenüberstehen.


Wir befinden uns noch in der frühen Spekulationsphase, in der die Leute die Datei implementieren und hoffen, dass sie eines Tages nützlich sein könnte.


Wird Semrush diese Datei implementieren?


Wir haben llms.txt auf einer unserer Schwesterseiten, Search Engine Land, implementiert, um zu sehen, ob es irgendwelche nennenswerten Vorteile in Bezug auf KI-Sichtbarkeit und Traffic bietet. Wenn du neugierig bist, kannst du dir die Datei[hier](https://searchengineland.com/llms.txt) ansehen.


Wir werden die Ergebnisse in den kommenden Monaten beobachten und diesen Artikel mit unseren Erkenntnissen aktualisieren.


Falls Sie auch auf Ihrer eigenen Website mit llms.txt experimentieren möchten, finden Sie unten eine Schritt-für-Schritt-Anleitung zur Implementierung.


## So erstellen Sie eine LLMs.txt-Datei (Schritt für Schritt)


Da es sich um einen technischen Vorgang handelt, empfiehlt es sich, einen Entwickler in den Prozess einzubeziehen, während Sie diese drei Schritte befolgen:


### 1. Entscheiden Sie, welche Inhalte Sie präsentieren möchten.


Bevor Sie eine Datei erstellen, legen Sie fest, welche Seiten oder Abschnitte Ihrer Website für KI-Crawler hervorgehoben werden sollen.


Angenommen, Sie möchten eine llms.txt-Datei für Ihre gesamte Website erstellen. Berücksichtigen Sie zumindest Folgendes:


- Produkt- oder Dienstleistungsseiten
- Aktuelle Blogbeiträge
- Preisseite
- Über uns-Seite
- Kontaktseite


Dies sind typischerweise die Seiten, die der KI einen guten Überblick darüber geben, was Ihr Unternehmen macht und wie Sie Ihren Kunden helfen.


### 2. Datei erstellen


Öffnen Sie einen Texteditor wie Notepad oder Visual Studio Code und erstellen Sie eine neue Datei mit dem Namen llms.txt.


Sie müssen die Datei mit Markdown formatieren. Auch hier sind die Entwickler hilfreich bei der Erstellung der Datei.


So könnte die Dateistruktur aussehen:


` # Website-Name
>Kurze Beschreibung Ihrer Website


Wichtige Hinweise:
- Alleinstellungsmerkmal oder wichtiges Detail zu Ihrem Unternehmen
- Ein weiterer wichtiger Hinweis zu Ihren Leistungen
- Dritter wichtiger Punkt, der Ihr Angebot definiert


## Produkte
- \[Produktname 1\](https://example.com/product-1): Kurze Beschreibung der Hauptmerkmale und Vorteile Ihres Produkts
- \[Produktname 2\](https://example.com/product-2): Kurze Beschreibung der Hauptmerkmale und Vorteile Ihres Produkts
- \[Produktname 3\](https://example.com/product-3): Kurze Beschreibung der Hauptmerkmale und Vorteile Ihres Produkts


## Blog-Inhalte
- \[Blogbeitragstitel 1\](https://example.com/blog-post-1): Kurze Beschreibung des Inhalts und Nutzens dieses Blogbeitrags
- \[Blogbeitragstitel 2\](https://example.com/blog-post-2): Kurze Beschreibung des Inhalts und Nutzens dieses Blogbeitrags
- \[Blogbeitragstitel 3\](https://example.com/blog-post-3): Kurze Beschreibung des Inhalts und Nutzens dieses Blogbeitrags


## Unternehmen
- \[Über uns\](https://example.com/about): Unternehmenshintergrund, Mission und Teaminformationen
- \[Kontakt\](https://example.com/contact): So erreichen Sie unser Team
- \[Preise\](https://example.com/pricing): Übersicht über Tarife, Funktionen und Kosten für die Nutzung unserer Produkte`


### 3. Laden Sie die Datei auf Ihre Website hoch.


Platzieren Sie Ihre fertige Datei an der richtigen Stelle, damit KI-Crawler sie theoretisch finden können.


Der genaue Speicherort hängt vom Umfang Ihrer llms.txt-Datei ab:


- Wenn die Datei Ihre gesamte Website umfasst, laden Sie sie in Ihr Stammverzeichnis hoch (z. B. „https://\[IhreDomain\].com“), sodass sie unter „https://\[IhreDomain\].com\]/llms.txt“ erreichbar ist.
- Wenn es sich bei der Datei speziell um Dokumentation handelt, legen Sie sie im entsprechenden Unterverzeichnis ab (z. B. „https://\[docs.yourdomain.com\]/llms.txt“).


Sie benötigen die Hilfe eines Entwicklers, um die Datei tatsächlich hochzuladen. Diese Datei muss auf Ihrem Server abgelegt werden – üblicherweise über Ihr Webhosting-Kontrollpanel, wie beispielsweise cPanel.


Melden Sie sich bei Ihrem Hosting-Anbieter an und navigieren Sie dann zum cPanel > Dateimanager.


Wechseln Sie dann in das richtige Verzeichnis. Wenn Ihre llms.txt-Datei für die gesamte Website bestimmt ist, gehen Sie zum Ordner public_html/. (Das ist das Stammverzeichnis der meisten Domains.)


Wenn es sich um eine Subdomain wie „https://\[docs.yourdomain.com\]“ handelt, navigieren Sie zu dem Ordner, der dieser Subdomain zugewiesen ist – oft mit dem Namen /docs/ oder ähnlich.


Laden Sie dort Ihre llms.txt-Datei hoch und speichern Sie die Änderungen.


Sobald die Datei hochgeladen ist, überprüfen Sie, ob alles funktioniert, indem Sie einen neuen Tab öffnen und die URL direkt aufrufen.


Sie können auch eine schnelle Überprüfung Ihrer Website mit dem[Site Audit](https://www.semrush.com/siteaudit/) Tool von Semrush durchführen, um zu bestätigen, dass Ihre llms.txt-Datei korrekt erkannt wird.


In diesem Fall wurde eine llms.txt-Datei gefunden, daher ist die Meldung „Nicht gefunden“ inaktiv (ausgegraut).


Vergessen Sie außerdem nicht, die Datei regelmäßig zu aktualisieren. Überprüfen Sie regelmäßig die Links, um veraltete Seiten zu entfernen. Fügen Sie außerdem Links zu den neuen Inhalten hinzu, die Sie der Website hinzufügen.


Teilen


[Tushar Pol](https://de.semrush.com/blog/user/203456689/)


Tushar has been involved in SEO for the past six years, specializing in content strategy and technical SEO. He gained his experience in agencies, where he worked on various ecommerce and B2B clients. On the Semrush blog, he writes about SEO and marketing based on experience drawn from his client work, focusing on sharing practical and effective strategies. His goal is to turn Semrush blog into the ultimate destination for learning SEO and web marketing.
