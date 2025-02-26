---
layout: default
parent: Construction tribune
nav_order: 1
title: Ventouses
---

# le systéme a ventouses

## Description
On appelle une ventouse une calotte de caoutchouc qui peut être maintenue par la pression de l'air sur une surface plane. [source]https://www.larousse.fr/dictionnaires/francais/ventouse/81431

Le systéme qu'on crée est simple, la pompe a vide est reliée a une ventouse et en paralele une valve de remise a l'atmosphere.

la pompe tourne quand on attrape et on garde un élément sur le robot
la pompe s'éteint et la valve s'ouvre quandon veut lacher un élément

## V1

l'idée est d'avoir une stabilité de la boite de conserve pour pouvoir la déplacer sans que sa postion sur X Y relative au robot soit modifiée lors du déplacement, c'est pourquoi l'idée d'avoir 2 point d'appui pour bloquer
le mouvement est venue. 
Voici le résultat
![Support ventouse V1](Ventouses/SupportVentouseV1.png)

Cette version avait 2 failles:
* Les 2 ventouses doivent etre absolument en contact avec la boite de conserver avant de pouvoir créer une dépression
* Le rayon de courbure ne prenais pas en compte la taille des ventouses

Neanmoins elle nous a permis de voir que le systéme de ventouses est assez puissant pour souslever les boites de conserve

## V2

![V2](Ventouses/V2.jpg)
Cette version à la bonne forme et a 2 canneaux d'air pour resoudre le probléme d'étancheité de prise


## V3
![V3](Ventouses/V3.jpg)

Cette version a directement les adaptateurs ventouse intégrée dans l'impression, ça permet de reduire des fuites possible qu'impose une connexion filté 

![V3.1](Ventouses/V3_1.jpg)

Cette version a été testé sur le robot, et un soucis est en sorti, les ventouses etaient trop rigide, et avec un mouvement rectligne les Conservent ne s'asspirent pas et sont plutôt poussée vers l'avant.
Modification également sur le design de l'accroche, désormai on peut rajouter des vis pour bloquer l'acctioneur sur l'accroche

## V4
![V4](Ventouses/V4.jpg)

On a modifiée le desing pour adapter des ventous de ON Robot sur le préhenseur empruntée a gestionaire de l'usine école.
les ventouses sont plus souple, et prennent plus rapidement la boite de conservation sans pousser trop loin la boite de conserve.

# Particularité

Chaque version de cette actionneur a une même type d'acchroche au robot, en quelques action le préhenseur peut etre echangée pour un autre, le systéme qui permet ceci est la suivante

![alt text](Ventouses/Attachement.png)


![alt text](Ventouses/Attachementexemple.png)

# Problémes rencontrée

Avec la forme en "c", la boitte de conserve est prise fixe au niveau de l'axe x et l'axe Z, si on considére le centre du préhenseur un réferentiel cartésien dont l'axe Z est dans la direction des tuyeau de ventouses et l'axe y tracée par un trait entre les 2 ventouses
suivant la régle de la main droite.

L'axe y n'est pas contraint assez pour emmpecher a la boite de conserve a tourner autour de cet axe, le poid de celle ci est alors assez pour la faire pivoter et laisser tomber les planches au dessus d'elle


# Conclusion 
La ventouse de préhension est un bon concept, il faut neamoins la rafinner pour avoir quelque chose d'utilisable, il faut quelque chose pour la soutenir ainsi que soutenir les planches.