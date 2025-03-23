---
layout: default
nav_order: 4
title: Centre de calcul 
---

# Concept

Avec les technologies et les algorithmes qui avancent et s'améliorent en continu, la puissance de calcul dans un robot est souvent restreinte due à un manque d'espace dans le châssis.
Un moyen de résoudre ce problème d'espace est de déporter cette puissance de calcul dans un centre de calcul, un dispositif posé en bord de table autorisé à communiquer avec le robot.
Ce dispositif peut également être équipé de capteurs tels que des lidars, des caméras, utiles à la détection.

Le projet consiste à développer un centre de calcul capable de détecter et de suivre les positions des tags AruCo sur les robots, les obstacles et la table de jeu. L'objectif est de déterminer les positions des robots en temps réel et d'enregistrer ces données dans une base de données InfluxDB. Un affichage en direct des positions est également prévu, ainsi qu'un post-traitement des données via Grafana.

# Contraintes

- **BAU**
- **Espace prédéfini**
- **Précision** : Les positions des tags doivent être détectées avec une grande précision pour assurer la fiabilité des données.
- **Temps réel** : Le système doit être capable de traiter et d'afficher les données en temps réel.
- **Robustesse** : Le matériel doit être robuste et capable de fonctionner dans des conditions variées.
- **Portabilité** : Le centre de calcul doit être facilement transportable et installable sur différents sites.

# Ce qui a été fait jusqu'à présent

- **Développement logiciel** : Un logiciel a été développé en Python, permettant la mise en place de la détection des tags AruCo, ainsi qu’un enregistrement des positions des centres des tags dans InfluxDB.
- **Affichage en direct** : Le logiciel comprend la création d'une interface 2D pour visualiser les positions des tags en temps réel.
- **Grafana** : Intégration de Grafana pour le post-traitement des données.
- **Réalisation physique** : Assemblage du matériel et tests préliminaires.

## Choix de Matériel

- **Raspberry Pi 5** : Utilisée comme centre de calcul principal en raison de sa puissance de traitement et de sa compacité.
- **Caméra** : Connectée à la Raspberry Pi pour la détection des tags AruCo. Pour l’heure, il s’agit d’une webcam branchée en USB à la carte.
- **Perche** : Conçue pour fixer la caméra à la bonne hauteur, et permettre son inclinaison selon l'angle souhaité.
- **Boîtier** : Conçu pour englober la carte ainsi que tous les périphériques ajoutés, par souci de protection et de facilité de déplacement et d’installation.

## Raspberry HAT

Nous avons conçu une HAT ESP32 pour la Raspberry Pi, cette carte est compatible avec les anciennes et nouvelles cartes de robots.

# Réalisation Physique

- **Boîtier** : Découpé à l'imprimante laser pour accueillir la Raspberry Pi et les composants associés.
- **Perche** : Prévue pour permettre de fixer la caméra selon une hauteur variable. Une pièce supplémentaire permet l’inclinaison de la caméra selon différents angles en la coulissant de haut en bas. La contrainte des règles de la Coupe de France de Robotique imposait une hauteur maximum de 160 cm.
- **Boutons** : Un bouton de marche/arrêt et un bouton d'arrêt d'urgence ont été prévus. Le code pour le bouton de marche/arrêt a été rédigé, et la schématique pour le bouton d'arrêt d'urgence a été réalisée.
- **Écran** : Prévu pour manipuler la Raspberry Pi et observer le rendu des données en direct.

# Programmation

## Librairies utilisées

Les principales librairies utilisées dans la réalisation de ce programme sont OpenCV pour le traitement de l’image, Tkinter pour le rendu graphique en temps réel, `influxdb_client` et `paho.mqtt.client` pour les communications en dehors de la carte et la gestion de la base de données, `threading` pour la gestion des différentes tâches en parallèle et enfin `time` pour enregistrer l’heure du relevé des données et gérer la durée du match.

## Description du Programme

Le programme développé pour le centre de calcul de la Coupe de France de Robotique est conçu pour détecter et suivre les positions des tags AruCo en temps réel. Il utilise une caméra connectée à une Raspberry Pi pour capturer les images et les traiter afin d'identifier les tags. Les positions détectées sont ensuite enregistrées dans une base de données InfluxDB et affichées sur une interface graphique en temps réel. Les relevés des positions s’effectuent durant 100 secondes, soit la durée d’un match officiel.

## Technique de Communication

Le programme est actuellement configuré pour fonctionner de manière autonome, mais une intégration MQTT est en cours d'implémentation. Cette intégration permettra de faciliter la communication avec les robots et d'autres composants du système. MQTT est un protocole léger et efficace pour les communications machine-to-machine (M2M), idéal pour les environnements à faible bande passante ou à faible puissance.

### Communication avec InfluxDB

La communication principale du programme se fait avec la base de données InfluxDB, qui est utilisée pour stocker les positions des tags AruCo détectés.
Le client InfluxDB est initialisé avec une URL, un token d'authentification, et une organisation spécifique. Il faut donc renseigner ces éléments spécifiques à la base de données dans laquelle on veut enregistrer les données relevées.
Les positions des tags sont enregistrées dans une base de données appelée ici `aruco_data`. Chaque enregistrement est horodaté avec un timestamp précis. Les données sont structurées sous forme de points, chaque point contenant l'ID du tag, les coordonnées du centre `x` et `y`, et un timestamp.

## Traitement de l'Image

Le traitement de l'image est réalisé à l'aide de la bibliothèque OpenCV. Voici les principales étapes :

- **Capture Vidéo** : La caméra capture des images en temps réel.
- **Conversion en Niveaux de Gris** : Les images sont converties en niveaux de gris pour simplifier le traitement.
- **Détection des Tags AruCo** : Les tags AruCo sont détectés à l'aide d'un dictionnaire prédéfini, ici l’`aruco_DICT_4X4_250`, spécifique aux tags utilisés pour la Coupe de France de Robotique.
- **Calcul des Centres** : Les centres des tags détectés sont calculés pour déterminer leur position.
- **Transformation de Perspective** : Une transformation de perspective est appliquée pour rectifier les tags détectés et obtenir une vue normalisée.

## Rendu Graphique

L'interface graphique est développée à l'aide de la bibliothèque Tkinter. Elle affiche une carte en 2D représentant le terrain de jeu, où les positions des tags sont mises à jour en temps réel. Les positions sont représentées par des points verts, et les identifiants des tags sont affichés à côté.

Le programme inclut également un bouton "Quitter" pour arrêter proprement l'application, libérant ainsi les ressources de la caméra et fermant les connexions à la base de données.

## Installation et Configuration d'InfluxDB et Grafana sur Raspberry Pi 5

### Installation d'InfluxDB

1. **Installation d'InfluxDB** :
   Téléchargez et installez InfluxDB en utilisant les commandes suivantes :
   ```bash
   wget https://repos.influxdata.com/influxdb.key
   echo "deb [signed-by=/etc/apt/keyrings/influxdb.key] https://repos.influxdata.com/debian stable main" | sudo tee /etc/apt/sources.list.d/influxdb.list
   sudo apt update
   sudo apt install influxdb2
   ```

2. **Démarrage du Service InfluxDB** :
   Démarrez le service InfluxDB et configurez-le pour qu'il démarre automatiquement au démarrage.
   ```bash
   sudo systemctl start influxdb
   sudo systemctl enable influxdb
   ```

3. **Création de la Base de Données** :
   L’interface d’InfluxDB est disponible à l’adresse suivante : `http://localhost:8086`.
   Lors de la création de la base de données, il est nécessaire de renseigner le nom de l’organisation, le nom du bucket, et de noter le token généré, qui sont utilisés dans le logiciel afin de permettre la communication avec la base de données.

### Installation de Grafana

1. **Installation de Grafana** :
   Pour installer Grafana, il est conseillé de suivre la documentation suivante : [Grafana Installation Guide](https://grafana.com/docs/grafana/latest/setup-grafana/installation/debian/).

2. **Démarrage du Service Grafana** :
   Démarrez le service Grafana et configurez-le pour qu'il démarre automatiquement au démarrage.
   ```bash
   sudo systemctl start grafana-server
   sudo systemctl enable grafana-server
   ```

### Configuration de Grafana pour Récupérer les Données d'InfluxDB

1. **Accès à l'Interface Grafana** :
   Ouvrez un navigateur web et accédez à `http://localhost:3000`. La première connexion nécessite de renseigner les identifiants et mot de passe `admin`, `admin`.

2. **Ajout d'une Source de Données** :
   - Allez dans `Connexion` > `Data Sources`.
   - Cliquez sur `Add data source` et sélectionnez `InfluxDB`.
   - Configurer la source de données avec les paramètres de la base de données InfluxDB. Nous avons sélectionné le query language Flux. Les paramètres à renseigner sont les suivants :
     - **URL** : `http://localhost:8086`
     - **Organisation** : Nom de l’organisation sous InfluxDB
     - **Token** : Token de la base de données.
   - Cliquez sur `Save & Test` pour vérifier la connexion.

3. **Création d'un Tableau de Bord** :
   - Allez dans `Create` > `Dashboard` > `Add new panel`.
   - Configurez un nouveau panneau pour visualiser les données des tags AruCo.
   - Utilisez des requêtes Flux ou tout autre query language sélectionné pour récupérer et afficher les données souhaitées.

En suivant ces étapes, InfluxDB et Grafana seront configurés sur la Raspberry Pi 5 pour collecter, stocker et visualiser les données des tags AruCo.

# Problèmes Rencontrés et Solutions

## Traitement de l'Image

**Problème** : La reproduction du terrain de jeu et l'adaptation des perspectives posaient des défis, notamment en raison des déformations dues à l'angle de la caméra.

**Solution** : Pour résoudre ce problème, une mise à l'échelle a été envisagée en utilisant les graduations du vrai terrain de jeu. Une transformation de perspective sous OpenCV a été appliquée pour redresser les éléments déformés. Cette approche permet de corriger les distorsions et d'obtenir une vue plus fidèle du terrain.

## Installation de Grafana

**Problème** : Des difficultés ont été rencontrées lors de l'installation de Grafana en suivant le tutoriel de la page d'installation officielle.

**Solution** : Il a été découvert que le tutoriel dans les documents officiels était plus adapté et complet. En suivant ce guide, l'installation de Grafana a pu être réalisée avec succès.

## Communication MQTT

**Problème** : Le protocole MQTT, initialement prévu pour la communication entre les composants du système, a cessé de fonctionner sans raison apparente.

**Solution** : Malgré plusieurs tentatives, la cause du dysfonctionnement n'a pas pu être identifiée. En conséquence, la technique de communication via MQTT a été temporairement abandonnée. D'autres méthodes de communication sont en cours d'évaluation pour pallier ce problème.

# Conclusion

A travers ce projet de centre de calcul, nous avons commencé à développer une solution au traitement des déplacements de robots et à leur analyse dans un dispositif externe. Grâce à l'utilisation de technologies telles que la Raspberry Pi 5, InfluxDB, et Grafana, nous avons pu mettre en place un système capable de détecter et de suivre les positions des tags AruCo en temps réel, tout en offrant une interface de visualisation directe et un post-traitement des données disponible à tout moment.

Les défis rencontrés, notamment en matière de traitement d'image et de communication entre les composants, ont été surmontés grâce à des solutions techniques adaptées. La mise à l'échelle et la transformation de perspective ont permis d'obtenir une représentation fidèle du terrain de jeu, tandis que l'intégration d'InfluxDB et de Grafana a assuré une gestion robuste et une visualisation claire des données.

Les perspectives d'évolution incluent l'optimisation de l’algorithme de traitement d'images, l'amélioration du post-traitement des données, et de leur utilisation à des fins décisionnelles durant les matchs. Un développement de l’interface du logiciel, en permettant une sélection de diverses options (écran d’étalonnage de la caméra, lancement de la collecte des données) est étudiée. De même, une amélioration et un développement des caractéristiques physiques sont prévus afin d’optimiser au mieux l’utilisation du centre de calcul.

En conclusion, ce projet a ouvert la voie à de futures innovations et développements des robots pour la coupe de France de robotique. 
