# Proposta di patch alla tabella della letteratura

Stato: PROPOSTA, in attesa di approvazione. Né `literature/litterature_summary.csv` né il `.xlsx` sono stati modificati. Per approvare, indica gli identificatori (P1, P2, ...). Il `.xlsx` contiene commenti a thread che un'applicazione automatica potrebbe perdere: propongo di applicare le patch a una copia e di confrontarle con te prima di sostituire l'originale.

Le verifiche citate (**[PDF]**) sono state fatte con `pdftotext` sui file in `literature/papers/`.

## Correzioni di anagrafica

| ID | Riga (Excel) | Campo | Ora | Proposta | Evidenza |
|---|---|---|---|---|---|
| P1 | 6 | File Name | `Umer-IEEEAccess2024` | `Hassanpour-IEEEAccess2022` | **[PDF]** intestazione "A. Hassanpour et al.: Differential Privacy Preservation in Robust Continual Learning"; la voce `.bib` ha già questa chiave |
| P2 | 35 | File Name | `Bunzel-IEEETrusttCom2025` | `Bunzel-IEEETrustCom2025` | nome del PDF |
| P3 | 3 | Year / File Name | 2024 | **da decidere**: 2024 (fascicolo) o 2022 (online) | **[PDF]** ricevuto 9/1/2022, pubblicato online 31/8/2022, vol. 21 n. 2 marzo/aprile 2024; il `.bib` dice 2022. Tabella e `.bib` vanno resi coerenti, qualunque anno si scelga |
| P4 | 6 | Year | 2022 | invariato | coerente con Hassanpour; resta solo da fare P1 |

## Righe da aggiungere

| ID | Paper | Proposta |
|---|---|---|
| P5 | Han-NeurIPS(WS)2022 "Training Time Adversarial Attack Aiming the Vulnerability of Continual Learning" | Nuova riga: poisoning, attack, availability, regolarizzazione; versione estesa di Han-ICIP2023 secondo la nota di quest'ultimo |
| P6 | Liu-AAAI2024 (FL), Liu-CoLLA2022 (unlearning), Huang-USENIX2024 | Se si vuole tracciare ciò che è escluso: righe con inclusione "out" o "da decidere" e motivo. Alternativa: elenco in `docs/` senza toccare la tabella |

## Colonne e normalizzazione

| ID | Proposta |
|---|---|
| P7 | Nuova colonna `Inclusion` (`in`, `out`, `borderline`) e `Inclusion note`, secondo D4 in `docs/planning/decisions.md` |
| P8 | `Atk / Def`: separare il ruolo (attack, defense, both, none) dalla conoscenza dell'attaccante (WB, BB). Oggi "attack (BB)" mescola le due cose |
| P9 | `Violation`: valori in un formato solo (`integrity`, `availability`, `confidentiality`, separati da `; `). Questo recepisce anche D2 (oggi la riga di Hassanpour dice "privacy") |
| P10 | `Persistence`: valori `yes`, `no`, `NA`; le sfumature ("they cheat a bit", "could work with multiple steps") in `Notes` |
| P11 | `Target Metric`: lista chiusa; il commento sulla cella L2 suggerisce forgetting, performance corrente, forward transfer, ma le righe usano ASR, SASR, FWT, targeted SR. Da decidere quale insieme |
| P12 | Nuove colonne `Sample specificity` e `Error specificity` (D5). La dimensione `Target Task` esiste già come colonna J e dovrebbe diventare la tabella separata di D3 con i valori past / current / future; per ora solo 5 righe sono compilate |
| P13 | `Quality`: definire in una riga di legenda cosa significano Q1 e A++/A/A-/B, oppure usare due colonne separate (quartile di rivista, ranking di conferenza) |

## Esito delle verifiche che toccano la tabella

Controlli fatti sul testo dei PDF ora leggibili. Le correzioni proposte sono nella colonna "Proposta".

| Riga | Paper | Campo | Valore in tabella | Esito [PDF] | Proposta |
|---|---|---|---|---|---|
| 12 | Guo-USENIX2025 | scenario | TIL | Il testo non usa i termini di scenario; esperimenti su Split CIFAR-10 con LwF; Huang-ICML2026 lo definisce TIL | tenere TIL, annotare "da Huang-ICML2026" |
| 14 | Du-IJCNN2023 | scenario / metodi | DIL; solo EWC | Esperimenti su Rotation MNIST con EWC; l'abstract dichiara anche scenari task e class incremental | tenere DIL, annotare che l'abstract cita anche CIL; il commento N14 è confermato |
| 24 | Abbasi-CVPR2024 | scenario | CIL | Benchmark 10-Split CIFAR-100 e 10-Split miniImageNet; il testo non usa "class-incremental" | scenario da confermare leggendo il paper per esteso |
| 25 | Yu-AAAI2024 | ruolo | attack (BB) | Il termine "black-box" non compare nel testo estratto; la nota in tabella descrive impostazioni (dati dello step noti, mappa di predizione del vecchio modello nota, struttura del modello ignota) | tenere BB, verificare |
| 22 | Huang-USENIX2024 | nota "might be unrelated" | vuoto | Il paper riguarda unlearning in MLaaS con modello aggiornato in continuazione, non CL contro il forgetting | aggiornare la nota e marcare come borderline (D4) |
| 26 | Perla-WACV2025 | ruolo | attack (BB) | Confermato: il modello bersaglio dà solo hard label | nessuna |
| 27 | Mi-ICCV2025 | scenario | vuoto | Contiene sia Task-IL sia Class-IL; forgetting accelerato e gradient obfuscation confermati | compilare "TIL, CIL" |
| 7 | Khan-IJCNN2022 | scenario | vuoto | Cita TIL e DIL | compilare dopo lettura più estesa |
| 8 | Chen-TMLR2022 | scenario | vuoto | Titolo e abstract: class-incremental con dati non etichettati interrogati | compilare "CIL" |
| 9 | Chou-CW2022 | contenuto | TODO | Il testo estratto è codificato male; l'abstract letto con un estrattore diverso conferma CL + adversarial training con crescita e potatura del modello | resta fuori ambito (D4) |
| 4 | Umer-IEEEAccess2024 (AACL) | scenario | vuoto | Dichiara TIL, DIL e CIL e metodi replay (generativi ed esatti) | compilare scenari |

Le altre affermazioni controllate in `docs/analysis/initial_issues.md` restano confermate (Nguyen BB, Gao TIL, Luo DIL, Li DIL+TIL, Jiang EEIL, Bunzel, Pham, Hassanpour, Liu-AAAI2024 FL).

## Stato dopo l'approvazione

Approvate e applicate in `literature/litterature_summary_v2.csv` (copia, originali intatti): P1, P2, P3 (2024), P5, P6, P7, P8, P9, P12 (colonne create vuote). Non applicate: P4 (invariata), P10 (da spiegare), P11 (da decidere), P13 (non necessaria).

Nuova verifica: **Abbasi-CVPR2024**. Il paper descrive il CL come approcci "multi-head" con una testa per task (figura del threat model con Head 1 ... Head t-1), quindi lo scenario mi sembrava task-incremental. L'utente ha confermato che il paper tratta CIL: la tabella resta com'e. Yu-AAAI2024: il threat model lascia ignoti struttura e parametri del modello, coerente con "attack (BB)". Chou-CW2022: abstract confermato (CL + adversarial training con progressive neural network), fuori ambito. Guo-USENIX2025: nessuna menzione esplicita di TIL nel testo; resta l'indicazione indiretta di Huang-ICML2026.

P10 approvata e applicata nella copia v2. P11 lasciata com'e per ora.
