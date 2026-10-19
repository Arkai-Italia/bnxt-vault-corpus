# 📐 LIMEN VAULT — Specifica Architetturale del Vault (v0.8.0)

## 1. Dimensionamento e Limiti Operativi
- **Capacità Catalogo (`VAULT_CATALOG.json`):** Supporto esteso fino a 1 GiB di catalogo JSON strutturato.
- **I/O Streaming Bounded:** Buffer di lettura/scrittura a memoria controllata (128 KB per blocco di trasferimento), prevenendo picchi incontrollati di RAM.
- **Deduplicazione Atomica:** I file identici all'interno dell'archivio vengono deduplicati tramite fingerprint SHA-256 senza spreco di spazio su disco.

## 2. Compatibilità Obsidian
- Tutti i documenti in `20_RAW_SOURCES/` sono file standard Markdown UTF-8 compatibili con Obsidian.
- I collegamenti relativi `[[Nome_Nota]]` e `` sono preservati per la navigazione bidirezionale.
