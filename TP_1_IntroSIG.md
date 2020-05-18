# TP partie 1. Introduction aux SIG, prise en main de QGIS

## TP Affichage des couches et navigation dans les données

Ouvrez Quantum GIS 3.10 Desktop

### Utilisation de données vecteur

Examinez la liste des fichiers du répertoire `data/IGN_ETALAB/ADMIN-EXPRESS-COG`.

Affichez le shapefile des limites d'arrondissements (`ARRONDISSEMENT_DEPARTEMENTAL_CARTO.shp`) et de communes d'Occitanie (`COMMUNE_CARTO.shp`).

1. Ouvrez la table attributaire de la couche `ARRONDISSEMENT_DEPARTEMENTAL_CARTO` (clic-droit + _Ouvrir la table d'attributs_) et trier selon la colonne "INSEE_DEP". Quels sont les arrondissements du département de l'Aude (11) ?
2. Ouvrez les propriétés de la couche `COMMUNE_CARTO` (clic-droit + _Propriétés_). _Quels sont les caractéristiques de ce shapefile (système de coordonnées, géométrie, liste des attributs) ?_
3. Ouvrez la table attributaire de la couche `COMMUNE_CARTO` (clic-droit + _Ouvrir la table d'attributs_). Observez la barre de titre de la fenêtre : quel est le nombre de communes ?
4. Essayez d’identifier la commune de Juvignac sur la carte en la sélectionnant dans la table attributaire. _Pour trouver rapidement la commune dans la table, vous pouvez utiliser la fonction de tri ou de filtrage._
5. D’après le champ "POPULATION", quelle est la population de Juvignac ?

### Utilisation de l'outil _Identifier les entités_

Utilisez l'outil _Identifier_ sur Juvignac et déplier _(Dérivé)_ : quel est la longueur du périmètre de la commune ?

### Utilisation de données raster

Affichez la dalle Scan25 de Saint-Guilhem-Le-Désert. Répertoire : `data/IGN_ESR/SCAN_25`, fichier `SC25_TOPO_0740_6300_L93_E100.jp2`.

Zoomez sur la dalle (clic-droit + _Zoomer sur la couche_).

Inspectez les propriétés du raster (clic-droit + _Propriétés_), onglet _Information_ et tentez de répondre aux questions suivantes :

1. Quelle est la résolution du raster (_Taille du pixel_) ?
2. Quelle est la taille du raster en pixels (_Largeur_, _Hauteur_) ?
3. Par déduction, quelle est la surface couverte par le raster ?
4. Quel est le système de coordonnées (_SCR_) ?
5. Quelles sont les coordonnées du point d'origine, en haut à gauche (_Origine_) ?

Affichez l'orthophoto de Puéchabon. Répertoire : `data/IGN_ETALAB/BD_ORTHO_0M50`, fichier `34-2015-0745-6305-LA93-0M50-E080.jp2`.

1. Quelle est la résolution de chaque raster ?
2. Quelle est la taille en pixels ?
3. Par déduction, quelle est la surface couverte par le raster ?
4. Quel est le système de coordonnées ?
5. Quelles sont les coordonnées du point d'origine, en haut à gauche (_Origine_) ?

### Ordre des couches

Modifiez l'ordre des couches en faisant glisse la dalle Scan25 au-dessus puis en dessous des orthophotos

### Utilisation du service de données WMTS de l’IGN

Pour voir les données topo de l’IGN sur toute la métropole, définissez une nouvelle connexion à un serveur WMS avec les paramètres suivants :

- Nom = `IGN WMTS Lambert 93`
- URL =
 `http://wxs.ign.fr/......................../wmts?SERVICE=WMTS&VERSION=1.0.0&REQUEST=GetCapabilities` 

!!!note

	L'URL valide avec la clé Géoportail figure dans le fichier `Parametre_WMTS_IGN_CEFE.txt` du répertoire `data/IGN_ESR`

Puis ajoutez la couche intitulée "SCAN25 Topographique L93"

### Outil de mesure

Mesurez (approximativement) la longueur maximale sur la commune de Montpellier (distance entre les 2 points les plus éloignés de la commune).

### Groupe de couches

Sélectionnez les 2 couches  `ARRONDISSEMENT_DEPARTEMENTAL_CARTO` et `COMMUNE_CARTO` dans le panneau des couches, et groupez-les en 'ADMIN OCCITANIE' avec clic-droit + _Grouper la sélection_

### Affichage / désaffichage des panneaux

Fermez les panneaux _Couches_ et _Identifier les résultats_. Affichez les de nouveau avec le menu _Vue / Panneaux_

### Création d'un projet

Enregistrez le projet sous le nom `occitanie.qgs`

## TP Couche de points "texte délimité"

Dans un tableur (exemple : LibreOffice Calc ou Microsoft Excel), ouvrez le fichier des Stations Météorologiques de l'Hérault. Répertoire `data\Occitanie\CD34`, fichier `Donnees_Climato_Dept34.xlsx`. Dans quelles colonnes sont enregistrées les coordonnées X et Y des stations ?

Enregistrez les données au format .txt UTF-8, avec séparateur : Tabulations (noms des colonnes sur la 1ère ligne).

Ouvrez le fichier `stations.txt` obtenu et affichez les points dans QGIS avec l'outil Texte délimité. Les coordonnées des stations sont en Lambert 93 (identifiant : **IGNF:LAMB93** ou **EPSG:2154** dans la liste des SCR)

Sauvegardez la couche au format Shapefile : `stations.shp` avec un clic-droit + _Enregistrer sous_ sur la couche.

## TP Installation et utilisation d'extensions

- Installez l'extension **QuickMapServices** avec le menu _Extension / Installer, gérer des extensions_. Cherchez l'extension dans la liste et cliquez sur le bouton _Installer l'extension_. Une nouvelle entrée doit apparaître dans le menu _Internet_
- Afficher le fond de carte OSM Standard
