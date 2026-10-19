# 🏗️ DELPO QC — Architettura e Dipendenze Tecniche

## 1. Componenti del Sistema
- **Frontend Applicativo:** Next.js con Tailwind CSS e supporto PWA offline-first per ispettori sul campo.
- **Backend & Storage:** PostgreSQL Supabase con Row-Level Security, bucket protetti per archiviazione foto ad alta risoluzione e trigger automatici per calcolo metriche.
- **Motore Statistico AQL:** Modulo algoritmico integrato per il calcolo dei piani di campionamento secondo tabelle normalizzate.

## 2. Privacy e Connettività
- Ecosistema informativo chiuso senza dipendenze da provider di invio mail di terze parti.
- Caching locale delle checklist durante l'ispezione su linea di montaggio con sincronizzazione al ripristino della connettività.
