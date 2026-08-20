---
schema_version: "1.0.0"
document_id: "504b24f156d5a1e49cce800422e18df07bb203bb0278aaa3374b157fe5579232"
company_key: "proto-labs-inc-common-stock"
company: "Proto Labs Inc."
source_id: "proto-labs-inc-common-stock-news-import-023410ffc267"
canonical_url: "https://www.protolabs.com/it-it/risorse/blog/considerazioni-sull-orientamento-dei-pezzi-stampati-in-3d/"
published_at: null
first_seen_at: "2026-07-23T22:34:45.210065+00:00"
fetched_at: "2026-07-28T21:18:37.293716+00:00"
content_hash: "sha256:cdc6db432c0f778a9cb12c5c70e8e169ac2bfd1e2089b1ec4788e6be3c8a3e78"
---

# L’orientamento delle parti per la stampa 3D: come influisce sulla resistenza, la precisione e sulle finiture superficiali

Può sembrare scontato optare per l'orientamento che offre la minima altezza z e quindi il tempo di costruzione più rapido. Tuttavia, l'orientamento in base alla sola altezza z potrebbe non essere sempre il più sensato in termini di requisiti del pezzo. Ci sono alcuni altri fattori tecnici che entrano in gioco quando si pensa all'orientamento dei pezzi per la stampa 3D. Quando si pensa all'orientamento dei pezzi per la stampa 3D, in particolare per la stereolitografia (SLA), si pensa ad una matita. Che cosa ci vorrebbe per stampare in 3D questa matita strato per strato, sia in orizzontale che in verticale? Stamparla in orizzontale richiederebbe meno tempo, perché è più piatta quando è appoggiata su un fianco, mentre stamparla in verticale richiederebbe molto più tempo, perché è alta e richiede più strati disegnati impilati in sequenza uno sull'altro. La stampa in orizzontale richiederebbe anche un maggior numero di strutture di supporto rispetto alla stampa verticale della matita, che richiederebbero ulteriore tempo per essere rimosse per ottenere un pezzo finito. Quindi, la scelta dell'orientamento potrebbe dipendere dai requisiti di resistenza o di finitura superficiale della matita, e si potrebbe anche voler orientare il pezzo ad angolo. La scelta dell'orientamento della costruzione[in stereolitografia](https://www.protolabs.com/it-it/servizi/stampa-3d/stereolitografia/) richiede una serie di considerazioni. Vediamole ora!


### Orientamento dei pezzi stampati in 3D per la resistenza


Vediamo innanzitutto una rapida illustrazione che spiega le implicazioni generali dell'accumulo di strati nella stampa 3D e delle forze esterne sui pezzi. Quando le forze di tensione sono normali, o perpendicolari, agli strati, il pezzo tende a indebolirsi. Quando le forze di tensione sono parallele, o in linea con gli strati, il pezzo è più forte. Quando le forze sono normali agli strati, il pezzo è generalmente in grado di sopportare una maggiore compressione rispetto alle forze in linea con gli strati. Si possono trarre due conclusioni:


1. Stampare con un orientamento tale che gli strati siano allineati con l'asse in cui le forze di tensione sono maggiori
2. Stampare con un orientamento tale che gli strati intersechino l'asse in cui le forze di compressione sono più elevate.


Nel caso teorico della matita, l'orientamento orizzontale della costruzione sarà probabilmente adatto per tenere conto delle forze di tensione e flessione che la matita incontrerà durante l'uso. Bisogna notare che queste linee guida si applicano sempre alle parti FDM (Fused Deposition Modelling). Le parti SLA presentano un'anisotropia molto minore rispetto a quelle FDM, poiché gli strati sono legati chimicamente, e la risoluzione ottenuta con la SLA è molto più elevata rispetto alla FDM. Tuttavia, i materiali termoindurenti possono richiedere una riflessione aggiuntiva sull'orientamento del pezzo per massimizzare la resistenza delle caratteristiche critiche del pezzo.


---


Una parte stampata in 3D e orientata come mostrato sopra presenta una contrazione differenziale. La rapida transizione da una sezione trasversale piccola a una grande ha causato la contrazione di questo pezzo.


### Orientamento per evitare la contrazione differenziale


In generale, quanto più disuguale è la distribuzione del materiale in un pezzo, tanto più è probabile che esso sia vittima di un fenomeno noto come ritiro differenziale. Alcune geometrie e dimensioni dei pezzi sono intrinsecamente più inclini a ritirarsi perché non hanno un supporto adeguato dagli strati di materiale precedenti. Per illustrare questo fenomeno, si pensi a una lettera maiuscola "H". Quando la sezione trasversale appare strato per strato al centro, tende a tirare verso l'interno le gambe di supporto, causando l'inarcamento del pezzo nella parte inferiore. Per evitare il ritiro differenziale, ci sono un paio di opzioni. Spesso costruiamo le parti SLA ad angolo per ridurre la superficie e quindi le sollecitazioni complessive di ogni strato. Ciò contribuisce anche ad aumentare la dimensionalità dei pezzi e la precisione delle tolleranze. Inoltre, costruiamo i pezzi in posizione distesa, in modo che ogni strato formato abbia la stessa superficie dello strato sottostante e non vi siano grandi oscillazioni della sezione trasversale. Quando sono necessarie caratteristiche del pezzo critiche dal punto di vista dimensionale o ad alta risoluzione, consigliamo di orientare i pezzi in modo da posizionare tali caratteristiche sulla superficie rivolta verso l'alto del pezzo. Si tenga presente che la costruzione dei pezzi in posizione obliqua può dare origine a pezzi con una finitura superficiale non ideale, se l'aspetto estetico è fondamentale.


La contrazione differenziale può essere evitata orientando il pezzo in modo da eliminare le variazioni di sezione da piccole a grandi (vedi sopra) o orientando il pezzo ad angolo (vedi a destra).


Se l'obiettivo è quello di ottenere caratteristiche estetiche uniformi, costruire i pezzi in verticale. Se si costruisce in obliquo, le linee di strato saranno visibili, come nel caso del cilindro angolato.


---


### Orientare in modo da ottenere la finitura superficiale desiderata


L'effetto lucido è visibile sulla "H" costruita in piano. La "H" costruita in obliquo evidenzia le linee di livello e gli effetti visivi che possono avere sul testo.


Come regola generale, il nostro team può scegliere un orientamento diverso per offrirvi un pezzo della migliore qualità. Una cosa importante da tenere a mente è che se non si specifica l'orientamento desiderato della parte (orizzontale o verticale), sceglieremo l'orientamento che riteniamo essere il miglior compromesso tra il tempo di costruzione più rapido possibile e una buona ottica. Comunicarci quali sono gli aspetti più importanti per le vostre parti stampate in 3D ci aiuta a determinare il modo migliore per orientare la vostra parte durante il processo di costruzione. In generale, il risultato più importante è che se avete un'esigenza particolare per una parte stampata in 3D, sia che si tratti di caratteristiche critiche, di considerazioni sui costi o di una finitura superficiale specifica desiderata, assicuratevi di comunicarlo in anticipo al nostro team di ingegneri, in modo da poter garantire che la vostra parte soddisfi i vostri obiettivi specifici.
