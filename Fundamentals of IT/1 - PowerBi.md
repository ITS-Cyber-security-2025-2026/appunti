Tags: [[ITS1]], [[PowerBi]]

Per importare i file .xml (Excel) si può usare la funzione *power query*.

Power Bi serve per creare Dashboard con il fine di implementare la visualizzazione dei dati.

Procedimento:

Avvio una nuova **Power Query**.
- Carico i dati dalla cartella Excel
- Click su ***trasforma i dati***
- In ***Filtri*** --> ***Rimuovi Vuoti*** (per eliminare una cella vuota)
- A destra troviamo la sezione ***Passaggi applicati*** in cui possiamo muoverci all'interno del nostro workflow
- sopra ad ogni colonna abbiamo la sezione ***Tipo di dati***

Dobbiamo sistemare alcuni dati perchè importati in maniera erronea:
- **Prezzo** --> diventa ***numero decimale fisso***

### Scheda Home

- ***Gestisci***
	- ***Duplica*** serve a duplicare i dati
	- ***Riferimento*** serve a collegare una copia all'originale
- ***Scegli colonne*** serve a selezionare delle colonne specifiche
- ***Rimuovi colonne*** si può usare per togliere colonne indesiderate
- ***Mantieni Righe*** serve per conservare le righe selezionate.
- ***Rimuovi Righe*** serve a togliere le Righe indesiderate
- ***Dividi colonna*** ci permette di separare gli elementi di una colonna in due colonne separate in base a molteplici criteri
	- in base al delimitatore
	- da non cifra a cifra
- ***Raggruppa*** mi fa i subtotali delle colonne
- ***Merge di Query*** -----> ***Unisci query***
	- Scelgo il CodiceCliente della query vendite e lo collego al CodiceCliente della query clienti.
	- ***Tipi di Join*** ---> come in SQL (inner, outer, full, left Anti, right Anti...)
 
### scheda ***Trasforma*** 

- Dopo aver selezionato una casella con un errore, troviamo la funzione ***Sostituisci errori***, questa prenderà gli errori selezionati nel file e permetterà di sostituirne il valore con un inserimento manuale.
- Possiamo anche usare la funzione ***Sostituisci Valore*** che ci permette di cambiare il valore di una casella. Possiamo usarla per cambiare il valore ad una casella con *null* assegnato.
- Dopo aver selezionato una colonna contenente del testo, sotto la sezione ***Formato*** troviamo funzioni che ci permettono di auto-formattare il testo
- Sia nella scheda ***Trasforma*** che in quella ***Home*** troviamo la funzione ***Raggruppa***.
  Questa funzione ci permette di creare una nuova Query di dati effettuando un Operazione sulle colonne specificate e di aggiungere alla Query altre colonne.


### scheda ***Aggiungi Colonna***

- Dopo averla selezionata, possiamo mantenere una colonna e usare ***Estrai*** per estrarre dei dati in base ad un criterio specifico e inserirlo in una nuova colonna; se avessimo voluto rimuovere la colonna originale dovevamo fare lo stesso però dalla scheda ***Trasforma***.
- Dopo aver selezionato una colonna di tipo *Data* possiamo usare la funzione ***Data***, Se volessimo fare una nuova colonna con l'eta di un cliente basandoci sulla colonna DataDiNascita otterremo con la funzione ***Età*** una nuova colonna in *minuti*, possiamo ottenere una nuova colonna in anni con la funzione ***Durata*** per poi crearne una nuova arrotondata con la funzione ***Arrotonda***.
- ***Colonna personalizzata*** ci permette di creare una colonna in base alle nostre esigenze, inserendo una formula da noi scelta (es. IVA --> formula = \[prezzo] * 22 / 100).
  Dovremo poi arrotondare la colonna ottenuta per *numero decimale fisso*.
  Poi possiamo creare una nuova colonna *Totale* e assegnargli una formula personalizzata (=\[prezzo] + \[IVA]).
- ***Colonna condizionale*** invece ci da la possibilità di creare una nuova colonna in base ad una condizione specifica (es. prezzo > 600 alto || prezzo > 400 medio || else basso)
- ***Colonna Indice*** serve ad aggiungere un indice qualora non sia presente


Quando abbiamo terminato di aggiungere modifiche al file nella query possiamo andare sulla scheda **Home** e selezionare ***Chiudi e Applica***.


<hr>


Una volta chiusa una Power Query troveremo 3 distinte sezioni:
- ***Visualizzazione Report***
- ***Visualizzazione Tabella***
- ***Visualizzazione Modello***

In ciascuna potremo svolgere diverse funzioni:
in **Report** possiamo lavorare con i grafici e la visualizzazione dei dati stessi, in **Tabella** possiamo visualizzare le tabelle importate RAW come sono ed infine in **Modello** possiamo vedere le relazioni tra le varie tabelle importate.

Quando dobbiamo importare diversi file Excel contenenti diverse Tabelle, dobbiamo **per forza** importarle separatamente, una per volta. Se dovessimo avere invece le diverse tabelle nello stesso file Excel ma in fogli diversi non incontreremmo alcun problema di importazione.


