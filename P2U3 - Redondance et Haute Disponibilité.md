

*P2U3 - Redondance et Haute Disponibilité*

# Mots Clés

PVST :
Boucle physique :
Boucle fermée :
Aggregation de lien :
Arbre recouvrant :
Commutateur racine :
Graph en arbre :
HSRP (Host Standby Router Protocol) :
Chemin le plus court :
Tempête de diffusion :
Commutatuer élu :

# Problématique

- *Comment réduire le trafic en retirant les boucles infnies ?*

# Sommaire

**Etudes**
- Theorie des Graphes (Arbres binaires)
- PVST (Spanning Tree)
- Etherchannel
- HSRP

**Réalisation**
- Implémentation sous Cisco

# Etudes
## Théorie des graphes
## PVST (Spanning Tree)

Objectif du protocole est de gérer les boucles dans un réseau local dans le cas de l'utilisation de lien redondant.

**Description**
- Protocole Couche 2,
- Permet de gérer les boucles dans un réseau local,
- Permet de régler des problèmes comme :
	- Les tempetes de broadcast,
	- La duplication de trame.

**Fonctionnement**
- Bloque un des ports du switch lorsque l'une possibilité de boucle est détectée.
- Il existe 2 types de PVST
    - pvst (Mode par défault sur les commutateurs)
    - rapid-pvst

**Configuration**
```
Switch(config)# spanning-tree mode [mode]		// Configuration du mode (pvst|rapid-pvst)

Switch# show spanning-tree [%interface]				// Vérification

VLAN0001
Spanning tree enabled protocol rstp
Root ID Priority 28673
Address 0008.e4ff.ec11
Cost 23
Port 9 (GigabitEthernet0/1)
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec

Bridge ID Priority 32769 (priority 32768 sys-id-ext 1)
Address 0026.4585.2100
Hello Time 2 sec Max Age 20 sec Forward Delay 15 sec
Aging Time 300 sec

Interface Role Sts Cost Prio.Nbr Type
------------------- ---- --- --------- -------- --------------------------------
Gi0/1 Root FWD 19 128.9 P2p
```
## Etherchannel

Etherchannel permet de regrouper plusieurs liaisons ethernet en des "liens-virtuels" (port-channel) dans le but d'augmenter la bande passante et la redondance.

Il existe 2 manières de créer une agrégation de lien :
- En forcant l'agrégation (Utilisation du mode *trunk* sur les interfaces des switch)
- En utilisant un protocole de négociation

Il existe 2 protocoles d'agrégation de lien :
- PAGP
- LACP

### PAGP (Port Aggregation Protocol)
3 Status de configuration
- **On** - Sert a déclarer une agrégation active,
- **Désirable** - Mode qui *fait la demande* avec le switch d'en face pour créer l'agrégation,
- **Auto** - Mode qui *attend* la négociation pour devenir une agrégation.

### LACP (Link Aggregation Control Protocol)
3 Status de configuration
- **On** - Sert a déclarer une agrégation active,
- **Active** - Mode qui *fait la demande* avec le switch d'en face pour créer l'agrégation,
- **Passive** - Mode qui *attend* la négociation pour devenir une agrégation.

### Configuration
```
Switch1(config)# interface range f0/13 - 15				// Choose interfaces to link
Switch1(config-if-range)# channel-group [n] mode [mode]	// Select Mode (Typically Switch1 is active and switch 2 is passive)
Switch1(config-if-range)# channel-group 1 mode ?		// Show all modes
  active     Enable LACP unconditionally
  auto       Enable PAgP only if a PAgP device is detected
  desirable  Enable PAgP unconditionally
  on         Enable Etherchannel only
  passive    Enable LACP only if a LACP device is detected
Switch1(config-if-range)# no shutdown

Switch1# show etherchannel summary		// Vérification
Flags:  D - down        P - bundled in port-channel
        I - stand-alone s - suspended
        H - Hot-standby (LACP only)
        R - Layer3      S - Layer2
        U - in use      f - failed to allocate aggregator

M - not in use, minimum links not met
        u - unsuitable for bundling
        w - waiting to be aggregated
        d - default port

Number of channel-groups in use: 1
Number of aggregators:           1

Group  Port-channel  Protocol    Ports
------+-------------+-----------+-----------------------------------------------
1      Po1(SD)         LACP      Fa0/13(D)   Fa0/14(D)   Fa0/15(D)  
```

## HSRP (Hot Standby Router Protocol)

### Description
Le HSRP est un protocole propriétaire Cisco permettant d’obtenir une continuité de service LAN sur les routeurs.
Celui-ci permet à un routeur d’être le secours d’un autre routeur situé sur le même réseau Ethernet.

Le principe de fonctionnement est que tous les routeurs émulent une adresse IP virtuelle qui sera utilisée comme passerelle par les équipements du réseau LAN. Pour cela, chacun des routeurs configurera son HSRP avec un niveau de priorité. Celui qui disposera du plus grand sera actif. Les autres seront passifs en attendant la perte du premier routeur.

### En-tête
Nom | Taille (Octet)| Description
----|---------------|------------
Version | 1 | Indique la version du protocole HSRP utilisée (1 seule version a ce jour, la 0)
OpCode | 1 | **Hello** : Le routeur est actif (0), **Coup** : Le routeur veut devenir la passerelle active (1) **Resign** : Le routeur cède sa place de passerelle active.
Etat | 1 | Défini l'état du routeur qui envoi le message (0=initial, 1=learn, 2=listen, 4=Speak, 8=Standby, 16=active).
HelloTime | 1 | Interval en secondes entre deux messages de type «hello».
HoldTime | 1 | Délai en secondes au delà duquel un routeur est considéré comme hors service si aucun message «hello» n’est reçu de sa part.
Priorité | 1 | Influence le choix de la passerelle active. La préférence va à la plus grande priorité. (Défaut = 100)
Groupe | 1 | Identifiant du groupe HSRP.
Authentification | 8 | 8 caractères (8x8bits) définissant un mot de passe en clair.
Adresse IP Virtuelle | 4 | Adresse IP virtuelle pour le groupe HSRP en question.

### Etats du routeur
- **Initial** : Etat initial lorsque HSRP se met route ou qu’un changement de configuration a lieu.
- **Learn** : Le routeur n’a pas encore appris son adresse IP virtuelle, ni reçu de messages «hello». Le routeur est en
attente d’un message du routeur actif.
- **Listen** : Le routeur connait son adresse IP virtuelle mais n’est ni le routeur actif, ni standby et attend un
message de ceux-ci.
- **Initial** : Etat initial lorsque HSRP se met route ou qu’un changement de configuration a lieu.
- **Speak** : Le routeur participe à l’élection du routeur actif et émet les messages «hello» périodiques.
- **Standby** : Le routeur est candidat pour devenir le prochain routeur actif et envoi des messages «hello»
périodiques.
- **Active** : Le routeur est la passerelle active et route le trafic destiné à l’adresse MAC virtuelle du groupe.

### Configuration
*Configuration du routeur R1*
```
R1(config)# interface fastEthernet0/1
R1(config-if)# ip address 192.168.0.1 255.255.255.0
R1(config-if)# standby 1 ip 192.168.0.254
R1(config-if)# standby 1 priority 110
R1(config-if)# standby 1 preempt
R1(config-if)# standby 1 track Serial 0/0 11
R1(config-if)# no shutdown
R1(config-if)# exit
```
*Configuration du routeur R2*
```
R2(config)#interface fastEthernet0/1
R2(config-if)#ip address 192.168.0.2 255.255.255.0
R2(config-if)#standby 1 ip 192.168.0.254
R2(config-if)#standby 1 preempt
R2(config-if)#standby 1 track Serial 0/0 1
R2(config-if)#no shutdown
R2(config-if)#exit
```

*Les differentes commandes*
- `standby priority [number]` : Permet de définir une prioritée au routeur. (100 Par défaut)
- `standby preempt` : Permet d'accélérer le processus délection.
- `standby ip [IPv4 Address]` : Indique l'adresse IP virtuelle partagée entre les routeurs.
- `standby track [Track Number]` : Permet de superviser une interface et de baisser la valeur de la priorité si celle-ci devenait "Down".
- `standby authentification` : Permet de remplacer le mot de passe (Mot de passe par défaut : "Cisco")

### Sécurité
2 Principales faiblesses :
- Mots de passe d'autentification sont en clair
- Les paquets unicast sont acceptés

# Réalisations
