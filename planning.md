# Formation Initiation au SIG avec QGIS, mars 2020

cyril.bernard@cefe.cnrs.fr

Lieu: CEFE, salle polyvalente. Pour s'inscrire : <https://evento.renater.fr/survey/initiation-au-sig-avec-qgis-2020-ox2rtko1>

## 1. Prise en main de QGIS, formats de données SIG

### Mercredi 18 mars, 9h30-12h30

**Programme :** Il s'agit d'une introduction aux données spatiales, pour les débutants complets en QGIS. Je présenterai les principaux formats de données vectorielles et raster.
Nous parlerons aussi de QGIS : de quelles briques logicielles est-il fait ? que peut-on faire avec ?
Et bien sûr nous commencerons à manipuler QGIS, avec l'affichage et exploration des données.

1. Introduction aux SIG : données vectorielles et raster
2. QGIS et les logiciels SIG Opensource. Installation du logiciel
3. Les extensions : installation et utilisation
4. Affichage et navigation dans les données
5. Questions / réponses

**Durée :** 3 h

**Prérequis :** aucun. Amener un ordinateur portable avec QGIS 3.4 installé. QGIS existe sur Windows, Linux et Mac. Les 3 systèmes sont acceptés.  

**Mots-clés :** *information géographique, données géoréférencées, formats de données, QGIS, afficher des données, identifier, géométrie, format WKT, champs, table attributaire, type de champ, OGR, GDAL, ESRI Shapefile, GeoTIFF, Arc/Info ASCII Grid, KML, échelle, précision, résolution*

## 2. Systèmes de coordonnées

### Mercredi 18 mars, 14h00-16h30

**Programme :** Comment passe-t-on de coordonnées en longitude/latitude à des coordonnées projetées en mètres ? Quelles sont les implications des systèmes de coordonnées sur les calculs de surface ou de distance ? A quoi correspondent les différents systèmes de coordonnées sur les cartes IGN ? Comment transformer un shapefile d'un système de coordonnées à un autre ? Quelques questions que nous traiterons dans cette séance.

1. Systèmes de coordonnées géographiques vs projetés
2. Les différents systèmes de coordonnées en France
3. Gestion des systèmes de coordonnées dans QGIS : projection à la volée, transformations, codes EPSG et définitions PROJ4
4. Utilisation du plugin 'QuickMapServices' : websig et système Pseudo-Mercator
5. Questions / réponses. Pour ceux qui rencontrent des problèmes de coordonnées, je vous propose de travailler ensemble avec vos propres données

**Durée :** 2 h 30

**Prérequis :** connaître les formats de données Shapefile et GeoTiff car nous utiliserons ce type de fichiers

**Mots-clés** : *coordonnées géographiques, coordonnées projetées, terre, ellipsoïde, datum géodésique, GPS, projection, cartographie, calculs de distance, transformation, projection à la volée, ré-échantillonnage, PROJ4, Lambert 93, WGS84, UTM, Pseudo-Mercator*

## 3. Définir des styles pour explorer et mettre en valeur les données. Mettre en page des cartes pour les enregistrer en PDF ou les insérer dans des documents

### Jeudi 19 mars, 14h00-17h30

**Programme :** Cette séance est consacrée à la définition de styles dans QGIS, indispensables à la création de cartes thématiques et à l'exploration des données. Les styles consistent notamment à définir des règles de correspondance entre couleurs et valeurs numériques sur des données vectorielles ou raster. Nous verrons également la mise en page de cartes pour générer des documents PDF ou des images (PNG, JPEG).  

1. Habillage des données : application de styles sur des données vectorielles
2. Habillage des données : application de styles sur des données raster
3. Gestionnaire de symboles
4. Etiquettes
5. Mise en page de documents avec le composeur d’impression

**Durée :** 3 h 30

**Prérequis :** connaître les principaux formats de données SIG (shapefile, GeoTiff) et les fonctions de base de QGIS (vues en séance 1)

**Mots-clés :** *catégoriser, classer, choroplèthe, couleurs, symboles, quantiles, intervalles égaux, styles, étiquettes, QML, légendes, mise en page, PDF, JPG, PNG, résolution, format, échelle*

## 4. Utilisation de données vectorielles : création, requêtes, traitements, analyses

### Vendredi 20 mars, 9h30-12h30 et 14h00-18h00

**Programme :** Cette séance est consacrée à l'édition, aux traitements et à l'analyse des données vectorielles dans QGIS. Pour commencer, quelques exemples d'utilisation des outils de sélection (par attribut ou par localisation), et de la calculatrice de champs. Une partie importante de cette session sera consacrée aux outils de géotraitements - en particulier l'intersection de couches - avec un exemple de calcul sur la taille effective de maille (un indice de fragmentation). Les outils de saisie seront abordés brièvement.

**Durée :** 1 journée (7 h).

**Prérequis :** connaître les formats de données vectoriels et leur utilisation dans les SIG, bien connaître les styles dans QGIS (cf. séance 3)

**Mots-clés :** *sélection par attributs, critère, requêtes spatiales, jointure, relations spatiales, calcul de surface, calcul de longueur, mises à jour, outils de saisie, photo-interprétation, découpage, intersection, recouvrements, analyse spatiale*

1. Requêtes attributaires et spatiales. Jointures
2. Création de nouvelles couches et outils de numérisation
3. Edition des données attributaires : la calculatrice de champ
4. Création de grilles vecteurs pour analyser des données  
5. Traitements sur des données vectorielles : intersection

## 5. Traitements et analyse sur les raster avec QGIS

### Lundi 23 mars, 14h00-18h00

**Programme :** Cette séance est consacrée aux outils raster de QGIS. Ces outils reposent très largement sur les utilitaires GDAL (gdalwarp, gdal\_rasterize, gdal\_translate, gdal\_merge). Nous testerons ces utilitaires pour convertir des rasters, appliquer des masques, assembler des rasters,  rastériser des données vectorielles. Nous passerons en revue les outils QGIS pour modèles numériques de terrain, la création de raster de distance et utiliserons la calculatrice raster pour combiner des données.

**Durée :** 4 h

**Prérequis :** connaître les formats de données raster et leur utilisation dans les SIG, maîtriser les styles dans QGIS.

**Mots-clés :** *utilitaires GDAL, rasterisation, ré-échantillonnage, raster de distance, analyse spatiale, calculatrice raster, isolignes, statistiques zonales*

1. Les utilitaires GDAL : reprojeter, rastériser, assembler
2. Opérations sur les MNT : calcul de pente, orientation, isolignes
3. Extraction de données raster sur une couche de points, statistiques zonales
4. Analyses en mode raster : croiser des données rasters avec la calculatrice
