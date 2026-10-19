# 🛡️ LIMEN VAULT — Modello di Sicurezza Locale

## 1. Principi di Difesa
1. **Sandboxing e Confinamento Path Traversal:** Rifiuto sistematico di percorsi con componenti `..`, drive assoluti o backslash malformati.
2. **Protezione Link Simbolici:** Ispezione ricorsiva di tutti i componenti del percorso per impedire scritture attraverso symlink diretti a directory esterne.
3. **Pubblicazione Atomica Staging:** I file decompressi transitano in directory isolate di staging temporanee e vengono pubblicati mediante hardlink con directory anchor.
4. **Isolamento delle Directory di Sistema:** Le cartelle `00_SYSTEM`, `80_AI_OUTPUTS` e i file `.git` sono rigorosamente protetti da sovrascritture esterne.
