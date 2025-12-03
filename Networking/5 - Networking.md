Tags: [[ITS1]], [[Networking]]

### Port Forwarding

Se io mi trovo su una rete pubblica non mi posso connettere un servizio interno alla mia rete privata.
Avrò bisogno di fare il ***port forwarding***, questo ci permette di specificare al Gateway che, in seguito ad una richiesta proveniente dall'esterno per una porta specifica, questa verrà reindirizzata verso un indirizzo ed una porta interno solitamente non raggiungibile dall'esterno.

Consideriamo che:
>se cerchiamo di connettere il gateway 100.0.0.2 su porta 8088 (porta predeterminata per la regola)
---> verremo invece inoltrati all'indirizzo 172.19.0.200 porta 80 (porta su cui risponde la webcam ufficio)

>se cerchiamo di connettere il gateway 100.0.0.2 su porta 8033 (porta predeterminataper la seconda regola)
---> verremo invece inoltrati all'indirizzo 172.19.0.201 porta 80 (porta su cui risponde la webcam giardino)


Andremo quindi sul GATEWAY e per la WebCam giardino inseriremo:
```GATEWAY

GATEWAY>
GATEWAY>ena
GATEWAY#conf t
Enter configuration commands, one per line. End with CNTL/Z.

GATEWAY(config)#ip nat inside source static ?
	A.B.C.D Inside local IP address
	tcp Transmission Control Protocol
	udp User Datagram Protocol

GATEWAY(config)#ip nat inside source static tcp 172.19.0.201 80 100.0.0.2 8033

GATEWAY(config)#

```

>**Traduzione**:
> ip nat inside source static >protocollo usato< >ip del servizio interno + porta< >ip richiesto da fuori + porta< 

Vediamo quindi per la WebCam in ufficio che il comando sarà il seguente:

```GATEWAY

GATEWAY(config)#ip nat inside source static tcp 172.19.0.200 80 100.0.0.2 8088

```


Per visualizzare le ***traduzioni NAT*** possiamo fare come di seguito:

```GATEWAY

GATEWAY(config)#do show ip nat translation

Pro Inside global Inside local Outside local Outside global
tcp 100.0.0.2:1029 192.168.0.100:1029 80.0.0.100:80 80.0.0.100:80
tcp 100.0.0.2:1030 192.168.0.100:1030 80.0.0.100:80 80.0.0.100:80
tcp 100.0.0.2:8033 172.19.0.201:80 --- ---
tcp 100.0.0.2:8088 172.19.0.200:80 --- ---

GATEWAY(config)#do clear ip nat translation *
GATEWAY(config)#
GATEWAY(config)#do show ip nat trans

Pro Inside global Inside local Outside local Outside global
tcp 100.0.0.2:8033 172.19.0.201:80 --- ---
tcp 100.0.0.2:8088 172.19.0.200:80 --- ---

GATEWAY(config)#

```

> N.B. i port forwarding non verranno mai tolti dalla tabella di traduzione NAT.

Se proviamo ora ad accedere dall'esterno alla Webcam Giardino tramite l'indirizzo 100.0.0.2:8033 riusciremo a raggiungerla.


<hr>


### Subnetting


Quando ci troviamo con una rete al cui interno vi sono molti Host avremo un'organizzazione molto confusa.

Sarebbe quindi utile avere un modo per suddividere una rete in tante diverse ***sottoreti*** che distinguono i vari piani/settori/uffici di un'azienda.
Le motivazioni dono diverse, le più importanti sono:
- per posizione;
- per gruppo o funzione;
- per tipologia di Device usato;

Ci vieni dunque in aiuto il ***subnetting*** che svolge esattamente questa funzione.

*Come calcoliamo però ciascuna porzione di network e come suddividerlo?*

Prendiamo l'esempio di una rete **172.16.0.0/16** .

--> sappiamo che 172.16. | 0.0        (perchè la rete è in /16)
			Network   |   Host

Come si crea una subnet mask?

![[Schermata del 2025-11-21 17-19-51.png]]

Possiamo prendere in prestito la prima porzione di Host e suddividerla in una sottorete, questa diventerà in /24 perche appunto gli abbiamo rubato un ottetto alla subnet mask.

Potremo quindi creare fino a 256 subnet di cui 2 li perderemo perchè uno sarà **l'indirizzo di rete (172.16.0.0/24)** e uno di **broadcast (172.16.255.0/24)**.

![[Schermata del 2025-11-21 14-28-28.png]]

Posso inoltre prendere in prestito da una sottorete altri bit per creare un'uteriore sottorete, quindi:

**172.16.0.0/24**  --->  **172.16.0.0/26** , **172.16.64.0/26** , **172.16.128.0/26** , ecc.. incrementando di 2 elevato al numero di bit rimasti.


![[Schermata del 2025-11-21 14-38-36.png]]

Incrementiamo di **64** perche l'incremento di calcola elevando al numero di posizioni rimanenti il numero di bit presi in prestito.

> 2 bit su 8 presi in prestito ==> 2^6 = 64.

E se volessimo fare il subnetting di una rete in /26?

![[Schermata del 2025-11-21 15-13-55.png]]

Nel caso di una rete in /15 procederemo come segue:

![[Schermata del 2025-11-21 15-33-55.png]]

#### Questo si chiama FLSM (Fixed Length Subnet Mask)
Creiamo una subnet di una lunghezza fissa.


Se volessimo ottimizzare gli Host assegnabili ad una rete possiamo fare subnetting tenendo in considerazione quanti host vogliamo assegnare a ciascuna sottorete.
Immaginiamo dunque una rete come segue:
![[Schermata del 2025-11-21 17-04-50.png]]

Teniamo a mente che per sapere il numero di Host in base ai bit della subnet mask possiamo basarci su questo schema:

![[Schermata del 2025-11-21 17-06-03.png]]


Possiamo quindi calcolarci le porzioni di subnetting riadattate in base agli Host cosi:

![[Schermata del 2025-11-21 17-07-48.png]]

Se dovessi sforare il numero di Host (es. 500), calcolo a ritroso quindi se:
256 Host ---> 8 bit
512 Host ---> ***9 bit***

Se invece ci troviamo con un numero molto alto di Host possiamo ragionare per approssimazione e contare quanti range di 250 ci servono per raggiungere il numero di Host richiesti.

Esercizio di Esempio:

> Partendo dalla rete 172.16.0.0/16 fai il subnetting in base al numero di host richiesto.

![[Schermata del 2025-11-21 19-12-21.png]]
