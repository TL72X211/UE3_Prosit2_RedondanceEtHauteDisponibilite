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