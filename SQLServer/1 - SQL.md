Tags: [[ITS1]], [[SQL]], 

è un database relazionale --> tante tabelle legate tra loro da relazioni

La parte più difficile è la costruzione stessa dei database/tabelle relazionali.

Il linguaggio SQL si articola in 4 diversi comandi principali:

1. Selezionare Dati
2. Insert
3. Update
4. Delete

<hr>


#### Iniziamo con la creazione di un Database

Immaginiamo il caso di un'azienda commerciale.
Avremo al suo interno diversi settori:

**Fornitori** ---> (compra articoli) --> **Azienda** --> (vende articoli) --> **Clienti**

**L'Azienda** dovrà gestire diversi dati:
- Articoli
- Categoria Articoli
- Vendite/Articoli Venduti
- Acquisti/Materie Prime
- ecc..

#### Creiamo la prima tabella
Definiamo la tabella come tale, iniziando definendo le colonne


| ID  | NOME     | Descrizione            |     |
| --- | -------- | ---------------------- | --- |
| 1   | WD40     | lubrificante meccanico |     |
| 2   | spazzole | ...                    |     |

Impostiamo la colonna ID per essere di tipo **int** e come chiave primaria usando il tasto con l'icona della chiavetta in alto nella barra degli strumenti.


<hr>

Abbiamo creato 3 tabelle: Articoli, Dipendenti e CategoriaArticoli

**Che relazione c'è fra queste tabelle?**

prendiamo in analisi le due tabelle Articoli e CategorieArticoli, **ogni articolo** avrà una singola categoria di appartenenza e di conseguenza **ogni Categoria** avrà molti articoli che vi appartengono.


