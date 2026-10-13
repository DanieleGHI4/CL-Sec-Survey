# Incongruenze, lacune e questioni aperte

Questo documento raccoglie ciò che richiede una verifica o una tua decisione. Non modifico nulla di quanto segnalato. Il contenuto della letteratura è in `literature_overview.md`, le tassonomie in `taxonomy_comparison.md`, i file in `project_inventory.md`.

## In breve

Il materiale è coerente nell'impostazione, ma ci sono tre problemi pratici. Primo, la tabella della letteratura ha alcune incongruenze di anagrafica (una chiave file duplicata che nasconde un paper, una chiave con refuso, un anno discordante tra tabella e bibliografia) e diversi PDF presenti nel repository senza una riga, oppure righe senza PDF. Secondo, le tre versioni di tassonomia non sono equivalenti: la differenza più sostanziale è che l'asse delle righe significa "dato attaccato" per le difese in T1/T3 e "momento in cui agisce la difesa" in T2. Terzo, il manoscritto contiene almeno un'affermazione ("no works on clean label attacks") che è contraddetta da due paper presenti nel repository, e ne contiene un'altra (essere "i primi a fare una survey") che non è documentata da nessuna ricerca nei file.

Le sezioni seguenti separano ciò che ho verificato sui PDF da ciò che emerge dalla sola lettura dei file di progetto.

## 1. Verifiche sui paper

Esito delle verifiche fatte sul testo estratto dai PDF (strumenti e limiti in `project_inventory.md`).

| Affermazione | Fonte controllata | Esito |
|---|---|---|
| La riga 6 della tabella (differential privacy) è il paper di Hassanpour | testo di `Hassanpour-IEEEAccess2022.pdf`: intestazione "A. Hassanpour et al.: Differential Privacy Preservation in Robust Continual Learning" | Confermato. Il nome file in tabella (`Umer-IEEEAccess2024`) è sbagliato |
| `Umer-IEEEAccess2024.pdf` è "Adversary Aware Continual Learning", difesa contro backdoor | testo del PDF, autori Umer e Polikar | Confermato. Usa campioni di difesa con un pattern percettibile |
| Nguyen-AAAI2025 è un attacco black-box | testo: "black-box setting (where the attacker has no access to the training model or procedure)", anche clean-label | Confermato |
| Gao-NeuralNetworks2025 è in scenario TIL | testo: "The CL scenario is task-incremental setting" | Confermato |
| Luo-CIKM2026 è in scenario DIL | testo: tre fasi "domain-incremental" | Confermato |
| Li-IJCNN2022 è in DIL e TIL | testo: "both domain and task incremental learning scenarios"; è un attacco clean-label | Confermato |
| Guo-USENIX2025 è in TIL | Il testo estratto di Guo non contiene i termini di scenario. Huang-ICML2026 lo descrive come "LTB ... most advanced persistent backdoor attack for Task-Incremental Learning (TIL)" e lo adatta a CIL sostituendo le teste multiple | Confermato solo per via indiretta |
| Jiang-TDSC2024 usa EEIL, un solo passo dopo il pretraining | testo: "this work focuses on EEIL" | EEIL confermato; il "solo 1 passo" non ricontrollato |
| Bunzel valuta FGSM e PGD in CIL con AT | abstract: CIFAR-10, FGSM, PGD, CIL, AT adattato al CL, perdita di accuratezza pulita | Confermato |
| Pham-ECCV2024 non è CL vero | abstract e Fig. 1: "sequential backdoor learning" contro difese di fine-tuning | Confermato, coerente con la nota |
| Liu-AAAI2024 (due copie) è un attacco al CL | PDF: "Persistent Backdoor Attack on Federated Learning" (FCBA); le due copie hanno lo stesso MD5 | Non è un attacco al CL |
| Huang-USENIX2024 (UBA-Inf) è fuori tema | prima pagina: backdoor attivato dall'unlearning in MLaaS | La prima pagina non menziona il CL; il resto non è stato letto |
| Liu-CoLLA2022 | PDF: "Continual Learning and Private Unlearning" | È in `others/`, non ha riga in tabella |
| Han-NeurIPS(WS)2022 | PDF: "Training Time Adversarial Attack Aiming the Vulnerability of Continual Learning" | È in `attacks/`, non ha riga; la nota di Han-ICIP2023 lo definisce versione estesa |
| Umer-IJCNN2020 usa solo MNIST | PDF: MNIST permuted e split | Coerente. "EWC only" non ricontrollato |
| Kumari, Zhu: descrizioni in tabella | abstract | Coerenti |
| Jiang: anno di pubblicazione | PDF: manoscritto ricevuto 9 gennaio 2022, accettato 21 agosto 2022, data di pubblicazione online 31 agosto 2022, fascicolo vol. 21 n. 2 marzo/aprile 2024 | Tabella e chiave dicono 2024; il `.bib` dice 2022. Entrambi hanno un riscontro nel PDF |

**Seconda passata (dopo l'installazione di poppler).** Con `pdftotext` ho riletto i paper che prima non erano leggibili. Gli esiti sono in `docs/verification/table_patch_proposal.md` (sezione "Esito delle verifiche"). In sintesi: Perla (BB, hard label), Mi (Task-IL e Class-IL, forgetting accelerato e gradient obfuscation), Du (Rotation MNIST, solo EWC), Chen (CIL con dati non etichettati), Umer-IEEEAccess2024 (TIL, DIL, CIL, replay) sono coerenti con la tabella o permettono di completarla. Restano dubbi: Abbasi (scenario CIL non confermato dal testo), Yu-AAAI2024 (il termine black-box non compare), Guo (TIL solo per via indiretta) e Chou (testo illeggibile). Huang-USENIX2024 non è del tutto estraneo al tema: il suo threat model include modelli aggiornati in continuazione con richieste di unlearning, quindi lo classifico "borderline" invece di "fuori tema".

Tutte le verifiche sono mirate (abstract, scenario, metodi, parole chiave), non letture integrali.

## 2. Incongruenze nella tabella della letteratura

1. **Chiave file duplicata.** `Umer-IEEEAccess2024` compare alle righe 4 (Adversary Aware CL) e 6 (Differential Privacy Preservation). La riga 6 è il paper di Hassanpour (verificato). Chi filtra per `File Name` ottiene due righe e perde il collegamento con il PDF di Hassanpour.
2. **Refuso.** `Bunzel-IEEETrusttCom2025` nella tabella, `Bunzel-IEEETrustCom2025` nel PDF.
3. **Anno di Jiang** discordante tra tabella/chiave (2024) e `.bib` (2022), vedi sopra: serve decidere quale usare nella citazione.
4. **PDF senza riga in tabella** (dopo la correzione di 1 e 2): Han-NeurIPS(WS)2022, Liu-AAAI2024 (in due cartelle), Liu-CoLLA2022.
5. **Righe senza PDF**: "Federated Continual Learning against Data Posioning" (refuso nel titolo, commento "TO CHECK"), "Qualcosa di Federated Persistent backdoor" (segnaposto) e "Catastrophic Learning: A New Attack Vector..." (stato "probabilmente da togliere").
6. **Cartella e classificazione non coincidono.** Wang-ICML2022 e Zhou-CVPR2024 sono in `defenses/` ma in tabella sono "other" senza ruolo di difesa. Yu-AAAI2025 e Pham-ECCV2024 sono in `attacks/` ma sono "other" (Yu) o un attacco che non è CL (Pham). Zhu-CCS2026 è in `others/` ma in tabella è "defense". Non è chiaro quale dei due criteri vada considerato valido.
7. **La colonna `Type` mescola livelli diversi.** Contiene tipi di attacco (backdoor, poisoning, evasion), categorie di difesa (differential privacy) e categorie "other (...)". Nel quadro di partenza la backdoor è una forma di poisoning, quindi le due categorie non sono alternative: Du-IJCNN2023 è "model inversion; poisoning; backdoor" e Umer-IJCNN2020 (titolo: "Adversarial Backdoor Attacks") è classificato "poisoning". La tabella stessa si chiama `Poisoning` nel foglio Excel pur contenendo evasion e difese.
8. **La colonna `Atk / Def` mescola ruolo e conoscenza dell'attaccante** ("attack (BB)" accanto a "attack"), usa "-" per cinque righe che non sono né attacco né difesa, "attack/defense" per Bunzel, e lascia vuoto per Huang-USENIX2024.
9. **Valori non uniformi nelle colonne CL-specifiche.** `Persistence` ha "yes", "Yes", "NA (but could work...)", "yes (but they cheat a bit...)"; `Target Metric` mescola metriche (ASR, SASR, FWT, Accuracy) con quantità (forgetting, "targeted SR", "untargeted SR"); `Violation` ha lo stesso valore scritto come "integrity/availability" (con a capo) e "integrity availability" (senza separatore) e in due casi cela una coppia. Il commento sulla cella L2 ("forgetting, current perf, fwd-t") suggerisce che la colonna `Target Metric` dovrebbe riportare solo forgetting, performance corrente e forward transfer (lettura mia del commento): oggi non è così.
10. **Colonna `Quality` non definita.** Mescola quartili di rivista (Q1) con etichette che sembrano di ranking di conferenza (A++, A, A-) e B; non confrontabili tra loro. Tre righe sono vuote e una vale "-".
11. **Note troncate o incomplete.** Wang-ICML2022 termina con "task-free is a particular scenario, which is not"; Chou ha "TODO"; Kumari rimanda all'Appendice B per "info on robustness for CL" (da verificare); tre righe hanno stato "da leggere" / "da finire di leggere" / "probabilmente da togliere" (Huang-USENIX2024, Luo-CIKM2026, Catastrophic Learning).
12. **Commenti nel `.xlsx` ancora aperti.** A7 Khan "CHECK"; H13 Nguyen "check" (sul ruolo BB, ora confermato dal PDF); N14 Du "only rotation MNIST" (confermato dal PDF: Rotation MNIST, solo EWC); A26 Perla "Forse questi non hanno capito niente di poisoning"; B29 riga federata "TO CHECK". Chiusi (done): M16 Umer-IJCNN2020 "only EWC...". Il commento su Perla è rilevante per la classificazione: anche la nota di Yu-AAAI2024 dice che non è chiaro se l'effetto sia vero poisoning o il calo di accuratezza tipico dopo un training avversario indiretto. Entrambi i lavori sono oggi classificati "poisoning".
13. **Copertura della tabella.** Le colonne CL-specifiche sono compilate per il 15-50% delle righe (vedi `literature_overview.md`), e quasi tutte per gli attacchi. Non c'è un criterio scritto di inclusione o esclusione dei paper: le righe fuori tema (federated learning, unlearning) o dubbie non sono marcate come escluse.

## 3. Lacune nella letteratura censita

- Celle della tassonomia senza lavori: sponge (attacchi e difese stateful), membership inference, model extraction, difese contro la perdita di privacy da poisoning, sanitizzazione dei dati di training in CL. Il brainstorming lo dice esplicitamente e chiede di darne un nome e spiegare come cambierebbero rispetto all'adversarial ML (commento su D2). Che davvero non esistano lavori è un'affermazione sulla letteratura che, nei file, non è supportata da una ricerca documentata.
- Una sola difesa contro backdoor in CL (Umer-IEEEAccess2024) e una sola per la privacy (Hassanpour).
- Nessun paper censito su federated continual learning, anche se due righe lo annunciano.
- `Notes.docx` ha una sezione "Discarded papers" vuota: non c'è traccia di quali lavori siano stati scartati e perché. Questo conta, perché Liu-AAAI2024 e Huang-USENIX2024 potrebbero esservi rientrati.
- Nessuna ricerca documentata per l'affermazione "No prior survey on CL security" (sezione Related work) e "we are the first to survey" (abstract). Non l'ho verificata e non posso dire se sia vera; serve una ricerca sistematica da registrare in `docs/verification/`.
- Nove paper del repository non hanno voce nel `.bib` (elenco in `project_inventory.md`).
- Il campo "2026" è composto da lavori la cui sede e il cui stato di pubblicazione non sono stati verificati.

## 4. Questioni sulle tassonomie

Dettaglio in `taxonomy_comparison.md`. In sintesi:

- Tre versioni non allineate (T1, T2, T3); la terza è dentro al manoscritto e quindi è quella che il lettore vedrebbe.
- Asse delle righe con significato diverso per le difese (dato attaccato in T1/T3, momento di azione in T2).
- "Confidentiality" in T1/T3, "Privacy" in T2 e nel testo del manoscritto.
- Marcatura NA incoerente tra le versioni.
- Task, tempo, superficie d'attacco e learning setting sono dichiarati importanti ma non rappresentati.
- Il termine "targeted" è usato per tre cose distinte: specificità sul campione, specificità sull'errore (metriche "targeted SR" / "untargeted SR") e specificità sul task. `Notes.docx` pone la domanda esplicitamente ("Is this untargeted because we attack all previous data?") e un commento dice "Lasciamo targeted", ma la domanda sulla definizione resta aperta.
- Rimane il TODO su cosa significhino Training-from-Scratch, Fine-Tuning e Model-Training nel CL.

## 5. Problemi nel manoscritto (`main.tex`)

Non ho compilato il documento, quindi non so se produce errori; le osservazioni vengono dalla lettura del sorgente.

1. **Affermazione contraddetta dai paper.** La sezione 3 dice, sotto "Data Manipulation Constraint": "no works on clean label attacks". Li-IJCNN2022 è un attacco clean-label in CL (verificato; la nota in tabella lo dice) e Nguyen-AAAI2025 dichiara un setting clean-label (verificato). Probabilmente la frase voleva indicare un'altra cosa (ad esempio clean-label con ottimizzazione bilevel), ma così com'è è inesatta.
2. **Gerarchia dei titoli.** Nella sezione Threat Model compaiono `\subsubsection` (Attacker's Goal, Knowledge, Capability) senza `\subsection` padre, seguiti da `\subsection{Task-aware threat model}`.
3. **Due sottosezioni "Privacy"** nella sezione 4: una con contenuto (model inversion, DP) e una in rosso con "No works here".
4. **Tabella e figura con la stessa didascalia** ("Summary of attacks and defenses across CL security dimensions.") ed etichetta della figura `fig:temporal_plots_tesseract` che non corrisponde al contenuto descritto. Il contenuto della figura non è verificabile con i miei strumenti.
5. **La sezione 4 usa solo il test-time** (dichiarato in rosso; la parte training-time è commentata) ma il manoscritto intitola la sezione "Attacks against Continual Learning" e la sezione 5 è vuota.
6. **Pacchetti mancanti.** `\ding` richiede `pifont`; `Green` e `BrickRed` richiedono `xcolor` con `dvipsnames`. Le macro non sono usate, quindi oggi non producono errori.
7. **Citazione delle survey sul CL.** Wang 2024, De Lange 2021 e Zhou 2024 sono nel `.bib` ma non citate (la sezione 2 è vuota).
8. **Scadenze nei titoli delle sezioni** ("fine ottobre", "fine novembre", "fine dicembre"): vanno rimosse prima della sottomissione, e indicano che la sezione 2 (vuota) è la prima in scadenza. Non è indicato l'anno.
9. **Lo stile di citazione** ha un commento aperto sul tipo di parentesi richiesto dalla rivista (`\setcitestyle{square}`).
10. **Autori e rivista.** Il `\journal` è "Pattern Recognition"; non ho verificato che il formato `5p` sia coerente con la rivista.

## 6. Questioni di repository

- `.gitignore` esclude tutta la cartella `literature/` ("Original scientific papers"). Questo esclude anche `litterature_summary.csv` e `.xlsx`, che non sono PDF ma sono l'indice principale della letteratura: oggi non sono sotto controllo di versione (`git ls-files` non li elenca). Se è voluto per i soli PDF (diritti d'autore, dimensioni), basterebbe restringere la regola; se è voluto per tutto, va saputo.
- Il manoscritto è una "copia locale" del progetto Overleaf: nessun file descrive come si sincronizza.
- `elsarticle.dtx` e `.ins` sono sorgenti del template; sono tracciati in git ma non servono alla compilazione.
- Il file `litterature_summary.*` ha un refuso nel nome (non lo modifico perché è un nome stabilito).

## 7. Decisioni che servono da te

Questa sezione descrive le decisioni come erano poste; lo stato attuale è in `docs/planning/decisions.md` (D1-D8 tutte prese; restano aperti il nome dei valori di Target Task, il trattamento di Liu-CoLLA2022 e Huang-USENIX2024, e l'approvazione delle patch).

1. **Scheletro della tassonomia.** (a) Mantenere T1/T3 (violazione come asse, difese allineate), che è già nel manoscritto e conserva il legame attacco-difesa; (b) adottare T2 (tempo x Data/Model per le difese), più vicina al framework delle sei strategie ma che disallinea difese e violazioni; (c) tenerne una come tabella e l'altra come figura, dichiarando che sono viste diverse. Implicazione: la scelta determina dove si collocano DP, unlearning e adversarial training.
2. **Confidentiality o Privacy** come nome della terza violazione, da applicare a tutto il manoscritto.
3. **Dove mettere task e tempo.** Come colonna/riga aggiuntiva della griglia, come tabella separata dei paper (come già fa la colonna `Target Task`), o come parte del modello di minaccia soltanto. Implicazione: la griglia attuale non distingue backdoor latenti e persistenti.
4. **Criteri di inclusione.** I lavori che usano il CL come strumento per la robustezza o viceversa, quelli di federated learning, quelli di unlearning (Zhu, Liu-CoLLA2022, Huang-USENIX2024) sono dentro o fuori l'ambito principale? Oggi il manoscritto li tratta solo in "Related work", e non è indicato per quali.
5. **Terminologia di "targeted".** Mantenere un solo termine con tre qualificatori (sample, error, task) o separare i termini.
6. **Correzioni alla tabella** (chiave duplicata, refuso, nuove righe per i PDF mancanti, normalizzazione dei valori, colonna `Quality`): vuoi che le proponga come patch separate da approvare, o che la tabella resti com'è finché non decidi?
7. **Controllo di versione di `literature/`:** vedi sezione 6.
8. **Verifica dei paper non leggibili:** vuoi installare un lettore PDF (poppler) per completare la sezione 1, oppure preferisci verificarli a mano?
