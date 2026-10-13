# Panoramica della letteratura censita

Fonte principale: `literature/litterature_summary.csv` (identico al `.xlsx` per i dati; il `.xlsx` aggiunge 7 commenti). Dove ho controllato un'informazione sul PDF lo indico con **[PDF]**; senza etichetta, l'informazione viene solo dalla tabella (non verificata). I limiti di lettura dei PDF sono in `project_inventory.md`. Le incongruenze sono in `initial_issues.md` e qui sono richiamate solo con il rimando.

## Come è fatta la tabella

34 righe dati. Colonne (le intestazioni di gruppo sono nella riga 1 del foglio):

- Anagrafica: File Name, Paper Name, Venue, Year, Quality, Notes.
- Gruppo "Generic Attack Framework": Type, Atk / Def, Violation, Target Task.
- Gruppo "CL specific": Persistence, Target Metric, CL methods / categories, CL scenario.
- Una colonna finale senza intestazione con lo stato di lettura (`ok`, `da leggere`, `da finire di leggere`, `probabilmente da togliere`).

Quanto è compilata: Type e Atk/Def 31 righe su 34; Violation 21; CL methods 17; CL scenario 16; Target Metric 14; Persistence 8; Target Task 5; stato di lettura 15. Le colonne CL-specifiche sono quindi compilate quasi solo per gli attacchi più recenti o già letti. Il significato della colonna `Quality` (Q1, A++, B, ...) non è definito in nessun file.

## Composizione

Per anno: 2020 (1), 2021 (1), 2022 (7), 2023 (4), 2024 (7), 2025 (7), 2026 (5), senza anno (2). Il campo è dunque concentrato dopo il 2022; il blocco 2026 è composto da lavori in uscita (ICML, CCS, CIKM, AAAI, arXiv).

Per `Type` (colonna G): poisoning 9, backdoor 7, evasion 6, "other (CL for robustness)" 3, "other (rob for CL)" 3, model inversion/poisoning/backdoor 1, differential privacy 1, "other (CL for backdoor unlearning)" 1, vuoto 3.

Per `Atk / Def`: attack 15, attack (BB) 3, defense 7, attack/defense 1, "-" 5, vuoto 3.

Per `Violation`: integrity 12, availability 5, integrity+availability 3, privacy 1, vuoto 13.

Per `CL scenario` (solo 16 righe compilate): CIL 10 (di cui uno "1 step dopo il pretraining"), DIL 2, TIL 2, DIL+TIL 1, DIL+CIL 1.

Aree dove la tabella è sbilanciata:

- Attacchi al training con violazione di integrità: soprattutto backdoor. Con violazione di disponibilità: avvelenamento che induce forgetting.
- Difese: 7 righe, quasi tutte su evasion (adversarial training in CL). Una sola difesa contro backdoor (Umer-IEEEAccess2024), una sola per la privacy (Hassanpour, vedi sotto).
- Nessun lavoro censito su sponge, membership inference, model extraction, né su difese di sanitizzazione dei dati.

## Gruppi di lavori

### A. Attacchi di backdoor (training time, integrità)

| Chiave | Sede | Idea (dalla tabella) | Scenario | Metodi CL | Persistenza | Metrica |
|---|---|---|---|---|---|---|
| Jiang-TDSC2024 | IEEE TDSC | Backdoor impiantata in un modello pretrained con logit bassi; i nuovi task tendono ad assegnare le etichette future perché i vecchi logit sono bassi. Evita rilevatori pre-deployment | CIL, un solo passo dopo il pretraining | replay (EEIL) **[PDF: EEIL confermato]** | NA | untargeted SR |
| Gao-NeuralNetworks2025 | Neural Networks | Backdoor su pochi task, poi addestramento pulito spingendo l'etichetta bersaglio verso un'ancora | TIL **[PDF: "task-incremental setting"]** | SI, EWC, MAS, iCaRL | sì (con rinnovo dell'attacco dopo task puliti) | targeted SR |
| Guo-USENIX2025 | USENIX Sec. | Ancora il trigger su parametri stabili tra i task | TIL (secondo la tabella e secondo Huang-ICML2026 **[PDF di Huang: "LTB ... for Task-Incremental Learning"]**) | EWC, SI, LwF, DGR, A-GEM | sì | targeted SR |
| Nguyen-AAAI2025 | AAAI | Backdoor su CL basato su prompt, sfrutta il prompt pool | CIL | architetture prompt-based | sì | targeted SR |
| Luo-CIKM2026 | CIKM | Backdoor di fairness persistente per MLLM (PFBA) | DIL **[PDF: tre fasi "domain-incremental"]** | O-LoRA, SEFE, Replay | sì, tutti i task futuri | Accuracy |
| Huang-ICML2026 | ICML | PBTO: backdoor persistente e targeted, ancorata a sottospazi strutturali invarianti; supera il limite di LTB (testato solo in TIL) | CIL | iCaRL | sì, tutte le esperienze CIL future | ASR |
| Pham-ECCV2024 | ECCV | Metodo del solo attaccante (apprendimento sequenziale) per rendere la backdoor resistente al fine-tuning | non applicabile | - | - | - |

Nota per Nguyen: la tabella segna "attack (BB)" e c'è un commento "check". **[PDF]** Nguyen dichiara un modello black-box (nessun accesso al modello o alla procedura), clean-label e con più fornitori di dati: la classificazione BB è coerente con il paper.

Pham-ECCV2024: la riga lo classifica come "other (CL for robustness)" ma anche come "attack/integrity"; le note dicono che non c'è vero CL (due soli passaggi: iniezione della backdoor e fine-tuning). **[PDF: confermato, il paper parla di "sequential backdoor learning" contro le difese basate su fine-tuning]**.

### B. Avvelenamento che induce forgetting (training time, disponibilità o integrità)

| Chiave | Sede | Idea | Scenario | Metodi CL | Metrica |
|---|---|---|---|---|---|
| Umer-IJCNN2020 | IJCNN | Backdoor/avvelenamento per forgetting mirato e "false memory" | DIL, CIL (solo MNIST) **[PDF: MNIST permuted e split]** | solo EWC | - |
| Umer-IJCNN2021 | IJCNN | Stessa idea su altri metodi CL (regolarizzazione e generativi) | non compilato | non compilato | - |
| Li-IJCNN2022 | IJCNN | Avvelenamento clean-label mirato a un task specifico | DIL, TIL **[PDF: "domain and task incremental"]** | EWC, SI | forgetting |
| Han-ICIP2023 | ICIP | Avvelenamento dei metodi a regolarizzazione per far dimenticare i task passati mantenendo il corrente (versione di conferenza di Han-NeurIPS(WS)2022) | non compilato | non compilato | - |
| Kang-ICML2023 | ICML | Avvelena il generatore del generative replay | CIL | DGR | forgetting (tutti i task precedenti) |
| Abbasi-CVPR2024 | CVPR | BrainWash: forgetting massimo sui task passati, performance corrente buona; model inversion e ottimizzazione bilevel | CIL | EWC, MAS, RWalk, AFEC, ANCL, ER, ER-ACE | forgetting (tutti i precedenti) |
| Du-IJCNN2023 | IJCNN | Deep inversion come passo generale degli attacchi di avvelenamento al CL | DIL (commento: solo rotazioni MNIST) | solo EWC | forgetting mirato, targeted SR |
| Yu-AAAI2024 | AAAI | Attacco BB a segmentazione semantica continua tramite perturbazione dei campioni incrementali | CIL | distillazione per segmentazione | forgetting |
| Perla-WACV2025 | WACV | Attacco BB alla replay memory (esemplari) in CIL; DER e MEMO vulnerabili, FOSTER e WA più robusti | CIL | replay (+ distillazione/architettura) | forgetting |

Han-NeurIPS(WS)2022 è nel repository ma non ha una riga propria: il suo titolo **[PDF]** è "Training Time Adversarial Attack Aiming the Vulnerability of Continual Learning" ed è citato solo nella nota di Han-ICIP2023.

### C. Attacchi di evasione

- Liu-AAAI2026 (AAAI): SAE, esempi avversari "sostenibili" attraverso gli aggiornamenti CIL; ASR e SASR; metodi Replay, MEMO, DER, Foster, WA, BiC, iCaRL, PODNet. **[PDF: titolo e concetto di sustainability confermati]**.
- Bunzel-IEEETrustCom2025 (IEEE TrustCom): FGSM e PGD in CIL, la trasferibilità temporale degli esempi persiste; adversarial training aiuta ma degrada l'accuratezza pulita. **[PDF: CIFAR-10, FGSM, PGD, CIL, AT adattato al CL, trade-off confermati]**. Nella tabella è marcato "attack/defense" e valutato solo con iCaRL.

### D. Difese

| Chiave | Sede | Contenuto | Contro cosa |
|---|---|---|---|
| Khan-IJCNN2022 | IJCNN | Robustezza avversaria in CL | evasion |
| Chen-TMLR2022 | TMLR | Dati non etichettati interrogati vicino ai pochi esemplari passati + distillazione; "prima versione con AT" | evasion |
| Bai-ICASSP2023 | ICASSP | Robustezza avversaria in CL | evasion |
| Mi-ICCV2025 | ICCV | Rivela due problemi che nascono dalla combinazione: AT accelera il forgetting, il CL causa gradient obfuscation; propone un rimedio | evasion |
| Umer-IEEEAccess2024 | IEEE Access | "Adversary Aware CL": campioni di difesa con un pattern percettibile simile al trigger dell'attaccante. **[PDF: confermato, autori Umer e Polikar]** | backdoor |
| Hassanpour-IEEEAccess2022 | IEEE Access | "Differential Privacy Preservation in Robust CL": integra DP nel CL (varianti di A-GEM) e valuta anche la robustezza a evasion. **[PDF: titolo e autore Hassanpour confermati]** | privacy (e evasion) |
| Zhu-CCS2026 | ACM CCS | Riformula backdoor e unlearning come processo sequenziale in ottica CL; propone BI-BAU, valido per unlearning completo di backdoor. **[PDF: abstract confermato]** | backdoor (non è una difesa per sistemi CL) |

Punti di attenzione su queste righe (dettagli in `initial_issues.md`): la riga della difesa DP porta il nome file `Umer-IEEEAccess2024`, già usato dalla difesa backdoor; per Khan, Mi, Zhou non ho potuto leggere il PDF.

### E. Lavori che usano robustezza e CL come strumenti reciproci ("other")

- "CL for robustness" (usare il CL per aggiornare difese o attacchi): Chou-CW2022 (CL + AT, ancora "TODO" nelle note), Zhou-CVPR2024 (difesa continua con pseudo-replay anisotropo e isotropo; la nota la giudica "useless"), Pham-ECCV2024 (vedi sopra).
- "Robustness for CL" (tecniche avversarie per migliorare il CL, non per proteggerlo): Kumari-NeurIPS2022 (RAR, campioni avversari vicino al confine di forgetting **[PDF: abstract confermato]**), Wang-ICML2022 (memoria task-free distributionally robust), Yu-AAAI2025 (AAKR, usa attacchi avversari per trattenere conoscenza in segmentazione continua).

### F. Righe senza PDF o fuori scopo

- "Federated Continual Learning against Data Posioning" (riga 29): nessun nome file, nessun metadato; commento "TO CHECK". Il titolo contiene un refuso ("Posioning").
- "Qualcosa di Federated Persistent backdoor" (riga 30): segnaposto, non un titolo.
- "Catastrophic Learning: A New Attack Vector on Continual Learning Networks" (arXiv 2026, riga 36): attacco alla plasticità ("learning blockers"), poisoning, integrità, metrica FWT, iCaRL/DER/ER-ACE, CIL; stato "probabilmente da togliere". Nessun PDF nel repository. La nota segnala che assume troppa conoscenza dei dati passati e futuri e controllo sulle etichette.
- Huang-USENIX2024 (UBA-Inf): riga con quasi tutti i campi vuoti e stato "da leggere"; la nota dice "might be unrelated". **[PDF, prima pagina]** UBA-Inf è un attacco backdoor attivato dall'unlearning in MLaaS: la prima pagina non parla di CL. Una lettura successiva mostra che il threat model include modelli aggiornati in continuazione (CT-MLaaS) con richieste di unlearning: non è un attacco ai metodi CL, ma non è nemmeno del tutto estraneo. Classificazione proposta: borderline (`docs/planning/decisions.md`, D4).
- Liu-AAAI2024 (due copie identiche, `attacks/` e `others/`): **[PDF]** "Beyond Traditional Threats: A Persistent Backdoor Attack on Federated Learning" (FCBA). È federated learning, non CL, e non ha una riga nella tabella.
- Liu-CoLLA2022 (`others/`): **[PDF]** "Continual Learning and Private Unlearning" (CLPU-DER++). Riguarda unlearning privato in CL, non ha riga nella tabella.

## Temi trasversali ricavabili dalla tabella e dagli appunti

Questi sono osservazioni di sintesi, non conclusioni verificate:

- La persistenza (l'effetto sopravvive agli aggiornamenti successivi) è la dimensione più ricorrente nelle backdoor CL recenti (Gao, Guo, Nguyen, Luo, Huang-ICML2026, Liu-AAAI2026 per l'evasione).
- Quasi tutti gli attacchi di avvelenamento "contro il passato" mantengono alta la performance corrente per restare nascosti (Kang, Abbasi, Yu-AAAI2024, Perla, Han): le note in `extra/Notes.docx` sottolineano che questo rende ambigua la distinzione targeted/untargeted.
- La replay memory e il generatore sono superfici di attacco specifiche del CL (Perla; Kang).
- Il model inversion è presentato come passo abilitante per avvelenare i task passati (Du; Abbasi): è un'affermazione dei lavori e degli appunti, e la tassonomia 2 la sfrutta mettendo "Task Model Inversion" nei test-time privacy.
- L'interazione tra adversarial training e CL non è banale (Mi: forgetting accelerato e gradient obfuscation; Bunzel: costo in accuratezza pulita).
