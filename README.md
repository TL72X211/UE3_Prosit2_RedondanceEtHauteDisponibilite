# UE3_Prosit2_RedondanceEtHauteDisponibilite

## Equipe
 * Emilien - Animateur
 * Raph - Secrétaire
 * Fantou - Scribe
 * Flo - Gestionnaire

## Mots-Clés

 * PVST : 
 * Boucle physique : Pierre M
 * Boucle fermée : Pierre M
 * Agregation de lien : Nicolas
 * Arbre couvrant : Nicolas
 * Commutateur racine : Florian
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
## Spanning Tree

Le protocole Spanning-tree est un protocole qui est par défaut actif sur les switchs CISCO. Il permet de faire de la redondance (ou résiliance) sur l’infrastructure. Le protocole permet un blocage « logique » des boucles infinies qui paralysent le réseau 
Il permet dans une infrastructure d’empêcher les « tempêtes de broadcast », les « duplications de trames » et les « instabilités de la table CAM » en désactivant un port.
Le protocole standardisé est l’ IEEE 802.1d

Voici différentes version du protocole : 
	- STP : Protocole de base
	- PVST : Per VLAN STP, une instance STP par VLAN. Uniquement avec ISL
	- PVST+ : Per VLAN STP fonctionnant avec dot1q.
	- RSTP+ : STP Amélioré pour réduire la durée du passage d’un port en « blocking » à « forwarding » (IEEE 802.1w)
	-  MSTP : Multiple Spanning Tree Protocole. Une instance de RSTP par groupe de VLANs

Voici comment l’implémenter :

![stp_1](/ress/stp_1.png)
![stp_2](/ress/stp_2.png)
![stp_3](/ress/stp_3.png)
![stp_4](/ress/stp_4.png)

## PVST 

Quand plusieurs VLAN existent dans un réseau Ethernet commuté, STP peut fonctionner de façon indépendante sur chacun des VLAN séparément. Ce mode de fonctionnement a été baptisé PVST(+) par Cisco. C'est le mode par défaut sur les commutateurs de la marque. Il s'agit d'un développement propriétaire qui est également pris en charge par certains fournisseurs concurrents.

PVST fonctionne uniquement avec Cisco Inter-Switch Link (ISL). PVST+ est utilisé avec dot1q.


 * Etherchannel 

![etherchannel](/ress/etherchannel.png)

L’Ethernetchannel est une agrégation de liens. Le but est de connecter plusieurs interfaces physiques d’un switch à un autre pour augmenter la bande passante. Une agrégation peut compter 8 interfaces physiques. Il existes 2 protocoles d’agrégation : PAgP et LACP qui permettent de transformer les plusieurs liens physiques en un seul lien logique . 
La norme de cette technologie est la norme IEE 802.1AX

#### PAgP (Port Aggration Protocol)
Ce protocole est un protocole propriétaire Cisco.
Voici les 3 statuts dans sa configuration :
- On : sert à déclarer une agrégation active
- Désirable : C’est un mode qui fait la demande avec le switch d’en face pour créer l’agrégation
- Auto : C’est un mode qui attend la négociation pour devenir une agrégation.

![PAgP](/ress/pagp.png)

#### LACP (Link Aggregation Control Protocol)

Ce protocole est un protocole standard qui peut être utilisé avec différents constructeurs. 
Voici les 3 statuts de sa configuration :

- On : Sert à déclarer une agrégation active
- Active : C’est un mode qui fait la demande avec le switch d’en face pour créer l’agrégation
- Passive : C’est un mode qui attend la négociation pour devenir une agrégation

![lacp](/ress/lacp.png)

Voici comment implémenter ces protocoles :

![](/ress/imp_1.png)
![](/ress/imp_2.png)

## HSRP
 
Le Hot Standby Router Protocol est un protocole propriétaire cisco qui est implémenté sur un routeur. Il permet de faire du fail-over sur un groupe de routeur. 
Pour se faire les routeurs se voient attribuer une adresse ip virtuelle ainsi qu’un numéro (de 0 à 255). Ainsi on pourra élire le routeur ayant le plus petit numéro comme celui ayant la priorité la plus élevé. Il devient alors actif alors que tous les autres deviennent passif. Périodiquement le routeur actif envoie un message « Hello » pour signaler aux autres qu’il est encore en fonctionnement.
Ce protocole ne permet pas de faire du load-ballancing
Voici son implémentation :

![](/ress/hsrp.png)

## LACP 
## Théorie des graphes (Arbres binaires + Recherche en profondeur (et non pas en largeur)) 
## Implémentation et paramétrage sur Cisco

### Réalisation
## Réaliser une maquette des changements
## Arbres Binaire - Langage au choix (Hacker Rank)
## Recherche en profondeur (Hacker Rank)