Tags: [[Java]], [[ITS1]]

<hr>


Un algoritmo è la descrizione di un metodo di soluzione di un problema che:
- Sia eseguibile
- Sia priva di ambiguita
- Arrivi ad una conclusione in un tempo finito. 

# Java

**Vantaggi:**
- è un linguaggio orientato agli oggetti
-  Sicuro
-  Robusto
- Pensato per la rete
- Dinamico
- Multithreaded
- Fatto da programmatori per programmatori

**Svantaggi:**
- Non adatto per interfacciarsi con l'hardware
- Calo delle prestazioni
- Necessita della JVM (Java Virtual Machine)

<hr>


### JRE vs JDK

**JRE**
Java Runtime Environment è un pacchetto contenente tutto il necessario per eseguire un programma compilato in Java

**JDK**
Java Development Kit contiene tutto ciò che contiene JDK assieme ad un compiler (javac) e tools, permettendo di creare e compilare programmi in Java.


<hr>

# **Java è case-sensitive**

Un programma in Java è costituito da piu classi.

Ogni classe racchiude al suo interno delle funzioni.


```Java
public class PrimaClasse {

}
```

Public --> rende la funzione accessibile da qualsiasi altra classe

Class --> definisce una classe

PrimaClasse --> nome della classe


```Java
public class classe1 {
public static void main(String[] args) {
System.out.println("ciao");
}
}
```

Per eseguire il programma java possiamo clickkare tasto destro sulla classe nel file explorer e selezionare "**Run as..**" --> Java Application.


<hr>


```Java
public class classe1 {
public static void main(String[] args) {
System.out.println("ciao ");
System.out.println(args[0]);
}
}
```

In questo caso la stringa ```System.out.println(args[0]);``` va a "pescare" il primo argomento tra quelli dichiarati.

Per dichiararli andiamo su "**Run**" --> "**Run Configurations**"
Nella finestra che si apre successivamente andremo nella sezione "**Arguments**" e definiremo nella prima casella di testo gli argomenti facenti parte dell'array, separando ciascuna stringa da uno spazio.


<hr>


Possiamo inserire un breakpoint ad una specifica linea per sfruttarlo in modalità debug.

sulla colonna dei numeri delle linee, premendo tasto destro, possiamo selezionare "**Toggle Breakpoint**". Facendo cosi potremo poi eseguire il programma in modalità debug usando il tasto in alto con l'icona di un ragno/insetto.

Cosi facendo eseguendo il programma analizzeremo una istruzione per volta.
Possiamo ad esempio usare il tasto "**Step Over**" per andare avanti di un istruzione.

