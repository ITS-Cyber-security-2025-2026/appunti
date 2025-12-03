Tags: [[ITS1]], [[Networking]]

### IPv6

Alla fine degli anni 90 vi è stato il bisogno di creare un alternativa ad IPv4, questo perche si prevedeva che presto si sarebbero esauriti gli indirizzi pubblici.
Sono stati dunque creati gli indirizzi IPv6 per ovviare a questo problema.

A livello di proprietà non cambia molto, possiedono le stesse caratteristiche se non che IPv6 si scrive in Esadecimale e che si perde il concetto di indirizzo pubblico e privato.

Inoltre è importante sapere che non esiste il concetto di Broadcast in IPv6, si inviano infatti messaggi di Multicast a tutti ma non si definiscono di Broadcast.

Non abbiamo però disattivato gli IPv4 dopo la creazione degli IPv6, attualmente questi coesistono e a permetterlo sono 3 metodi:

- ***Dual Stack*** --> gli host fanno girare entrambi allo stesso tempo, IPv4 ***+*** IPv6.
- ***Tunneling*** ---> I pacchetti IPv6 inviati vengono tunnelizzati sotto forma di pacchetto IPv4
- ***Translation*** ---> Non esiste più il concetto di NAT, la traduzione avviene tra protocolli IPv4 e IPv6.

##### Come si presenta un indirizzo IPv6?

![[Schermata del 2025-11-28 09-31-25.png]]

Un indirizzo IPv6 ha in totale 64 bit.
La prima porzione di 48 bit indica il ***Global Prefix***.
La quarte porzione di 16 bit rappresenta invece la porzione di ***Subnetting***.
Gli ultimi 64 bit rappresentano invece la porzione di ***Host*** che, nel caso dell IPv6, si chiamerà ***Interface ID***.
![[Schermata del 2025-11-28 10-12-09.png]]

Dunque i ***primi 4 Hextetti*** si chiamano ***Prefisso***, gli ***ultimi  4 Hextetti*** sono invece l'***Interface ID***.


Nella rappresentazione di un indirizzo IPv6 si applica la Regola dell' ***Omit Leading Zeros***.
Quando si scrive un indirizzo IPv6, ogniqualvolta vi sono degli zero all'inizio di un Hextetto questi vengono ***omessi*** per facilità di scrittura.
Al loro posto si inseriscono i ***due punti due volte***.
Possiamo farlo solo una volta per indirizzo, questo perchè altrimenti vi sarebbero delle ambiguità.

> *Esempio*
> IPv6 ---> **2001 : 0DB8 : ACAD : 0001 : 0000 : 0000 : 0000 : 0001**
> Diventa prima --> **2001 : DB8 : ACAD : 1 : 0 : 0 : 0 : 1**
> IPv6 dopo la regola ---> **2001 : DB8 : ACAD : 1 : : 1**


Vi sono diverse tipologie di indirizzi nell'IPv6:

- ***Unicast***
- ***Multicast***
- ***Anycast***

#### Unicast

Questi identificano in modo univoco un'interfaccia su un dispositivo abilitato IPv6.

Suddividiamo gli indirizzi ***Unicast*** in sottocategorie:

- **Global Unicast** --> è simile ad un IPv4 pubblico, questi sono *univoci* e *instradabili* su internet.
- **Link-Local** --> Vengono usati per comunicare tra dispositivi nello stesso *Link-Locale*.
- **Loopback** --> Indirizzo della macchina locale (si scrive ***::1***).
- **Unspecified Address** --> Rappresentato dai doppi due punti ( ***: :*** ), indica ***tutte le reti***.
- **Unique Local*** --> equivalenti agli indirizzi privati IPv4.
- **Embedded IPv4** ---> ***Da evitare***.


<hr>

### Link-Local

Nel momento in cui un colleghiamo due dispositivi in una rete *Link-Local*, questi genereranno un indirizzo randomico *Link-Local* (in caso di conflitto vi è un protocollo che lo risolve).

I vari dispositivi in una rete apparterranno ad una rete Link-Local per ogni porta connessa. Queste non comunicheranno fra di loro,,possiamo dunque assegnare uno stesso indirizzo *Link-Local* a più porte e non andranno mai in conflitto.

Una volta assegnato l'indirizzo di rete ad un router, questo sarà il *Gateway* per gli altri dispositivi.
Gli altri dispositivi invieranno dei pacchetti ***LLR*** (Link Local Request) per ottenere la prima porzione dell'indirizzo di rete, a cui aggiungeranno la seconda porzione, generata randomicamente o derivata dall'indirizzo MAC.


<hr>


### IPv6 in Packet tracer

Dopo aver creato una piccola rete con ***Gateway, Switch, 1 PC e 1 Server***, andiamo nella CLI Gateway.
Qui **DOBBIAMO** attivare l'IPv6 con il seguente comando:

```GATEWAY

Router>ena
Router#conf t
	Enter configuration commands, one per line. End with CNTL/Z.

Router(config)#hostname GATEWAY
GATEWAY(config)#do wr
	Building configuration...

[OK]
GATEWAY(config)#ipv6 unicast-routing

```

Possiamo poi assegnare gli indirizzi *link-local* e *global* e attivare poi l'interfaccia.

```GATEWAY

GATEWAY(config)#interface fa0/0
GATEWAY(config-if)#ipv6 address fe80::1 link-local <-- Link-local address
GATEWAY(config-if)#ipv6 address 2001:db8:acad:1::1/64 <-- Global address
GATEWAY(config-if)#no shut
GATEWAY(config-if)#

	%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

```


Possiamo ora modificare l'indirizzo *link-local* in modalita statica del PC in **FE80::2**.

Andiamo poi nel ***Server*** e gli assegnamo IPv6 Address, LLAddress e Gateway in modalità statica. Questo perche su TUTTI i device chiave nell'infrastruttura è meglio che abbiamo queste impostazioni statiche, ondevitare che, se dovessero spegnarsi e riaccendersi, debbano essere ri-configurati tutti i dispositivi dipendenti da essi.

Abbiamo poi connesso un *Router ISP*, un altro *Switch* e altri *due server* e li abbiamo configurati alla stessa maniera con indirizzi ovviamente differenti.

Attiviamo ora il protocollo ***RIP*** all'interno delle nostre reti.
Andiamo sul GATEWAY, selezioniamo le interfacce connesse e attiviamo il protocollo ***RIP*** come di seguito.

```GATEWAY

GATEWAY#conf t
	Enter configuration commands, one per line. End with CNTL/Z.

GATEWAY(config)#interface fa0/0
GATEWAY(config-if)#ipv6 rip cisco enable
GATEWAY(config-if)#interface fa0/1
GATEWAY(config-if)#ipv6 rip cisco enable

```

Facciamo lo stesso procedimento per il *Router ISP*.

Ora aggiungiamo altri due router, questi saranno 2 Gateway distinti: uno abilitato solo al protocollo IPv4 e l'altro sia per IPv4 che IPv6.
Ci aggiungiamo anche Switch e PC nell Router GW4 e 2 Switch e 2 Server al Router GW46.

Abilitiamo tutte le interfacce e configuriamole.
Abbiamo configurato tutto, compreso il protocollo RIP IPv4, però se volessimo far comunicare le due reti IPv6 avremmo bisogno di creare un ***Tunnel virtuale***.

Definiamo la porzione di rete del tunnel, poi andiamo nella CLI del ISP.
Qui configuriamo il tunnel, una volta che accediamo questa interfaccia verrà creata in automatico.
Dobbiamo poi configurarla come una interfaccia IPv6 e attivare il protocollo di RIP IPv6.
Infine è necessario dichiarare dove inizia il Tunnel (***Source***) e dove finisce (***Destination***).
Il *Source* verrà definito dall'interfaccia da dove inizia il Tunnel mentre la *Destination* verrà definito dall'indirizzo IP dell'interfaccia di arrivo del tunnel.

```ISP

ISP>ena
ISP#conf t
	Enter configuration commands, one per line. End with CNTL/Z.

ISP(config)#interface tunnel0
ISP(config-if)#
	%LINK-5-CHANGED: Interface Tunnel0, changed state to up

ISP(config-if)#ipv6 address 3000::1/64
ISP(config-if)#ipv6 address FE80::1 link-local
ISP(config-if)#ipv6 rip cisco enable
ISP(config-if)#tunnel source s0/0/0
ISP(config-if)#tunnel destination 70.0.0.2
ISP(config-if)#
	%LINEPROTO-5-UPDOWN: Line protocol on Interface Tunnel0, changed state to up

ISP(config-if)#tunnel mode ipv6ip
ISP(config-if)#

```


<hr>


### VLAN IPv4

![[Schermata del 2025-12-02 15-52-55.png]]

Vi sono solo due modi in cui uno Switch può inoltrare i frame:
- ***Store and Forward***
- ***Cut Through***

---> **Store and Forward:** Lo switch riceve tutto il frame, lo controlla per eventuali errori con l'FCS, se è tutto ok lo inoltra sennò lo scarta. Questo produce una maggiore latenza.

---> **Cut Through:** Lo Switch legge solo l'inizio del frame (MAC destinazione), poi lo inoltra subito senza aspettare tutto il frame. Questo può inoltrare frame con errori.

***PROBLEMA:*** Lo Switch non comprende il protocollo IP. Se lanciassimo un broadcast questo verrebbe inviato a tutte le reti, possiamo quindi risolvede questo problema con le ***VLAN***.

La ***VLAN (Virtual LAN)***  è la segmentazione logica di una rete. Agisce nel livello 2 degli Switch, questi vengono divisi in più reti virtuali indipendenti anche se fisicamente rimangono lo stesso dispositivo.

> Vantaggi della VLAN:
> 	Maggiore sicurezza (separa il  traffico)
> 	Meno Broadcast (rete piu efficente)
> 	Migliora l'organizzazione (Grupi Logici es. Uffici, Reparti..)
> 	Flessibilità (non dipende dalla posizione fisica)

*Un PC connesso alla VLAN 10 non potrà mai comunicare con nessun'altro dispositivo senza l'uso di un router.*


Vi sono diverse tipologie di VLAN, ciascuna legata allo scopo di ciascuna:

- **VLAN Default** --> VLAN 1, tutte le porte di default sono li, tutti i dispositivi possono comunicare tra loro. Questa è meglio che venga lasciata vuota per motivi di sicurezza

- **Data VLAN** --> serve a portare dati in giro per il network.
  
- **Management VLAN** --> Queste hanno un indirizzo IP e servono per poter gestire, configurare e monitorare le interfacce del VLAN da remoto.
  
- **VLAN Voice** --> è dedicata alla comunicazione, viene usata per chiamate (VoIP) e mantiene la sua priorità alta per assicurare fluidità nella comunicazione.
  
- **VLAN Nativa** --> è l'unica autorizzata a far passare il traffico non marchiato **802.1q**.
  
- **Trunk Channel** --> serve per mandare i dati delle altre VLAN tramite un cavo fisico. Possiede una porta che non viene assegnata a nessun'altra VLAN.

##### Configurazione di VLAN separate 

```Switch

vlan >numero<
name >nome-host<
do sh vlan
interface range fa0/1-8
switchport mode access
switchport access vlan >numero<
interface range fa0/21-23 ---> spegno questo range di porte
shut


```

Configuriamo poi l'IP:

```Switch

interface >portavlan<
ip address >ip< >subnet<
interface >porta gigabit<
switch port mode trunk
do show int trunk 

```


In questo modo sugli Switch che ho configurato vedrò `mode on`, negli altri `auto`.
Si vedrà uno spanning tree per ogni VLAN, questo per distribuire i carichi.

Quando creo un range di porte delle VLAN, vengono poi tolte dalla VLAN 1.

Nello standard Ethernet i valori associabili alle VLAN possono essere fino a 4096, questo perche il VLAN ID è codificato usando 12 bit. Tuttavia i valori ***0*** e ***4096*** sono riservati.