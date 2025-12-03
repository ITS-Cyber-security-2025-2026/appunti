
Tags: [[ITS1]], [[Java]]

#### OPERATORI
Operatori sono sequenze di caratteri che permettono la manipolazione dei dati.

**ARITMETICI**
	+    -    *    /    %    ++    --

**CONCATENAZIONE STRINGHE**
	+
	Altamente inefficente. Meglio evitare di concatenare stringhe.

**OPERATORI DI CONFRONTO**
	== != >< <= >=

**OPERATORI LOGICI**
	!    &    &&    |    ||    ^

**OPERATORI DI ASSEGNAZIONE**
	=
	+=    -=    /=    *=    %=

**VALUTAZIONE CONDIZIONATA**
es --> 
```Java
int c (a < 2 ? 3: 4) ;
```

La funzione esempio in parole povere significa: 
	*Dichiarato l'int C, se A è minore di 2 allora ritorna 3 sennò ritorna 4.*

è uguale a scrivere:

```Java
int c;
if (a < 2) {
c = 3;
}else{
c = 4;
}
```


<hr>

#### **STRUTTURE DI CONTROLLO DECISIONALI**
Sono strutture che permettono di deviare il corso del programma nel corso della sue esecuzione in seguito ad una condizione.

Di queste la più usata e importante è la ***if / else***

```Java
if (condizione) {
se condizione1 vera eseguo questa {istruzione};
else if se condizione2 vera eseguo questa {istruzione};
else if se condizione3 vera eseguo questa {istruzione};
else se tutte le altre non sono vere eseguo questa {istruzione};
```

Un'altra struttura di controllo decisionale importante è lo ***switch***

```Java
switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
  default:
    // code block
}
```

funziona allo stesso modo di usare la struttura ***if/else***. Definisco prima un'espressione da valutare nella sezione *expression*, successicamente nella sezione *case* determino i casi per cui il codice che segue debba essere eseguito. Determino infine una condizione *default* per il  caso in cui nessun caso possa essere applicato all'espressione.

```java
int day = 4;
switch (day) {
  case 6:
    System.out.println("Today is Saturday");
    break;
  case 7:
    System.out.println("Today is Sunday");
    break;
  default:
    System.out.println("Looking forward to the Weekend");
}
// Outputs "Looking forward to the Weekend"
```


**STRUTTURE DI CONTROLLO ITERATIVE**

Queste strutture permettono di ripetere un codice ogniqualvolta una condizione si avvera.

la struttura ***WHILE*** controlla prima la condizione e poi esegue la  condizione.

la struttura ***FOR*** itera una azione per ogni caso della condizione.

la struttura ***DO...WHILE*** non viene comunemente usata, esegue un'azione ALMENO una volta e successivamente la ripete solo se la condizione avviene.

	WHILE
```java
int i = 0;
while (i < 5) {
  System.out.println(i);
  i++;
}
```

	FOR
```java
for (int i = 0; i < 5; i++){
	System.out.println(i);
}
```

**BREAK e CONTINUE**

L'operatore *continue* ci permette di bypassare l'esecuzione di un codice nel caso la condizione si avveri.
***Esce dal blocco corrente ma non ferma il ciclo.***

L'operatore *break* invece, blocca l'ulteriore esecuzione del codice quando la condizione si avvera.
***Esce dal blocco e ed interrompe il ciclo.***

```java
for (int i = 0; i < 6; i++) {
  if (i == 2) {
    continue;
  }
  if (i == 4) {
    break;
  }
  System.out.println(i);

```


<hr>

#### METODI
Un metodo è una sequenza di istruzioni con un nome.
Si chiama un metodo per eseguire le sue istruzioni.

```Java
class Esempio () {
	void stampa() {
		System.out.println("Ciao a tutti")
	}
}
```

Nel codice soprastante stiamo solamente definendo il metodo *stampa ()*.

```Java
class Esempio2 () {
	int somma(int a, int b) {
		int c = a + b;
		return c;
	}
}
```

L'operatore *return* ci permette di avere un output dal metodo *somma* per poterlo poi utilizzare successivamente.

```Java
class Esempio3 () {
	int somma(int a, int b) {
		int c = a + b;
		return c;
	}
	int somma(int a, int b, int c) {
		int d = a + b + c;
		return d;
	}
}
```

Java distingue i metodi in base alle *firme* (argomenti) dei metodi stessi.
Anche nel caso seguente stiamo definendo due metodi separati perche il primo argomento non è della stessa tipologia. **(int != long)**

```Java
class Esempio3 () {
	int somma(int a, int b) {
		int c = a + b;
		return c;
	}
	int somma(long a, int b) {
		int c = a + b;
		return c;
	}
}
```


Nell'esempio seguente chiamiamo la funzione somma con **2** argomenti, Java quindi, tra tutti i metodi possibili, andrà a utilizzare il metodo che ha lo stesso tipo e quantità di argomenti.

```Java
public class Esempio {
	public static void main(String[]a) {
		int numero = 4;
		int altroNumero = 5;
		int laSomma = somma(numero, altroNumero)
		
		void somma(int a, int b) {
			int c = a + b;
			return c;
	}
		int somma(int a, int b, int c) {
			int d = a + b + c;
			return d;
	}
		int somma(long a, int b) {
			int c = a + b;
			return c;
	}
}
```

in questo caso solo la prima funzione combacia con tipo e quantità di argomenti richiesti quando abbiamo precedentemente chiamato la funzione.

***Se definiamo un metodo VOID questo non ritornerà nessun valore***

***Se definiamo un metodo INT, LONG, STRING, ecc.. questo avrà bisogno dell'operatore RETURN per ritornare un valore. Dovremo definire il modulo dello stesso tipo del valore che vogliamo ritornare.***

> quando uso un metodo, uso dei valori che mi restituiscono un risultato, i valori vengono poi scartati e viene tenuto solo il risultato. Questo per liberare la memoria.

L'istruzione *return* fa terminare il metodo immediatamente, possiamo tuttavia avere diversi return all'interno dello stesso metodo.

```Java
public class Esempio {
	public static void main(String[]a) {
		int numero = 4;
		int altroNumero = 5;
		int laSomma = compare(numero, altroNumero)
		
		void compare(int a, int b) {
			if (a > b) {
				return a;
			}else{
				return b
		}
	}
}
```

Si può evitare di usare più *return* utilizzando una variabile d'appoggio, come in seguito.

```Java
public class Esempio {
	public static void main(String[]a) {
		int numero = 4;
		int altroNumero = 5;
		int laSomma = compare(numero, altroNumero)
		
		void compare(int a, int b) {
			int ritorno;
			if (a > b) {
				ritorno = a;
			}else{
				ritorno = b;
		}
		return ritorno;
	}
}
```









#### Esercizi fatti in classe

1) Crea un programma che richiede 3 numeri all'utente e li confronta per controllare se sono tutti uguali, se sono tutti diversi o se non sono tutti uguali.

```Java
import java.util.Scanner;

public class esCondizioni {
	public static void main(String[] args)
	{

		System.out.println("Ciao, dammi tre numeri.");

		Scanner numero = new Scanner(System.in);

		int a = numero.nextInt();
		int b = numero.nextInt();
		int c = numero.nextInt();

		if (a == b && b == c) {
			System.out.println("sono tutti uguali");
		}else if (a != b && b != c && a != c){
			System.out.println("sono tutti diversi");
		}else{
			System.out.println("non sono tutti uguali");
		}
	}
}
```

2) Crea un programma che prende 3 numeri dati dall'utente e controlla che siano in ordine crescente, decrescente o che non siano in nessun ordine. Il programma deve anche gestire il caso di due numeri uguali, riconoscendo che non c'è nessun ordine.

```Java
import java.util.Scanner;

public class esCondizioni {
	public static void main(String[] args)
	{
		System.out.println("Ciao, dammi tre numeri.");
		
		Scanner numero = new Scanner(System.in);
		
		int a = numero.nextInt();
		int b = numero.nextInt();
		int c = numero.nextInt();
		
		if (a > b && b > c) {
			System.out.println("sono in ordine decrescente");
		}else if (a < b && b < c){
			System.out.println("sono in ordine crescente");
		}else{
			System.out.println("non sono in nessun ordine");
		}
	}
}
```

3) Prendo 3 numeri dall'utente, crea 3 moduli diversi che ritornano una variabile boolean True solo se:
- I 3 numeri sono tutti uguali
- I 3 numeri sono tutti diversi
- I 3 numeri sono in ordine crescente

```Java
import java.util.Scanner;

public class esCondizioni {
	public static void main(String[] args)
	{
		int ciclo = 3;
		
		while (ciclo > 0) {
		System.out.println("Ciao, dammi tre numeri.");
	
		Scanner numero = new Scanner(System.in);
		
		double a = numero.nextDouble();
		double b = numero.nextDouble();
		double c = numero.nextDouble();
		boolean finale = comparazione(a, b, c);
		System.out.println(finale);
		ciclo--;
		}
	}

	static boolean comparazione(double a, double b, double c) {
		boolean risultato = false;
		
		if (a == b && b == c) {
			risultato = true;
		} else if (a < b && b < c) {
			risultato = true;
		} else if (a != b && b != c && c != a) {
			risultato = true;
		} return risultato;
	}
}
```



4) Scrivi un programma che legge una parola e un numero, il modulo dovrà stampare la parola tante volte quanto è il numero inserito.

```Java
import java.util.Scanner;

public class esCondizioni {
	public static void main(String[] args) {
	int ciclo = 3;
	while (ciclo > 0) {
		System.out.println("Ciao, dammi tre numeri.");
		
		Scanner numero = new Scanner(System.in);
		
		double a = numero.nextDouble();
		double b = numero.nextDouble();
		double c = numero.nextDouble();
		boolean finale = comparazione(a, b, c);
		
		System.out.println(finale);
		ciclo--;
		}
	}

	static boolean comparazione(double a, double b, double c) {
		boolean risultato = false;
		
		if (a == b && b == c) {
			risultato = true;
		} else if (a < b && b < c) {
			risultato = true;
		} else if (a != b && b != c && c != a) {
			risultato = true;
		} return risultato;
	}
}
```


5) Dati 4 numeri input, controllare se vi sono due coppie o non ci sono due coppie.

```Java
import java.util.Scanner;

public class esCondizioni {
	public static void main(String[] args) {
		System.out.println("Ciao, dammi 4 numeri");
		
		Scanner numero = new Scanner(System.in);
		int a = numero.nextInt();
		int b = numero.nextInt();
		int c = numero.nextInt();
		int d = numero.nextInt();
		
		controllo(a, b, c, d);
	}

	static void controllo(int a, int b, int c, int d) {
		if (a == b || a == c || a == d) {
			if (b == c || b == d || c == d) {
				System.out.println("due coppie");
			} else {
				System.out.println("non due coppie");
			}
		}
	}
}
```

