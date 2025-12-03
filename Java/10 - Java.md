Tags: [[ITS1]], [[Java]], [[PostGreSQL]]

Nella lezione precedente abbiamo visto la base di PostGreSQL: come creare un database e una query per popolarlo.

Abbiamo quindi visto la sezione `Tables`. Oggi vedremo invece la sezione `Sequences`, questa ci permette di inserire uno script in Java o SQL per automatizzare un processo.
Questi ci danno due vantaggi:
- Sono sicuri, perche interni al database
- Sono veloci.

Vi sono una serie di porte chiuse nel nostro PC, un programma di questo tipo può aprire una porta TCP per permettere la comunicazione con l'esterno.


<hr>


#### Driver
I driver sono classi java che implementano metodi definiti dalle specifiche JDBC.

Ce ne sono di 4 tipologie:
- Type 1 --> Collegamento JDBC - ODBC
- Type 2 --> Collegamento JDBC - Driver Nativo
- Type 3 --> Driver Java puro con accesso via  middleware
- Type 4 --> Driver Java puro con accesso diretto al DB

Il Type 4 è il più utilizzato, quelli precedenti sono deprecati e non più utilizzati.

***Ogni database ha un suo driver JDBC. Infatti lo si può scaricare in formato .jar sal sito web del fornitore del database.***

Questa libreria va messa nel ***classpath*** o aggiunta nel progetto del nostro applicativo se vogliamo farlo connettere a quel determinato database.


Come fa il mio programma a raggiungere il motore del database?
Devo specificare una ***Stringa di connessione***:

`JDBC    :    tipo    :    infoconn`

*tipo* ---> tipologia di database
*infoconn* ---> specifiche per localizzare l'istanza del database ed il database stesso. (URL)


<hr>


#### Driver Manager

Viene in seguito creato un oggetto di tipo *Connection* che rappresenta una connessione attiva con il database.
Questo si crea con il `DriverManager` usando il metodo 
`getConnection()`.

L'interfaccia di DriverManager offre diversi metodi per preparare le query SQL da inviare tramite oggetti, ad esempio:
- `Statement`
- `PreparedStatement`

`PreparedStatement` viene precompilato dalla connessione e risulta più efficente di `Statement`.


<hr>



Abbiamo modificato il codice creato la lezione precedente per includere il metodo `DriverManager`.

```Java
package it.corsoCyber2025.lezione9;

import java.sql.*;
import java.util.*;
import javax.swing.JOptionPane;

public class Province {

    private Map<String, String> province;

    public Province() {
        province = new HashMap<String, String>();

        // Carico le province

        Connection conn = null;

        PreparedStatement pStmt = null; // Statement ottimizato

        ResultSet rs = null; // Set dei risultati(righe della tabella)

        try {
            final String url = "jdbc:postgresql://localhost:5432/corso";

            final Properties props = new Properties();

            props.setProperty("user", "postgres");

            props.setProperty("password", chiediPassword());

            conn = DriverManager.getConnection(url, props);

            System.out.println("Connessione al database avvenuta");

            pStmt = conn.prepareStatement(
                "SELECT cod, descr FROM public.province ORDER BY cod"
            );

            rs = pStmt.executeQuery();

            while (rs.next()) {
                String cod = rs.getString("cod");

                String des = rs.getString("descr");

                province.put(cod, des);
            }
        } catch (Exception e) {
            System.err.println("Non sono riuscito a caricare le province.");

            System.err.println("errore: " + e.getLocalizedMessage());
        } finally {
            try {
                if (rs != null) {
                    rs.close();
                }

                if (pStmt != null) {
                    pStmt.close();
                }

                if (conn != null) {
                    conn.close();
                }
            } catch (Exception e) {
                // non faccio nulla

            }
        }
        /*

* province.put("VE", "Venezia"); province.put("PD", "Padova");

* province.put("VI", "Vicenza"); province.put("RO", "Rovigo");

* province.put("VR", "Verona"); province.put("BL", "Betlemme");

* province.put("TV", "Treviso");

*/

    }

    private String chiediPassword() {
        String password = null;

        password = JOptionPane.showInputDialog(
            null,
            "Inserisci la password utente postgres"
        );

        return password;
    }

    public String stampaDescrizione(String codiceProv) throws Exception {
        if (!province.containsKey(codiceProv)) {
            throw new Exception("Provincia non censita: " + codiceProv);
        }

        String descr = province.get(codiceProv);

        System.out.println("La descrizione è " + descr);

        return descr;
    }
}

```


In questo modo useremo il metodo `driverManager` per gestire l'accesso al database senza lasciare la password in chiaro nello script.
Abbiamo quindi scambiato la password in chiaro con una funzione che richiede all'utente la password, implementando cosi la sicurezza del nostro database.

