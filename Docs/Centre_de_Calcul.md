---
layout: default
nav_order: 1
title: Centre de calcul 
---

# Le concept

Avec les technologies et les algorithmes qui avancent et s'ammeiliorent en continue, la puissance de calcul dans un robot est souvent restreint due a un manque d'espace dans le chassis.

Un moyen de résoudre cette probléme d'espace est de déporter cette puissance de calcul dans un centre de calcul, un dispositif posée en bord de table autorisée a communiquer avec le robot.

Ce dispositif peux également etre équipée de capteurs tel que des lidar, des caméras, utiles a la detection.

# les contraintes
* BAU
* Espace prédéfinie



# Ce qui a été fait jusqu'a présent

## choix de matériel

Dans l'optique de tester le concept, nous nous somme tounrné vers un raspberry Pi, un mini pc pas plus grand que la taille d'une porte-monaie. et un esp32-S3 pour s'interfacer avec le robot.


## Raspberry HAT

Nous avons conçue une HAT esp32 pour la raspberry PI, cette carte est compatible avec les anciennes et nouvelles cartes de robots

