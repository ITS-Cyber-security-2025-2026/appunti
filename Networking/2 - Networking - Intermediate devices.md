Tags: [[Networking]], [[ITS1]]

# L1 - Physical Layer

- Repeater
Possiede una singla porta. Prolunga un segnale elettrico, non è possibile trasmettere e ricevere allo stesso tempo perchè potrebbe verificarsi una collisione.

- HUB
è un Repeater con più porte, tuttavia può solo o trasmettere o ricevere (half-duplex).
Massimo 5 HUB, su 4 segmenti 3 devono essere popolati e 2 non devono essere popolati.

In un **HUB**, **tutti i dispositivi condividono lo stesso dominio di collisione**:  
cioè, se **due dispositivi trasmettono insieme**, i segnali **si scontrano** (collisione) e i dati vanno persi.

1 Dominio di collisione
1 Dominio di Broadcast

## L2 - Data Link Layer

- Bridge
Hardware che impara la topografia di rete. Questo connette due porzioni di rete, a differenza dell'HUB isola le collisioni. Le connessioni qua sono ancora Half-Duplex.

- Multi-port Bridge
Vi è una possibilità di collisione pari a 0, ma vi sono tanti domini di collisione quante porte connesse.
La possibilità di collisione è pari a zero perche la comunicazione qua avviene in Full-Duplex.

N° domini di collisione
1 Dominio di Broadcast

- Switch
Questo lavora al livello due usando la tabella CAM per immagazzinare gli indirizzi MAC address per creare una mappa logica degli host collegati nella stessa rete in modo da permettergli di comunicare.
Uno Switch può avere un indirizzo IP, questo serve solo alla gestione delle impostazioni dello Switch stesso da remoto.

## L3 - Networking Layer

- **Router**  
    È un dispositivo che **connette più reti diverse** tra loro e **instrada i pacchetti** in base agli **indirizzi IP** (non più MAC come al livello 2).  
    Il router crea una **tabella di routing** che gli permette di decidere il percorso migliore per inviare i pacchetti verso la destinazione.
    
    - Ogni interfaccia del router appartiene a una **rete diversa**.
        
    - Isola **i domini di broadcast** (a differenza di uno switch, che ne mantiene solo uno).
        
    - Funziona in **Full-Duplex**.
        
    
    🔹 **N° domini di collisione:** pari al numero di interfacce.  
    🔹 **N° domini di broadcast:** pari al numero di interfacce (ogni rete collegata è un dominio di broadcast separato).

- **Multilayer Switch (Layer 3 Switch)**  
    È uno switch che, oltre alle normali funzioni del livello 2, **può operare anche a livello 3**, cioè può **instradare pacchetti IP** come un router.
    
    - Utilizza **hardware dedicato (ASIC)** per eseguire il routing molto più velocemente rispetto a un router tradizionale (che lo fa via software).
        
    - Permette di separare le VLAN (Virtual LAN) e farle comunicare tra loro grazie al **routing inter-VLAN**.
        
    - Può avere indirizzi IP di gestione e indirizzi IP logici per le interfacce virtuali (SVI).
        
    
    🔹 **N° domini di collisione:** uno per ogni porta (come uno switch).  
    🔹 **N° domini di broadcast:** uno per ogni VLAN configurata.


