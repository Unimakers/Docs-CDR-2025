---
layout: default
nav_order: 3
title: Pompes_a_vide
---

# Les pompes a vide

Pour alimenter le systeme pneumatique dans les actioneurs, il nous faut des pompes a vide.
ceux que le club possedaient etait des petit moteur DC avec élement pompe vissé dessus.

//voici une representation de cette pompe.

seulement un souci, les moteurs n'étaient pas de même tension nominale de fonctionnement, certain 12v, d'autre 5v, puis 3,7, d'autre même des tension de 6,4 volt.

non seulement ce sont des tension hors la tension logique d'un microcontroleur, le Microcontroleur n'est pas capable de fournir un tel courant d'appel ni de fonctionnement qu'un moteur demande.
c'est pourquoi un système, ou circuit de commande, doit etre mise en place.


## la réflexion

Alors que notre carte electronique mére possède des connceteur d'alimentation de differentes plages de tension, les souci est le nombre de pin de commande disponible depuis l'esp32.
Il se trouve que la carte mére posséede une extiontion de GPIO conçue pour des afficheur ou la commande de servo moteurs. c'est ce dernier point qui nous interesse.
Si un servo moteur peut etre controlée par un microprocesseur, nous pouvons également controler un moteur classique avec non?

Le standard servomoteur est de 3 fils: masse; alimentation; signal; 

Ok nous avons l'idée de comment on pourrais faire fonctionner le moteur, il nous faut un circuit de signal de commande a prèsent.

après des recherches très rapide, j'ai schématisée le circuit que je voulais.

![Circuit Mosfet moteur](Pompes_a_vide/CircuitMosfet.png)

