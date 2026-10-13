# Inventario del progetto

Data dell'analisi: 2026-10-12. Questo documento descrive solo cosa c'è e dove. Il contenuto della letteratura è in `literature_overview.md`, il confronto tra tassonomie in `taxonomy_comparison.md`, i problemi aperti in `initial_issues.md`.

## Metodo e limiti di lettura

- Nella prima passata nessun lettore PDF era disponibile; ho estratto il testo con uno script Python locale, con risultati parziali. In seguito (decisione D8 in `docs/planning/decisions.md`) è stato installato `poppler`: tutti i 35 PDF producono ora testo con `pdftotext`.
- La seconda passata ha verificato sul testo la maggior parte dei paper sospesi (esiti in `docs/verification/table_patch_proposal.md`). Restano non verificati a fondo: Chou-CW2022 (codifica dei font alterata anche con `pdftotext`), lo scenario di Abbasi-CVPR2024, il ruolo "black-box" di Yu-AAAI2024, e per tutti i paper la lettura è stata mirata (abstract, scenario, metodi, parole chiave), non integrale.
- Le affermazioni di questo documento, di `literature_overview.md` e `taxonomy_comparison.md` marcate **[PDF]** sono state controllate sul testo originale; le altre vengono solo dai file di progetto.

## Struttura

```
CL-Sec-Survey/
  CLAUDE.md
  .gitignore
  literature/
    litterature_summary.csv   (34 righe dati)
    litterature_summary.xlsx  (stesso contenuto + 7 commenti)
    papers/attacks/   22 PDF
    papers/defenses/   8 PDF
    papers/others/     5 PDF
  taxonomy/
    CLSec Taxonomy.xlsx       (tassonomia 1, tabella)
    CLSec-Taxonomy2.pptx      (tassonomia 2, una slide)
  extra/
    Notes.docx                (appunti di survey + bozza LaTeX + 2 commenti)
    Future Work Brainstorming.xlsx (griglia della tassonomia 1 con paper e idee, 3 commenti)
  CL_Security_Survey/         (copia locale Overleaf)
    main.tex                  (470 righe)
    bibliography.bib          (30 voci)
    images/CLSec Slides.pdf   (figura della tassonomia, 1 pagina)
    elsarticle.dtx, .ins, elsarticle-harv.bst (template Elsevier)
  docs/analysis|planning|verification|writing   (vuote, tranne questi file)
```

Stato git: ramo `main`, un solo commit (`01c0a04`), albero pulito prima di questa analisi.

## Materiali

### literature/

| File | Contenuto | Note |
|---|---|---|
| `litterature_summary.csv` / `.xlsx` | Tabella riassuntiva. 34 righe dati (31 con nome file, 3 senza). Il nome del file contiene un errore di ortografia ("litterature") | Il `.xlsx` ha una tabella Excel di nome `Poisoning` (A2:N36) e 7 commenti a thread, assenti nel `.csv` |
| `papers/attacks/` | 22 PDF | Include 2 file che non sono attacchi al CL (Liu-AAAI2024, Huang-USENIX2024) |
| `papers/defenses/` | 8 PDF | Include 2 lavori che la tabella non classifica come difese (Wang-ICML2022, Zhou-CVPR2024) |
| `papers/others/` | 5 PDF | Include Zhu-CCS2026 che la tabella classifica come difesa |

La divisione in cartelle `attacks/defenses/others` non coincide con la colonna `Atk / Def` della tabella (dettagli in `initial_issues.md`).

### taxonomy/

- `CLSec Taxonomy.xlsx`: griglia 2x3. Righe: dato di test / dato di training, ciascuna divisa in Attacks e Defenses. Colonne: Integrity, Availability, Confidentiality. Una nota (cella G2) dice che le difese in rosso sono "mancanti" (nessun approccio ad hoc per il CL); i colori non sono recuperabili dal mio estrattore, quindi non so quali celle siano rosse (le celle marcate "NA" sono verosimilmente quelle).
- `CLSec-Taxonomy2.pptx`: una slide (esportata da Google Slides). Righe: Training Time / Test Time. Colonne attacchi: Integrity, Availability, Privacy. Colonne difese: Data, Model. Il file ha una slide di note vuote.

### extra/

- `Notes.docx`: appunti in italiano e inglese. Sezioni: "Instructions" (indicazioni su come impostare la survey), "Discarded papers" (intestazione senza elenco), "Survey notes" (Poisoning, Evasion, Privacy, Others), "Other Notes" (testo LaTeX commentato sul modello di minaccia temporale e sul task-aware threat model), "Wild Patterns Notes" (riassunto del framework di Biggio e Roli e di Cinà et al. applicato al CL, con voci marcate NEW e TODO). Due commenti: "Da capire interconnessione con attack specificity" e "Lasciamo targeted".
- `Future Work Brainstorming.xlsx`: stessa griglia della tassonomia 1, riempita con i paper per cella e con idee di lavoro futuro. Contiene tre commenti con istruzioni di editing (vedi `initial_issues.md`).

### CL_Security_Survey/ (manoscritto)

Template Elsevier `elsarticle`, opzione `5p`, rivista target "Pattern Recognition", autori in 3 affiliazioni (Cagliari, Roma Sapienza, Pisa). Stato delle sezioni in `main.tex`:

| Sezione | Scadenza indicata nel titolo | Stato |
|---|---|---|
| Abstract | - | Solo punti elenco e commenti di pianificazione |
| 1 Introduction | fine dicembre | Solo "TODO" in rosso e due contributi in elenco |
| 2 Continual Learning | fine ottobre | Solo scaletta (scenari, metodi, metriche) |
| 3 Threat Model | fine novembre | Sottosezioni su goal, knowledge, capability; tabella `tab:security_cl`; figura; "CL Attack Surfaces", "CL security issues"; sottosezione "Task-aware threat model" con formalizzazione |
| 4 Attacks against CL | fine novembre | Solo parte test-time (evasion, backdoor activation, privacy); la parte training-time è commentata |
| 5 CL Defenses | - | Solo intestazioni vuote |
| 6 Related work | fine dicembre | Due paragrafi sulle sinergie CL / robustezza |
| 7 Concluding remarks | fine dicembre | Vuota |

Altre osservazioni sul manoscritto:

- Le macro `\daniele`, `\battista`, `\maura`, `\lea` servono a note tra autori; `\yes` e `\no` usano `\ding` ma `pifont` non è caricato (verosimile errore di compilazione se le macro venissero usate; oggi non lo sono, e `Green`/`BrickRed` richiedono anche l'opzione `dvipsnames` di `xcolor`, non presente).
- `bibliography.bib` ha 30 voci: 25 sono i paper della tabella, 2 sono di concetto ("Wild patterns" di Biggio e Roli 2018; Cinà et al. 2023 "Wild patterns reloaded"), 3 sono survey sul CL (Wang 2024 TPAMI, De Lange 2021, Zhou 2024 TPAMI). Le 5 voci non-paper-della-tabella non sono citate in `main.tex` (la sezione 2 è vuota).
- Tutti e 25 i paper del `.bib` compaiono in qualche `\cite` di `main.tex`, ma 9 solo in righe commentate (Umer-IEEEAccess2024, Umer-IJCNN2020, Umer-IJCNN2021, Li-IJCNN2022, Han-ICIP2023, Kang-ICML2023, Abbasi-CVPR2024, Perla-WACV2025, Yu-AAAI2024: sono il blocco "Training-time CL Security" commentato). Paper con PDF ma senza voce `.bib`: Bunzel-IEEETrustCom2025, Han-NeurIPS(WS)2022, Huang-USENIX2024, Liu-AAAI2024, Liu-CoLLA2022, Zhu-CCS2026, Luo-CIKM2026, Liu-AAAI2026, Huang-ICML2026.
- `images/CLSec Slides.pdf` è la figura `fig:temporal_plots_tesseract`. Il suo testo non è recuperabile dai miei strumenti; presumo (non verificato) che sia l'esportazione del `.pptx` della tassonomia 2, perché il produttore è Google Slides.
- Il manoscritto contiene una terza versione della tassonomia (la tabella LaTeX), diversa dalle due in `taxonomy/`: vedi `taxonomy_comparison.md`.

## PDF: elenco completo per cartella

attacks (22): Abbasi-CVPR2024, Bunzel-IEEETrustCom2025, Du-IJCNN2023, Gao-NeuralNetworks2025, Guo-USENIX2025, Han-ICIP2023, Han-NeurIPS(WS)2022, Huang-ICML2026, Huang-USENIX2024, Jiang-TDSC2024, Kang-ICML2023, Li-IJCNN2022, Liu-AAAI2024, Liu-AAAI2026, Luo-CIKM2026, Nguyen-AAAI2025, Perla-WACV2025, Pham-ECCV2024, Umer-IJCNN2020, Umer-IJCNN2021, Yu-AAAI2024, Yu-AAAI2025.

defenses (8): Bai-ICASSP2023, Chen-TMLR2022, Hassanpour-IEEEAccess2022, Khan-IJCNN2022, Mi-ICCV2025, Umer-IEEEAccess2024, Wang-ICML2022, Zhou-CVPR2024.

others (5): Chou-CW2022, Kumari-NeurIPS2022, Liu-AAAI2024 (copia identica di quella in `attacks/`, stesso hash MD5), Liu-CoLLA2022, Zhu-CCS2026.

Totale: 35 file, 34 contenuti distinti.
