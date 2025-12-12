Tags: [[ITS1]], [[Networking]]


***Comandi usati per la configurazione delle VLAN:***

```Switch

Switch>ena
Switch#conf t
	Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#hostname SW-A

SW-A(config)#vlan 50
SW-A(config-vlan)#name ITS

SW-A(config-vlan)#vlan 60
SW-A(config-vlan)#name OSPITI

SW-A(config-vlan)#vlan 70
SW-A(config-vlan)#name UFFICI
SW-A(config-vlan)#do sh vlan

	VLAN Name Status Ports
	
	---- -------------------------------- --------- -------------------------------
	
	1 default active Fa0/1, Fa0/2, Fa0/3, Fa0/4
	
	Fa0/5, Fa0/6, Fa0/7, Fa0/8
	
	Fa0/9, Fa0/10, Fa0/11, Fa0/12
	
	Fa0/13, Fa0/14, Fa0/15, Fa0/16
	
	Fa0/17, Fa0/18, Fa0/19, Fa0/20
	
	Fa0/21, Fa0/22, Fa0/23, Fa0/24
	
	Gig0/1, Gig0/2
	
	50 ITS active
	
	60 OSPITI active
	
	70 UFFICI active
	
	1002 fddi-default active
	
	1003 token-ring-default active
	
	1004 fddinet-default active
	
	1005 trnet-default active
	...
	  ...
	  

SW-A(config-vlan)#exit

SW-A(config)#interface range fa0/1-10
SW-A(config-if-range)#switchport mode access
SW-A(config-if-range)#switchport acces vlan 50

SW-A(config-if-range)#interface range fa0/11-15
SW-A(config-if-range)#switchport mode access
SW-A(config-if-range)#switchport acces vlan 60

SW-A(config-if-range)#interface range fa0/16-20
SW-A(config-if-range)#switchport mode access
SW-A(config-if-range)#switchport acces vlan 70

SW-A(config-if-range)#interface g0/1
SW-A(config-if)#switchport mode trunk

SW-A(config-if)#

	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/1, changed state to up

SW-A(config-if)#do sh interf trunk

	Port Mode Encapsulation Status Native vlan
	
	Gig0/1 on 802.1q trunking 1
	
	  
	
	Port Vlans allowed on trunk
	
	Gig0/1 1-1005
	
	  
	
	Port Vlans allowed and active in management domain
	
	Gig0/1 1,50,60,70
	
	  
	
	Port Vlans in spanning tree forwarding state and not pruned
	
	Gig0/1 none

SW-A(config-if)#exit
SW-A(config)#exit
SW-A#

	%SYS-5-CONFIG_I: Configured from console by console

SW-A#
SW-A#
SW-A#
SW-A#show vtp status
	
	VTP Version capable : 1 to 2
	
	VTP version running : 1
	
	VTP Domain Name :
	
	VTP Pruning Mode : Disabled
	
	VTP Traps Generation : Disabled
	
	Device ID : 0090.0C7D.7600
	
	Configuration last modified by 0.0.0.0 at 3-1-93 00:09:37
	
	Local updater ID is 0.0.0.0 (no valid interface found)
	
	...
	...

SW-A#ena
SW-A#conf t

SW-A(config)#vtp domain cisco

	Changing VTP domain name from NULL to cisco

	SW-A(config)#%SW_VLAN-6-VTP_DOMAIN_NAME_CHG: VTP domain name changed to cisco.

```


## ***Topologie VLAN***
### LEGACY INTERVLAN ROUTING

In questa topologia abbiamo creato 3 *VLAN*, le abbiamo suddivise in uno Switch e l'abbiamo collegato al GATEWAY.![[Schermata del 2025-12-05 11-35-40.png | 500]]
Qui colleghiamo ogni ultima porta del range associato alla VLAN ad una porta sul GATEWAY.
Quindi:

- *VLAN 50* ---> range fa0/1-10 ---> fa0/10 collegata alla fa0/0 del GATEWAY
- *VLAN 60* ---> range fa0/11-15 ---> fa0/15 collegata alla Eth0/0 del GATEWAY
- *VLAN 70* ---> range fa0/16-20 ---> fa0/20 colelgata al GATEWAY

Questa topologia non viene piu usata perche poco conveniente e di configurazione molto lunga.


### ROUTER ON A STICK

Questa topologia prevede che tutte le VLAN vadano alla stessa velocita. Avremo un'unica connessione al GATEWAY su una interfaccia Gigabit, questa verrà suddivisa in piu *Sotto-interfacce*, ciascuna associata ad una VLAN.![[Schermata del 2025-12-05 11-35-54.png  | 500]]
Per fare cio dovremo pero definire in entrambe le interfacce della connessione gigabit switch-gateway in modalità *Trunk*.

```Switch

SW-A(config)#interface g0/2

SW-A(config-if)#switchport mode trunk

```

```Gateway

Router(config)#interface g0/0

Router(config-if)#no shut
Router(config-if)#

	%LINK-5-CHANGED: Interface GigabitEthernet0/0, changed state to up

  

	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0, changed state to up

```


Nel Gateway possiamo poi suddividere l'interfaccia usata indicandogli un "id" della ***Sotto-interfaccia***.
Per comodità gli indicheremo come nome lo stesso numero della vlan.
Si definisce con `g0/0.50` *50* = numero della vlan.

```Gateway

Router(config)#interface g0/0.50
	%LINK-3-UPDOWN: Interface GigabitEthernet0/0.50, changed state to down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0.50, changed state to up
	
```

Se ora provassimo ad associare un indirizzo IP a questa sotto interfaccia otterremmo un errore.
Questo perche nelle sotto-interfacce per poter associare un indirizzo dovremo aver prima assegnato e detto al Router a quale *VLAN* appartiene quella *sotto-interfaccia*.
Per risolvere questo problema useremo il comando `encapsulation dot1Q >numVLAN<`.

```Gateway

Router(config)#interface g0/0.50
Router(config-subif)#

	%LINK-3-UPDOWN: Interface GigabitEthernet0/0.50, changed state to down

	%LINEPROTO-5-UPDOWN: Line protocol on Interface GigabitEthernet0/0.50, changed state to up

Router(config-subif)#
Router(config-subif)#ip address 10.1.0.254 255.255.0.0

	% Configuring IP routing on a LAN subinterface is only allowed if that
	subinterface is already configured as part of an IEEE 802.10, IEEE 802.1Q,
	or ISL vLAN.

Router(config-subif)#encapsulation dot1Q 50
Router(config-subif)#ip address 10.1.0.254 255.255.0.0

	ripetiamo per ogni VLAN

```


### L3 SWITCHING

Qui utilizzeremo uno Switch Layer 3 permettendogli di fare Routing.

Vi possono essere due scenari:

- Nel primo associeremo una porta dello SwitchL3 a ciascuna VLAN.
  Cosi avremo una topografia uguale alla *InterVLAN Routing*.
  ![[Schermata del 2025-12-05 11-36-06.png | 500]]
  Avremo il vantaggio di avere le porte rimanenti libere per altre VLAN, tuttavia non potremo associare un indirizzo IP alla porte collegate alle VLAN, potremo pero associarlo alla VLAN corrispondente alla porta.

***Scenario 1***

```SwitchL3

Switch>ena
Switch#conf t
	Enter configuration commands, one per line. End with CNTL/Z.
Switch(config)#no cdp run
Switch(config)#hostname GATEWAY-2
GATEWAY-2(config)#interface fa0/1
GATEWAY-2(config-if)#switchport mode access
GATEWAY-2(config-if)#switchport access vlan 50
	% Access VLAN does not exist. Creating vlan 50

GATEWAY-2(config-if)#interface fa0/2
GATEWAY-2(config-if)#switchport mode access
GATEWAY-2(config-if)#switchport access vlan 60
	% Access VLAN does not exist. Creating vlan 60

GATEWAY-2(config-if)#interface fa0/3
GATEWAY-2(config-if)#switchport mode access
GATEWAY-2(config-if)#switchport access vlan 70
	% Access VLAN does not exist. Creating vlan 70

GATEWAY-2(config-if)#do wr
	Building configuration...
	[OK]

GATEWAY-2(config-if)#vlan 50
GATEWAY-2(config-vlan)#name ITS
GATEWAY-2(config-vlan)#vlan 60
GATEWAY-2(config-vlan)#name OSPITI
GATEWAY-2(config-vlan)#vlan 70
GATEWAY-2(config-vlan)#name UFFICI
GATEWAY-2(config-vlan)#do sh vlan

	VLAN Name Status Ports
	
	---- -------------------------------- --------- -------------------------------
	
	1 default active Fa0/4, Fa0/5, Fa0/6, Fa0/7
	
	Fa0/8, Fa0/9, Fa0/10, Fa0/11
	
	Fa0/12, Fa0/13, Fa0/14, Fa0/15
	
	Fa0/16, Fa0/17, Fa0/18, Fa0/19
	
	Fa0/20, Fa0/21, Fa0/22, Fa0/23
	
	Fa0/24, Gig0/1, Gig0/2
	
	50 ITS active Fa0/1
	
	60 OSPITI active Fa0/2
	
	70 UFFICI active Fa0/3
	
	1002 fddi-default active
	
	1003 token-ring-default active
	
	...
	
GATEWAY-2(config-vlan)#interface vlan 50
GATEWAY-2(config-if)#
	%LINK-5-CHANGED: Interface Vlan50, changed state to up
	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan50, changed state to up

GATEWAY-2(config-if)#ip address 10.1.0.254 255.255.0.0
GATEWAY-2(config-if)#

GATEWAY-2(config-if)#interface vlan 60
GATEWAY-2(config-if)#
	%LINK-5-CHANGED: Interface Vlan60, changed state to up
	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan60, changed state to up

GATEWAY-2(config-if)#
GATEWAY-2(config-if)#ip address 10.2.0.254 255.255.0.0
GATEWAY-2(config-if)#interface vlan 70
GATEWAY-2(config-if)#
	%LINK-5-CHANGED: Interface Vlan70, changed state to up
	%LINEPROTO-5-UPDOWN: Line protocol on Interface Vlan70, changed state to up

GATEWAY-2(config-if)#ip address 10.3.0.254 255.255.0.0
GATEWAY-2(config-if)#exit
GATEWAY-2(config)#ip routing

```



- Nel secondo scenario configureremo un canale di *Trunk* in cui gestiremo tutte le VLAN che ci interessano.
  Questa configurazione è invece piu simile alla *Router on a Stick*.
  
  Non abbiamo modo di dimostrarlo con *Packet Tracer* perche non è supportato.


---

### ***EtherChannel***

Un **EtherChannel** permette di aggregare più link Ethernet in un unico canale logico, aumentando banda e ridondanza tra dispositivi di rete (switch–switch o switch–server).

![[Schermata del 2025-12-05 12-53-18.png]]

**PAgP (Port Aggregation Protocol)**

- Proprietario Cisco
    
- Negozia automaticamente il canale
    
- Modalità: _auto_ / _desirable_

**LACP (Link Aggregation Control Protocol)**

- Standard IEEE 802.3ad
    
- Compatibile tra vendor diversi
    
- Modalità: _passive_ / _active_


***Proprietà che devono essere uguali sulle porte del bundle***

Perché l’EtherChannel funzioni, le interfacce devono avere **configurazioni identiche**, altrimenti il canale non si forma.

Le principali impostazioni da uniformare sono:

- **Speed e duplex** → stessi valori per tutte le porte
- **Modalità di porta** → tutte _access_ (stessa VLAN) o tutte _trunk_ (stesse VLAN allowed, stessa native VLAN)
- **MTU** → identica
- **Configurazioni STP** → coerenti (portfast/bpduguard/costi)
- **Modalità di aggregazione** compatibile:
    
    - PAgP: _auto_ ↔ _desirable_
    - LACP: _passive_ ↔ _active_
    - _on_ richiede configurazione manuale simmetrica

> In pratica, ogni proprietà che influenza il livello 2 deve essere allineata, altrimenti lo switch rifiuta l’aggregazione.


***Configurazione:***

```Switch1

Switch>ena
Switch#conf t
	Enter configuration commands, one per line. End with CNTL/Z.

Switch(config)#hostname SW-A
SW-A(config)#interface range fa0/1-4
SW-A(config-if-range)#channel-group ?
	<1-6> Channel group number
SW-A(config-if-range)#channel-group 1 mode ?
	active Enable LACP unconditionally
	auto Enable PAgP only if a PAgP device is detected
	desirable Enable PAgP unconditionally
	on Enable Etherchannel only
	passive Enable LACP only if a LACP device is detected

SW-A(config-if-range)#channel-group 1 mode on
SW-A(config-if-range)#

	Creating a port-channel interface Port-channel 1
	%LINK-5-CHANGED: Interface Port-channel1, changed state to up
	%LINEPROTO-5-UPDOWN: Line protocol on Interface Port-channel1, changed state to up

SW-A(config-if-range)#interface po1
SW-A(config-if)#switchport mode trunk

```

Ripetiamo gli stessi passaggi nel secondo Switch, questo perchè le proprietà DEVONO essere uguali da entrambe le interfacce della connessione.


---


### Virtual Router / HSRP

```Router

Router>enable

Router#

Router#configure terminal

Enter configuration commands, one per line. End with CNTL/Z.

Router(config)#interface Serial0/0/0

Router(config-if)#ip address 60.0.0.2 255.0.0.0

Router(config-if)#ip address 60.0.0.2 255.255.255.252

Router(config-if)#no shutdown

Router(config-if)#

%LINK-5-CHANGED: Interface Serial0/0/0, changed state to up

  

Router(config-if)#

Router(config-if)#exit

Router(config)#interface Serial0/0/1

Router(config-if)#

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/0, changed state to up

ip address 80.0.0.1 255.0.0.0

Router(config-if)#ip address 80.0.0.1 255.255.255.252

Router(config-if)#no shutdown

Router(config-if)#

%LINK-5-CHANGED: Interface Serial0/0/1, changed state to up

  

%LINEPROTO-5-UPDOWN: Line protocol on Interface Serial0/0/1, changed state to up

  

Router(config-if)#

Router(config-if)#exit

Router(config)#interface FastEthernet0/0

Router(config-if)#ip address 50.0.0.100 255.0.0.0

Router(config-if)#ip address 50.0.0.100 255.255.255.0

Router(config-if)#no shutdown

Router(config-if)#

%LINK-5-CHANGED: Interface FastEthernet0/0, changed state to up

  

%LINEPROTO-5-UPDOWN: Line protocol on Interface FastEthernet0/0, changed state to up

  

Router(config-if)#

Router(config-if)#

Router(config-if)#exit

Router(config)#hostname R-B

R-B(config)#router rip

R-B(config-router)#network 80.0.0.0

R-B(config-router)#network 60.0.0.0

R-B(config-router)#network 50.0.0.0

R-B(config-router)#do wr

Building configuration...

[OK]

R-B(config-router)#

R-B(config-router)#

R-B(config-router)#

R-B(config-router)#version 2

R-B(config-router)#do wr

Building configuration...

[OK]

R-B(config-router)#exit

R-B(config)#interface fa0/0

R-B(config-if)#standby 1 ip 50.0.0.254

R-B(config-if)#standby 1 priori

%HSRP-6-STATECHANGE: FastEthernet0/0 Grp 1 state Speak -> Standby

ty

%HSRP-6-STATECHANGE: FastEthernet0/0 Grp 1 state Standby -> Active

^

% Invalid input detected at '^' marker.

R-B(config-if)#standby 1 priority 150

R-B(config-if)#standby 1 preempt

R-B(config-if)#

```