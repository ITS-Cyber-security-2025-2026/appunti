Tags: [[ITS1]], [[Java]]

### Costruzione di una classe

Quando vado a creare un nuovo oggetto vado ad usare un nuovo metodo, chiamato ***costruttore***.

```Java
// creo il nuovo oggetto (1)

Persona tizio = new Persona("Mario", "Rossi", "PD");

// definisco l'oggetto e il costruttore

public Persona() { //oggetto (2)
	this.nome= "";
	this.cognome = "";
	this.provincia = "";
	}

//costruttore (3)
public Persona(String nome, String cognome, String prov) {
	this.nome = nome;
	this.cognome = cognome;
	provincia = prov;
	}

```

Il flusso funzionerà come segue:
	Creo il nuovo oggetto passando come argomenti le sue caratteristiche (1)
	Chiamando il costruttore Persona (3) assegno alla proprietà del nuovo oggetto persona (2) i dati forniti in precedenza.

Se non forniamo un costruttore, Java ne crea uno automatico di default che svolgerà la funzione di un costruttore.

Se uso la parola chiave ***this*** risolvo le ambiguità legate al nome della proprietà.
Comunico a Java che la nuova proprietà all'interno della funzione ha lo stesso nome dell'argomento passato ma che ciò che segue *this.* è la proprietà dell'oggetto mentre quello che gli assegno è l'argomento passato.

##### ***null***
```Java

Persona tizio = new Persona("Mario", "Rossi", "PD");

Persona caio = null;

...
```

la parola chiave ***null*** ci permette di creare un oggetto senza associarvi nessuna proprietà.
Questo per poterlo utilizzare successivamente assegnandoli dei valori.

Se volessi appunto assegnare dei valori alle proprietà dell'oggetto dovrò PRIMA definire un nuovo oggetto persona nominato allo stesso modo.

```Java

caio.nome = "caio"; //ERRORE --> caio non è stato inzializzato, non esiste

caio = new Persona();

caio.nome = "caio"; //ERRORE --> esiste e quindi gli quò essere associata una proprietà

```


<hr>


### Controllo d'Accesso

Vi sono 4 controlli d'accesso in Java ma noi vedremo solo i due principali:

- public --> chiunque può usare il membro al suo interno

- private --> solo il codice della classe stessa può usare il membro al suo interno

Prendiamo l'esempio del codice seguente:

```Java

package it.corsoCyber2025.lezione6;

import java.util.Date;

public class Persona {
	
	private String nome;
	private String cognome;
	private String indirizzo;
	private Date dateDiNascita;
	public String coloreCapelli;
	
	public String getNome() {
		return nome;
	}
}



```

Se prendiamo l'esempio di una persona. Ho creato una classe persona che definisce dei membri senza assegnarli ad un oggetto tramite diverse proprietà.
Per logica, informazioni quali *nome, cognome, indirizzo e data di nascita* saranno a noi ignote fino a che non le chiediamo alla persona interessata. Il colore dei capelli invece si può vedere senza chiederlo.

Nel caso di un programma usando ***private / public*** definiamo quali membri possano essere condivisi e utilizzati al di fuori del programma e quali debbano invece rimanere al suo interno.

Possiamo poi rendere successivamente pubblici questi membri usando la generazione automatica di eclipse:

> *Evidenzio le proprietà > Tasto destro > Source > Generate Getters and Setters*


![[Schermata del 2025-11-19 14-57-18.png]]

Risultato:
```Java
package it.corsoCyber2025.lezione6;

import java.util.Date;

public class Persona {

    private String nome;
    private String cognome;
    private String indirizzo;
    private Date dateDiNascita;
    public String coloreCapelli;

    public String getNome() {
        return nome;
    }

    public String getCognome() {
        return cognome;
    }

    public String getIndirizzo() {
        return indirizzo;
    }

    public Date getDateDiNascita() {
        return dateDiNascita;
    }

    public String getColoreCapelli() {
        return coloreCapelli;
    }
}

```

Aggiungiamo quindi un costruttore dell'oggetto Persona che userà i membri creati precedentemente come proprietà.

```Java

public Persona(String nome, String cognome, String indirizzo, Date dataNasc) {
	this.nome = nome;
	this.cognome = cognome;
	this.dataDiNascita = dataNasc;
	this.indirizzo = indirizzo;
}

```

Esempio di Classe *Aula* esterna che utilizza i membri *public* della classe *Persona*. 

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
    }
}

```


##### ***final***

Questa parola chiave specifica che una proprietà non cambierà MAI.

Posso specificarlo sia alla dichiarazione della proprietà:

```Java
private String nome;
private String cognome;
private String indirizzo;
private Date dataDiNascita;
public final String coloreCapelli; // Qui
private String codiceFiscale;

```

Oppure lo definiamo all'interno de costruttore:

```Java

public Persona(String nome, String cognome, String indirizzo, Date dataDiNascita) {
	
	super();
	this.nome = nome;
	this.cognome = cognome;
	this.indirizzo = indirizzo;
	this.dataDiNascita = dataDiNascita;
	coloreCapelli = "Neri"; // Qui

}

```


> *N.B. --> La parola chiave **This** risolve le ambiguità legate all'utilizzo dello stesso nome per proprietà e argomento passato. Se l'argomento possiede un nome diverso non ci sar' il bisogno dell'uso della parola chiave this.

