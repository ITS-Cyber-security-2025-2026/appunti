Tags: [[ITS1]], [[Java]], [[PostGreSQL]]

Nella scorsa lezione abbiamo creato un programma che ritornava il nome della provincia completa partendo dalla sigla, avendone specificato la correlazione tramite una **Mappa**.

In questa Lezione vogliamo passare quella mappa in una base di dati e useremo ***PostGreSQL***.

Installiamo PostgreSQL e pgAdmin_4 dal sito ufficiale; una volta avviato quest'ultimo dovremo settare una password da ricordarci che ci permetterà di entrare dentro al database da noi selezionato.

Una volta all'interno del database ne creiamo uno con:
> *Click destro* ---> *Create* ---> *Database*

Assegnamo un nome al nuovo database e questo verrà creato all'interno del **Server Selezionato**.

![[Schermata del 2025-11-27 16-24-48.png]]

![[Schermata del 2025-11-27 16-25-06.png]]

Successivamente entriamo nel database e andiamo su:

> *Schemas* ---> *public* ---> *Tables*

e creiamo una nuova tabella con:

> *Tasto destro* ---> *Create* ---> *Table*

![[Schermata del 2025-11-27 16-25-39.png]]
![[Schermata del 2025-11-27 16-27-09.png]]

Andiamo poi su ***Columns*** e definiamo le colonne che vogliamo inserire all'interno della tabella.
![[Schermata del 2025-11-27 16-28-37.png]]

Possiamo poi entrare nella tabella creata e inserire i dati che vogliamo:

> *Tasto destro* ---> *View/Edit data* ---> *All Rows*

![[Schermata del 2025-11-27 16-30-02.png]]

per inserire dei dati possiamo cliccare nell'icona dedicata e, una volta popolata con tutti i dati a nostra scelta dobbiamo ricordarci di caricarli nel database usando il tasto ***Save Data Changes***.

![[Schermata del 2025-11-27 16-31-58.png]]![[Schermata del 2025-11-27 16-32-09.png]]


<hr>


### Comunicare con un database tramite Java

Dobbiamo inizialmente specificare una ***Stringa di connessione*** per poter comunicare col Server, Questa avreà la seguente forma:

`JDBC:tipo:infoconn`

Avremo poi bisogno di usare un ***Driver*** che comunichi la connessione con il database al nostro programma di Java.

Questo richiede l'uso di ***DriverManager*** e in particolare del metodo ***openConnection***

Andiamo dunque in **Eclipse** e creiamo un nuovo progetto **Maven**.

> *Tasto destro sulla colonna progetti* ---> *New* ---> *Other* ---> ***Maven Project***

Assegnamoli il *Group Id* e l'*Artifact Id*.

![[Schermata del 2025-11-27 16-38-43.png]]

Vedremo quindi il progetto Maven creato nella sezione *Packages*.
Se tutto è corretto troveremo al suo interno un file ***pom.xml***, questo è il cuore del progetto, ci permette di dare qualsiasi istruzione al progetto Maven da eseguire.

Apriamo il file *pom.xml* e andiamo sulla repository maven central per ottenere la dependency da inserirci.![[Schermata del 2025-11-27 16-50-03.png]]

Creiamo un tag `<dependencies></dependencies>` e vi inseriamo al suo interno il codice evidenziato nell'immagine sovrastante.

![[Schermata del 2025-11-27 16-51-22.png]]

Andiamo poi nella cartella ***src/main/java*** e creiamo un nuovo file in cui andremo a testare la connessione appena creata.

In questa classe scriveremo un programma che ci permetterà di parlare direttamente con il database:

```Java
package it.corsoCyber2025.lezione9;

import java.sql.*;
import java.util.*;

public class ConnessioneDatabase {

    public static void main(String[] args) {
        PreparedStatement pStmt = null;

        ResultSet rs = null;

        try {
            final String url = "jdbc:postgresql://localhost:5432/CORSO";

            final Properties props = new Properties();

            props.setProperty("user", "postgres");

            props.setProperty("password", "password");

            Connection conn = DriverManager.getConnection(url, props);

            System.out.println(conn.getMetaData().getDatabaseProductVersion());

            pStmt = conn.prepareStatement(
                "Select \"COD\",\"DESCR\" from public.\"PROVINCE\""
            );

            rs = pStmt.executeQuery();

            while (rs.next()) {
                String cod = rs.getString("COD");

                String descr = rs.getString("DESCR");

                System.out.println(cod + " " + descr);
            }
        } catch (SQLException e) {
            System.out.println(
                "Error connecting to the Database " +
                Arrays.toString(e.getStackTrace())
            );
        } finally {
            try {
                rs.close();

                pStmt.close();
            } catch (Exception e) {}
        }
    }
}

```

Nella sezione di codice seguente abbiamo formattato i nomi delle colonne e della tabella perche se li chiamiamo con tutti caratteri maiuscoli andrà in conflitto. La best-practise è usare nomi con solo caratteri ***minuscoli*** per evitare conflitti nei programmi.

```Java
...
 pStmt = conn.prepareStatement(
                "Select \"COD\",\"DESCR\" from public.\"PROVINCE\""
            );
...
```

