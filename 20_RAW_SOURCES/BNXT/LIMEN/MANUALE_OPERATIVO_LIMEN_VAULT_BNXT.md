# 🏛️ LIMEN VAULT — Manuale Operativo (Edizione BNXT)

## 1. Introduzione all'Edizione BNXT
**LIMEN VAULT FOR BNXT EDITION** è l'infrastruttura di memoria aumentata e intelligenza artificiale locale progettata per consentire la consultazione istantanea, sicura e offline dell'intero corpus documentale BNXT e dei manuali applicativi CRM, PWA QC, DELPO e LIMEN.

Il Vault viene fornito pre-popolato e preindicizzato: all'avvio l'applicazione verifica l'integrità dei manifesti e riutilizza immediatamente gli indici lessicali e vettoriali precalcolati, senza richiedere lunghe elaborazioni al primo utilizzo.

---

## 2. Struttura del Vault BNXT
Il Vault adotta una struttura pulita, standardizzata e compatibile con Obsidian:

```text
Vault BNXT/
├── 00_SYSTEM/                      ← configurazione, catalogo e indici gestiti da LIMEN
│   ├── VAULT_CATALOG.json          ← catalogo generale dei documenti e hash SHA-256
│   └── VAULT_SEARCH_INDEX.json     ← indice lessicale BM25 e metadati di ricerca
├── 20_RAW_SOURCES/
│   └── BNXT/
│       ├── RAG/                    ← 3.018 documenti originali [INDICIZZATO]
│       ├── ALTRI_DOCUMENTI/        ← 1.913 documenti originali [CONSERVATO / INDICIZZATO]
│       ├── WIKI/                   ← 39 note strutturate della WIKI interna [INDICIZZATO]
│       ├── INDICE_WIKI.md          ← mappa e indice navigabile della WIKI
│       ├── CRM/                    ← manuale operativo e specifiche CRM consolidati
│       ├── QC/                     ← manuale operativo PWA QC read-only
│       ├── DELPO/                  ← manuale e architettura del sistema DELPO QC
│       └── LIMEN/                  ← documentazione ufficiale dell'edizione BNXT
└── 80_AI_OUTPUTS/                  ← cartella di lavoro per risposte AI ed elaborazioni
```

---

## 3. Modal di Preparazione Step-by-Step
Durante il primo avvio ordinario o in caso di caricamento dell'archivio dedicato, l'utente viene guidato da un popup interattivo a 5 passaggi:
1. **Verifica del pacchetto:** Controllo dell'archivio ZIP, verifica dell'integrità SHA-256 e scansione del manifesto.
2. **Installazione del Vault:** Decompressione ordinata con collocazione atomica direttamente in `20_RAW_SOURCES/BNXT/`.
3. **Verifica dei documenti:** Validazione dei 4.971 file Markdown UTF-8 e integrazione dei manuali applicativi.
4. **Verifica degli indici:** Convalida dell'indice BM25 e degli indici semantici (modello locale `bge-m3`).
5. **Apertura del Vault:** Transizione a "Vault pronto" e accesso immediato all'area di ricerca e chat.

L'interfaccia supporta la riduzione nella barra di stato in basso per consentire all'utente di continuare a lavorare, l'annullamento atomico con conservazione dei file validi e il retry mirato per eventuali singoli elementi falliti.

---

## 4. Motore di Ricerca Ibrido & Citazioni Verificabili
- **Ricerca Lessicale BM25:** Indicizzazione deterministica del testo con estrazione in streaming a memoria bounded.
- **Ricerca Semantica Vettoriale:** Modello di embeddings locale (`bge-m3`) configurato per consentire il recupero concettuale avanzato anche in assenza di parole chiave esatte.
- **Citazioni e Apertura Fonti:** Ogni risposta dell'AI è corredata dalle citazioni puntuali dei documenti sorgente; un clic sulla citazione apre immediatamente il file originale al punto esatto.
- **Distinzione Conserva vs Indicizza:** I documenti di testo e le specifiche vengono indicizzati per il RAG, mentre i materiali tecnici o di supporto rimangono conservati integri nel Vault senza appesantire l'indice.

---

## 5. Sicurezza e Riservatezza Locale
- **Nessuna chiamata a OpenAI o provider cloud non autorizzati:** L'intera pipeline di ricerca, indicizzazione e generazione opera in locale sul computer dell'utente.
- **Esclusione Assoluta di Password e Segreti:** Nessuna credenziale reale o token di produzione è presente nel pacchetto distribuibile o negli indici di ricerca.
- **Protezione File System:** Il backend Tauri impedisce scritture non autorizzate fuori dal Vault e protegge le directory di sistema (`00_SYSTEM`, `.git`).
