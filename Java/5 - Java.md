Tags: [[ITS1]], [[Java]]

<hr>


Come possiamo testare quanto tempo ci mette un applicazione a completarsi?

Iniziamo importando il modulo *java.util.Date*.

 ```Java
 import java.util.Date;
 ```

successivamente, all'interno del main, prima che qualsiasi cosa venga eseguita, impostiamo una variabile Date iniziale:

```Java
Date inizio = new date();
```

e impostiamo una variabile di fine in coda a tutte le funzioni:

```Java
Date fine = new Date();
```

Creiamo una variabile che contenga la differenza tra fine e inizio riportandoci la durata e lo passiamo in un System.out che riporta un prompt con la durata in milisecondi.

```Java
long durata = fine.getTime()-inizio.getTime();
System.out.println("il programma impiega " + durata + "millisecondi");
```


Se non volessimo ottenere il risultato in millisecondi per renderlo piu leggibile ad un occhio esterno possiamo usare ***Google Guava***. Questo package ci permette di usare il modulo *Stopwatch*, questo funzionerà come un cronometro, farà partire il tempo e lo stopperà in automatico riportandoci i dati in base alla loro grandezza. Secondi, millisecondi, minuti ecc...


Per scaricarlo dobbiamo andare sulla repository ***Maven Central***.
Qui possiamo ottenere diverse repository third-party free to use.


<hr>


Come facciamo a integrarle in Eclipse?
La versione di Eclipse che abbiamo scaricato contiene un compiler Maven che recupera le librerie dopo avergli indicato le coordinate trovate sulla repository.

Nella creazione di un nuovo progetto andiamo su 
> *New* > *Other* > nella barra filtri cerchiamo *Maven* > *skip Archetype* > settiamo il Group Id e l'Artifact Id (it.corsoCyber2025.lezione6 - MisureTempo)

Avremo ora un nuovo progetto Maven.
In questo progetto il sistema predispone automaticamente delle cartelle separate per i file Java e per i Test. Inoltre le risorse, ovverosia tutte le scritte che il programma riporta saranno internazionalizzate: in base alla lingua impostata sul computer verranno selezionate in automatico.

Apriamo ora il file *pom.xml*, ci permetterà di fare diverse cose, in questo momento ad esempio ci permette di installare le dipendenze da Maven Central.

Creiamo un contenitore dependencies alla fine del file e vi inseriamo l'xml che troviamo sulla repository di Guava di Maven Central.

```xml
// originale
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>it.corsoCyber2025.lezione6</groupId>
<artifactId>MisureTempo</artifactId>
<version>0.0.1-SNAPSHOT</version>
</project>
```

```xml
//added dependencies
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
<modelVersion>4.0.0</modelVersion>
<groupId>it.corsoCyber2025.lezione6</groupId>
<artifactId>MisureTempo</artifactId>
<version>0.0.1-SNAPSHOT</version>
<dependencies>
<!-- https://mvnrepository.com/artifact/com.google.guava/guava -->
<dependency>
<groupId>com.google.guava</groupId>
<artifactId>guava</artifactId>
<version>11.0.2</version>
</dependency>
</dependencies>
</project>
```


Nel momento in cui salviamo il file questo scaricherà in automatico le dependencies di Guava.
Le troveremo nel seguente percorso:

```Linux
/home/-utente-/.m2/repository/com/google/guava/guava/11.0.2
```

```Windows
C:/Utenti/-utente-/.m2/repository/com/google/guava/guava/11.0.2
```

Qui troveremo piu di una libreria, perche?
A noi interesserà solo il file con estensione *.jar*, avremo poi un file .pom (nel caso l'estensione debba scaricare altre dependencies), dei file .sha1 (firme digitali che certificano l'autenticità dei file) e i file sorgenti che sono le classi in cui girerà la nostra estensione.


<hr>


Ora usiamo il metodo Stopwatch di Guava per implementare il codice di prima:

```Java
package it.corsoCyber2025.Lezione6;

import com.google.common.base.Stopwatch;

public class MisureTempo {

    public MisureTempo() {
        // TODO Auto-generated constructor stub
    }
    public static void main(String[] args) {
        // Creo il cronometro
        Stopwatch cronometro = new Stopwatch();
        // Avvio il cronometro
        cronometro.start();
        int numeroElementi = 1000000;
        // int numeroRandom= (int)(Math.random()*10000);
        System.out.println("carico i numeri");
        int[] NumeriCasuali = new int[numeroElementi];
        for (int i = 0; i < NumeriCasuali.length; i++) {
            int numeroRandom = (int) (Math.random() * 10000);
            NumeriCasuali[i] = numeroRandom;
        }
        System.out.println("Ho creato i numeri casuali");
        System.out.println(cronometro.toString());
        //
        System.out.println("questi sono i numeri pari");
        for (int i = 0; i < NumeriCasuali.length; i++) {
            int numeroRandom = NumeriCasuali[i];
            if (numeroRandom % 2 == 0) {
                System.out.println(numeroRandom);
            }
        }
        System.out.println("Questi sono i numeri in posizione pari");
        for (int i = 0; i < NumeriCasuali.length; i++) {
            int numeroRandom = NumeriCasuali[i];
            if (i % 2 == 0) {
                System.out.println(numeroRandom);
            }
        }
        System.out.println("Questo � l'output dell'array al contrario");
        for (int i = NumeriCasuali.length - 1; i > 0; i--) {
            System.out.println(NumeriCasuali[i]);
        }
        // Qui fermo il cronometro
        cronometro.stop();
        System.out.println("tempo impiegato: " + cronometro.toString());
    }
}

```


<hr>


### Classe

Una classe è il costrutto più importante: senza di essa non si può fare nessun programma.
Una classe è un modello per una categoria di oggetti.

Una classe dev'essere scritta in un file *.java* e chiamata per convenzione con la prima lettera maiuscola. non è obbligatorio ma è un best-practise.

Sintassi:
```Java
class NomeClasse {
	tipo proprietà1;
	tipo proprietà2;
	...
	tipo metodo1(argomenti) {
	...
	}
	tipo metodo2(argomenti) {
	...
	}
...
}
```


Java ma anche i linguaggi orientati agli oggetti dispongono di caratteristiche per definire una
efficiente rappresentazione del reale. In questa ottica, la classe è il concetto di una categoria di oggetti, mentre le sue istanze sono gli oggetti veri e propri.

***Proprietà e metodi***

1. Le proprietà sono delle variabili proprie di un oggetto, ad esso strettamente collegate.
2. I metodi sono delle azioni che l'oggetto può eseguire su richiesta.
3. Per accedere a proprietà e metodi si utilizza la notazione puntata.

Le proprietà di un oggetto vengono inizializzate in automatico dall'ambiente di esecuzione.


Vi sono delle classi speciali chiamate ***Java Bean*** che servono a memorizzare delle informazioni sotto forma di oggetto.

```Java
//Esempio di Java Bean

package it.corsoCyber2025.Lezione6;

import java.util.ArrayList;

public class LezioneJava {

    public class Persona {

        String nome;
        String cognome;
        Date DataNascita;
        String provincia;
    }
}

```

Questi sono dei contenitori puri, contengono informazioni e basta.
Possiamo poi importarlo in un'altra classe e lavorare con queste informazioni, anche aggiungendone di nuove.

```Java
package it.corsoCyber2025.Lezione6;

import java.util.ArrayList;

public class LezioneJava {

    public static void main(String[] args) {
    ArrayList<Persona>classe = new ArrayList<Persona>();
    Persona ludovico = new Persona();
    ludovico.nome = "Ludovico";
    classe.add(ludovico);
    // Imposto le altre proprieta
    Persona mohamed = new Persona();
    mohamed.nome = "Mohamed";
    classe.add(mohamed);
    
    System.out.println(ludovico.nome);
    System.out.println(classe.get(0).nome);
    }
}

```

```Java
// Gli output seguenti sono uguali, svolgono la stessa funzione
    System.out.println(ludovico.nome);
    System.out.println(classe.get(o).nome);
```


