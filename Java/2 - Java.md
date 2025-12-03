Tags: [[ITS1]], [[Java]]

<hr>


### Compiling

Al di fuori di un IDE possiamo andare nella directory dove il nostro file java è salvato e compilarlo usando il comando ```javac``` .

![[Schermata del 2025-10-24 14-22-45.png]]


<hr>


### Modulo Import

Quando parte il runtime, questo carica solo le funzioni minime indispensabili per funzionare. Se volessimo usare delle funzioni addizionali dobbiamo usare il modulo Import per aggiungerle.


```Java
import java.util.*;

public class Benvenuto {
	public static void main(String[] args)
	{
// stampo una domanda sulla periferica di input

		System.out.println("Ciao, come ti chiami?");

// creo un oggetto utile per leggere da input

		Scanner input = new Scanner(System.in);

// Leggo una riga dalla periferica di Input

		String nome = input.nextLine();

// Stampo un saluto personalizzato

		System.out.println("Ciao " + nome + ", benvenuto!");
	}
}
```

In questo caso abbiamo sfruttato la funzione "Scanner" che permette al programma di leggere ciò che viene digitato dall'utente a terminale.

<hr>


### Altre funzioni importate

Con lo stesso principio possiamo usare moltissime altre funzioni. Qui sotto c'è l'esempio della funzione ```JOptionPane.showMessageDialog```, questa ci riporterà un prompt all'interno di un dialog box.

```java
JOptionPane.showMessageDialog(null,
	"Ciao " + nome + ", benvenuto!");
```


<hr>


### Commenti

Singola linea -->    //

Multi linea -->    /*

Commento JavaDoc -->    /**

Quest'ultimo ci permette di creare una pagina HTML nella finestra javadoc, rendendo facilmente leggibili delle istruzioni

<hr>

Java è un linguaggio **case-sensitive**, ogni istruzione deve terminare con un ";"

<hr>


Java ha bisogno di **Variabili** per funzionare.
In Java le variabili sono tipizzate, bisogna per forza definirne la tipologia.

Vi è un altro tipo di Variabili in Java, chiamate **Costanti**.
Queste una volta assegnate non possono essere modificate.

```Java
final double PI_GRECO=3.14
```

Per definire la costante utilizziamo il termine ```final```.

Le variabili inutilizzate finiscono nel "Garbage collector".


Vi sono alcune keyword riservate a funzioni di Java che non possono esser utilizzate per definire le variabili (es: double).

La convenzione per definire le **Variabili** è il camelCase, mentre per le **Costanti** si utilizza lo snake_case.


<hr>


Java ci da gratuitamente 8 tipi di variabili che non sono oggetti.

**Byte, Short, int, long, float, double, char, boolean**


è importante ripetere che queste variabili NON SONO OGGETTI.


<hr>


### Qual'è la differenza di questi ultimi con un oggetto?

Mentre queste variabili sono identificate semplicemente da un  valore, un oggetto è una sovrastruttura che contiene diverse caratteristiche, essa contiene un valore ma puoi anche agire su di esso.

```Java

int anno = 2025;   --> variabile

String nome = new String("Alessandro");  --> oggetto

```

nel caso della variabile possiamo utilizzarla per delle operazioni, mentre con l'oggetto possiamo operare sull'oggetto, come ad esempio ottenerne un'abbreviazione.

```Java

String abbreviazione = nome.substring(0,3);

```


<hr>

```Java

// ottengo i caratteri dell'oggetto nome partendo dall'indice

int anno = 2025;

String nome = new String ("Alessandro");

String nome1 = "Gianni";

int numCaratteri = nome.length();

for (int pos=4; pos < numCaratteri;pos++) {

char carattere = nome.charAt(pos);

System.out.print(carattere);

}

// Controllo se i due oggetti nome e nome1 sono uguali

boolean uguali = nome.equals(nome1);

boolean stessoOggetto = nome == nome1;

System.out.print(uguali);

System.out.print(stessoOggetto);

```


<hr>


```Java

// converto il risultato di una divisione in un numero double
int numero = 3;
int altroNumero = 2;
double divisione = (double)numero/(double)altroNumero;
long numeroLungo = numero;

// Faccio casting per dichiarare a Java che forzi la conversione del numero precedentemente dichiarato come long in int
long numeroLunghissio = 34272338;
int numeroMenoLungo = (int) numeroLunghissimo;

```


<hr>


```Java

import java.util.Scanner;

  
public class AltroEsercizio {
public static void main (String[]args) {

Scanner inputUtente = new Scanner(System.in);

int a = inputUtente.nextInt();

float b = inputUtente.nextFloat();

float r3 = (float)a+b;

System.out.println(r3);

}
}

```

Nel programma qui sopra eseguo un'operazione dopo aver ottenuto due input dall'utente.

Devo prima definire l'input dell'utente e per richiederlo devo poi definire le singoli variabili che voglio che siano chieste, castando la loro tipologia in float se non fossero di questa

**N.B.** Quando devo inserire un float a console, questo si scrive con la virgola (,) e non con il punto (.) come si fa invece nel codice.

