
# UE3_Prosit2_RedondanceEtHauteDisponibilite

## Equipe
 * Emilien - Animateur
 * Raph - Secrétaire
 * Fantou - Scribe
 * Flo - Gestionnaire

## Mots-Clés

 * PVST : 
 * Boucle physique : 
 * Boucle fermée : 
 * Aggregation de lien : 
 * Arbre recouvrant : 
 * Commutateur racine : 
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


https://reussirsonccna.fr/wp-content/uploads/2012/06/STP_block1.png


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


