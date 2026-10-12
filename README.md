# 🎵 Chinook Data Warehouse Design

Progetto accademico realizzato per il corso di **Big Data Analytics \& Machine Learning**.  
L'obiettivo è la transizione da un database transazionale (OLTP) a un modello dimensionale (**Data Warehouse / OLAP**) per analizzare le vendite e i comportamenti d'acquisto di un digital media store basato sul dataset **Chinook**.

\---

## 🎯 Idea e Obiettivo del Progetto

Il punto di partenza è il database relazionale **Chinook**, che modella un negozio digitale di musica (brani, artisti, album, fatture, clienti e dipendenti).

Il progetto si concentra sull'**analisi degli ordini e delle vendite** con un approccio multidimensionale, permettendo di interrogare e aggregare i dati secondo diverse prospettive di business:

* **Tempo**: andamento temporale delle vendite.
* **Brano**: performance per traccia, genere e album.
* **Cliente**: abitudini d'acquisto e distribuzione geografica.
* **Ordine**: volume e composizione dei singoli carrelli.

\---

## 📐 Metodologia di Progettazione (DFM)

Il Data Warehouse è stato modellato seguendo la metodologia del **Dimensional Fact Model (DFM)** attraverso diverse fasi:

1. **Reverse Engineering**:

   * Ricostruzione dello schema concettuale (EER / E-R) a partire dal DDL MySQL (`Chinook\_MySql.sql`).
2. **Definizione del Fatto**:

   * Individuazione del fatto principale: **`InvoiceLine`** (la riga di dettaglio dell'ordine/acquisto del brano).
3. **Elaborazione dell'Albero degli Attributi**:

   * **Costruzione iniziale**: mappatura di tutti gli attributi raggiungibili dal fatto.
   * **Potature**: eliminazione degli attributi operativi o non rilevanti ai fini dell'analisi statistica (es. recapiti secondari, dettagli tecnici dei file, gerarchie del personale).
   * **Innesti**: collegamento diretto al fatto di dimensioni rilevanti come `CustomerId` e `InvoiceDate` per semplificare e velocizzare le analisi.
   * **Reificazione**: modellazione esplicita della gerarchia temporale:  
$$\\text{InvoiceDate} \\longrightarrow \\text{Giorno} \\longrightarrow \\text{Mese} \\longrightarrow \\text{Trimestre} \\longrightarrow \\text{Anno}$$

\---

## 📊 Dimensioni e Glossario delle Misure

Tutte le metriche individuate sono **additive lungo ogni dimensione**:

|Misura|Formula concettuale|Descrizione|
|-|-|-|
|**Valore Totale Vendite**|`SUM(UnitPrice \* Quantity)`|Fatturato complessivo generato dalle vendite|
|**Numero Totale Ordini**|`COUNT(DISTINCT InvoiceId)`|Conteggio degli ordini univoci effettuati|
|**Numero Brani per Ordine**|`SUM(Quantity)` group by `InvoiceId`|Volume di articoli acquistati per transazione|
|**Spesa e Ordini per Cliente**|`COUNT(DISTINCT InvoiceId)`, `SUM(UnitPrice \* Quantity)`|Frequenza d'acquisto e valore monetario per singolo cliente|

\---

## 📁 Struttura della Repository

```text
├── Chinook\_MySql.sql        # Script DDL e dati di popolamento del DB sorgente
├── E-R.mwb                  # Modello relazionale/EER (MySQL Workbench)
├── Schema ER.pdf            # Diagramma E-R generato
├── ReverseEngineering.pdf   # Schema risultante dal processo di reverse engineering
├── AlberoAttributi.png      # Albero degli attributi e gerarchie dimensionali
├── Progetto Big Data.pptx   # Presentazione completa del progetto (step DFM e metriche)
└── README.md                # Documentazione del progetto
```

\---

## 

