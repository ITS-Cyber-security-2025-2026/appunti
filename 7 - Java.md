Tags: [[ITS1]], [[Java]]

### Eccezioni

Ogni programma deve essere scritto in modo da agire nel caso il flusso principale incontri degli ostacoli.
Per evitare dei blocchi in seguito a degli errori e permettere al programma di continuare ad andare utilizzeremo le ***Eccezioni***.

Queste sono delle situazioni impreviste gestibili.

Vi sono 5 parole chiave fondamentali per le eccezioni:
- ***try***
- ***catch***
- ***finally***
- ***throw***
- ***throws***

##### Come si usano?

Quando sviluppiamo un codice che potrebbe potenzialmente scatenare un'eccezione, possiamo circondarlo da un blocco ***try*** seguito da uno o più blocchi ***catch***.

```Java

try {
	...
	...
	}
	catch {
	...
	}

```

<hr>

##### try / catch

Prendiamo in esempio il codice della classe Aula.java:

```Java
package it.corsoCyber2025.lezione6;

import java.util.Date;

public class Aula {

    public static void main(String[] args) {
        Persona pers = new Persona(
            "Jacopo",
            "Adinolfi",
            "via xx",
            new Date(2001, 02, 01),
            "xxxxxxx"
        );

        String nome = pers.getNome();
        System.out.println("Il nome è: " + nome);

        pers = new Persona(
            "Riccardo",
            "Belloni",
            "Via yyyy",
            new Date(2001, 02, 02),
            "yyyyyyy"
        );

        nome = pers.getNome();
        System.out.println("Il nome è: " + nome);

        Persona insegnante;
        nome = insegnante.getNome(); //qui vi è l'ERRORE
    }
}

```

Quando andiamo ad eseuire il programma l'oggetto ***insegnante*** questo non sarà presente in memoria perchè non è ancora stato definito.
Otterremo a console il seguente errore:

```Java

Exception in thread "main" java.lang.Error: Unresolved compilation problem:

The local variable insegnante may not have been initialized

  

at it.corsoCyber2025.lezione6.Aula.main(Aula.java:17)

```


Per mitigare questo problema possiamo inserire all'interno di un *try* il codice di Aula.java, seguito da un *catch* al cui interno stamperemo in output il motivo dell'eccezione.

```Java

package it.corsoCyber2025.lezione6;

import java.util.Date;

public class Aula {

    public static void main(String[] args) {
	    try {
        Persona pers = new Persona(
            "Jacopo",
            "Adinolfi",
            "via xx",
            new Date(2001, 02, 01),
            "xxxxxxx"
        );

        String nome = pers.getNome();
        System.out.println("Il nome è: " + nome);
        
        Persona insegnante;
        nome = insegnante.getNome(); //qui vi è l'ERRORE

        pers = new Persona(
            "Riccardo",
            "Belloni",
            "Via yyyy",
            new Date(2001, 02, 02),
            "yyyyyyy"
        );

        nome = pers.getNome();
        System.out.println("Il nome è: " + nome);

        } catch (NullPointerException npe) {
        System.err.println("è stata usata una variabile non inizializzata");
        }
    }
}


```

![[Schermata del 2025-11-19 16-16-52.png]]

##### finally

Potrebbe essere che nel mio programma debba fare delle azioni indifferentemente dalla riuscita del programma.
Possiamo quindi usare la parola chiave ***finally*** per includere alla fine del codice qualcosa che venga eseguito sempre indifferentemente dal risultato del programma.

```Java

package it.corsoCyber2025.lezione6;

import java.util.Date;

public class Aula {

    public static void main(String[] args) {
	    try {
        Persona pers = new Persona(
            "Jacopo",
            "Adinolfi",
            "via xx",
            new Date(2001, 02, 01),
            "xxxxxxx"
        );

        String nome = pers.getNome();
        System.out.println("Il nome è: " + nome);
        
        Persona insegnante;
        nome = insegnante.getNome(); //qui vi è l'ERRORE

        pers = new Persona(
            "Riccardo",
            "Belloni",
            "Via yyyy",
            new Date(2001, 02, 02),
            "yyyyyyy"
        );

        nome = pers.getNome();
        System.out.println("Il nome è: " + nome);

        } catch (NullPointerException npe) {
        System.err.println("è stata usata una variabile non inizializzata");
        } catch (Exception npe) {
        System.err.println("Vi è stato un errore imprevisto");
        } finally {
        System.out.println("Questa istruzione viene eseguita sempre");
        }
    }
}


```
![[Schermata del 2025-11-19 16-24-51 1.png]]


Le eccezioni in Java sono **Oggetti**, possiamo usare quindi dei metodi ad essi associati per ottenere maggiori informazioni sull'errore che si è presentato.
Ad esempio possiamo usare *npe.nullStackTrace();* per riportare la linea del codice in cui si è verificato l'errore.

Le Eccezioni sono Gerarchiche, verranno mostrati quindi gli errori in ordine di importanza.

<hr>


### Metodi che lanciano eccezioni

Sono metodi che usano al loro interno un ***throw*** pero senza un *try..catch* che lo gestisce.
Determineremo una condizione per cui la parola chiave *throw* lancerà un'Eccezione, dovremmo pero indicare nella funzione anche la parola chiave ***throws Exceptions*** per specificare alla funzione che può essere lanciata un'eccezione all'interno della funzione.

```Java
package it.corsoCyber2025.lezione6;

public class Calcoli {

    public int somma(int a, int b) {
        return a + b;
    }

    public int divisione(int a, int b) throws Exception {
        if (b == 0) {
            throw new Exception("Il divisore non può essere zero");
        }

        return a / b;
    }
}

```

Tuttavia una volta chiamata quella funzione dovremo gestire l'eccezione con un *try...catch*, altrimenti Java ci darà errore prima dell'esecuzione.

