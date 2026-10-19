---
title: "Enduro Tech — Ricerca, prototipi, studi e dataset"
language: it
compiled_on: "2026-10-18"
category: "06"
records: 14
complete_market_census: false
---

# Ricerca, prototipi, studi e dataset

> Ricerca compilata il **18 ottobre 2026**. Enduro motociclistico; prodotti adventure, stradali e MX sono marcati quando affini. Verifica documentale tramite fonti web, non test di laboratorio o prove di utilizzo. Le caratteristiche sono attribuite alle fonti, non certificate da questo report. Disponibilità e prestazioni locali non dedotte dalla sola presenza di una pagina.

## TECH-092 — Honda CR ELECTRIC — relazione tecnica 2026

- **Stato documentato:** RICERCA_TECNICA_PUBBLICATA.
- **Pertinenza:** Motocross elettrico; trasferimento potenziale enduro.
- **Tipo di novità:** Pubblicazione tecnica recente su prototipo.
- **Data annuncio/pubblicazione:** 2026-03-01.

**Fatti riportati dalle fonti:** Honda R&D Technical Review 2026 pubblica il lavoro di Kato e colleghi, DOI 10.69239/hondatechnicalreview.2026_38_3. Abstract: launch control, gestione energia, batteria a L, tensione fino a 400 V, revisione raffreddamento e rigidità telaio. È disponibile un PDF tecnico, ma questa sintesi usa abstract e metadati recuperati.

**Lettura editoriale / possibile utilità:** Interesse: documentazione ingegneristica più solida di una sola presentazione commerciale.

**Limiti e informazioni non verificate:** Non prova arrivo commerciale di una CRF enduro elettrica. Prestazioni di gara e autonomia da motocross non trasferibili a escursioni enduro.

**AI effettivamente documentata:** Non verificata / non dichiarata nelle evidenze raccolte.

**Fonti:**

  - [Honda Technical Review 2026](https://www.jstage.jst.go.jp/article/hondatechnicalreview/38/0/38_2026_38_3/_article/-char/en) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-093 — Classificazione fondo con deep learning su moto elettrica

- **Stato documentato:** PAPER_2025.
- **Pertinenza:** Visione moto; studio su ghiaia.
- **Tipo di novità:** Ricerca pubblicata; prodotto enduro ND.
- **Data annuncio/pubblicazione:** 2025-02-14.

**Fatti riportati dalle fonti:** Venancio e colleghi, Frontiers in AI 2025, DOI 10.3389/frai.2025.1520557: confronto fra reti CNN per classificare superficie con attenzione alla ghiaia, valutando accuratezza e tempo inferenza su Raspberry Pi. L’abstract indica EfficientNetV2 come compromesso migliore nel proprio esperimento.

**Lettura editoriale / possibile utilità:** Interesse: precedente concreto per un avviso fondo a basso consumo su dispositivo embedded.

**Limiti e informazioni non verificate:** Non dimostra riconoscimento universale di ostacoli, buche o aderenza enduro. Nessuna accuratezza numerica generalizzata riportata qui; dati e condizioni originali vanno letti prima di scegliere un modello.

**AI effettivamente documentata:** Deep learning per classificazione immagini, non LLM.

**Fonti:**

  - [Paper Frontiers](https://www.frontiersin.org/journals/artificial-intelligence/articles/10.3389/frai.2025.1520557/full) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-094 — Postura multi-IMU — proposta Halmstad 2025–2027

- **Stato documentato:** PROPOSTA_TESI_APERTA.
- **Pertinenza:** Studio moto, non specificamente enduro.
- **Tipo di novità:** Proposta accademica corrente nel periodo, attività effettiva ND.
- **Data annuncio/pubblicazione:** Periodo indicato 2025–2027.

**Fatti riportati dalle fonti:** Scheda CAISR: progetto master “Posture Estimation for Motorcycle Riders Using Multi-IMU Systems and Video-Based Ground Truth”, periodo autunno 2025–primavera 2027 e stato Open. Prevede sei IMU giacca più IMU casco, sincronizzazione video, sensor fusion e verifica errori articolari.

**Lettura editoriale / possibile utilità:** Interesse: un programma di validazione coerente per confrontare sensori indossabili e video.

**Limiti e informazioni non verificate:** È una proposta di progetto, non prova che un team stia già eseguendo test o abbia raggiunto risultati. Autore non compilato e metriche finali non presenti. Non chiamarlo prodotto o studio concluso.

**AI effettivamente documentata:** ML opzionale nella proposta; centrale la fusione sensori.

**Fonti:**

  - [Scheda CAISR/Halmstad](https://mw.hh.se/caisr/index.php?title=Posture_Estimation_for_Motorcycle_Riders_Using_Multi-IMU_Systems_and_Video-Based_Ground_Truth) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-095 — Postura Pilota — tesi Bologna/Ducati 2025

- **Stato documentato:** TESI_ABSTRACT.
- **Pertinenza:** MotoGP; trasferimento da validare.
- **Tipo di novità:** Studio recente, non app pubblica verificata.
- **Data annuncio/pubblicazione:** 2025-07-16.

**Fatti riportati dalle fonti:** Tesi magistrale di Massimo Rondelli, discussa 16 luglio 2025, sviluppata con Ducati MotoGP. Abstract: stabilizzazione video con YOLOv8s-OBB sul logo, segmentazione parti corpo con YOLOv8m-SEG e ricostruzione di parti occluse con PSPNet/ResNet101. Full text non disponibile per scelta autore.

**Lettura editoriale / possibile utilità:** Interesse: affronta il problema reale di telecamera in movimento e parti del pilota invisibili.

**Limiti e informazioni non verificate:** MotoGP e enduro hanno abiti, posture e sfondi diversi. Metriche dell’abstract non dimostrano accuratezza di angoli 3D o un coach enduro; ricostruzione di parti occluse resta inferenza.

**AI effettivamente documentata:** Computer vision e segmentazione; risultati limitati all’abstract accessibile.

**Fonti:**

  - [Tesi Università Bologna](https://amslaurea.unibo.it/id/eprint/35534/) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-096 — Comportamento motociclista con XGBoost e IMU

- **Stato documentato:** PAPER_2024.
- **Pertinenza:** Strada; metodologia trasferibile.
- **Tipo di novità:** Ricerca pubblicata precedente, ancora metodologicamente utile.
- **Data annuncio/pubblicazione:** 2024.

**Fatti riportati dalle fonti:** Sensors 2024 “Classifying Motorcyclist Behaviour with XGBoost Based on IMU Data”, DOI 10.3390/s24031042, testa classificazione di cinque comportamenti. Circa 80% di accuratezza riportata per quattro classi; sorpasso non identificato affidabilmente. Gli autori chiedono test con altre moto e altri piloti.

**Lettura editoriale / possibile utilità:** Interesse: modelli relativamente leggeri per riconoscere stati di guida da sensori.

**Limiti e informazioni non verificate:** Non è un classificatore già validato per wheelie, cadute lente, salti o fatica in enduro. Accuratezza interna al campione non è affidabilità universale.

**AI effettivamente documentata:** XGBoost e interpretazione SHAP.

**Fonti:**

  - [Sensors paper](https://www.mdpi.com/1424-8220/24/3/1042) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-097 — YCOR — Yamaha/CMU Off-Road Dataset

- **Stato documentato:** DATASET_RICERCA.
- **Pertinenza:** Percezione terreno; non dataset enduro italiano.
- **Tipo di novità:** Dataset esistente, non rilascio 2026.
- **Data annuncio/pubblicazione:** Ricerca storica; data pagina ND.

**Fatti riportati dalle fonti:** AirLab descrive 1076 immagini da quattro località tra Pennsylvania e Ohio, in tre stagioni, con annotazioni poligonali di terreno/vegetazione/ostacoli. Split riportato: 931 training e 145 validazione, senza sovrapposizione sessione ma con sovrapposizione località.

**Lettura editoriale / possibile utilità:** Interesse: punto di partenza per studiare segmentazione del fondo e separazione corretta dei dati.

**Limiti e informazioni non verificate:** Non prova idoneità a sentieri europei o vista da casco. Prima di training commerciale verificare licenza e download effettivi. Il numero di classi dichiarato nella pagina e la lista testuale non sono perfettamente allineati: nessun conteggio classi consolidato.

**AI effettivamente documentata:** Dataset per visione/segmentazione.

**Fonti:**

  - [YCOR AirLab](https://theairlab.org/yamaha-offroad-dataset/) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-098 — Dataset roughness — immagini, GPS, IMU e ruota

- **Stato documentato:** PAPER_DATASET_2021.
- **Pertinenza:** Veicoli offroad; trasferimento moto da validare.
- **Tipo di novità:** Fondamento storico, non studio corrente certificato.
- **Data annuncio/pubblicazione:** 2021.

**Fatti riportati dalle fonti:** Paper IEEE ICAS 2021, DOI 10.1109/ICAS49788.2021.9551147, descrive oltre 12000 immagini di terreno con GPS, IMU e sensore velocità ruota. Confronta otto schemi per etichettare rugosità a partire dall’accelerazione verticale.

**Lettura editoriale / possibile utilità:** Interesse: legare ciò che si vede a una misura fisica dell’effetto del terreno sul mezzo.

**Limiti e informazioni non verificate:** Dataset non dichiarato specifico per moto enduro. Rugosità, percorribilità e aderenza non sono la stessa variabile. Questa scheda usa abstract; accesso ai dati e licenza non verificati.

**AI effettivamente documentata:** Supervisione per modelli visivi; non modello pronto enduro.

**Fonti:**

  - [IEEE dataset roughness](https://ieeexplore.ieee.org/document/9551147/) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-099 — RND / MUDD — riconoscere piloti e numeri nel fango

- **Stato documentato:** PREPRINT_DATASET_2024.
- **Pertinenza:** Gare offroad e servizi foto.
- **Tipo di novità:** Dataset pubblicato e applicazione riferita dagli autori.
- **Data annuncio/pubblicazione:** 2024-02-12.

**Fatti riportati dalle fonti:** Tyo e colleghi pubblicano “Beyond the Mud”, arXiv 2402.08025: oltre 6300 immagini per OCR numeri gara e re-identificazione piloti. I benchmark descrivono trasferimento debole dei modelli generici e miglioramenti dopo fine-tuning, senza eliminare errori. Gli autori riferiscono impiego dei modelli su PerformancePhoto.

**Lettura editoriale / possibile utilità:** Interesse: precedente molto pertinente per ricerca foto automatica negli eventi enduro.

**Limiti e informazioni non verificate:** Ricerca di immagini non è cronometraggio ufficiale o riconoscimento infallibile. Licenza immagini, diritti fotografi e utilizzo commerciale dei dataset da verificare. Non equiparare bounding box/OCR a identità personale certa.

**AI effettivamente documentata:** OCR e re-identificazione visiva con fine-tuning.

**Fonti:**

  - [Beyond the Mud](https://arxiv.org/abs/2402.08025) — Primaria: ricerca/autori.
  - [Precedente Reading Between the Mud](https://arxiv.org/abs/2311.09256) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-100 — Controllo termico neurale di motore elettrico

- **Stato documentato:** PAPER_2026.
- **Pertinenza:** Due ruote hub-motor; non enduro centrale.
- **Tipo di novità:** Ricerca 2026.
- **Data annuncio/pubblicazione:** 2026-03-13.

**Fatti riportati dalle fonti:** Scientific Reports, Deshmukh e colleghi, 13 marzo 2026, DOI 10.1038/s41598-026-37505-y: propone modello termico fisico più stimatore neurale, indice salute motore e derating di coppia legato a rischio termico e condizioni sensori.

**Lettura editoriale / possibile utilità:** Interesse: precedente per limitare surriscaldamento e gestire la prestazione elettrica in uso intenso.

**Limiti e informazioni non verificate:** Il caso è hub motor: trasferimento a powertrain enduro centrale non dimostrato. Sintesi da abstract/obiettivi; non vengono attribuite prove racing o prodotto commerciale.

**AI effettivamente documentata:** Stimatore neurale ibrido con modello fisico.

**Fonti:**

  - [Paper controllo termico](https://www.nature.com/articles/s41598-026-37505-y) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-101 — Reinforcement learning per sospensioni offroad

- **Stato documentato:** PAPER_CONFERENZA_2022.
- **Pertinenza:** Veicoli terreno; non moto enduro provata.
- **Tipo di novità:** Ricerca storica trasferibile, non roadmap industriale.
- **Data annuncio/pubblicazione:** 2022.

**Fatti riportati dalle fonti:** ISTVS 2022, Zhuang e colleghi, DOI 10.56884/BCOR8152: controllo semiattivo di smorzatore magnetoreologico con apprendimento per rinforzo e modellazione Gaussian Process. L’abstract descrive confronti con altri controlli su eccitazioni casuali, bump e sinusoidali.

**Lettura editoriale / possibile utilità:** Interesse: studio di regolazione automatica con consumo inferiore a sospensione pienamente attiva.

**Limiti e informazioni non verificate:** Non è prova di una forcella enduro AI acquistabile. Geometria, massa non sospesa, stabilità e controllo di una moto richiedono validazione dedicata; sintesi dall’abstract.

**AI effettivamente documentata:** Reinforcement learning e Gaussian Process.

**Fonti:**

  - [Paper ISTVS](https://2022.istvs.org/papers/1128) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-102 — AI per manutenzione off-highway — review 2026

- **Stato documentato:** REVIEW_2026.
- **Pertinenza:** Macchine agricole/miniera/costruzione; affine.
- **Tipo di novità:** Revisione recente di ricerca 2014–2025.
- **Data annuncio/pubblicazione:** 2026-08-06.

**Fatti riportati dalle fonti:** Frontiers Mechanical Engineering, agosto 2026, DOI 10.3389/fmech.2026.1877194: review di 51 studi selezionati sulla prognostica di macchine off-highway. Sintetizza metodi supervisionati, reti, autoencoder e transfer learning, con attenzione a carichi variabili e scarsità sensori.

**Lettura editoriale / possibile utilità:** Interesse: chiarisce perché una demo di previsione guasti non basta per un servizio affidabile sul campo.

**Limiti e informazioni non verificate:** Non è review specifica su moto enduro. Le prestazioni dei trattori o dei mezzi minerari non possono essere attribuite a motori 2T/4T moto.

**AI effettivamente documentata:** Sintesi di famiglie ML; non software utilizzabile.

**Fonti:**

  - [Review PHM off-highway](https://www.frontiersin.org/journals/mechanical-engineering/articles/10.3389/fmech.2026.1877194/full) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-103 — Yamaha MOTOROiD2 / MOTOBOT — dimostratori autonomi

- **Stato documentato:** DIMOSTRATORI_RICERCA.
- **Pertinenza:** Tecnologie moto generali; non enduro disponibile.
- **Tipo di novità:** Dimostratori, non roadmap prodotto enduro.
- **Data annuncio/pubblicazione:** ND.

**Fatti riportati dalle fonti:** MOTOROiD2 usa controllo bilanciamento AMCES, sensori e camere per interazione con pilota e autonomia a bassa velocità. MOTOBOT è invece robot che aziona i comandi di una moto non convertita in veicolo autonomo, studiando prestazione e traiettorie.

**Lettura editoriale / possibile utilità:** Interesse: ricerca su bilanciamento, riconoscimento e relazione uomo-macchina.

**Limiti e informazioni non verificate:** Non costituiscono annuncio di enduro autonoma o assistente che recupera la moto caduta. Programmi storici: le pagine non certificano attività continuativa o una data commercializzazione 2027.

**AI effettivamente documentata:** Visione e controllo sperimentale; capacità secondo progetto.

**Fonti:**

  - [MOTOROiD2](https://global.yamaha-motor.com/design_technology/design/concept/motoroid2/) — Primaria: produttore.
  - [MOTOBOT](https://global.yamaha-motor.com/design_technology/technology/motobot/) — Primaria: produttore.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-104 — imec smartGLAZ — visiera con guida ottica

- **Stato documentato:** RICERCA_STORICA_2018_2020.
- **Pertinenza:** Casco moto; ottica trasferibile.
- **Tipo di novità:** Fondamento storico; stato successivo ND.
- **Data annuncio/pubblicazione:** 2018-05-01 / 2020-04-30.

**Fatti riportati dalle fonti:** Progetto imec.icon con durata indicata 1 maggio 2018–30 aprile 2020. Obiettivo: separare motore ottico dalla visiera usando guide d’onda e ottiche diffrattive. Target pubblicati includono trasparenza oltre 80%, campo diagonale 15 gradi e immagine virtuale a 5–10 metri.

**Lettura editoriale / possibile utilità:** Interesse: vincoli di peso, distanza focale e visiera sostituibile per un display indossato.

**Limiti e informazioni non verificate:** Sono obiettivi di progetto, non prestazioni di prodotto acquistabile. La pagina usa ancora futuro ma riporta date concluse: non etichettare come studio corrente nel 2026.

**AI effettivamente documentata:** Non verificata / non dichiarata nelle evidenze raccolte.

**Fonti:**

  - [imec smartGLAZ](https://www.imec-int.com/en/what-we-offer/research-portfolio/smartglaz) — Primaria: ricerca/autori.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

## TECH-105 — Bosch MSC / ARAS — sensori e assistenza predittiva stradale

- **Stato documentato:** TECNOLOGIA_FORNITORE.
- **Pertinenza:** Strada/adventure; trasferimento offroad da validare.
- **Tipo di novità:** Tecnologia industriale disponibile; non novità enduro specifica.
- **Data annuncio/pubblicazione:** ND.

**Fatti riportati dalle fonti:** Bosch descrive MSC con sensori ruote e IMU per controllo frenata/coppia in base alla dinamica. ARAS combina radar, gestione motore, freni e interfaccia per assistenze di contesto.

**Lettura editoriale / possibile utilità:** Interesse: illustra differenza fra percezione ambiente, stima stato e intervento sul veicolo.

**Limiti e informazioni non verificate:** Non è rilevamento automatico di radici o guida autonoma enduro. Algoritmi di controllo non sono per definizione AI. Le stime di incidenti evitabili citate da Bosch riguardano il proprio contesto stradale, non gare enduro.

**AI effettivamente documentata:** Non verificata / non dichiarata nelle evidenze raccolte.

**Fonti:**

  - [Bosch MSC](https://www.bosch-mobility.com/en/solutions/braking-systems/motorcycle-stability-control/) — Primaria: produttore.
  - [Bosch ARAS](https://www.bosch-mobility.com/en/solutions/assistance-systems/advanced-rider-assistance-systems-2w/) — Primaria: produttore.

**Consultazione:** 2026-10-18. La data dell’ultima modifica della pagina, salvo indicazione esplicita, è ND; un risultato indicizzato può precedere questa consultazione.

---

