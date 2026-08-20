---
schema_version: "1.0.0"
document_id: "6ad6b5267600fc367efd55fd7d5e84d42495e7f4f7ada80ec5f4582370886435"
company_key: "semrush-holdings-inc-class-a-common-stock"
company: "SEMrush Holdings Inc."
source_id: "semrush-holdings-inc-class-a-common-stock-news-import-272a4eb27f08"
canonical_url: "https://it.semrush.com/blog/google-search-console-errors/"
published_at: "2025-05-13T14:54:46+00:00"
first_seen_at: "2026-07-22T16:50:18.182588+00:00"
fetched_at: "2026-07-28T21:30:14.907341+00:00"
content_hash: "sha256:1609819ddebd6b8678d7d2f6edb73f03b911e6c14960cc2258ec2a2a737d8e58"
---

# Errori di Google Search Console: come identificarli e correggerli

Abbiamo tradotto questo articolo dall'inglese.[Clicca qui](https://www.semrush.com/blog/google-search-console-errors/) per leggere l'articolo originale. Se noti qualche problema con i contenuti, non esitare a contattarci all'indirizzoreport-osteam@semrush.com .


Google Search Console è un servizio web che consente di monitorare e gestire la presenza del tuo sito web nei risultati di ricerca di Google.


Ti aiuta anche a identificare errori potenzialmente dannosi per il tuo sito web. Ecco cosa devi sapere su questi errori e come risolverli.


## Comprendere gli errori di Google Search Console


Se Googlebot riscontra dei problemi durante la scansione del tuo sito, tali problemi potrebbero essere segnalati come errori in Google Search Console.


È importante esaminare questi aspetti.


Perché?


Innanzitutto, possono avere un impatto negativo sulla visibilità del tuo sito nelle pagine dei risultati dei motori di ricerca ([SERP](https://www.semrush.com/blog/serp/) ).


Ad esempio, errori irrisolti possono impedire a Google di indicizzare correttamente il tuo sito web. Le pagine non indicizzate non verranno visualizzate nei risultati di ricerca.


Anche gli errori irrisolti del sito web possono avere un impatto negativo sull'esperienza dell'utente. Questo è un male per i visitatori. Ed è un fattore di ranking fondamentale per Google, quindi scarse prestazioni in quest'area possono far sì che Google posizioni il tuo sito più in basso anche nei risultati di ricerca.


Diamo un'occhiata a come correggere gli errori di Google Search Console.


## Tipi di errori di Google Search Console e come risolverli


### Errori del codice di stato


Gli errori del codice di stato in Google Search Console riguardano i codici di stato HTTP restituiti quando Googlebot tenta di eseguire la scansione delle tue pagine web.


I codici di stato HTTP vengono trasmessi ogni volta che un sito viene caricato. Quando un utente digita un URL o un bot visita il sito, viene inviata una richiesta al server di quel sito. Il server risponde con un codice di stato HTTP che fornisce al browser (o al bot) maggiori informazioni sulla pagina.


I codici di stato HTTP sono numeri a tre cifre. Ad esempio, "200", "301" o "404".


Codici di stato diversi trasmettono informazioni diverse sul successo o sul fallimento di una richiesta.


Ad esempio, il codice di stato "200" segnala che tutto funziona correttamente e che è possibile accedere alla pagina come previsto.


Tuttavia, alcuni codici di stato indicano che si sono verificati problemi di accesso alla pagina web. Questi vengono visualizzati come errori in Google Search Console. Perché possono avere un impatto negativo sulla capacità dei crawler e degli utenti di accedere al tuo sito.


Per verificare la presenza di errori nel codice di stato, apri[Google Search Console](https://search.google.com/search-console/about) e clicca su “ **Pagine** ” nel menu a sinistra.


Vedrai una ripartizione delle pagine del tuo sito web a seconda che siano "indicizzate" o "non indicizzate".


È possibile che alcune pagine non siano state indicizzate a causa di un errore nel codice di stato.


Per verificare se è così, scorri fino alla fine del rapporto. Vedrai una tabella con il titolo "Perché le pagine non vengono indicizzate".


Se si verificano errori di stato, nella sezione "Motivo" verrà visualizzato uno dei seguenti messaggi:


- Errore del server (5xx)
- Non trovato (404)
- Richiesta non autorizzata (401)
- Morbido 404
- Errore di reindirizzamento (3xx)


Vediamo cosa significano i diversi errori del codice di stato e come risolverli.


#### Errori del server (5xx)


Un codice di stato a tre numeri che inizia con "5" indica un errore del server. Ciò indica un problema con il server che ospita il tuo sito web.


Ecco come appare un errore del server in Google Search Console:


Le cause più comuni dello stato 5xx includono:


- **Errori di programmazione** : Bug o errori di codifica negli script lato server (ad esempio, PHP, Python, Ruby) che alimentano il tuo sito web
- **Problemi di configurazione del server** : configurazioni errate nelle impostazioni del server o nel software del server web (ad esempio, Apache, Nginx)
- **Esaurimento delle risorse** : se il server esaurisce le risorse (come memoria o potenza di elaborazione), potrebbe avere difficoltà a gestire le richieste, causando errori interni del server
- **Problemi con il database** : Problemi con il database su cui si basa il tuo sito web, come problemi di connessione o errori del server del database, possono innescare un errore 5xx


#### Come correggere un errore del server (5xx) in Google Search Console


- **Rivedi le modifiche recenti.** Se l'errore ha iniziato a verificarsi dopo recenti aggiornamenti o modifiche al tuo sito web, valuta la possibilità di annullare tali modifiche per verificare se il problema si risolve.
- **Contatta il tuo provider di hosting.** Se non sei sicuro di come risolvere l'errore del server o se è correlato all'infrastruttura del server, contatta il tuo provider di hosting per assistenza. Potrebbero essere in grado di identificare e risolvere il problema.
- **Risorse del server di prova.** Assicurati che il tuo server disponga di risorse sufficienti (CPU, memoria, spazio su disco) per gestire il traffico e le richieste al tuo sito web. Se necessario, valuta la possibilità di aggiornare il tuo piano di hosting.


Dopo aver identificato e risolto la causa sottostante l'errore del server 5xx, utilizza Google Search Console per richiedere una nuova scansione delle pagine interessate.


Per farlo, inserisci l'URL interessato nella casella di ricerca nella parte superiore di Google Search Console:


Quindi fare clic su " **RICHIEDI INDICIZZAZIONE** ":


In questo modo si chiede a Googlebot di rivisitare le pagine e di aggiornare il proprio indice con le informazioni corrette.


#### Errori non trovati (404)


L'errore "404 Not Found" è un codice di risposta HTTP standard che viene restituito quando un server non riesce a trovare il contenuto associato all'URL richiesto.


Nel contesto di Google Search Console, significa che Googlebot ha provato a scansionare una pagina del tuo sito, ma il server ha restituito un errore 404 perché non è riuscito a trovare il contenuto della pagina.


Ecco alcuni motivi comuni per cui potrebbe verificarsi un errore 404:


- **Eliminazione o rimozione della pagina** : se hai eliminato intenzionalmente una pagina o l'hai rimossa dal tuo sito web e Googlebot tenta di scansionarla, il server restituirà un errore 404
- **Modifiche URL senza reindirizzamenti appropriati** : Se hai modificato la struttura URL del tuo sito web senza implementare reindirizzamenti appropriati (ad esempio reindirizzamenti 301), i vecchi URL potrebbero generare errori 404
- **Errore di battitura o URL digitato in modo errato** : è possibile che ci sia stato un errore di battitura o un errore nell'URL fornito a Googlebot. Assicurati che gli URL nella tua mappa del sito e i link interni siano corretti e portino a pagine esistenti.


#### Come trovare gli errori 404 in Google Search Console


Apri Google Search Console e clicca sul report “ **Pagine** ”.


Scorri verso il basso per vedere "Perché le pagine non vengono indicizzate". Qui potrai vedere se qualche pagina presenta un errore 404.


#### Come correggere un errore 404 in Google Search Console


- **Implementa i reindirizzamenti 301.** Se hai spostato deliberatamente e definitivamente una pagina in una nuova posizione, usa i reindirizzamenti[301](https://www.semrush.com/blog/301-redirects/) per indirizzare il vecchio URL a quello nuovo.
- **Aggiorna i link interni.** Se nel tuo sito web sono presenti link a pagine eliminate, aggiorna tali link in modo che puntino a URL validi. Puoi trovare questi link utilizzando[Site Audit](https://www.semrush.com/siteaudit///) di Semrush.
- **Controlla i link esterni.** Se i siti web esterni rimandano a pagine inesistenti sul tuo sito, potresti contattare tali siti e chiedere loro di aggiornare i loro link.
- **Invia una mappa del sito.** Assicurati che la mappa del sito del tuo sito web sia aggiornata e rifletta accuratamente la struttura attuale del tuo sito. Quindi[invia la mappa del sito](https://www.semrush.com/blog/submit-sitemap-to-google/) a Google Search Console.


#### Errori di richiesta non autorizzata (401)


Un errore con il codice di stato HTTP "401" in Google Search Console indica che una richiesta era "non autorizzata".


Fonte dell'immagine:[Onely](https://www.onely.com/blog/how-to-fix-blocked-due-to-unauthorized-request-401/)


Gli errori 401 si verificano quando:


- La pagina contiene contenuti protetti da password, il che significa che Googlebot e altri crawler non possono accedere alla pagina
- Esistono blocchi IP o restrizioni di accesso che impediscono agli indirizzi IP di Googlebot di accedere alla pagina
- Sono stati impostati accidentalmente problemi di configurazione specifici del crawler


#### Come correggere un errore 401 di autorizzazione non autorizzata in Google Search Console


- **Controlla le impostazioni di autenticazione.** Se la pagina richiede l'autenticazione (ad esempio, è necessario effettuare l'accesso per visualizzare il contenuto), rivedere le impostazioni di autenticazione per assicurarsi che siano configurate correttamente.
- **Accesso di prova.** Verificare manualmente l'accesso alle pagine interessate provando ad accedervi tramite un browser web o utilizzando strumenti come la funzione Visualizza come Google in Google Search Console. Ciò può aiutare a identificare eventuali problemi di accesso.
- **Rivedi le restrizioni di accesso.** Controlla eventuali blocchi IP o restrizioni di accesso sul tuo server o sito web. Assicurati che gli indirizzi IP di Googlebot non siano bloccati e che siano attive le autorizzazioni necessarie per la scansione.
- **Verifica l'accesso dell'agente utente.** Verifica che la configurazione del tuo sito web consenta l'accesso agli user agent dei motori di ricerca più comuni, tra cui Googlebot. Se sono presenti regole che limitano l'accesso a determinati agenti utente, assicurarsi che siano configurate in modo appropriato.


Dopo aver risolto il problema dell'accesso non autorizzato, utilizza Google Search Console per richiedere una nuova scansione delle pagine interessate.


#### Errori soft 404


Gli errori soft 404 in Google Search Console si verificano quando una pagina sembra una pagina normale (restituisce un codice di stato 200), ma il contenuto o determinati segnali sulla pagina indicano che dovrebbe essere trattata come se non esistesse.


Ciò può creare confusione per i motori di ricerca come Google. E può avere un impatto sul modo in cui le tue pagine vengono indicizzate e visualizzate nei risultati di ricerca. Google Search Console li segnala come errori.


Gli scenari più comuni che portano agli errori soft 404 includono:


- **Pagine vuote** : le pagine vuote o con pochissimo contenuto potrebbero generare errori soft 404. Sebbene il server restituisca un codice di stato "200 OK", la mancanza di contenuto suggerisce che la pagina non fornisce informazioni significative agli utenti.
- **Pagine reindirizzate** : se una pagina viene reindirizzata a un altro URL, ma il contenuto dell'URL di destinazione è scarso o non pertinente, Google potrebbe interpretarlo come un soft 404
- **Pagine di errore personalizzate** : se il tuo sito web ha una pagina di errore personalizzata che mostra all'utente un messaggio di errore, anche se ha un codice di stato "200" OK", ciò potrebbe generare errori soft 404


#### Come correggere un errore Soft 404 in Google Search Console


- **Rivedi il contenuto della pagina.** Esaminare il contenuto delle pagine contrassegnate come errori soft 404. Assicurarsi che forniscano informazioni significative agli utenti. Se il contenuto non è sufficiente, valuta la possibilità di migliorarlo o di reindirizzare gli utenti a una pagina più pertinente.
- **Verifica i reindirizzamenti.** Se una pagina viene reindirizzata, assicurati che l'URL di destinazione contenga contenuti pertinenti. Evita di reindirizzare gli utenti a pagine che non sono correlate al contenuto della pagina originale.
- **Controlla le pagine di errore personalizzate.** Se il tuo sito web utilizza pagine di errore personalizzate, assicurati che forniscano informazioni utili e che non vengano scambiate per contenuti vuoti o irrilevanti.


### Errori di reindirizzamento


I reindirizzamenti indirizzano gli utenti e i crawler dei motori di ricerca da un URL a un altro.


Ad esempio, se una pagina non esiste più, puoi inserire un reindirizzamento per indirizzare automaticamente i visitatori a una pagina correlata che **esiste** .


Gli errori di reindirizzamento si verificano quando un reindirizzamento non riesce per qualche motivo. Questi errori possono avere un impatto negativo sul modo in cui i motori di ricerca come Google analizzano, indicizzano e classificano il tuo sito web.


Gli errori di reindirizzamento più comuni includono:


- **Catene di reindirizzamento** : si verifica quando un URL reindirizza a un altro, e poi il secondo URL reindirizza a un terzo, formando una sequenza di reindirizzamenti. Le catene di reindirizzamento possono rallentare il caricamento delle pagine e avere un impatto negativo sull'esperienza utente e sull'efficienza della scansione dei motori di ricerca.
- **Cicli di reindirizzamento** : si verificano quando l'URL di destinazione finale reindirizza a un URL precedente nella catena. Ciò può causare un ciclo infinito di reindirizzamenti, impedendo il caricamento della pagina e causando errori di scansione.
- **Reindirizzamenti errati** : se una pagina viene reindirizzata a un URL errato o irrilevante, può verificarsi un errore di reindirizzamento. I reindirizzamenti devono essere impostati con precisione e indirizzare gli utenti e i motori di ricerca verso un URL di destinazione pertinente.


### Come correggere un errore di reindirizzamento in Google Search Console


Identifica eventuali errori di reindirizzamento in Google Search Console visitando il report " **Pagine** ". Scorri verso il basso per vedere "Perché le pagine non vengono indicizzate".


- **Analizza le catene di reindirizzamento.** Se vengono identificate catene di reindirizzamento, esaminarle e semplificarle. Idealmente, un reindirizzamento dovrebbe condurre direttamente alla destinazione finale, senza passaggi intermedi non necessari.
- **Correggi i loop di reindirizzamento.** Se vengono rilevati loop di reindirizzamento, identificare l'origine del loop e correggere le configurazioni di reindirizzamento per interromperlo. Assicurarsi che l'URL di destinazione finale sia corretto.
- **Controlla i target di reindirizzamento.** Verificare che gli URL a cui vengono reindirizzate le pagine siano accurati e pertinenti. I reindirizzamenti errati possono confondere gli utenti e i motori di ricerca.
- **Aggiorna i link interni.** Se hai modificato gli URL e implementato i reindirizzamenti, aggiorna i link interni del tuo sito in modo che puntino direttamente ai nuovi URL, riducendo così la necessità di reindirizzamenti.
- **Utilizzare codici di stato di reindirizzamento corretti.** Quando si implementano i reindirizzamenti, utilizzare codici di stato HTTP appropriati. Ad esempio, un reindirizzamento 301 indica uno spostamento permanente, mentre un reindirizzamento 302 segnala uno spostamento temporaneo.


## Problemi di scansione e indicizzazione


I problemi di scansione e indicizzazione si verificano quando i crawler di Google non riescono a raggiungere una pagina a cui dovrebbero riuscire. Oppure quando qualcosa impedisce loro di aggiungere una pagina al loro indice.


Puoi verificare questi problemi nel report " **Pagine** ".


Apri il report e scorri verso il basso fino alla tabella "Perché le pagine non vengono indicizzate".


Potresti riscontrare i seguenti problemi.


### Bloccato da Robots.txt


Se una pagina è bloccata da robots.txt, questo potrebbe rappresentare un problema. Ma solo se non vuoi che quella pagina venga bloccata. Alcune pagine sono bloccate intenzionalmente.


Robots.txt è un file presente sul tuo sito web e contiene istruzioni per i crawler dei motori di ricerca. Di solito è la prima pagina che visitano sul tuo sito web.


È possibile includere istruzioni nel file robots.txt per indicare ai bot di ricerca di non eseguire la scansione di determinate pagine. Ad esempio, pagine di amministrazione non accessibili senza un login.


Per farlo, è necessario utilizzare la direttiva "disallow".


Tuttavia, è possibile che una pagina venga bloccata per errore dal file robots.txt.


In Google Search Console, se vedi una pagina che riporta la dicitura "bloccata da robots.txt", ma vuoi che Google esegua la scansione di quella pagina, puoi modificare il file robots.txt. Quindi, utilizza Google Search Console per richiedere una nuova scansione delle pagine interessate.


### Come risolvere le pagine bloccate da Robots.txt


Puoi esaminare e risolvere eventuali problemi con il tuo file robots.txt utilizzando lo strumento[Site Audit](https://www.semrush.com/siteaudit///) .


Apri lo strumento e clicca su “ **+ Crea progetto** ”.


Inserisci il dominio del tuo sito web. Se vuoi, puoi anche dare un nome al progetto.


Fare clic sul pulsante " **Crea progetto** ".


Configura eventuali impostazioni aggiuntive desiderate per l'audit del sito. Quindi fare clic su " **Avvia audit del sito** ".


Una volta completato l'audit, vai alla scheda " **Problemi** " e cerca "robots.txt".


Fai clic su eventuali problemi relativi al file robots.txt, ad esempio "Il file robots.txt presenta errori di formato".


Verrà visualizzato un elenco delle righe non valide nel file.


Fare clic su " **Perché e come risolverlo** " per ottenere istruzioni specifiche su come risolvere l'errore.


## Problemi di usabilità e di esperienza utente


I problemi dei siti web che causano esperienze utente scadenti sono preoccupanti per due motivi.


Innanzitutto, non vuoi che i visitatori del tuo sito abbiano esperienze negative. Crea sfiducia nel tuo marchio. E riduce le possibilità che quella persona interagisca nuovamente con i tuoi contenuti.


Anche le esperienze utente negative sono negative perché le esperienze utente positive sono un fattore di ranking importante per la Ricerca Google. Le pagine con esperienze utente scadenti non avranno buoni risultati nei risultati di ricerca.


### Problemi relativi ai Core Web Vitals


I Core Web Vitals sono un insieme di parametri incentrati sull'utente che misurano le prestazioni di caricamento, l'interattività e la stabilità visiva di una pagina.


In sostanza, i Core Web Vitals misurano quanto un sito web sia veloce e piacevole da usare.


Le tre metriche principali sono:


- **Largest Contentful Paint (LCP)** : il tempo impiegato affinché l'elemento di contenuto più grande (come un'immagine o un blocco di testo) diventi visibile sullo schermo dell'utente
- **Interazione con la prossima vernice (INP)** : quanto bene la pagina web risponde alle interazioni dell'utente
- **Spostamento cumulativo del layout (CLS)** : quante volte la pagina si sposta durante il caricamento


Segnalazioni di Google Search Console quando si verificano problemi di prestazioni dei Core Web Vitals.


È possibile identificare questi problemi utilizzando lo strumento[Site Audit](https://www.semrush.com/siteaudit//) . E ricevi suggerimenti su come risolverli.


### Come risolvere i problemi di Core Web Vitals


Apri[Audit del sito](https://www.semrush.com/siteaudit//) . Scegli un progetto esistente oppure clicca su “ **+ Crea progetto** ”.


Configurare le impostazioni e fare clic su " **Avvia audit sito** ".


Tra pochi minuti vedrai i risultati.


Nel report "Panoramica", vedrai un widget sulla destra chiamato "Core Web Vitals". Fai clic su " **Visualizza dettagli** ".


Vedrai le tue prestazioni in base alle metriche Core Web Vitals per 10 pagine del tuo sito.


Scorri verso il basso fino a "Metriche" e potrai vedere le prestazioni suddivise nei tre Core Web Vitals.


Sotto ognuno di essi è presente un elenco intitolato "Miglioramenti principali" con suggerimenti per migliorare le tue prestazioni in quell'area.


### Errori HTTPS in Google Search Console


Google privilegia i siti dotati di certificato SSL. Si tratta di un certificato di sicurezza che crittografa i dati inviati tra un sito web e un browser.


Una volta ottenuto un certificato SSL, tutte le pagine del tuo sito web dovrebbero iniziare con "https://".


Qualsiasi pagina che inizi con "http://" potrebbe indicare problemi di sicurezza per i motori di ricerca. Quindi è meglio risolverli quando si presentano.


Apri Google Search Console e clicca su “HTTPS” nel menu a sinistra.


La tabella mostrerà se sul tuo sito sono presenti URL non HTTPS.


### Come correggere gli errori HTTPS in Google Search Console


Se riscontri errori HTTPS, scorri verso il basso nella pagina HTTPS in Google Search Console. Vedrai una tabella chiamata "Perché le pagine non vengono servite tramite HTTPS".


Fare clic sulla riga del problema per vedere quali URL sono interessati.


Una volta visualizzato l'elenco degli URL, puoi rimuovere dal tuo sito web tutti i link a queste pagine e chiedere a Google Search Console di eseguire una nuova scansione del tuo sito.


## Azioni manuali in Google Search Console


Le azioni manuali in Google Search Console si riferiscono alle sanzioni imposte a un sito web da revisori umani che lavorano presso Google. Quando questi revisori individuano violazioni delle linee guida di Google, possono adottare misure manuali per penalizzare il sito.


Le azioni manuali sono diverse dalle penalità algoritmiche, che vengono applicate automaticamente dagli algoritmi di Google.


I motivi più comuni per le azioni manuali includono:


- **Collegamento non naturale** : se un sito è coinvolto in schemi di collegamento, ha acquistato collegamenti o partecipa a scambi di collegamenti che violano le linee guida di Google, potrebbe ricevere un'azione manuale
- **Contenuto scarso o duplicato** : i siti web con contenuti di bassa qualità o duplicati potrebbero essere soggetti ad azioni manuali per violazione delle linee guida sulla qualità dei contenuti
- **Cloaking o reindirizzamenti furtivi** : se un sito mostra contenuti diversi agli utenti e ai motori di ricerca o utilizza reindirizzamenti ingannevoli, potrebbe ricevere un'azione manuale
- **Markup strutturato spam** : L'uso errato del markup dei dati strutturati (schema.org) per manipolare i risultati di ricerca può portare ad azioni manuali
- **Spam generato dagli utenti** : i siti Web che consentono agli utenti di generare contenuti (commenti, forum) potrebbero essere soggetti ad azioni manuali se non riescono a moderare e controllare efficacemente lo spam
- **Contenuto hackerato** : se un sito viene compromesso e contiene contenuti dannosi o irrilevanti, Google potrebbe intraprendere un'azione manuale per proteggere gli utenti


Se viene imposta un'azione manuale, verrai avvisato tramite Google Search Console.


**Fonte immagine** :[Google](https://support.google.com/webmasters/thread/7879895/regarding-manual-actions-in-search-console?hl=en)


La notifica ti fornirà dettagli sul problema. Oltre a una guida su come risolvere il problema e i passaggi per richiedere una revisione dopo aver apportato le correzioni.


Una volta risolto il problema, inseriscilo in Google Search Console e fai clic su " **Richiedi revisione** ".


Se hai risolto correttamente il problema, il team di Google lo contrassegnerà come risolto e le sanzioni potrebbero essere revocate.


### Problemi di sicurezza in Google Search Console


Quando vengono identificati problemi di sicurezza, questi possono avere un impatto negativo sulla visibilità del tuo sito nei risultati di ricerca. Inoltre, Google potrebbe etichettare il tuo sito come potenzialmente dannoso per gli utenti.


Il rapporto Problemi di sicurezza in Google Search Console fornisce informazioni su potenziali minacce o problemi di sicurezza rilevati sul tuo sito web.


I problemi di sicurezza più comuni segnalati in Google Search Console includono:


- **Infezioni da malware** : Google potrebbe rilevare malware sul tuo sito, indicando che è stato compromesso. E ci sono software dannosi che potrebbero danneggiare i visitatori.
- **Contenuto hackerato** : se il tuo sito web è stato hackerato, gli aggressori potrebbero inserire contenuti o link indesiderati. Google ti avviserà se rileva contenuti hackerati.
- **Ingegneria sociale** : Google identifica i casi in cui il tuo sito potrebbe essere coinvolto in attacchi di ingegneria sociale, come phishing o pratiche ingannevoli


### Passaggi per risolvere i problemi di sicurezza:


Utilizza Google Search Console per visualizzare e risolvere eventuali problemi di sicurezza.


Apri Google Search Console. Passare alla sezione Azioni manuali di sicurezza & sul lato sinistro dello schermo. Fare clic su “ **Problemi di sicurezza** .”


Se ci sono problemi segnalati, li vedrai qui.


Google fornirà informazioni sul tipo di problemi di sicurezza rilevati. Oltre a raccomandazioni o azioni per risolvere i problemi.


Se vengono rilevati malware o contenuti hackerati, pulisci e metti in sicurezza il tuo sito web. Rimuovere qualsiasi codice dannoso, aggiornare le password e implementare misure di sicurezza aggiuntive.


Dopo aver risolto i problemi di sicurezza, invia il tuo sito per una revisione tramite Google Search Console. Google valuterà se i problemi sono stati affrontati in modo adeguato.


## Prevenire gli errori di Google Search Console


Segui queste best practice per impedire che si verifichino errori in Google Search Console.


- **Eseguire controlli regolari del sito.** Esegui controlli regolari del tuo sito web per identificare e risolvere tempestivamente eventuali problemi tecnici.
- **Mantieni aggiornata la tua mappa del sito XML.** Assicurati che il tuo sito web abbia una mappa del sito XML che includa tutte le pagine pertinenti. Invia la mappa del sito ai motori di ricerca tramite Google Search Console per facilitarne la scansione e l'indicizzazione.
- **Ottimizza il tuo file robots.txt.** Utilizza un file robots.txt ben strutturato per stabilire quali parti del tuo sito devono essere analizzate dai motori di ricerca. Rivedere e aggiornare regolarmente questo file.
- **Assicurati di avere un certificato SSL valido.** Assicurati che il tuo sito web abbia un certificato SSL valido, che garantisca una connessione sicura (HTTPS). Google privilegia i siti sicuri e questo influisce positivamente sul posizionamento.
- **Ottimizza le immagini.** Ottimizza le immagini per un caricamento più rapido. Utilizzare nomi di file descrittivi e includere testo alternativo per l'accessibilità e la SEO.
- **Utilizza una struttura URL solida.** Mantieni gli URL semplici, descrittivi e intuitivi. Se possibile, evita di utilizzare parametri URL complessi.


Gli strumenti Semrush ti aiutano a tenere sotto controllo le prestazioni SEO tecniche del tuo sito web. Grazie a loro, puoi individuare i problemi prima che degenerino.


Utilizza[Audit del sito](https://www.semrush.com/siteaudit//) per scoprire eventuali problemi attuali del tuo sito. E come risolverli.


Inoltre, riceverai email automatiche nel momento in cui si verifica un potenziale nuovo problema. Così sarai sempre al top delle tue prestazioni SEO.
