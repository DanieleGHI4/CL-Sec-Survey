# Confronto tra le proposte di tassonomia

In questa fase non scelgo tra le proposte. Descrivo cosa dice ciascuna, dove coincidono e dove no, e quali punti restano ambigui. Le proposte sono tre, non due: oltre alle due in `taxonomy/`, il manoscritto contiene una terza versione.

| Sigla | Fonte | Forma |
|---|---|---|
| T1 | `taxonomy/CLSec Taxonomy.xlsx` (arricchita con paper e idee in `extra/Future Work Brainstorming.xlsx`) | Griglia: {dati di test, dati di training} x {attacchi, difese} x {Integrity, Availability, Confidentiality} |
| T2 | `taxonomy/CLSec-Taxonomy2.pptx` | Griglia: {Training Time, Test Time} x attacchi {Integrity, Availability, Privacy} e difese {Data, Model} |
| T3 | `CL_Security_Survey/main.tex`, tabella `tab:security_cl` (e la figura `images/CLSec Slides.pdf`, non leggibile dai miei strumenti) | Come T1, con le celle delle difese di training ampliate |

Per orientarsi: tutte e tre sono evoluzioni del quadro di "Wild Patterns" (Biggio e Roli) e di "Wild Patterns Reloaded" (Cinà et al.), riassunto in `extra/Notes.docx` (sezione "Wild Patterns Notes"). Lo schema di partenza distingue obiettivo dell'attaccante (violazione di sicurezza, specificità sul campione, specificità sull'errore), conoscenza, capacità, e per le difese sei strategie (sanitizzazione dei dati di training, robust training, ispezione del modello, sanitizzazione del modello, ricostruzione del trigger, sanitizzazione dei dati di test).

## In sintesi

T1 e T3 sono sostanzialmente la stessa tassonomia: i contenuti delle celle coincidono, e T3 differisce solo perché elenca più nomi di difesa. T2 è invece una riorganizzazione: usa il tempo (training/test) per le righe e distingue le difese in difese sui dati e difese sul modello, invece di allinearle alle violazioni di sicurezza. Questo cambia il significato dell'asse delle righe per le difese (vedi sotto), ed è l'ambiguità più importante.

Tutte e tre separano attacchi e difese e usano il tempo (o il tipo di dato) come asse principale; i nomi degli attacchi di training sono quasi identici (backdoor persistente/latente, targeted forgetting, indiscriminate forgetting). Nessuna delle tre rappresenta nella griglia le dimensioni che gli appunti e il manoscritto considerano distintive del CL: il bersaglio temporale (task passato, corrente, futuro), l'orizzonte di persistenza e la superficie d'attacco (stream, memoria, generatore, prompt).

## Contenuto cella per cella

Attacchi:

| Cella | T1 (xlsx) | T2 (pptx) | T3 (LaTeX) |
|---|---|---|---|
| Training / Integrity | persistent backdoor; latent backdoor; targeted forgetting | Persistent / Latent Backdoor; Targeted Forgetting | uguale a T1 |
| Training / Availability | indiscriminate forgetting | Indiscriminate Forgetting | uguale a T1 |
| Training / Confidentiality-Privacy | privacy leakage poisoning | Privacy Leakage Poisoning (NA) | privacy leakage poisoning (senza NA) |
| Test / Integrity | evasion attack | Evasion; Persistent Transfer Attack | evasion attack |
| Test / Availability | continual sponge (NA) | Persistent Sponge (NA) | continual sponge (NA) |
| Test / Confidentiality-Privacy | model inversion (for past tasks); membership inference (NA); model extraction (NA) | Task Model Inversion; Task Model Extraction; Membership Inference (nessun NA) | model inversion (for past tasks); membership inference (NA); model extraction assente |

Difese:

| Cella | T1 (xlsx) | T2 (pptx) | T3 (LaTeX) |
|---|---|---|---|
| Training / Integrity | model regularization | (per colonna Data/Model, non per violazione) | c. robust training, c. data sanitization, model inspection, c. model sanitization, trigger reconstruction |
| Training / Availability | NA | - | continual data sanitization |
| Training / Confidentiality | NA | - | NA |
| Test / Integrity | Continual AT | - | Continual AT |
| Test / Availability | stateful defenses (NA) | - | stateful defenses (NA) |
| Test / Confidentiality | continual differential privacy | - | continual differential privacy |
| T2 colonna Data | - | Training Data Sanitization (riga training); Test Data Sanitization (riga test) | - |
| T2 colonna Model | - | Model Inspection, Model Sanitization, Trigger Reconstruction, Continual Robust Training (riga training); "-" (riga test) | - |

Nota sulla colonna "NA": nei file xlsx e LaTeX significa "nessun lavoro esistente", come spiega la cella G2 di T1 ("in rosso difese mancanti (per i quali non ci sono approcci ad-hoc per CL)") e il commento in `Future Work Brainstorming.xlsx` cella D2 ("in casi dove non esiste alcun lavoro, dargli un nome e spiegare come cambierebbe rispetto a AML e perché"). Il prefisso "c." in T3 e "continual" in T1 è, a mia lettura (interpretazione, non verificata con gli autori), il modo di dare un nome a celle senza letteratura.

## Differenze e sovrapposizioni

### Sovrapposizioni

- Gli attacchi di training (backdoor, forgetting mirato e indiscriminato) sono nominati allo stesso modo in T1, T2 e T3.
- Sponge è marcato NA in tutte e tre.
- L'adversarial training continuo come difesa all'evasione compare in T1, T3 e (con altro nome, "Continual Robust Training") in T2.
- La sanitizzazione dei dati di test e di training compare in T2 e (parzialmente) in T3.

### Differenze

1. **Nome dell'asse delle violazioni.** T1 e T3 usano "Confidentiality" per la terza colonna, T2 usa "Privacy". In `Notes.docx` e nel LaTeX ("Security Violation: integrity, availability, privacy") i due termini si alternano. Non è solo lessicale: nel framework di partenza si parla di privacy, e la tassonomia dei CIA usa confidentiality.
2. **Cosa organizza le difese.** In T1 e T3 le difese sono nelle stesse colonne degli attacchi (si legge: "cosa difende da una violazione di integrità?"). In T2 sono in colonne separate Data/Model, e non sono legate alla violazione. Di conseguenza le difese contro la privacy (DP) e contro la disponibilità non hanno una collocazione esplicita in T2.
3. **Significato dell'asse delle righe per le difese.** In T1/T3 la riga indica il dato su cui agisce l'attacco che la difesa contrasta (la "Continual AT" sta in "Test data" perché contrasta l'evasione, anche se agisce a training time). In T2 la riga è il momento in cui agisce la difesa, tanto che "Continual Robust Training" sta in "Training Time". La stessa difesa cambia riga a seconda della proposta. Per gli attacchi i due significati coincidono, per le difese no.
4. **Attacchi test-time persistenti.** T2 aggiunge "Persistent Transfer Attack" (integrità) e "Persistent Sponge". T1/T3 hanno solo "evasion attack". Nella letteratura censita esistono lavori che potrebbero ricadere in "Persistent Transfer Attack" (Liu-AAAI2026 sull'esempio avversario che sopravvive agli aggiornamenti CIL; Bunzel sulla trasferibilità temporale) ma nessun file indica questo collegamento: è una mia ipotesi di lettura.
5. **Livello di dettaglio delle difese di training.** T1 ha una sola etichetta ("model regularization"), T3 cinque, T2 cinque (con sanitizzazione dei dati separata). L'etichetta "model regularization" di T1 non coincide con nessuna delle sei strategie di "Wild Patterns". Il brainstorming assegna a quella cella Umer-IEEEAccess2024, un paper che **[PDF]** usa campioni di difesa con un pattern percettibile: la corrispondenza con "regularization" non emerge dal suo abstract.
6. **Marcatura NA e copertura dei privacy attack.** T1 e T3 segnano NA per membership inference e model extraction, T2 no (le elenca come attacchi esistenti, con il prefisso "Task" davanti a inversion ed extraction). Non è chiaro se T2 intenda che esistono lavori o se ha semplicemente omesso il marcatore. T3 ha perso "model extraction" rispetto a T1. Sulle difese di training T3 mette "continual data sanitization" in Availability mentre T1 mette NA; il brainstorming (cella D5) descrive la sanitizzazione dei dati in CL come "più complicata" ma non cita lavori.
7. **Model inversion.** In T1/T3 è un attacco test-time di confidenzialità ("per i task passati"), sostenuto da Du-IJCNN2023 (Brainstorming cella E2). In `Notes.docx` e nella nota di Du nella tabella, però, il model inversion è descritto come **passo abilitante** dell'avvelenamento che induce forgetting ("every poisoning attack that induce forgetting require a privacy attack!"). Nelle griglie manca un modo per rappresentare questa dipendenza tra una cella di integrità/disponibilità e una di privacy.
8. **"Persistent" e "Latent" nello stesso nome.** T1/T2/T3 usano "persistent / latent backdoor". Nel testo LaTeX commentato la differenza è temporale: attacchi immediati (t_i = t_a, es. evasion) contro attacchi latenti (t_a > t_i, es. backdoor per task futuri). Un attacco può essere sia latente sia persistente; la griglia non distingue.
9. **Targeted forgetting in Integrity.** Tutte e tre mettono "targeted forgetting" tra le violazioni di integrità, e "indiscriminate forgetting" in disponibilità. La tabella dei paper però assegna a Umer-IJCNN2020 e Du-IJCNN2023 entrambe le violazioni, e il brainstorming (D4) mette Umer-IJCNN2020 in entrambe le celle. Resta aperto se la distinzione tra integrità e disponibilità segua la specificità sul campione o quella sul task.
10. **Differential privacy e unlearning.** DP compare solo in T1/T3 (Test/Confidentiality). Hassanpour-IEEEAccess2022 la applica però all'addestramento, quindi la sua riga potrebbe essere "Training" per il criterio di T2. `Notes.docx` collega il privacy in CL anche all'unlearning (Liu-CoLLA2022 in `others/`): nessuna tassonomia ha una cella per l'unlearning.

### Dimensioni discusse altrove ma assenti dalle griglie

| Dimensione | Dove è discussa | Stato |
|---|---|---|
| Task specificity (task passato, corrente, futuro) | `Notes.docx` ("(NEW) Task Specificity"), riga commentata in `main.tex`, colonna "Target Task" della tabella (5 righe compilate) | In `Notes.docx` un commento dice "Da capire interconnessione con attack specificity" e un altro "Lasciamo targeted": c'è già una decisione parziale e una perplessità, che non posso risolvere |
| Tempo: injection time, activation time, orizzonte di persistenza H | `Notes.docx` e `main.tex` (commenti) | Il nome della figura (`fig:temporal_plots_tesseract`) lascia pensare a una versione a più dimensioni; non verificato |
| Superficie d'attacco: dati correnti, replay memory, generatore, parametri/prompt | `main.tex` ("CL Attack Surfaces"), commenti sull'abstract | Elenco in prosa, non nelle griglie |
| Learning setting: Training-from-Scratch, Fine-Tuning, Model-Training | `main.tex` ("Influence on Learning Setting"), `Notes.docx` | `Notes.docx` ha un TODO esplicito: cosa significhino TS/FT/MT nel CL non è stato definito |
| Knowledge: white-box / black-box, stato della conoscenza del passato | `main.tex`, `Notes.docx` | La tabella dei paper ha 3 righe "attack (BB)" |
| Specificità dell'errore (error-specific / error-generic) | `main.tex`, `Notes.docx` ("I haven't found direct information of this") | Assunta invariata dal caso statico, non verificata |
| Metodo CL (regolarizzazione, replay, architettura/prompt) e scenario (TIL, CIL, DIL) | Colonne della tabella; `Notes.docx` ("alcuni attacchi/difese funzionano solo per alcune categorie") | Solo nella tabella dei paper; la domanda di ricerca è esplicitata in `Notes.docx` |

### Copertura della letteratura rispetto alle celle

Il brainstorming mappa questi paper (cella -> chiavi), lo stato indicato è quello del file:

- Test / Integrity / difese: Khan, Chen, Bai, Mi (tutti evasion + CL).
- Test / Confidentiality / attacchi: Du-IJCNN2023. Difese: Hassanpour.
- Training / Integrity / attacchi: backdoor (Jiang, Gao, Guo, Nguyen); poisoning mirato (Umer-IJCNN2021, Li, Umer-IJCNN2020). Difese: Umer-IEEEAccess2024.
- Training / Availability / attacchi: Han-ICIP2023, Kang, Abbasi, Umer-IJCNN2020; black-box: Perla, Yu-AAAI2024.

Paper della tabella non ancora assegnati a nessuna cella: Luo-CIKM2026, Huang-ICML2026 (backdoor persistenti), Liu-AAAI2026 e Bunzel (evasione), Zhu-CCS2026 (unlearning di backdoor), Pham-ECCV2024, Huang-USENIX2024, e le righe senza PDF. Al contrario, nessuna cella cita Kumari, Wang, Yu-AAAI2025, Zhou, Chou: sono lavori "other" che il manoscritto tratta in "Related work" come sinergie, non come attacchi o difese.

## Domande che le tassonomie lasciano aperte

Sono elencate in `initial_issues.md` con riferimento alle decisioni da prendere; qui le richiamo per chiarezza:

1. La riga delle difese indica il bersaglio o il momento?
2. "Confidentiality" o "Privacy"?
3. Le dimensioni task e tempo vanno nella tabella o come assi separati?
4. Dove vanno DP e unlearning?
5. Le celle NA devono restare nella tassonomia, e come si distingue "nessun lavoro" da "non ha senso" (come per i privacy attack sul training, Brainstorming cella E4)?
