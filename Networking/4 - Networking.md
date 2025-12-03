Tags: [ITS1], [[Networking]]

<hr>


Siamo arrivati alla configurazione completa di piu reti interconnesse localmente.

Ora quindi sarebbe utile salvare tutte le impostazioni su un dispositivo che svolga solo questa funzione. Utilizzeremo quindi un Server TFTP.

Questo usa la porta 69 e lavora solamente con protocollo UDP.

Aggiungiamo quindi un Server TFTP assegnandoli fisicamente l'indirizzo 172.16.0.60.

![[Schermata del 2025-11-14 09-36-20.png]]

Ci spostiamo poi sullo switch a cui esso è direttamente collegato e copiamo la startup config nel Server TFTP per poi eliminare la startup config e forzare un riavvio con *reload* e vedere che lo switch è tornato allo stato iniziale.

```switch

Password:
SWA>ena
Password:
SWA#wr
Building configuration...
[OK]
SWA#copy start tftp
Address or name of remote host []? 172.16.0.60
Destination filename [SWA-confg]?
Writing startup-config...!!

[OK - 1430 bytes]
1430 bytes copied in 0 secs

SWA#
SWA#erase start
Erasing the nvram filesystem will remove all configuration files! Continue? [confirm]

[OK]
Erase of nvram: complete

%SYS-7-NV_BLOCK_INIT: Initialized the geometry of nvram
SWA#reload
Proceed with reload? [confirm]

C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(25r)FX, RELEASE SOFTWARE (fc4)

Cisco WS-C2960-24TT (RC32300) processor (revision C0) with 21039K bytes of memory.

2960-24TT starting...
...
```


Ora ci troviamo con uno switch reimpostato quasi alle impostazioni di fabbrica, proviamo quindi a riprendere le startup-config che abbiamo salvato nel Server TFTP.
Dobbiamo prima di tutto impostare l'interfaccia ip per raggiungere il Server TFTP, poi possiamo usare il comando *copy tftp start* per ottenere la startup-config salvata precedentemente.

N.B. è importante quando ci chiede di salvare dire di no perche senno al reload verrà caricata la startup-config nuova appena usata.

```switch
Switch>ena
Switch#
Switch#copy tftp start
Address or name of remote host []? 172.16.0.60
Source filename []? SWA-confg
Destination filename [startup-config]?
	Accessing tftp://172.16.0.60/SWA-confg....

	Loading SWA-confg from 172.16.0.60: !

[OK - 1430 bytes]

1430 bytes copied in 3.003 secs (476 bytes/sec)

Switch#reload
System configuration has been modified. Save? [yes/no]:no
Proceed with reload? [confirm]

C2960 Boot Loader (C2960-HBOOT-M) Version 12.2(25r)FX, RELEASE SOFTWARE (fc4)
...

```


Se volessimo invece salvare il sistema operativo stesso sul TFTP?
Possiamo fare lo stesso procedimento indicando flash al posto di start.
Salviamo l'attuale sistema operativo sotto SWA-Os.

```switch

SWA#copy flash tftp
Source filename []? 2960-lanbasek9-mz.150-2.SE4.bin
Address or name of remote host []? 172.16.0.60
Destination filename [2960-lanbasek9-mz.150-2.SE4.bin]? SWA-Os
		Writing 2960-lanbasek9-mz.150-2.SE4.bin...!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

	[OK - 4670455 bytes]

4670455 bytes copied in 0.021 secs (17880366 bytes/sec)

```

Ora immaginiamo di trovarci senza piu un sistema operativo.
Partiamo cancellandolo con *del flash* (da non fare MAI se non necessario), per poi usare lo stesso procedimentocon *copy tftp flash* per ricaricarlo nel flash dello Switch.

```switch

SWA#del flash
Delete filename []?2960-lanbasek9-mz.150-2.SE4.bin
Delete flash:/2960-lanbasek9-mz.150-2.SE4.bin? [confirm]

SWA#
SWA#copy tftp flash
Address or name of remote host []? 172.16.0.60
Source filename []? SWA-Os
Destination filename [SWA-Os]?
	Accessing tftp://172.16.0.60/SWA-Os...
	Loading SWA-Os from 172.16.0.60: !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

	[OK - 4670455 bytes]

4670455 bytes copied in 0.033 secs (11378415 bytes/sec)

SWA#show flash
Directory of flash:/
  3 -rw- 4670455 <no date> SWA-Os
  2 -rw- 1430 <no date> config.text
  
64016384 bytes total (59344499 bytes free)

```


<hr>


Poniamo l'esempio di voler connettere un nuovo router al Gateway, in questo caso le due interfacce fastEthernet saranno gia occupate, dobbiamo quindi inserire una scheda seriale per poter creare la connessione.

Dopo aver salvato le startup config nel server TFTP per evitarne la perdita, possiamo spegnerlo e inserire la nuova scheda seriale. All'avvio possiamo controllare che venga rilevato con *show version*.

```Gateway
GATEWAY#show version

Cisco IOS Software, 1841 Software (C1841-ADVIPSERVICESK9-M), Version 12.4(15)T1, RELEASE SOFTWARE (fc2)
...
...
	System image file is "flash:c1841-advipservicesk9-mz.124-15.T1.bin"
...
...
Cisco 1841 (revision 5.0) with 114688K/16384K bytes of memory.
Processor board ID FTX0947Z18E
M860 processor: part number 0, mask 49
2 FastEthernet/IEEE 802.3 interface(s)
	2 Low-speed serial(sync/async) network interface(s)
191K bytes of NVRAM.
63488K bytes of ATA CompactFlash (Read/Write)

Configuration register is 0x2102

```

Possiamo notare anche la presenza del nome dell'OS, utile in caso di troubleshooting o di settaggio iniziale.

Passiamo al nuovo Router e, come in precedenza, spegniamo il router e aggiungiamo la scheda seriale.

Che cavi vengono utilizzati per questa connessione? Avremo due tipologie di cavi: Seriale DTE e Seriale DCE.

- DCE - si può specificare la velocità di comunicazione sul cavo.

- DTE - si adatta alla velocità specificata dal DCE.

Andiamo dunque a collegare il nuovo Router con cavo DCE al GATEWAY, gli assegnamo una rete a sè 172.17.0.0/16 (GATEWAY .1 ; Router .2).

Aggiungiamo poi uno switch e un PC collegandoli fra di loro e poi al nuovo router.
(Router .254 ; nuova subrete 192.168.0.0/24)

Ora andiamo sul GATEWAY, entriamo nelle configurazioni dell'interfaccia seriale s0/0/0 e impostiamo il suo IP nella comunicazione seriale.

```GATEWAY

GATEWAY>ena
Password:
GATEWAY#
GATEWAY#conf t
Enter configuration commands, one per line. End with CNTL/Z.
GATEWAY(config)#interface s0/0/0
GATEWAY(config-if)#ip address 172.17.0.1 255.255.0.0
GATEWAY(config-if)#no shut
GATEWAY(config-if)#

```

Facciamo la stessa cosa dal nuovo Router aggiunto per attivare la comunicazione. Questo perche la comunicazione seriale va configurata da entrambi i lati della connessione. Vedremo quindi che alla sua accensione la linea si attiverà.

```R1

R1>ena
R1#conf t
Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#hostname R1
R1(config)#interface s0/0/0
R1(config-if)#ip address 172.17.0.2 255.255.0.0
R1(config-if)#no shut
R1(config-if)#
	%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up
R1(config-if)#
	%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

R1(config-if)#

```

Ora configuriamo l'interfaccia fastEthernet 0/0.
Assegnamo anche l'helper address per ottenere gli indirizzi IP in questa rete in maniera dinamica.

```R1

R1(config-if)#interface fa0/0
R1(config-if)#ip address 192.168.0.254 255.255.255.0
R1(config-if)#no shut
R1(config-if)#
	%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

R1(config-if)#
R1(config-if)#ip helper-address 172.16.0.55
R1(config-if)#

```

Ora pero ancora non possiamo ottenere un IP dinamico, andiamo nel Server DHCP e impostiamo un nuovo Pool per questa nuova rete.

![[Schermata del 2025-11-14 11-04-27.png]]


Impostiamo ora una route statica per permettere al Router1 di arrivare al Server DHCP.
Dovremo specificargli l'interfaccia da usare per raggiungere le reti 10.0.0.0 - 255.0.0.0 e la rete 172.16.0.0 - 255.255.0.0.

```R1

R1#conf t
Enter configuration commands, one per line. End with CNTL/Z.

R1(config)#ip route 10.0.0.0 255.0.0.0 s0/0/0
R1(config)#ip route 172.16.0.0 255.255.0.0 s0/0/0

```

Controlliamo la tabella di routing per confermare le route statiche con *show ip route*.

```R1

S 10.0.0.0/8 is directly connected, Serial0/0/0
S 172.16.0.0/16 is directly connected, Serial0/0/0
C 172.17.0.0/16 is directly connected, Serial0/0/0
C 192.168.0.0/24 is directly connected, FastEthernet0/0

```

Bisogna SEMPRE inserire anche la route statica di ritorno, in questo momento il GATEWAY, una volta che riceve il pacchetto DHCP, non ha idea di dove inoltrarlo. Perche non ha alcuna route per il pacchetto nella sua routing table.

```GATEWAY

GATEWAY>ena
Password:
GATEWAY#
GATEWAY#conf t
	Enter configuration commands, one per line. End with CNTL/Z.
GATEWAY(config)#ip route 172.17.0.0 255.255.0.0 s0/0/0
...
...
GATEWAY#show ip route
C 10.0.0.0/8 is directly connected, FastEthernet0/1
C 172.16.0.0/16 is directly connected, FastEthernet0/0
	C 172.17.0.0/16 is directly connected, Serial0/0/0

```

Dobbiamo farlo ANCHE per la rete 192.168.0.0/24.
Questo per potergli permettere di inoltrare il pacchetto fino al Client finale.

```GATEWAY

GATEWAY#
GATEWAY#conf t
	Enter configuration commands, one per line. End with CNTL/Z.
GATEWAY(config)#ip route 192.168.0.0 255.255.255.0 s0/0/0
...
...

```

Ora se sul PC Client Proviamo ad ottenere un IP dinamico funzionerà.

Se un informazione arriva su un router ma non è indirizzata a nessun indirizzo presente nella routing table possiamo inserire un *Gateway of Last Resort*.

Possiamo impostarlo con: *ip route 0.0.0.0 0.0.0.0 s0/0/0*

<hr>

Vogliamo ora sostituire il protocollo statico sul GATEWAY con un protocollo RIP.
Andiamo in primis a cancellare la route verso la rete 192.168.0.0/24

```GATEWAY

GATEWAY(config)#no ip route 192.168.0.0 255.255.255.0 s0/0/0

```

Facciamo lo stesso nel Router1

```R1

R1(config)#
R1(config)#no ip route 10.0.0.0 255.0.0.0 s0/0/0
R1(config)#no ip route 172.16.0.0 255.255.0.0 s0/0/0

```

Ora possiamo configurare il protocollo di routing RIP.
Iniziamo usando il comando *router rip* per entrare nella modalita di configurazione del protocollo RIP.
Indichiamo poi le reti vicine che gia conosciamo con il comando network >ip addr<.

```R1

R1(config)#router rip
R1(config-router)#network 192.168.0.0
R1(config-router)#network 172.17.0.0
R1(config-router)#

```

Dobbiamo poi anche configurare il GATEWAY per accettare il protocollo RIP.

```GATEWAY

GATEWAY(config)#router rip
GATEWAY(config-router)#network 172.17.0.0
GATEWAY(config-router)#network 10.0.0.0
GATEWAY(config-router)#network 172.16.0.0
GATEWAY(config-router)#

```

se controlliamo l'IP route vedremo in fondo la route RIP:

```GATEWAY

GATEWAY(config-router)#do show ip route
C 10.0.0.0/8 is directly connected, FastEthernet0/1
C 172.16.0.0/16 is directly connected, FastEthernet0/0
C 172.17.0.0/16 is directly connected, Serial0/0/0
R 192.168.0.0/24 [120/1] via 172.17.0.2, 00:00:22, Serial0/0/0

```


Se al posto del nostro PC Client ci fosse un attore malevolo, che si finge router e si mette in ascolto per i pacchetti RIP, avremmo al momento una grave vulnerabilità.
Gli stiamo comunicando l'architettura della nostra rete.

Fondamentale inviare pacchetti di RIP solo dove vi è un Router a riceverli e utilizzarli.

Andiamo quindi su Router e GATEWAY e blocchiamo la comunicazione di pacchetti RIP mettendo l'interfaccia in modalità *Passiva*.

Il comando è ***passive-interface >interfaccia<*** nella modalità router rip.

```GATEWAY

GATEWAY(config)#router rip
GATEWAY(config-router)#passive-interface Fa0/1
GATEWAY(config-router)#passive-interface Fa0/0

```

```R1

R1(config)#route rip
R1(config-router)#passive-interface fa0/0

```


<hr>


### NAT

Gli indirizzi IPv4 si dividono in classi, queste sono utili ai router, vediamo come distinguerle:

- A - prima cifra dell'indirizzo da 0 a 127, il 127 non si può assegnare perchè appartiene al localhost (loopback). Queste sono sempre in /8;
- B - prima cifra dell'indirizzo da 128 a 191. Queste sono sempre in /16;
- C - prima cifra dell'indirizzo da 192 a 223. Queste sono sempre in /24;
- D - prima cifra dell'indirizzo da 224 a 239 non sono indirizzi assegnabili ad una macchina, sono indirizzi multicast;
- E - prima cifra dell'indirizzo da 240 a 255;

Un indirizzo IPv4 si dice di rete quando gli ultimi 3 ottetti equivalgono a 0. (es. 10.0.0.0)
Il primo indirizzo di una rete sarà sempre l'indirizzo di rete mentre l'ultimo (es. 10.255.255.255) sarà sempre l'indirizzo di broadcast.

Nel range di classe A gli indirizzi privati vanno da ***10.0.0.0 a 10.255.255.255***.

Nel range di classe B gli indirizzi privati vanno da ***172.16.0.0 a 172.31.255.255***.

Nel range di classe C gli indirizzi privati vanno da ***192.168.0.0 a 192.168.255.255***.

Nel range di classe D troviamo gli indirizzi di *multicast*.

Nel range di classe E inizialmente erano a scopo sperimentale ma ora sono stati distribuiti come indirizzi pubblici.


Il NAT (Network Address Translation) permette di tradurre indirizzi per assecondare le richieste fatte.
Un esempio è quello della richiesta di una pagina web: un indirizzo privato andrà a richiedere un indirizzo IP sulla porta 80, il NAT del Router tradurrà l'indirizzo privato in pubblico in modo da poter esaudire la richiesta fatta.

#### Configurazione NAT

1. Identificare parte ***Inside*** (indirizzi da tradurre) e parte ***Outside*** (indirizzi su cui operare la traduzione).
```RouterGateway

R(config)#interface Fa0/0
R(config-if)#ip nat inside
...
R(config)#interface Fa0/1
R(config-if)#ip nat outside

```

2. Creare il Pool di indirizzi pubblici su cui  operare la traduzione
   N.B. il nome del pool è case-SENSITIVE.
```RouterGateway

R(config)#ip nat pool >nome pool< 50.0.0.1 50.0.0.1 netmask 255.0.0.0
	questo è il range di indirizzi ^^^^^^^^^^^^^^^ pubblici

```

3. Indentificare il traffico autorizzato alla traduzione con un access-list.
   Possiamo definire regole *allow* o *deny*, queste seguono un ordine gerarchico, quella più in alto verrà letta per prima.
   Se non specifichiamo le regole il default è *deny*
```RouterGateway

HOMEGW(config)#access-list 10 permit host 10.0.0.1
HOMEGW(config)#access-list 10 permit host 10.0.0.2

```

4. Legare il Pool creato e i permit dell'access-list creata.
```RouterGateway

HOMEGW(config)#ip nat inside source list 10 pool >nome pool<

```
con questa configurazione solo il primo host a collegarsi ad internet ci riuscirebbe, gli altri dovrebbero aspettare la fine della sua connessione.
Per ovviare a questo problema e creare un PAT (Port Access Translation)usiamo la keyword ***overload***, questo prenderà in considerazione la combinazione di indirizzo privato e porta sorgente.
```RouterGateway

HOMEGW(config)#ip nat inside source list 10 pool >nome pool< overload

```

6. Ricordarsi di definire la *default-route (last-resort route)* .
```RouterGateway

HOMEGW(config)#ip route 0.0.0.0 0.0.0.0 50.0.0.2

```


Per controllare se il NAT/PAT ha funzionato dobbiamo controllare se sono state fatte delle traduzioni.
```RouterGateway

HOMEGW#sh ip nat translations

	Pro  Insideglobal    Insidelocal     Outsidelocal    Outsideglobal
	tcp  50.0.0.1:1025   10.0.0.1:1025   60.0.0.100:80   60.0.0.100:80

```



Per permettere agli host collegati alle sottoreti di omunicare verso l-esterno dobbiamo infine indicare nel GATEWAY di passare col protocollo RIP di passare anche la default-route.

```RouterGateway

GATEWAY(config-router)#default-information originate

```

