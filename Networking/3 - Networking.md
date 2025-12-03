Tags: [[ITS1]], [[Networking]]

### Cablatura

Vi sono due standard per la costruzione dei cavi.

- 568-A --> è lo standard americano
- 568/B --> stardard per il resto del mondo

I cavi **Straight-Through** sono assemblati con 4 cavi internamente interlacciati a coppie seguendo una corretta sequenza di colori.

568/B --> BiancoArancio + Arancio / BiancoVerde / Blu + BiancoBlu / Verde / BiancoMarrone + Marrone	
568/A --> BiancoVerde + Verde / BiancoArancio / Blu + BiancoBlu / Arancio / BiancoMarrone + Marrone	
	Di queste coppie di cavi vengono utilizzate la prima coppi BiancoArancio + Arancio per Tx- e Tx+ (Trasmissione) e la coppia BiancoVerde + Verde per Rx- e Rx+ (Ricezione).

Questa cablatura sarà uguale da entrambi i capi del cavo.


I cavi **Cross-Over**, rispetto all'estremità di partenza quella di fine avrà i cavi di Trasmissione e di Ricezione invertiti. Questo perché per logica i cavi di trasmissione dovranno essere letti dall'interfaccia di ricezione nell'host di ricezione.


I cavi **Roll-Over** NON sono cavi di rete, questi permettono di collegarci tramite l'interfaccia di console per gestire la configurazione di dispositivi di rete.


Uso di un cavo Cross-Over:
- Hub --- Switch
- PC --- PC
- Hub --- Hub
- Switch --- Switch
- Router --- Router
- Router --- Port

### HUB in PacketTracer

L'HUB agisce in maniera stupida, ogni dato che riceve lo inoltra a tutte le porte connesse tranne a quella sorgente e attende una risposta da quella giusta. Lo ripeterà sempre.


### Switch in PacketTracer

Lo Switch nonappena collegato popola lo spanning tree per evitare loop nella rete.

Una volta nella CLI dello Switch possiamo usare i seguenti comandi per visualizzare la MAC/address Table.

```Switch CLI
Switch>

Switch>enable

Switch#

Switch#

Switch#

Switch#show mac-

Switch#show mac-address-table

Mac Address Table

-------------------------------------------

  

Vlan Mac Address Type Ports

---- ----------- -------- -----

  

Switch#
```

Come si può vedere la MAC-address Table non è popolata, lo Switch potrà popolarla automaticamente tramite il ping tra host.

Dopo aver fatto ping da un Host ad un altro vedremo che la MAC-address table si è popolata con gli indirizzi MAC dei due Host. Facendo un ping tra le due coppie di PC vedremo successivamente la seguente MAC-address Table.

```Switch
Switch#show mac-address-table

Mac Address Table

-------------------------------------------

  

Vlan Mac Address Type Ports

---- ----------- -------- -----

  

1 0001.96cc.b75a DYNAMIC Fa0/3

1 000a.f399.6705 DYNAMIC Fa0/1

1 000c.855b.3e9b DYNAMIC Fa0/2

1 00d0.ff95.5400 DYNAMIC Fa0/4
```


<hr>


Il protocollo ARP serve a determinare l'indirizzo MAC conoscendo l'indirizzo IP di due dispositivi.
Nel caso di una richiesta ARP questa verrà inviata sotto messaggio di broadcast, se i Gratuitous ARP sono attivati questi invieranno indietro i dati nonostante non siano gli Host richiesti.


Possiamo sfruttare questo protocollo di comunicazione per un attacco ARP Poisoning, questo ci permette di fingerci il Gateway ed intercettare ogni comunicazione in entrata e in uscita (Man-in-the-middle).

<hr>

### Connessione di un intermediary device tramite cavo console

Per la configurazione di uno Switch da 0 dobbiamo connetterci tramite cavo console in quanto non abbiamo ancora assegnato un IP allo Switch.
Possiamo usare un software come *Putty* per attivare la connessione via cavo.


<hr>


### Modalità nella CLI dello Switch

```Switch

switch>   "user exec mode"


switch> enable


switch#   "privilege exec mode"
```


Nella modalità *user exec mode* abbiamo funzionalità limitate, quali:

```Switch

Exec commands:

	connect Open a terminal connection
	disable Turn off privileged commands
	disconnect Disconnect an existing network connection
	enable Turn on privileged commands
	exit Exit from the EXEC
	logout Exit from the EXEC
	ping Send echo messages
	resume Resume an active network connection
	show Show running system information
	ssh Open a secure shell client connection
	telnet Open a telnet connection
	terminal Set terminal line parameters
	traceroute Trace route to destination

```

usando il comando ***enable*** possiamo passare alla *privilege exec mode*, qui siamo al livello più alto possibile di amministrazione dello Switch.
Qui potrò principalmente usare il comando *show* per vedere le configurazioni e le info salvate nello Switch.

```Switch

Switch#?
Exec commands:

	[clear] Reset functions
	[clock] Manage the system clock
	[configure] Enter configuration mode
	[connect] Open a terminal connection
	[copy] Copy from one file to another
	[debug] Debugging functions (see also 'undebug')
	[delete] Delete a file
	[dir] List files on a filesystem
	[disable] Turn off privileged commands
	[disconnect] Disconnect an existing network connection
	[enable] Turn on privileged commands
	[erase] Erase a filesystem
	[exit] Exit from the EXEC
	[logout] Exit from the EXEC
	[more] Display the contents of a file
	[no] Disable debugging informations
	[ping] Send echo messages
	[reload] Halt and perform a cold restart
	[resume] Resume an active network connection
	[setup] Run the SETUP command facility
	[show] Show running system information

```

Solo da qui possiamo accedere alla *global configuration mode* che ci permette effettivamente di configurare le impostazioni e le funzioni dello Switch.
Usiamo il comando ***conf t***

```Switch

Switch#conf t
Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#?
Configure commands:
	
	[aaa] Authentication, Authorization and Accounting.
	[access-list] Add an access list entry
	[banner] Define a login banner
	[boot] Boot Commands
	[cdp] Global CDP configuration subcommands
	[clock] Configure time-of-day clock
	[crypto] Encryption module
	[default] Set a command to its defaults
	[do-exec] To run exec commands in config mode
	[dot1x] IEEE 802.1X Global Configuration Commands
	[enable] Modify enable password parameters
	[end] Exit from configure mode
	[exit] Exit from configure mode
	[hostname] Set system's network name
	[interface] Select an interface to configure
	[ip] Global IP configuration subcommands
	[line] Configure a terminal line
	[lldp] Global LLDP configuration subcommands
	[logging] Modify message logging facilities
	[mac] MAC configuration
	[mls] mls global commands
	[monitor] SPAN information and configuration
	[no] Negate a command or set its defaults
	[ntp] Configure NTP
	[port-channel] EtherChannel configuration
	[privilege] Command privilege parameters
	[sdm] Switch database management
	[service] Modify use of network based services
	[snmp-server] Modify SNMP engine parameters
	[spanning-tree] Spanning Tree Subsystem
	[tacacs-server] Modify TACACS query parameters
	[username] Establish User Name Authentication
	[vlan] Vlan commands
	[vtp] Configure global VTP state

```


<hr>


### Struttura di uno Switch


| Memoria ROM                                                                                              | Memoria RAM                                                                                                   | NVRAM                                                         | FLASH                             |
| -------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------- | --------------------------------- |
| al suo interno vi è<br>un firmware chiamato<br>**soft** che viene lanciato<br>all'interno della macchina | qui vengono salvati temporaneamente<br>i processi in corso ed effettuati fino allo <br>spegnimento del device | Memoria fissa che contiene le impostazioni salvate del device | Qui troviamo il sistema operativo |
|                                                                                                          | qui c'è la *running-config*<br><br>è il file di configurazione che gira in quell'esatto momento               |                                                               |                                   |

Una volta impostata una *running-config* avremo bisogno di salvarla nella memoria non-volatile NVRAM.

Per farlo useremo il  comando ***copy running-config startup-config*** .

Il file **startup-config** è un file standard salvato nella NVRAM che verrà letto all'avvio del dispositivo per aggiornare le *running-config* della sessione appena avviata. Ogni cambio che verrà effettuato durante la sessione corrente non si applicherà al prossimo avvio a meno che non venga salvato nel file di ***startup-config***.

```Switch

SWA#show sta
SWA#show startup-config
	startup-config is not present
SWA#copy running-config startup-config
	Destination filename [startup-config]?
	Building configuration...
	[OK]

```


<hr>


Al momento se ci dovessimo connettere di nuovo allo Switch non avremmo nessuna misura di sicurezza per l'accesso alla CLI del device.
Questo vuol dire che qualsiasi altra persona riesce ad accedere dalla stessa nostra connessione.

Dovrò in primis entrare nella modalità di configurazione del terminale, successivamente entrare nella configurazione della linea console con il comando
***lilne console 0***

specificare la password con il comando 
***password -password scelta-***.

ed infine usare il comando
***login***
per specificare che quella password ci saraà richiesta per il login alla macchina


```Switch

SWA#conf t
	Enter configuration commands, one per line. End with CNTL/Z.
SWA(config)#line con 0
SWA(config-line)#password cisco
SWA(config-line)#login

SWA(config-line)#exit
SWA(config)#exit
SWA#

	%SYS-5-CONFIG_I: Configured from console by console

SWA#
SWA#exit


	Press RETURN to get started!

  

  

  

	User Access Verification

  

Password:

```


Ogni singolo comando può essere usato solo nel livello di appartenenza, se volessi usare un comando in un livello a cui non appartiene posso inserire il comando usando ***do*** prima di esso.


```Switch

SWA(config)#do show run

	Building configuration...

  

	Current configuration : 1100 bytes

	!
...
...
...
...

```


<hr>


### Password encryption

In questo momento pero possiamo vedere la password in chiaro tramite il comando ***show running-config***.

Abilitiamo quindi la crittografia delle password tramite il comando seguente:

```Switch

SWA#conf t
	Enter configuration commands, one per line. End with CNTL/Z.

SWA(config)#service password-encryption

```

L'algoritmo usato è molto semplice MD7, più avanti utilizzeremo un algoritmo più complicato.


<hr>


Ora possiamo abilitare lo Switch alla comunicazione con altri dispositivi assegnando un indirizzo IP alla sua interfaccia VLAN.

Una volta che l'abbiamo assegnato dovremo PER FORZA assegnare le password alle linee di accesso. Questo per proteggervi l'accesso dalle altre linee.

Per fare ciò possiamo usare i comandi qua sotto.
N.B. una volta abilitata una VLAN assegnandoli un indirizzo IP, questa partirà in stato di DOWN (spenta) questa si accenderà nel momento in cui connetteremo un cavo alla VLAN.

```Switch

SWA#conf t
	Enter configuration commands, one per line. End with CNTL/Z.
SWA(config)#interface vlan 1
SWA(config-if)#ip address 172.16.0.200 255.255.0.0
SWA(config-if)#no shutdown

SWA(config-if)#
	%LINK-5-CHANGED: Interface Vlan1, changed state to up

```


<hr>


Ora impostiamo una password per la ***privilege exec mode***.

Andiamo in *config* e usiamo, come per la linea console, il comando *enable secret -password-*.

```Switch

SWA(config)#enable secret class
SWA(config)#

```

<hr>


Ora impostiamo una password per la rete VLAN

Entriamo prima nelle configurazioni della linea desiderata
	usiamo il comando *password*
		specifichiamo chevogliamo utilizzare la password appena settata per il login con il comando *login*
			salviamo la running-config con wr.

```Switch

SWA(config)#line vty 0 15
SWA(config-line)#password cisco
SWA(config-line)#login
SWA(config-line)#do wr
	Building configuration...

	[OK]
SWA(config-line)#

```



<hr>


### Configurazione accesso remoto SSH

Telnet non viene più utilizzato perchè ogni tipo di dato viene comunicato in chiaro, rendendola quindi una comunicazione facilmente intercettabile.

###### Crittografia simmetrica / asimmetrica
SSH è un protocollo asimmetrico.

| simmetrica                                                                                                                                                                                            | asimmetrica                                                                                                                                                                                                                                                                                          |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Viene fornito un algoritmo a tutti<br>Questo, unito alla chiave crittografica permette di criptare un dato in uscita.<br>La chiave usata per decriptare è la stessa, per questo si chiama simmetrica. | Vi è un algoritmo asimmetrico da entrambi i lati della comunicazione.<br>Ciascuno dei due lati della comunicazioni possiede una chiave crittografica pubblica e una privata.<br>Le chiavi pubbliche sono conosciute da tutti ma sono personali del proprio utente, quelle private invece no.<br><br> |


1) Fornire al dispositivo un Domain Name.
2) Fornire al dispositivo un nome differente da quello default.
	1) comando: **ip domain-name ->nome<-**
	2) comando:  hostname **-->nome<--**
3) Generare le chiavi crittografiche
	1) comando: **crypto key generate rsa**
4) Creare Database "locale" di utenti e password autorizzati ad usare SSH
	1)  comando: **username -->nomeUtente1<-- password -->password1<--**
	2)  ne possiamo creare quanti ne vogliamo in base al bisogno.
5)  Configurare gli accessi via rete (vty) per:
	1) accettare solo connessioni SSH
		1)  comandi: **line vty 0 15** --> **transport input ssh**
	2)  Gli utenti autorizzati sono solo quelli presenti nel database locale.
		1)  comando: **login local**.



```PC

C:\>telnet 172.16.0.200

	Trying 172.16.0.200 ...Open

  User Access Verification

	Password:

SWA>
SWA>enable

Password:
SWA#conf t

	Enter configuration commands, one per line. End with CNTL/Z.

SWA(config)#ip domain-name cisco

SWA(config)#crypto key generate ?

	rsa Generate RSA keys
	SWA(config)#crypto key generate rsa
	The name for the keys will be: SWA.cisco
	Choose the size of the key modulus in the range of 360 to 4096 for your
	General Purpose Keys. Choosing a key modulus greater than 512 may take
	a few minutes.

	How many bits in the modulus [512]: 4096

	% Generating 4096 bit RSA keys, keys will be non-exportable...[OK]

SWA(config)#
	*Mar 1 1:32:43.703: %SSH-5-ENABLED: SSH 1.99 has been enabled

SWA(config)#username utente1 password 1234

SWA(config)#username utente2 password 4321

SWA(config)#login local

SWA(config)#line vty 0 15

SWA(config-line)#transport input ?

	all All protocols
	none No protocols
	ssh TCP/IP SSH protocol
	telnet TCP/IP Telnet protocol
	SWA(config-line)#transport input ssh
	SWA(config-line)#login local
	SWA(config-line)#

SWA(config-line)#transport input ssh
SWA(config)#do wr
```


Andando ora nel terminale dello stesso pc e proviamo a connetterci in SSH:

```PC

C:\>ssh -l utente1 172.16.0.200

Password:

SWA>

```


<hr>


### Router
Possiamo configurare un router alla stessa maniera:

```Router

Router>enable
Router#conf t
	Enter configuration commands, one per line. End with CNTL/Z.
Router(config)#hostname GATEWAY
GATEWAY(config)#interface fa0/0
GATEWAY(config-if)#ip address 172.16.0.254 255.255.0.0
GATEWAY(config-if)#no shutdown
GATEWAY(config-if)#

	%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

GATEWAY(config-if)#ip domain-name cisco

GATEWAY(config)#crypto key generate rsa

	The name for the keys will be: GATEWAY.cisco
	Choose the size of the key modulus in the range of 360 to 2048 for your
	General Purpose Keys. Choosing a key modulus greater than 512 may take
	a few minutes.

	How many bits in the modulus [512]: 2048

	% Generating 2048 bit RSA keys, keys will be non-exportable...[OK]

GATEWAY(config)#enable secret class
	*Mar 1 0:10:46.611: %SSH-5-ENABLED: SSH 1.99 has been enabled
GATEWAY(config)#username utente1 password 1234
GATEWAY(config)#username utente2 password 4321
GATEWAY(config)#line con 0
GATEWAY(config-line)#password cisco
GATEWAY(config-line)#login
GATEWAY(config-line)#line vty 0 15
GATEWAY(config-line)#password cisco
GATEWAY(config-line)#transport input ssh
GATEWAY(config-line)#login local
GATEWAY(config-line)#
GATEWAY(config-line)#exit
GATEWAY(config)#service password-encryption
GATEWAY(config)#
GATEWAY(config)#do wr

Building configuration...

[OK]

```


Nel Routing, il percorso preferito prende in considerazione in primis il carico amministrativo (distanza dalla rete), successivamente la tipologia di protocollo (RIP, OSPF, EICMP, ecc..) ed infine la distanza.


<hr>


### Server DHCP

Ogniqualvolta un host nuovo si connette alla rete, questo gli assegna automaticamente un indirizzo IP, la subnet mask e il default gateway.

Partiamo dall'impostare sul router in mezzo tra la rete 1 e la rete 2 per inoltrare la richiesta DHCP con un IP ***helper-address*** sulla porta fa0/1.

> L'***helper-address*** dovrà essere impostato su ogni snodo in cui una porta riceverebbe in entrata messaggi di broadcast per la richiesta DHCP.

```Router
GATEWAY>enable
Password:
GATEWAY#conf t
Enter configuration commands, one per line. End with CNTL/Z.
GATEWAY(config)#
GATEWAY(config)#interface fa0/1
GATEWAY(config-if)#
GATEWAY(config-if)#ip helper-address 172.16.0.55
GATEWAY(config-if)#
GATEWAY#

%SYS-5-CONFIG_I: Configured from console by console
```

Dobbiamo poi impostare il range di indirizzi da poter dare in seguito ad una richiesta, questo è chiamato DHCP pool. 

![[Schermata del 2025-11-14 09-19-51.png]]


Il Default Gateway sarà quello della rete su cui il server DHCP è presente
Il DNS Server possiamo impostarlo su 8.8.8.8
Impostiamo l' IPv4 di partenza equivalente al primo ip utile della rete in cui gli host devono ricevere l'indirizzo DHCP.

Cosi facendo dovremmo riuscire ad ottenere un indirizzo IP dinamico sugli Host dell'altra rete.

***Questo processo va ripetuto per ogni rete in cui vogliamo assegnare gli indirizzi IP in modo Dinamico***

