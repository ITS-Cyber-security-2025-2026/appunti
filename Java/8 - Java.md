Tags: [[ITS1]], [[Java]]


### Collezioni

Una collezione è un insieme di oggetti omogenei. 

### Mappe
Una mappa invece è una struttura che permette di associare ad un oggetto una chiave.

Queste si suddividono in **Hash** o **Tree**.

*Hash*  --> Viene creato un hash univoco che viene utilizzato per identificare che l'oggetto sia univoco.

*Tree* --> Gli oggetti vengono inseriti in ordine e l'ID associato altrettanto.

```Java

Map<String, String> province = new HashMap<String, String>();

```

Di seguito vi è un esempio di Mappa:

```Java

public EsempioMappa() {

Map<String, String> province = new HashMap<String, String>();
province.put("VE", "Venezia");
province.put("PD", "Padova");
province.put("VI", "Vicenza");
province.put("RO", "Rovigo");
province.put("VR", "Verona");
province.put("BL", "Belluno");

String descrizioneProv = province.get("PD");
System.out.println("la provincia con codice PD è " + descrizioneProv);

```

Vediamo che in una *Mappa* abbiamo due tipologia di valori:
- **keySet** -----> chiave associata ad un singolo valore
- **values** -----> valore inserito nella Mappa

Di seguito abbiamo un programma che stampa *chiavi* e *valori*.
```Java

public EsempioMappa() {

	Map<String, String> province = new HashMap<String, String>();
	province.put("VE", "Venezia");
	province.put("PD", "Padova");
	province.put("VI", "Vicenza");
	province.put("RO", "Rovigo");
	province.put("VR", "Verona");
	province.put("BL", "Belluno");
	
	String descrizioneProv = province.get("PD");
	System.out.println("la provincia con codice PD è " + descrizioneProv);
	
	Iterator<String> chiavi = province.keySet().iterator();
	while (chiavi.hasNext()) {
		String codiceProv = chiavi.next;
		String descrProv = province.get(codiceProv);
		System.out.println(codiceProv + " " + descrProv);
		}
}
```

In alternativa al ciclo `while`, possiamo usare un ciclo `for..each` come segue, tuttavia stamperemo solo i *valori*, **non** le *chiavi*:

```Java
...

for (String codiceProv : province.keySet()) {
	String descrProv = province.get(codiceProv);
	System.out.println(codiceProv + " " + descrProv);
	}
```


### Set
Una struttura di dati che non ammette duplicati.
Anche per i *Set* si possono usare **HashSet** e **TreeSet**.

```Java
// Esempio di Set di tipo String

Set<String> persone = new HashSet<String>();

// Esempio di Set di qualsiasi tipo

Set persone = new HashSet();

```

Nel *Set* io posso specificare il tipo di dato che ci vado ad inserire con ***<>***.
Se non volessi specificarlo lo ometto e basta.

Se provassimo ad inserirci dei dati di tipo diverso li accetterebbe ma risulta scomodo perchè avremmo diverse tipologie di dato mischiate nello stesso Set e l'accesso ad essi sarebbe confusionale.

```Java

// Esempio di aggiunta a Set di qualsiasi tipo
// XXXXXXX ACCESSO CONFUSIONALE XXXXXXXX

Set persone = new HashSet();
persone.add(new Integer(3));
persone.add("Mario");
persone.add(new Date());

```

La Best-Practise è quella di creare per ogni tipologia di dato un *Set* a sè per poi successivamente collegare fra di loro i dati tra i vari Set.

```Java

Set<String> persone = new HashSet<String>();
persone.add("Mario");
persone.add(new Integer(8)); // <-- Risulta Errore di compilazione
persone.add("Gino");

```

Se volessimo chiamare ciascun'oggetto all'interno del Set possiamo usare il metodo ***for each***.
Questo viene anche chiamato ***Iterator***.

```Java
for (String persona : persone) {
}
```

equivale a scrivere:
```Java
Iterator<String> iterator = peersone.iterator();
while(iterator.hasNext()) {
String persona = iterator.next();
System.out.println(persona);
}
```


Vediamo la differenza tra **HashSet** e **TreeSet** nell'output: 
	*TreeSet* mi ordinerà i dati mentre *HashSet* me li riporterà in ordine casuale.

Nei Set abbiamo la possiblità di usare il metodo ***.contains***. Questo ci ritornerà un boolean **true** se il dato è contenuto all'interno del Set, invece **false** se non è contenuto nel Set.


### Lista
Ammette qualsiasi dato duplicato.


