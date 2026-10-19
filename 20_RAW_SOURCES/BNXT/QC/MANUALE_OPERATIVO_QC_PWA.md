# 🔍 BNXT PWA QC — Manuale Operativo (Edizione BNXT)

## 1. Scopo e Architettura dell'Applicazione
La **PWA QC BNXT** è una Progressive Web App standalone mobile-first progettata specificamente per gli ispettori e gli operatori esteri presso gli stabilimenti produttivi partner.

### Caratteristiche Fondamentali:
- **100% Sola Lettura:** L'applicazione è puramente consultativa. Non effettua scritture, aggiornamenti o cancellazioni su database, garantendo la totale integrità dei dati operativi.
- **Oscuramento Rigoroso dei Prezzi:** Le colonne e i valori economici (`unit_price`, `line_total`, valori preventivo) sono completamente rimossi sia nelle query SQL/RLS che nelle interfacce utente.
- **Design Dark Motorsport:** Palette ad alto contrasto ottimizzata per ambienti di fabbrica (sfondo nero opaco `#0D0E10`, card grafite `#1A1C20`, indicatori giallo fluo `#E2F150`).

---

## 2. Consultazione Commesse e Ordini (PO)
Dalla schermata principale della PWA:
1. **Ricerca Immediata:** Barra di ricerca per numero d'ordine (PO number), pilota o brand.
2. **Scanner Fotocamera / Barcode:** Funzionalità integrata per scansionare il codice a barre o QR code apposto sulla scheda tecnica del capo in produzione.
3. **Filtro Fabbrica:** L'operatore visualizza unicamente le commesse afferenti al proprio stabilimento assegnato.

---

## 3. Dettaglio Ordine e Scheda Controllo Qualità
Accedendo alla vista di dettaglio della commessa (`/po/[id]`):
- **Avanzamento di Produzione:** Stepper grafico che evidenzia lo stato della lavorazione.
- **Checklist di Conformità QC:**
  - `qc_compliance_graphic`: Verifica fedeltà dei colori e posizionamento loghi sponsor rispetto al bozzetto approvato.
  - `qc_manufacturing`: Verifica cuciture sartoriali, cerniere, protezioni e assemblaggio.
  - `qc_size`: Controllo corrispondenza tabelle misure corporee del pilota.
  - `qc_pictures_uploaded`: Conferma avvenuto caricamento delle foto del capo finito.
  - `qc_outcome`: Esito aggregato (Approvato, Rifiutato, Da Rivedere).
- **Elenco Articoli:** Dettaglio taglie, numero di pezzi e personalizzazioni richieste per ciascun articolo.

---

## 4. Installazione come PWA e Funzionamento Offline
- **Installazione:** Dalla barra del browser (Chrome o Safari iOS) selezionare "Aggiungi a schermata Home" per eseguire l'applicazione a schermo intero come app nativa.
- **Service Worker:** Il file `public/sw.js` memorizza nella cache locale dell'operatore le viste statiche e le commesse consultate di recente, consentendone la consultazione anche in presenza di connettività instabile o assente in fabbrica.

---

## 5. Distinzione rispetto a DELPO
La **PWA QC BNXT** è un'interfaccia leggera e focalizzata per la visualizzazione immediata delle commesse sartoriali BNXT.
Il sistema **DELPO QC** è invece la piattaforma completa di audit, checklist configurabili AQL e gestione non-conformità CAPA.
