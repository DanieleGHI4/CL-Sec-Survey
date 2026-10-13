# Decisioni di progetto

Registro delle decisioni prese dall'utente. Fonte: conversazione del 2026-10-12 che ha seguito la prima analisi (`docs/analysis/`). I documenti in `docs/analysis/` descrivono lo stato PRIMA di queste decisioni e non sono stati riscritti; qui si indica come le decisioni li modificano.

## D1. Scheletro della tassonomia

Scelta: opzione (a), cioè T1/T3 (violazione di sicurezza come asse, difese allineate agli attacchi). T2 (`CLSec-Taxonomy2.pptx`) non è lo scheletro; le sue idee restano disponibili come riferimento.

Conseguenze aperte (non decise): la scelta fissa il significato dell'asse delle righe per le difese a "dato attaccato", ma restano da collocare le difese che agiscono a training time contro attacchi test-time (Continual AT) e DP; il confronto è in `docs/analysis/taxonomy_comparison.md`, differenza 3 e 10.

## D2. Nome della terza violazione

Scelta: **Confidentiality**. Da applicare a tutto il manoscritto: oggi `main.tex` usa "privacy" in più punti (ad esempio "Security Violation: integrity, availability, privacy" e le sottosezioni "Privacy"). Nessuna modifica fatta al manoscritto.

## D3. Dimensione task e tempo

Scelta: per ora una **tabella separata dei paper**, non una dimensione della griglia. La tabella separata non è ancora stata creata.

## D4. Criteri di inclusione

Regola: restano dentro solo i lavori che trattano la sicurezza del CL.
- Esclusi i lavori che usano il CL per la robustezza.
- Federated learning e unlearning: dentro solo se trattati ed usati esplicitamente per la sicurezza del CL.

Applicazione proposta ai lavori censiti (da confermare, non applicata alla tabella):

| Lavoro | Proposta | Motivo |
|---|---|---|
| Chou-CW2022, Zhou-CVPR2024, Pham-ECCV2024 | fuori | Usano il CL come strumento per la robustezza (caso esplicitamente escluso) |
| Kumari-NeurIPS2022, Wang-ICML2022, Yu-AAAI2025 | fuori (da confermare) | Il caso inverso (tecniche avversarie per migliorare il CL) non è stato nominato: li escludo per la regola generale, ma l'utente non lo ha detto esplicitamente |
| Zhu-CCS2026 | fuori | Usa la lente del CL per fare unlearning di backdoor in modelli generici, non tratta la sicurezza di un sistema CL |
| Liu-AAAI2024 | fuori | Federated learning, non CL |
| Liu-CoLLA2022 | da decidere | Unlearning privato in CL: riguarda la privacy dell'agente CL, ma è una funzionalità richiesta dall'utente, non una minaccia; non è chiaro se rientri in "sicurezza" |
| Huang-USENIX2024 | da decidere, probabilmente fuori | Il threat model include modelli aggiornati in continuazione (CT-MLaaS) con richieste di unlearning, ma non è un attacco ai metodi CL contro il forgetting |
| "Federated Continual Learning against Data Posioning" | dentro se esiste ed è sulla sicurezza del CL | Nessun PDF, titolo da verificare |
| Catastrophic Learning (arXiv 2026) | dentro per ambito, stato "da togliere" resta aperto | Attacco al CL, ma assunzioni giudicate irrealistiche |
| Hassanpour-IEEEAccess2022 | dentro | DP integrata nel CL; è privacy/confidentiality del CL |
| Tutti gli altri attacchi e difese su evasion, poisoning, backdoor in CL | dentro | |

## D5. Terminologia di "targeted"

Scelta: tre dimensioni distinte.

| Dimensione | Nome | Valori |
|---|---|---|
| Quali campioni sono colpiti | Sample specificity | sample-specific / sample-generic |
| Quale errore si vuole ottenere | Error specificity | error-specific / error-generic |
| Quale task è il bersaglio | **Target Task** | past / current / future (con "tutti" o "alcuni") |

"Targeted forgetting" e "indiscriminate forgetting" restano nomi di famiglie di attacchi e vanno definiti con le tre dimensioni. Va ancora aggiornato il testo di `docs/analysis/` e di `main.tex`, che usano "targeted" in modo indifferenziato.

## D6. Correzioni alla tabella

Scelta: proposte come patch separata da approvare. Vedi `docs/verification/table_patch_proposal.md`. La tabella non è stata modificata.

## D7. `.gitignore`

Scelta: restringere la regola ai soli PDF. Fatto: la riga `literature/` è diventata `literature/**/*.pdf`. Effetto: CSV e XLSX di `literature/` sono ora visibili a git (non ancora aggiunti né committati).

## D8. Lettore PDF

Scelta: installare. Fatto: `poppler` installato con Homebrew (`pdftotext` in `/opt/homebrew/bin`). Il sandbox bloccava la rete, quindi il comando è stato eseguito fuori dal sandbox su richiesta esplicita. Homebrew ha anche eseguito la pulizia automatica di vecchie cache e versioni (non richiesta da me, effetto standard del comando).

## Aggiornamento (secondo giro di decisioni)

- **D4, completata.** Tutto ciò che nella tabella è classificato "other" (e tutto ciò che sta in `others/`) è argomento legato ma escluso dal paper per ora: valore `related` nella nuova colonna `Inclusion`. Kumari, Wang, Yu-AAAI2025 restano fuori se riguardano il solo miglioramento del CL senza focus sulla sicurezza (condizione dell'utente; Wang e Kumari citano benefici di robustezza avversaria, da ricontrollare). Liu-CoLLA2022: fuori (`related`). Huang-USENIX2024: tenuto (`in`) con nota di borderline. Liu-AAAI2024 (federated learning, non CL): `out`.
- **D6, patch approvate:** P1, P2, P3 (anno del fascicolo: 2024), P5, P6 (si traccia ciò che è escluso), P7, P8, P9, P12. Non necessaria: P13. Da spiegare: P10. Da decidere insieme: P11.
- **Applicazione:** le patch sono in `literature/litterature_summary_v2.csv` (copia). Gli originali `.csv` e `.xlsx` non sono stati toccati, perché il `.xlsx` contiene commenti a thread che una conversione automatica perderebbe. Nel `.bib` è stato corretto solo `year` di Jiang-TDSC2024 (2022 -> 2024).
- **Priorità del lavoro:** non serve una visione completa del progetto. Ordinare lo stato dell'arte (in corso) e poi concentrarsi su come scrivere la sezione Continual Learning (piano in `docs/planning/cl_section_plan.md`).
- D7 (terminologia nei testi) e D3/D5 (tabella Target Task): rimandati.

## Aggiornamento (terzo giro)

- **P10 approvata e applicata** in `literature/litterature_summary_v2.csv`: `Persistence` ridotta a yes / no / NA, le sfumature spostate in `Notes` tra parentesi quadre.
- **P11:** per ora `Target Metric` resta com'e.
- **Abbasi-CVPR2024:** l'utente conferma che tratta CIL; la tabella resta com'e (la mia lettura "multi-head" non e stata seguita).
- **Piano sezione CL:** struttura 2.1-2.4 approvata; notazione della Sezione 3 approvata; nessun paragrafo dedicato a modelli preaddestrati e multimodali (il focus e il CL); online/task-free: al massimo una frase in 2.2 (default dell'assistente, modificabile).
- **Fonti di background:** scaricati in `literature/papers/background/` (preprint arXiv, non le versioni di rivista): Wang (arXiv 2302.00487 v3), De Lange (arXiv 1909.08383, versione d'autore TPAMI), Zhou (arXiv 2302.03648 v2). Non scaricato: van de Ven et al. (Nature Machine Intelligence 2022), il download automatico e stato rifiutato (403); DOI 10.1038/s42256-022-00568-3.
