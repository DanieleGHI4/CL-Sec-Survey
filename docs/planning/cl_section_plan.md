# Piano per la sezione "Continual Learning" (Sezione 2)

Stato: PROPOSTA, da approvare prima di scrivere il testo in `main.tex`. Il titolo attuale della sezione indica la scadenza "fine ottobre".

## Scopo

`main.tex` chiede una "brief overview, minimal info to understand the security part", con tre punti: scenari (class, task, domain incremental), metodi (replay ecc.), metriche. Propongo di limitare la sezione a ciò che le Sezioni 3-5 usano davvero, senza ripetere ciò che il modello di minaccia già dice (la formalizzazione `theta_t = A(theta_{t-1}, D_t, M_{t-1})` e l'elenco "CL security issues" sono già in Sezione 3; la Sezione 2 deve solo fornire gli ingredienti, con la stessa notazione).

Lunghezza indicativa: circa 1-1,5 pagine nel formato `5p`, una tabella.

## Struttura proposta

**2.1 Problema e notazione.** Flusso di task T_1..T_T, dati D_t, modello theta_t, regola di aggiornamento A, stato conservato M_{t-1} (buffer, generatore, statistiche di regolarizzazione, prompt). Definire qui M_{t-1} perché è il collegamento con le superfici d'attacco della Sezione 3.

**2.2 Scenari.** Task-, domain-, class-incremental. Perché contano per la sicurezza: identità del task al test e spazio delle etichette cambiano ciò che l'attaccante può bersagliare. Nella tabella dei paper lo scenario è dichiarato per 16 righe: CIL in 11 (10 puri più 1 misto), DIL in 4, TIL in 3 (alcune righe ne dichiarano due). Per la parte opzionale: online e task-free (Wang-ICML2022 è task-free ma è fuori ambito; Kang-ICML2023 discute perché il poisoning è raro nell'online).

**2.3 Famiglie di metodi e cosa conservano.** Una sola tabella: famiglia, oggetto conservato, esempi, attacchi della tabella che sfruttano quella famiglia.

| Famiglia | Oggetto conservato | Metodi citati nei paper censiti | Attacchi che lo sfruttano (dalla tabella) |
|---|---|---|---|
| Regolarizzazione | importanza dei parametri | EWC, SI, MAS, LwF, RWalk, AFEC, ANCL | Umer-IJCNN2020/2021, Li, Han, Du, Gao, Guo, Abbasi |
| Replay / esemplari | buffer | ER, ER-ACE, A-GEM, EEIL, iCaRL, DER, BiC, WA, PODNet, MEMO, FOSTER | Perla, Jiang, Liu-AAAI2026, Huang-ICML2026 |
| Replay generativo | generatore | DGR | Kang, Guo |
| Architettura / prompt / adapter | prompt pool, adapter LoRA | metodi prompt-based, O-LoRA, SEFE | Nguyen, Luo |
| Distillazione (segmentazione) | modello precedente | distillazione per segmentazione | Yu-AAAI2024 |

Attenzione: l'assegnazione di alcuni metodi a una famiglia è ambigua (iCaRL, DER, MEMO, FOSTER combinano replay con distillazione o espansione del modello; A-GEM è un metodo a vincoli che Guo tratta come replay). Le famiglie qui sopra seguono le categorie della tabella, non una classificazione verificata su una fonte: serve una fonte autorevole.

**2.4 Metriche.** Accuratezza media, forgetting (backward transfer), forward transfer, e la tensione stabilità-plasticità. Le metriche di attacco (ASR, SR, SASR) non vanno qui ma in Sezioni 3 e 4. Il commento sulla cella L2 della tabella ("forgetting, current perf, fwd-t") indica già le tre quantità che interessano.

## Cosa non fare in questa sezione

- Non ripetere i vincoli sulla sicurezza (nessun accesso al passato, impossibilità di monitorare le prestazioni passate): sono in Sezione 3.
- Non classificare gli attacchi per famiglia di metodi: è l'obiettivo della domanda di ricerca di `Notes.docx` ("alcuni attacchi/difese funzionano solo per alcune categorie") e va trattato nelle Sezioni 4-5, usando la tabella 2.3 come riferimento.
- Non scrivere una rassegna del CL: la sezione rimanda alle survey.

## Fonti: ciò che manca

Le definizioni di scenari, famiglie e metriche vanno ancorate a fonti primarie o a survey. Nel `.bib` ci sono tre survey sul CL (Wang 2024 TPAMI; De Lange 2021; Zhou 2024 sul class-incremental), non citate né presenti come PDF nel repository: non ho potuto verificare che contengano le definizioni che servono, e non le cito finché non le vedo. Nella lista di riferimenti di Guo-USENIX2025 compare "Three types of incremental learning" (Nature Machine Intelligence), che definisce i tre scenari, ma non è nel `.bib`. Non l'ho letto: da verificare.

Proposta operativa: aggiungere i PDF di queste fonti in una cartella dedicata (ad esempio `literature/papers/background/`) così da poter verificare le definizioni prima di scrivere; senza i PDF, la sezione dovrebbe appoggiarsi alle definizioni dei paper di attacco, che sono fonti secondarie.

## Decisioni che servono prima di scrivere

1. Struttura 2.1-2.4 approvata o da cambiare?
2. Includere o no online/task-free in 2.2 (una frase) oppure ometterli?
3. Fonti per le definizioni: aggiungi i PDF (survey più van de Ven et al. e gli articoli originali dei metodi), oppure scrivo appoggiandomi ai paper di attacco, dichiarandolo?
4. Livello di dettaglio sui modelli preaddestrati e sui modelli multimodali (Luo usa MLLM con adapter; Nguyen usa prompt su modelli preaddestrati): una frase oppure un paragrafo dedicato?
5. Notazione: usare quella già in Sezione 3 (`theta_t`, `D_t`, `M_{t-1}`, `T_t`)?
