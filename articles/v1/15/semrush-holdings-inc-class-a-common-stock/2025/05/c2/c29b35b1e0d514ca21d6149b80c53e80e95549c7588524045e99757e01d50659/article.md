---
schema_version: "1.0.0"
document_id: "c29b35b1e0d514ca21d6149b80c53e80e95549c7588524045e99757e01d50659"
company_key: "semrush-holdings-inc-class-a-common-stock"
company: "SEMrush Holdings Inc."
source_id: "semrush-holdings-inc-class-a-common-stock-news-import-272a4eb27f08"
canonical_url: "https://it.semrush.com/blog/llms-txt/"
published_at: "2025-05-13T16:36:13+00:00"
first_seen_at: "2026-07-22T16:50:18.182588+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:629bbb7ac42a805d663f2e45199180def9e92889c5cab52c56d025931814b201"
---

# Che cos'è LLMs.txt e dovresti usarlo?

Abbiamo tradotto questo articolo dall'inglese.[Clicca qui](https://www.semrush.com/blog/llms-txt/) per leggere l'articolo originale. Se noti qualche problema con i contenuti, non esitare a contattarci all'indirizzoreport-osteam@semrush.com .


## Che cos'è LLMs.txt?


Il file llms.txt è uno standard proposto per aiutare i modelli linguistici di grandi dimensioni (LLM) a comprendere e utilizzare meglio i contenuti dei siti web.


Ecco la specifica ufficiale :


L'idea è piuttosto semplice: invece di lasciare che i crawler AI vaghino per il tuo sito, fornisci loro un elenco curato dei tuoi contenuti più importanti. Per indicare all'intelligenza artificiale a quali contenuti del tuo sito dovrebbe effettivamente prestare attenzione.


Disponiamo già di standard come[robots.txt](https://www.semrush.com/blog/beginners-guide-robots-txt/) e[sitemap](https://www.semrush.com/blog/xml-sitemap/) per aiutare i motori di ricerca a navigare nei siti web in modo più efficiente. La particolarità di llms.txt è che è specificamente progettato per modelli di intelligenza artificiale che potrebbero utilizzare i tuoi contenuti per rispondere a domande o generare risposte per gli utenti.


Si ipotizza inoltre che l'implementazione di llms.txt potrebbe dare ai siti web maggiore visibilità nelle risposte generate dall'intelligenza artificiale e potenzialmente generare più traffico di riferimento.


Prima di scompattare llms.txt e valutare se vale la pena implementarlo, cerchiamo di capire perché è necessario creare un altro standard web.


## Quale problema sta cercando di risolvere LLMs.txt?


llms.txt è progettato per aiutare i crawler AI a esplorare i siti web in modo più efficace. Al momento, questi crawler si trovano ad affrontare due grandi sfide:


- **I siti web moderni sono difficili da leggere.** La maggior parte dei crawler AI può leggere solo l'HTML di base delle tue pagine, non il contenuto caricato da JavaScript. Ciò significa che llms.txt fornisce un formato chiaro e strutturato che aiuta i crawler di intelligenza artificiale a elaborare rapidamente le informazioni.
- **La maggior parte dei siti web contiene una sovrabbondanza di informazioni.** Quando i crawler AI visitano il tuo sito web, non sanno necessariamente cosa è importante. Se perdono tempo a recuperare pagine inutili (come vecchi post di blog), potrebbero generare risposte basate su informazioni non ottimali: llms.txt aiuta a risolvere questo problema.


llms.txt può anche ridurre le inefficienze nell'addestramento di modelli linguistici di grandi dimensioni.


La formazione degli LLM comporta un enorme costo computazionale. Grazie alla guida di llms.txt, gli LLM hanno meno probabilità di sprecare risorse su contenuti irrilevanti.


## Come sono strutturati i file LLMs.txt?


Secondo lo standard proposto, i file llms.txt devono essere strutturati e formattati in Markdown.


Markdown è un linguaggio di markup leggero che utilizza la sintassi di formattazione del testo normale per creare documenti strutturati. (È lo stesso formato utilizzato dagli sviluppatori nei file README di GitHub ed è facilmente analizzabile dai sistemi di intelligenza artificiale.)


Ecco alcuni elementi Markdown comuni che utilizzerai nel tuo file llms.txt:


- # per l'intestazione H1, ## per H2, ### per H3 e così via
- > per citazioni a blocchi per evidenziare descrizioni importanti
- - o * per i punti elenco negli elenchi non ordinati
- \[testo\](url) per i collegamenti ipertestuali al tuo contenuto
- : per aggiungere descrizioni accanto ai link per aiutare a spiegare a cosa portano
- \`\`\` per blocchi di codice quando si condividono esempi tecnici


La specifica ufficiale llms.txt fornisce un esempio molto elementare di come potrebbe apparire il tuo file. Ma se il tuo sito web è grande o complesso, potresti voler aggiungere più struttura, utilizzando H3 e H4 per creare sottosezioni, incorporando tabelle per organizzare i dati o includendo frammenti di codice per dimostrare l'uso dell'API.


Non c'è niente di sbagliato in questo. I file Markdown sono completamente leggibili dai crawler AI, quindi sei al sicuro finché utilizzi una sintassi valida. La struttura aggiuntiva potrebbe effettivamente fornire ai crawler AI più contesto.


Ecco un semplice esempio che segue le specifiche di base:


` # Nome azienda
>Breve descrizione di cosa fa la tua azienda


## Prodotti
- \[Prodotto 1\](https://example.com/product-1): Descrizione di questo prodotto
- \[Prodotto 2\](https://example.com/product-2): Descrizione di questo prodotto


## Documentazione
- \[Guida introduttiva\](https://example.com/docs/getting-started): Introduzione alla nostra piattaforma
- \[Riferimento API\](https://example.com/api): Documentazione API completa`


## I marchi utilizzano lo standard LLMs.txt?


Sì, alcune aziende SaaS e focalizzate sugli sviluppatori utilizzano già i file llms.txt sui loro siti web.


Tuttavia, l'adozione complessiva è piuttosto limitata. Secondo NerdyData, solo[951 domini](https://www.nerdydata.com/reports/llms.txt/b012b7e8-c50d-45e3-8719-0d72f097c3db) (una piccola frazione del web) avevano pubblicato un file llms.txt a luglio 2025.


Ecco alcuni esempi di aziende:


**Marca**


**Su cosa si concentra il file**


**La struttura generale**


[Faccia abbracciata](https://huggingface-projects-docs-llms-txt.hf.space/diffusers/llms.txt)


Documenti per sviluppatori


Utilizza più livelli di intestazioni (#, ##, ###, ####) per suddividere il contenuto in sezioni separate. Include anche esempi di codice completi, numerosi link e utili note. Nel complesso, sembra una base di conoscenza completa.


[Vercel](https://ai-sdk.dev/llms.txt)


Documenti per sviluppatori


Inizia con delle righe descrittive in alto, come title:, description: e tags: per dare un'idea della documentazione specifica che segue. E utilizza intestazioni chiare (#, ##, ###) per organizzare il contenuto in sezioni logiche. In ogni sezione troverai istruzioni dettagliate ed esempi pratici di codice.


[Zapier](https://docs.zapier.com/llms.txt)


Documenti per sviluppatori


Utilizza un numero limitato di titoli e crea una struttura molto semplice. Si compone principalmente di un lungo elenco di link e relative descrizioni che aiutano a spiegare a cosa portano.


[Cal.com](https://cal.com/docs/llms.txt)


Documenti per sviluppatori


Utilizza le intestazioni in alto (#, ##) e poi passa direttamente a un elenco molto lungo di link. I link non sono raggruppati in sezioni e non ci sono sottotitoli, riassunti o descrizioni.


Nota come ogni azienda affronta in modo diverso il proprio file llms.txt. Ognuno di essi utilizza una struttura diversa.


Non c'è niente di sbagliato in questo. Finché viene utilizzato un Markdown valido, il file è leggibile dalla macchina e può essere facilmente elaborato dai sistemi di intelligenza artificiale.


Inoltre, nessuna di queste aziende ha un file incentrato sul proprio sito web nel suo complesso. È una scelta personale che hanno fatto. Puoi creare un file incentrato sull'intero sito o solo su una sezione specifica.


## Dovresti usare LLMs.txt sul tuo sito?


Probabilmente non vale la pena dedicare del tempo all'uso di llms.txt in questo momento, a meno che non siate semplicemente curiosi e vogliate sperimentare.


Al momento llms.txt è solo uno standard proposto e non qualcosa di realmente utilizzato dalle principali aziende di intelligenza artificiale.


Nessuna delle aziende LLM come OpenAI, Google o Anthropic ha dichiarato ufficialmente di seguire questi file quando esegue la scansione dei siti web.


Anche John Mueller di Google lo ha confermato su Bluesky:


Detto questo, ci sono alcuni segnali interessanti.


Ad esempio, Anthropic ha pubblicato un file llms.txt sul proprio sito web. Ciò non significa che il loro crawler AI stia effettivamente utilizzando questi file, ma suggerisce che probabilmente sono almeno aperti all'idea.


Siamo ancora nella fase iniziale di speculazione, in cui le persone stanno implementando il file e sperano che un giorno possa rivelarsi utile.


Semrush implementerà questo file?


Abbiamo implementato llms.txt su uno dei nostri siti affiliati, Search Engine Land, per verificare se offre vantaggi significativi in termini di visibilità e traffico dell'IA. Se sei curioso, puoi dare un'occhiata al file[qui](https://searchengineland.com/llms.txt) .


Monitoreremo i risultati nei prossimi mesi e aggiorneremo questo articolo con le nostre scoperte.


Se vuoi sperimentare anche tu llms.txt sul tuo sito, di seguito trovi le istruzioni dettagliate su come implementarlo.


## Come creare un file LLMs.txt (passo dopo passo)


Si tratta di un aspetto tecnico, quindi è meglio coinvolgere uno sviluppatore nel processo, seguendo questi tre passaggi:


### 1. Decidi quali contenuti vuoi presentare


Prima di creare un file, stabilisci quali pagine o sezioni del tuo sito web devono essere evidenziate per i crawler AI.


Supponiamo che tu voglia creare un file llms.txt per l'intero sito web. Come minimo, considera:


- Pagine di prodotti o servizi
- Post del blog aggiornati
- Pagina dei prezzi
- Pagina Chi siamo
- Pagina dei contatti


In genere, queste sono le pagine che forniranno all'IA una buona idea di cosa fa la tua azienda e di come aiuti i clienti.


### 2. Crea il file


Apri un editor di testo come Blocco note o Visual Studio Code e crea un nuovo file denominato llms.txt.


È necessario formattare il file utilizzando Markdown. Anche in questo caso, gli sviluppatori sono utili per la creazione del file.


Ecco come potrebbe apparire la struttura del file:


` # Nome del sito web
>Breve descrizione del tuo sito web


Note importanti:
- Elemento differenziante chiave o dettaglio importante sulla tua attività
- Un'altra nota importante su cosa fai o non fai
- Terzo punto chiave che aiuta a definire la tua offerta


## Prodotti
- \[Nome prodotto 1\](https://example.com/product-1): Breve descrizione delle caratteristiche principali e dei vantaggi del tuo prodotto
- \[Nome prodotto 2\](https://example.com/product-2): Breve descrizione delle caratteristiche principali e dei vantaggi del tuo prodotto
- \[Nome prodotto 3\](https://example.com/product-3): Breve descrizione delle caratteristiche principali e dei vantaggi del tuo prodotto


## Contenuto del blog
- \[Titolo del post del blog 1\](https://example.com/blog-post-1): Breve descrizione di cosa tratta questo post del blog e perché è utile
- \[Titolo del post del blog 2\](https://example.com/blog-post-2): Breve descrizione di cosa tratta questo post del blog e perché è utile
- \[Titolo del post del blog 3\](https://example.com/blog-post-3): Breve descrizione di cosa tratta questo post del blog e perché è utile


## Azienda
- \[Chi siamo\](https://example.com/about): Background aziendale, missione e informazioni sul team
- \[Contatti\](https://example.com/contact): Come contattare il nostro team e mettersi in contatto
- \[Prezzi\](https://example.com/pricing): Panoramica di piani, funzionalità e costi per l'utilizzo dei nostri prodotti`


### 3. Carica il file sul tuo sito web


Inserisci il file completato nella posizione corretta in modo che i crawler AI possano teoricamente trovarlo.


La posizione esatta dipende dall'ambito del file llms.txt:


- Se copre l'intero sito web, caricalo nella directory principale (ad esempio, "https://\[tuodominio\].com") in modo che sia accessibile all'indirizzo "https://\[tuodominio.com\]/llms.txt"
- Se il file riguarda specificamente la documentazione, posizionarlo nella sottodirectory corrispondente (ad esempio, "https://\[docs.yourdomain.com\]/llms.txt)


Per caricare effettivamente il file avrai bisogno dell'aiuto di uno sviluppatore. Questo file deve essere posizionato sul tuo server, solitamente tramite il pannello di controllo del tuo web hosting, come cPanel.


Accedi al tuo provider di hosting e poi vai su cPanel > File Manager.


Quindi vai alla directory corretta. Se il file llms.txt è destinato all'intero sito, vai alla cartella public_html/. (Questa è la directory radice per la maggior parte dei domini.)


Se si tratta di un sottodominio come "https://\[docs.yourdomain.com\]", vai alla cartella assegnata a quel sottodominio, spesso denominata /docs/ o simile.


Carica lì il tuo file llms.txt e salva le modifiche.


Una volta caricato il file, verifica che tutto funzioni aprendo una nuova scheda e visitando direttamente l'URL.


Puoi anche eseguire un rapido audit del tuo sito web nello strumento[Site Audit](https://www.semrush.com/siteaudit/) di Semrush per confermare che il tuo file llms.txt venga rilevato correttamente.


In questo caso, viene trovato un file llms.txt, quindi l'avviso "Non trovato" è inattivo (disattivato).


Inoltre, non dimenticare di mantenere il file aggiornato nel tempo. Rivedi regolarmente i link per rimuovere le pagine obsolete. E aggiungi link ai nuovi contenuti che aggiungi al sito web.


Condividi


[Tushar Pol](https://it.semrush.com/blog/user/203456689/)


Tushar has been involved in SEO for the past six years, specializing in content strategy and technical SEO. He gained his experience in agencies, where he worked on various ecommerce and B2B clients. On the Semrush blog, he writes about SEO and marketing based on experience drawn from his client work, focusing on sharing practical and effective strategies. His goal is to turn Semrush blog into the ultimate destination for learning SEO and web marketing.
