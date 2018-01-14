
# UE3_Prosit2_RedondanceEtHauteDisponibilite

## Equipe
 * Emilien - Animateur
 * Raph - Secrétaire
 * Fantou - Scribe
 * Flo - Gestionnaire

## Mots-Clés

 * PVST : 
 * Boucle physique : /
 * Boucle fermée :  Boucle qui peut retourner à son état d'origine.
 * Aggregation de lien : Principe de l'ether, plusieurs liens physiques sur un lien logique.
 * Arbre recouvrant : Sous ensemble qui est un arbre, et qui connecte tout les sommets ensemble, dont la somme des poids des arrêtes est minimale.
 * Commutateur racine :  Commutateur à la racine de l'arbre.
 * Graph en arbre : 
 * HSRP (Host Standby Router Protocol) : 
 * Chemin le plus court : 
 * Tempête de diffusion : 
 * Commutatuer élu : 

## Contexte

### Quoi ?  
 Erreur dans la topologie (Pierre M)  
 Eviter les boucles infinie de requête ARP (Nicolas / Florian)

### Comment ?  
 En utilisant le PVST (Hugo)  
 En utilisant un algorythme de type arbre recouvrant (Maxime)

### Pourquoi ?  
 Réduire le nombre de transmission (Florian)

## Problématique
Comment réduire le trafic en retirant les boucles infinies ?

## Généralisation
Reconception  

## Hypothèses
 * Connaissance en théorie des graphes peuvent servir (Nico)
 * Dans un graph en arbre, les noeuds situés sur une même couches ne sont pas reliés entre eux (Max)

## Plan d'action
### Etudes
 * Sparming Tree
 * PVST
 * Ethernet channel
 * HSRP
 * LACP
 * Théorie des graphes (Arbres binaires + Recherche en profondeur (et non pas en largeur))
 * Implémentation et paramétrage sur Cisco

### Réalisation
 * Réaliser une maquette des changements
 * Arbres Binaire - Langage au choix (Hacker Rank)
 * Recherche en profondeur (Hacker Rank)


## I - Etude du Spanning-Tree


- Architecture "redondance" = avoir une route alternative

Le Spanning-Tree est un protocole de couche 2 , formalisée IEEED 802.1D, qui permet de garder une topologie physique redondante tout en créant un chemin logique unique.

Le Spanning Tree à pour but d'éviter trois problèmes :
- Tempête de broadcast (Broadcast, Frame qui tourne en boucle entre les switch)
- Duplication de trame (Deux switchs qui émettent sur le même @MAC)
- Instabilité de la table CAM (Duplication sur chaque Switch dans la table CAM, qui va réémettre pour trouver l'@ MAC, le recevant sur un autre switch (lui contenant des informations vides).

Le spanning Tree permet donc d'identifier et bloquer logiciellement cette boucle "physique".


![](https://reussirsonccna.fr/wp-content/uploads/2012/06/STP_block1.png)


Ainsi, si une panne intervient, le blocage s'annule.
Ce chemin est établi en fonction :
- Coût de liens entre les commutateurs
- Ce coût est une valeur inverse à la vitesse d'un port, car un lien rapide aura un coût moins élevé qu'un lien lent.

Activé sur un Switch, il envoi régulièrement des annonces (BPDU) pour adapter une éventuelle modification de la topologie sans boucle.(2 secondes par défaut)

Chaque commutateur prendra un ID unique nommée 'BID' :
- Priorité configurable (4 bits)
- Bridge Système ID Extension (12 bits))
- @MAC commutateur

Ce BID va permettre de sélectionner le Switch principal.

**FONCTIONNEMENT DE L'ALGORITHME**

1- Sélectionner un commutateur Root, tous ses ports transfèrent le trafic (ports designated). C'est le BID le 
# UE3_Prosit2_RedondanceEtHauteDisponibilite

## Equipe
 * Emilien - Animateur
 * Raph - Secrétaire
 * Fantou - Scribe
 * Flo - Gestionnaire

## Mots-Clés

 * PVST : Per VLAN STP, une instance de STP par VLAN (uniquement avec ISL)
 * Boucle physique : /
 * Boucle fermée : /
 * Aggregation de lien : Principe de l'Ether, plusieurs liens physiques -> Un lien logique.
 * Arbre recouvrant : sous-ensemble qui est un arbre et qui connecte tous les sommets ensemble) dont la somme des poids des arêtes est minimale.
 * Commutateur racine : Commutateur dans une structure d'arbre la plus haute, c'est à dire qui déssert toute les autres.
 * Graph en arbre : Un graphique représenté sous forme d'arbre.
 * HSRP (Host Standby Router Protocol) : /
 * Chemin le plus court : Chemin le plus cours vers la destination.
 * Tempête de diffusion : Trop de diffusion = Tempête de diffusion = Pagaille
 * Commutatuer élu : Le switch élu pour être principal

## Contexte

### Quoi ?  
 Erreur dans la topologie (Pierre M)  
 Eviter les boucles infinie de requête ARP (Nicolas / Florian)

### Comment ?  
 En utilisant le PVST (Hugo)  
 En utilisant un algorythme de type arbre recouvrant (Maxime)

### Pourquoi ?  
 Réduire le nombre de transmission (Florian)

## Problématique
Comment réduire le trafic en retirant les boucles infinies ?

## Généralisation
Reconception  

## Hypothèses
 * Connaissance en théorie des graphes peuvent servir (Nico)
 * Dans un graph en arbre, les noeuds situés sur une même couches ne sont pas reliés entre eux (Max)

## Plan d'action
### Etudes
 * Spanning Tree
 * PVST
 * Ethernet channel
 * HSRP
 * LACP
 * Théorie des graphes (Arbres binaires + Recherche en profondeur (et non pas en largeur))
 * Implémentation et paramétrage sur Cisco

### Réalisation
 * Réaliser une maquette des changements
 * Arbres Binaire - Langage au choix (Hacker Rank)
 * Recherche en profondeur (Hacker Rank)


## I - Etude du Spanning-Tree


- Architecture "redondance" = avoir une route alternative

Le Spanning-Tree est un protocole de couche 2 , formalisée IEEED 802.1D, qui permet de garder une topologie physique redondante tout en créant un chemin logique unique.

Le Spanning Tree à pour but d'éviter trois problèmes :
- Tempête de broadcast (Broadcast, Frame qui tourne en boucle entre les switch)
- Duplication de trame (Deux switchs qui émettent sur le même @MAC)
- Instabilité de la table CAM (Duplication sur chaque Switch dans la table CAM, qui va réémettre pour trouver l'@ MAC, le recevant sur un autre switch (lui contenant des informations vides).

Le spanning Tree permet donc d'identifier et bloquer logiciellement cette boucle "physique".


![](https://reussirsonccna.fr/wp-content/uploads/2012/06/STP_block1.png)


Ainsi, si une panne intervient, le blocage s'annule.
Ce chemin est établi en fonction :
- Coût de liens entre les commutateurs
- Ce coût est une valeur inverse à la vitesse d'un port, car un lien rapide aura un coût moins élevé qu'un lien lent.

Activé sur un Switch, il envoi régulièrement des annonces (BPDU) pour adapter une éventuelle modification de la topologie sans boucle.(2 secondes par défaut)

Chaque commutateur prendra un ID unique nommée 'BID' :
- Priorité configurable (4 bits)
- Bridge Système ID Extension (12 bits))
- @MAC commutateur

Ce BID va permettre de sélectionner le Switch principal.

**FONCTIONNEMENT DE L'ALGORITHME**

1- Sélectionner un commutateur Root, tous ses ports transfèrent le trafic (ports designated). C'est le BID le plus faible qui l'emporte.

2 - Sélection d'un **seul port Root** sur tous les commutateurs non-root, dont le coût vers le commutateur Root est le plus faible, il sera le seul à transférer du trafic vers le root.


3 - Sélection d'un **port Designated** pour chaque segment physique, qui connecte deux commutateurs quand c'est nécessaire.
C'est le port qui a le coût vers le commutateur root qui est le plus faible qui est sélectionné, c'est le seul à transférer le trafic (Vers ailleurs)


4 - Les ports Root et Designated transfrèent du trafic (état Forwarding), et les autres ports bloquent la liason (Etat "Blocking").


Pour calculer le coût d'un trajet depuis le root, vers le Switch, on utilise le BPDU (Bridge protocol Data Unit), qui calcule la Root Path Cost.


https://www.ciscomadesimple.be/wp-content/uploads/2011/12/CMSBE_F06_STP.pdf


**CONFIGURER LA PRIORITE SPANNING TREE**

spanning-tree vlan 1 root primary

*(revenant à faire priority 28672)*

ou 

spanning-tree vlan 1 priority 24576

La valeur donné sur la priorité doit-être un multiple de 4096

Pour voir les informations du STP :

*#Show spanning-tree vlan 1"
{...}

Liste complète des commandes : https://www.ciscomadesimple.be/wp-content/uploads/2011/12/CMSBE_F06_STP.pdf


## II - Etude de l'EtherChannel

EtherChannel = 3 liens pour faire un lien logique & 	augmenter le débit, c'est à dire fusionner plusieurs liens physiques par un seul lien logique.
Celà permet aussi que les autres ports prennent le relai si un lien tombe en panne.

Il existe deux protocoles d'agréagation : 
- PAgP 

Port Aggration Protocol :
Propriétaire cisco, il utilise trois statuts dans sa config :
- ON (Déclarer une agrégation active)
- Désirable (Mode qui fait la demande avec le switch d'en face pour créer l'agrégation)
- Auto : Attends la négociation pour devenir une agrégation.


*********************
- LACP

Permet de communiquer avec différent constructeurs :
- On = déclarer une agrégation active
- Active : Fait la demande avec le switch d'en face pour créer l'agrégation
- Passive:  Mode qui attend la négociation pour devenir une agrégation

**Configuration :**
https://ciscotracer.wordpress.com/2015/10/01/etherchannel-lacp-pagp/

![](https://ciscotracer.files.wordpress.com/2015/10/clip_image004_thumb.jpg?w=632&h=457)

Nb : On peut faire passer l'interface en trunk, et lui ajouter de la sécu avec port-sécurity ou bpduguard...

***********

Quand on n'en utilie pas, celà s'appelle le Static persistance ("On")


## III - Etude du HSRP :

Le HSRP est un protocole de redondance de Cisco permettant de mettre en place une tolérance de panne pour les passerelles par défaut.
Les entreprises disposant d'un accès WAN l'utilisent à tout les coups.
Ce protocole permet à un routeur d'être le secours d'un autre routeur situé sur le même réseau Ethernet.
Tout les routeurs émulent une @IP virtuelle utilisé par les équipements des LAN. Ainsi un seul routeur sera en fonctionnement jusqu'à sa désactivation, laissant la place au routeur de secours.

L'adresse MAC des routeurs est émulée, du type : 00:00:0C:07:AC:ZZ où ZZ représente le numéro du groupe.

L'entête HSRP est la suivante :

- OpCode : 
	-	0 = routeur Actif
	-	1 = routeur veut devenir passerelle active
	-	2 = Resign (Routeur cède sa place de passerelle active)
- Etat = Définit l'état du routeur qui envoi le message
	- 0 = Initial
	- 1 = Learn
	- 2 = Listen
	- 4 = Speak
	- 8 = StandBy
	- 16 = Active
- HelloTime =  Interval en secondes entre deux messages de type "hello"
- HoldTime = Délai en seconde au delà  duquel un routeur est considéré comme HS si aucun "hello" reçu de sa part
- Priorité : Influence le choix de la passerelle active (Max 100)
- Groupe : Identifiant du groupe HSRP
- Authentification : 8 caractères (8*8 bits) définissant un mdp en clair
- @IP Virtuelle : @IP virtuelle pour le groupe

A l'aide de ces priorité, si un tombe en panne, le prochain avec la meilleure priorité prend le relai.

**Config** : https://www.ciscomadesimple.be/wp-content/uploads/2014/01/CMSBE_F09_HSRP.pdf
 
 et ce lien, qui contient la liste des commandes :
http://www.frameip.com/hsrp-cisco-securite/

On retrouve de nombreuses commandes, comme Standby priority xxx, pour définir la priorité, ou encore standby preempt qui permet d’accélérer le processus d'élection.


**!!! Faiblesse du protocole**

- MDP authentification en claire, le mdp pour configurer peut-être intercepté en vu qu'il circule en multicast.
- Ce protocole accepte les paquets unicast, ce qu'il veut dire qu'il est vulnérable aux attaques. (Pas de contraites de sécurité liées au multicast)

**PROTEGER SON HSRP**
- Changer de numéro & ne pas conserver celui par défaut pour éviter les bots automatiques le case.
- Utiliser un mot de passe autre que par défaut (standby 3 authentication frameip)
- Filtrer les paquets pour protéger les flux, que ce soit en bord de LAN ou sur chaque routeur HSRP, n'autorisant qu'un datagramme UDP 1985 (indiquant qu'il le reçoit d'un routeur du même groupe)
- Utiliser ISPEC entre les routeurs, pour chiffrer les communications
- Utiliser Hashage MD5 pour hash les échanges, le mot de passe change de champ, il faut le configurer sur tout les routeurs. (standby 3 authentification md5 key-string toto)
- **VRRP : Ne pas utiliser HSRP mais préférer VRRP qui est un protoocle simmilaire plus avancé**



## IV - Théorie des graphes :

Un graphe est un ensemble de points nomées noeuds, reliés part des segments (arrêtes). Tout est relié à la racine.
Les niveaux correspondent à la profondeur.
Une forêt est un ensemble d'arbre.

On peut représenter les arbres à l'aide de tableaux : 
http://slideplayer.fr/slide/1796100/

Il est également possible d'appliquer les switch à un système d'arbre (algorithmes pour déterminer le chemin le plus court)

Il est également possible de programmer de nombreux arbres, ou de les représenter sous forme de matrice.

Il existe plusieurs types d'arbres :
- Arbre structurés 
	- Homogènes 
	- Hiérarchique
	- Cycliques
	- Centralisés/Polaire
- Quelconques
- Multipolaire

![](https://commons.wikimedia.org/wiki/File:Topologies_de_r%C3%A9seaux.png?uselang=fr)


