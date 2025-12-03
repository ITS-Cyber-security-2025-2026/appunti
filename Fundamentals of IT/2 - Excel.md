Tags: [[ITS1]], [[Excel]]


### Formule

- ***SE***
	=se(condizione;"opz1";"opz2")

	se annidato:
		=se(condizione;"opz1";se(condizione;"opz2";"opz3"))

- ***SOMMA***
	=SOMMA(cella1;cella2)

- ***; e :***
	In excel il punto e virgola ";" si utilizza per selezionare la stessa colonna, i due punti ":" invece si usano per selezionare un'area comprendente diverse colonne.

- ***formattazione condizionale***
	Evidenziamo le celle interessate e andiamo su *formato > condizione* per impostare una formattazione condizionale

- ***MEDIA***
	=MEDIA(cella1;cella2)

- ***MAX***
	=MAX(cella1;cella2)

- ***MIN***
	=MIN(cella1;cella2)

- ***Conta celle***
	=conta.valori(cella1;cella2)

- ***Conta numeri***
	=conta.numeri(cella1;cella2)

- ***somma.se***
	Permette di sommare i valori in cui la condizione specificata è veritiera.
		=somma.se(intervallo;"condizione";intervallo di somma)
		posso dettare una condizione ">100000" oppure usare la sintassi ">"&cella dove 'cella' è la comparazione che usiamo.

- ***media.se***
	ci permette di fare una media dei valori la cui condizione specificata è veritiera.
	=media.se(intervallo;"condizione";intervallo di media)

- ***conta.se***
	Possiamo anche contare quante volte si verifica una specifica condizione.
	=conta.se(intervallo;"condizione")

- ***somma.piu.se***
	Consente di sommare i valori che rispettano **più condizioni**.
	=SOMMA.PIÙ.SE(intervallo_somma; intervallo_criteri1; criterio1; [intervallo_criteri2; criterio2]...)

- ***media.più.se***
	Calcola la **media** dei valori che soddisfano **una o più condizioni**.
	=MEDIA.PIÙ.SE(intervallo_media; intervallo_criteri1; criterio1; [intervallo_criteri2; criterio2]...)

- ***conta.più.se***
	Conta **quante celle** soddisfano **più condizioni contemporaneamente**.
	=CONTA.PIÙ.SE(intervallo_criteri1; criterio1; [intervallo_criteri2; criterio2]...)

- ***cerca.vert***
	Cerca un valore nella prima colonna di una tabella e restituisce un dato presente in una **colonna a destra**.
	- _indice_colonna_ = numero della colonna da cui prelevare il dato
    
	- _intervallo_ = VERO per corrispondenza approssimativa, FALSO per corrispondenza esatta
	
	=CERCA.VERT(valore_cercato; tabella; indice_colonna; [intervallo])


- ***se.errore***
	Serve per **gestire gli errori** nelle formule: se la formula genera un errore, viene restituito un valore alternativo da te scelto.
	=SE.ERRORE(valore; valore_se_errore)

	esempio:
	=SE.ERRORE(CERCA.VERT(E2; A2:C100; 3; FALSO); "Non trovato")

- ***cerca.x***
	Cerca un **valore** in un intervallo e restituisce il **valore corrispondente** da un altro intervallo.  
	Supporta **ricerche verticali, orizzontali**, ricerca da **sinistra a destra**, valori **mancanti**, e **corrispondenze approssimate o esatte**.  
	=CERCA.X(valore_da_cercare; intervallo_di_ricerca; intervallo_di_risultato; [se_non_trovato]; [modalità_confronto]; [modalità_ricerca])

