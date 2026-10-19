# 📊 BNXT CRM — Specifica del Modello Dati

## 1. Schema Relazionale Principale
Il database è strutturato su PostgreSQL (Supabase) con politiche Row-Level Security (RLS) differenziate per ruolo:

- **`organizations`**: Clienti business, team di gara o rivenditori.
- **`contacts`**: Anagrafica dei singoli piloti, manager di team o referenti.
- **`leads`**: Canale di ingresso richieste da web, WhatsApp, fiere o inserimento manuale.
- **`design_projects`**: Progetti grafici collegati al pilota, contenenti bozze, loghi e varianti.
- **`quotes` / `orders`**: Preventivi emessi e ordini confermati con relative scadenze di produzione.
- **`order_items`**: Dettaglio articoli (es. tuta racing, guanti, sottotuta), taglie pilota, personalizzazioni e riferimenti grafici.
- **`factories`**: Stabilimenti produttivi (es. Indonesia, Cina) con associazione per operatore.

## 2. Politiche di Sicurezza e Riservatezza Economica
- Le colonne monetarie (`unit_price`, `line_total`, `tax_rate`, `discount_amount`) sono protette da viste sicure e policy RLS.
- Gli operatori con ruolo `factory_operator` e `read_only` non possono interrogare campi monetari né visualizzare i dati anagrafici completi dei clienti privati.
