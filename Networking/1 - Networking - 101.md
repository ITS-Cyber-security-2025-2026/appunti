Tags: [[ITS1]], [[Networking]]

Una **rete** è un sistema di comunicazione tra dispositivi interconnessi.

Per comunicare questi dispositivi hanno bisogno di regolo, queste vengono definite **Protocolli**.

I diversi Protocolli sono stati messi dentro dei livelli, a seconda del loro utilizzo, per non andare in conflitto e ogni livello offre servizi al livello che gli sta sopra.

# Ogni livello offre servizi al livello superiore

Iniziamo parlando del modello ISO/OSI

### ISO/OSI

#### 7 - Application Layer

Al suo interno ha protocolli che offrono servizi alle applicazioni di rete. Non per forza direttamente all'utente finale.

#### 6 - Presentation Layer

Al suo interno non contiene Protocolli ma **codifiche**.
Queste permettono di codificare/decodificare i dati in arrivo per essere letti e visualizzati dall'**application layer**.

#### 5 - Session Layer

Commutazione di pacchetto


#### 4 - Transport Layer

Questo, assieme al livello fisico, sono i livelli più complessi.


#### 3 - Network Layer

Qui vi troviamo protocolli di routing e protocolli routed.
es. ICMPv4, ICMPv6


#### 2 - Data Link Layer

Possiamo trovare qua le schede di rete. Queste vedono solo dei dati da dover trasportare.
Dalla metà superiore di questo livello si parla di software, da quella inferiore invece si parla di Hardware.
es. Switch

#### 1 - Physiscal Layer

Qui si parla puramente di Hardware.


<hr>

Vi sono diversi tipi di dispositivi che lavorano sui vari livelli

<hr>


### TCP/IP

#### 4 - Application Layer

Questo livello contiene i **livelli 7,6, 5** dell' ISO/OSI model.

#### 3 - Transport Layer

Questo livello equivale al **livello Transport** ISO/OSI model.

#### 2 - Internet Layer

Questo equivale al **livello Network** dell' ISO/OSI model.

#### 1 - Network Access Layer

Questo livello comprende i **livelli 2 e 1** dell'ISO/OSI model.



<hr>



| ISO/OSI                | TCP/IP |
| ---------------------- | ------ |
| 7 - Application Layer  | 4      |
| 6 - Presentation Layer |        |
| 5 - Session Layer      |        |
| 4 - Transport Layer    | 3      |
| 3 - Network Layer      | 2      |
| 2 - Data Link Layer    | 1      |
| 1 - Physical Layer     |        |
|                        |        |


<hr>


## 7 - Application Layer

I dati arrivano dall'applicazione dell'utente e il livello Application li traduce utilizzando i protocolli.

- Per la comunicazione web si utilizza il protocollo HTTP (HyperText Transfer Protocol).
- Quando voglio raggiungere un sito scrivendo il suo nome utilizzeremo il protocollo DNS (Domain Name System).
	Parlando di DNS possiamo vederlo in funzione usando il comando ```nslookup```. Questo risolve il nome del sito web nell'indirizzo del server associato.
	Quando vi troviamo scritto che è una ```non-authoritative answer``` significa che la risposta non è stata fornita direttamente dal server contattato ma che l'informazione è stata trovata in un registro.
		Questa informazione è considerata **record di tipo A**.

Vi sono diversi tipi di record:
	A --> risolve il nome dominio nell' indirizzo server.
	NS --> risolve il nome dominio in indirizzo server DNS.
	CNAME --> risolve gli alias del dominio nel vero e proprio.
	MX --> risolve tutti i domini in server di posta elettronica

- Per la communicazione via posta utilizziamo l'SMTP (Server Message Transfer Protocol).
- Per il trasferimento dati utilizzeremo l'FTP (File Transfer Protocol).


## 6 - Presentation Layer

Qui i dati diventano codificati per la visualizzazione tramite il livello Applicativo.


## 5 - Session Layer

Qui avviene la commutazione di pacchetto, si prende il dato da trasferire e si suddivide in "pezzi" più piccoli e facili da trasportare.

## 4 - Transport Layer

In questo livello si prende ognuno dei "pezzi" precedentemente creati e vi si aggiugono pezzi di informazioni, qui diventano **Segmenti**. Inizialmente vi si inserisce un'intestazione che conterrà diverse informazioni quali:
- Porta sorgente --> serve ad indentificare i punti d'uscita verso cui reindirizzare la risposta del server.
- Porta Destinazione --> porta verso cui indirizzare la richiesta

Queste ci servono per indirizzare il dato nella direzione corretta.
Vi sono 65525 porte utilizzabili, alcune sono pre-assegnate, le prime 1024 sono **Well-known ports**


Questi "pezzi di dati" a cui viene associato un header viene chiamato al livello 4 ***Segmento***.

I protocolli di trasporto principali sono solitamente **TCP** e **UDP**.


|     | **TCP**                      | **UDP**                                   |                                                                                      |
| --- | ---------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------ |
| 1   | ports                        | ports                                     | utilizzano entrambi le porte per comunicare                                          |
| 2   | orientato alla connessione   | Non orientato alla connessione            | necessità di stabilire una connessione stabile per comunicare (Three-Way Handshake)  |
| 3   | numera i segmenti            | non numera i segmenti                     | permette di effettuare ulteriori controlli sui segmenti                              |
| 4   | ritrasmette i segmenti persi | non si interessa della perdita di dati    | viene controllato l'integrità durante la trasmissione.                               |
| 5   | controllo del flusso di dati | non effettua controlli sui flussi di dati | permette di regolare la trasmissione dei dati in base alla capacità delle due parti. |


## 3 - Network Layer

Vengono presi i dati dal Layer precedente, distinguendo se segmento TCP o UDP e vi si aggiungono l'IP Sorgente e l'IP Destinatario.

- IP Sorgente --> identifica l'host di provenienza
- IP Destinatario --> Identifica l'host di destinazione

Qui vi sono 2 classi di algoritmi fondamentali: protocolli **Routing** e **Routed**.

> **Routed**
> 	IPV4 --> identifica un indirizzo in formato x.x.x.x dove x equivale ad un numero compreso tra 0 e 255. Questi indirizzi sono a 32bit in quanto ogni ottetto equivale ad 8bit, da qui il nome stesso.
> 	
> 	IPV6 --> identifica un indirizzo in formato x:x:x:x:x:x:x:x dove x equivale ad un valore esadecimale. 
> 		2001:0DB8:ACAD:0001:0000:0000:0000:0000
> 		equivale a 
> 		2001:DB8:ACAD:1:0:0:0:0
> 		piu compatto si puo scrivere
> 		2001:DB8:ACAD:1::
> 		
> Tutti questi sono **indirizzi logici**. Questo perche possono essere assegnati secondo una logica predeterminata.

Nel Network Layer, una volta assegnati gli indirizzi i segmenti prendono il nome di **Pacchetti**.

> **Routing**
> Vengono distinti in algoritmi di Routing **Interni (IGP - internal gateway protocol)** e **Esterni (EGP - external gateway protocol)**.
> 
> 	**IGP** 
> 	Ci fanno muovere internamente nei sistemi autonomi il piu efficacemente possibile.
> 	Vi sono 3 diverse tipologie di protocolli:
> 	**Distance Vector** --> RIP, IGRP
> 	**Link State** --> OSPF, ISIS
> 	**Hybrid** --> EIGRP
> 
> 	**EGP**
> 	Quando si arriva al confine di un sistema autonomo questo algoritmo ci permette di instradare il pacchetto verso la strada piu adeguata.
> 	**Path Vector** e **Cost Vector** --> BGP = Border Gateway Protocol



## 2 - Data Link

Prende i pacchetti dal livello superiore per come sono, non ha bisogno di interpretarli.
Questo è il primo livello che inserisce un **header** ed un **trailer**.

Nell'header vi sono 2 indirizzi, equivalenti alle schede di rete, **MAC Sorgente** e **MAC Destinazione**.

Aggiunge inoltre alla fine del pacchetto un **FCS Frame Check Sequence**, questo controlla l'integrità dei pacchetti.

Mentre l'IP è un indirizzamento logico e gerarchico, Il MAC è Fisico e FLAT.
Fisico --> non puo essere cambiato
FLAT --> identificabile 

Una volta aggiunte queste informazioni, i **pacchetti** cambieranno nome in **Frames**.

**Bandwidth** = Larghezza di banda, quanto posso inviare.
**Throughput** = L'interezza del pacchetto ricevuto/inviato
**Goodput** = La parte di Frame senza Header e Trailer

#### All'interno del Data Layer troviamo due diverse sezioni:

- **LLC** --> (Logical Link Control) si occupa di prendere i pacchetti dal livello superiore e inoltrarli al livello successivo
- **MAC** --> (Media Access Control) Spiega le regole con cui accedere ai media fisici (es. Ethernet)

- **CSMA/CD (Carrier Sense Multiple Access with Collision Detection)**:  
    Metodo usato nelle reti **Ethernet cablate**. I nodi ascoltano il canale prima di trasmettere; se avviene una **collisione**, la rilevano e **interrompono** la trasmissione, poi **ritentano dopo un tempo casuale**.
    
- **CSMA/CA (Carrier Sense Multiple Access with Collision Avoidance)**:  
    Metodo usato nelle reti **Wi-Fi (wireless)**. I nodi cercano di **evitare** le collisioni aspettando un tempo casuale prima di trasmettere e usando **meccanismi di conferma (ACK)** per assicurarsi che il messaggio sia arrivato.


Per evitare le Collisioni in ambito industriale si utilizzava la Topologia **Token Ring**.
Le collisioni dipendono principalmente dalla Topologia che si decide di usare.


## Physical Layer

Questo livello comprende tutto ciò che ha a che fare con i media fisici (hardware) di un'infrastruttura.

Qui i dati da noi comunicati sono trasformati in bit, impulsi elettrici o impulsi luminosi, a seconda della tecnologia usata a livello fisico.

Vi sono due azioni in questo livello: **CODING e SIGNALING**

- Coding = la scelta di come trasmettere i file a livello elettrico/ottico
	 +5V = Bit 1
	 -5V = Bit 0
