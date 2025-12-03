Tags: [[ITS1]], [[Java]]

### Package

è una collezione di classi con uno scopo preciso. 
Per poterli utilizzare bisogna dichiarare l'import all'inizio del programma.

```Java
import java.util.Scanner;
```

solo successivamente possiamo usare la funzione intrinseca del Package.

Quando si creano dei **Package** c'è una convenzione per la loro dichiarazione.

> it --> nazione
> foremaCorsoCyber2025 --> nome del Package
> corso --> nome del progetto


```Java
package it.foremaCorsoCyber2025.corso;
```

Partendo da un progetto vuoto possiamo andare nella cartella *src* e creare un nuovo Package, partirà un wizard per la sua creazione e l'assegnazione del suo nome. Una volta creato possiamo fare una classe nuova nella stessa cartella e, aprendola, vedremo lo statement del package già scritto in automatico.

Se volessimo usare una libreria esterna, creata da terze parti possiamo chiamarla usando lo statement ***import***.
```Java
package it.foremaCorsoCyber2025.corso;

import java.math.*;

public class Esempio {
	public esempi() {
		// TODO Auto-Generated constuctor stub
		Math.random();
	}
}
```

L'organizzazione dei Package rispecchia il filesystem del dispositivo, il nome del Package è ANCHE il percorso.

Apache POI è una libreria Java che permette di leggere e modificare file Microsoft Office.

<hr>

### Stringhe

La stringa è una concatenazione di caratteri.

La stringa in Java è un oggetto immutabile, vuol dire che alla sua modifica non viene cambiata l'allocazione di memoria originale ma ne viene occupata una nuova.

Le Stringhe sono contenute nel core di Java: **Java.lang**.

Nella documentazione di Java si possono vedere tutti i metodi utilizzabili (es. ```concat()```).

- Uguaglianza:
	Non bisogna mai utilizzare l'operatore ==
	Si usa solo il metodo ```equals```

```Java
String s1 = "ciao";

s2.equals(s1);

```

- Confronto:
	Posso confrontare la grandezza di una stringa con qualsiasi altra usando il metodo ```compareTo```
	Restituisce 0 se le stringhe sono uguali, un numero negative se s1 è minore di s2, un numero positive se s1 è maggiore di s2.

```Java
String s1 = "ciao";
String s2 = "addio";

s1.compareTo(s2);
```


<hr>


Nel seguente esercizio notiamo un'inefficenza nella generazione della string finale:
Java genera la stringa e ad ogni ripetizione la butta via per crearne un'altra, possiamo ovviare a questo problema usando ***StringBuilder***.

```Java

package it.corsoCyber2025.lezione5;

	public class lezione5Esercizi {
		public static void main(String[] args) {

			String prompt = "";
			int numero = 0;

			for (int i = 0; i < 10; i++) {
				if (numero % 3 == 0) {
					prompt = prompt + "numero multiplo di 3: " + numero + "\n";
// ^^^^^^^^^^^^^^^^^^^^^^^^ qui avviene l'inefficenza^^^^^^^^^^^^^^^^^^^^^^^^^
					numero++;
					continue;
				} else {
					i--;
					numero++;
				}
			}
			System.out.println(prompt);
		}
	}
	
```


```Java

package it.corsoCyber2025.lezione5;

	public class lezione5Esercizi {
	public static void main(String[] args) {
		
		//String prompt = "";
		StringBuilder prompt = new StringBuilder();
		int numero = 0;
		
		for (int i = 0; i < 10; i++) {
			if (numero % 3 == 0) {
				// prompt = prompt + "numero multiplo di 3: " + numero + "\n";
				prompt.append("numero multiplo di 3: ").append(numero).append("\n");
				numero++;
				continue;
			} else {
				i--;
				numero++;
			}
		}
		System.out.println(prompt);
	}
}

```



<hr>


### Array

Un ***Array*** è una collezione di oggetti dello stesso tipo a cui di fa riferimento con un nome comune.
Una volta che lo dichiaro non posso modificare la dimensione dell'Array.

```Java
String[] giorniSettimana = new String[7];
```

Questo statement non crea l'Array, dichiara solo la sua dimensione.

Per iterare fra tutti gli elementi di un Array possiamo usare un ciclo *for* specifico.

```Java
public static void main(String[] args) {
	String[] giorniSettimana = new String[7];
	giorniSettimana[0] = "Lunedi";
	giorniSettimana[0] = "Martedi";
	giorniSettimana[0] = "Mercoledi";
	giorniSettimana[0] = "Giovedi";
	giorniSettimana[0] = "Venerdi";
	giorniSettimana[0] = "Sabato";
	giorniSettimana[0] = "Domenica";
	
	for (String giorno : giorniSettimana) {
		System.out.println(giorno);
	}
	
	// Quello che segue è un codice che fa esattamente la stessa cosa di quello sopra, Java lo semplifica in automatico
	for (int i = 0; i < giorniSettimana.length; i++) {
		System.out.println(giorniSettimana[i]);
	}
}

```

Se non abbiamo popolato completamente l'Array gli slot vuoti otterranno valore *null*.

Algoritmi comuni nell'Array:
- Riempimento
- Somma e media
- Max e Min
- Elemento Separatore
```Java
Arrays.toString(variableArray);
```
- Ricerca Lineare
- Rimuovere e inserire un elemento
- Scambiare un elemento
- Copiare un Array
```Java
Arrays.copyOf(varArray, lungArray);

```


Possiamo creare anche Array a 2 dimensioni chiamata ***Matrice***.

```Java
int [] [] a = new int [3] [2];
A [1] [2] = 8
```

Gli Array sono belli ma scomodi, per avere dei gruppi di oggetti più dinamici sono stati creati gli ***ArrayList***.
Questi aggiungono 10 elementi ogni volta che sia necessario.

```Java
ArrayList<tipo> lista = new
Array:ist<tipo> ();
```

Posso usare solo oggetti, non vengono aggettati integer, double, float ecc... se volessi usarli dovrei prima convertirli in oggetti.

*qui di seguito il casting apposito per gli ArrayList*
>	boolean --> Boolean
>	char --> Character
>	int --> Integer
>	double --> Double
>	String --> String


```Java
public static void main(String[] args) {
	
	ArrayList<String>giorniSettimana = new ArrayList<String>();
	giorniSettimana.add("Lunedi");
	giorniSettimana.add("Martedi");
	giorniSettimana.add("Mercoledi");
	giorniSettimana.add("giovedi");
	giorniSettimana.add("Venerdi");
	giorniSettimana.add("Sabato");
	giorniSettimana.add("Domenica");
	
	for (String giorno : giorniSettimana) {
		System.out.println(giorno);
	}
	
	// Quello che segue è un codice che fa esattamente la stessa cosa di quello sopra, Java lo semplifica in automatico
	for (int i = 0; i < giorniSettimana.size; i++) {
		String giorno=giorniSettimana.get(i);
		System.out.println(giorno);
	}
}
```


Di seguito troviamo i metodi utilizzabili con ArrayList:
- .add(obj)   ----> aggiungi oggetto
- .add(posizione, obj)    ----> aggiungi oggetto a posizione specifica
- .remove(index)   ----> rimuovi oggetto di indice x
- .set(posizione, obj)   ----> imposta un oggetto alla posizione specificata
- .size()   ----> ritorna la grandezza dell ArrayList
- .get(posizione)   ----> ritorna l'oggetto alla posizione specificata
- .clear()   ----> pulisce completamente l'ArrayList



