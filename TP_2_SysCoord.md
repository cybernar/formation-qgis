# TP partie 2. Systèmes de coordonnées

## TP Utilisation en ligne de l'outil cs2cs

Dans un tableur, ouvrez le fichier `Donnees_Climato_Dept34.xlsx` dans le répertoire `data/Occitanie/CD34`. Ce fichier contient la position des stations météo départementales de l'Hérault, géoréférencées dans le système de coordonnées Lambert 93 (EPSG:2154).

Avec l'outil en ligne **MyGeoData CS2CS** ([https://mygeodata.cloud/cs2cs/](https://mygeodata.cloud/cs2cs/)) , convertissez en WGS84 (EPSG:4326) les coordonnées des 10 premières lignes du fichier.

## TP Identification du système de coordonnées

Ouvrez Quantum GIS 3.10 Desktop. Dans un nouveau projet, ajoutez les couches suivantes, dans cet ordre :

- Départements (`DEPARTEMENT_CARTO.shp` dans `data/IGN_ETALAB/ADMIN-EXPRESS-COG`)
- Lignes SNCF (`formes-des-lignes-du-rfn.shp` dans `data/SNCF`)

Identifiez le système de coordonnées du projet.

Identifiez le système de coordonnées de chaque couche.

Ajoutez la couche des gares géoréférencées à partir du fichier `liste-des-gares.csv` : vous avez le choix entre les coordonnées en Lambert 93 (X_L93;Y_L93), ou en WGS84 (X_WGS84;Y_WGS84).

Enregistrer cette couche de points dans un nouveau shapefile `liste_des_gares_l93.shp` en Lambert 93 (clic-droit sur la couche de points + _Exporter / Sauvegarder les entités sous_).

## TP Projection à la volée, calcul de longueur

### Exemple avec un fichier GPX

Dans un nouveau projet, ouvrir le fichier `monthaut2.gpx` dans le répertoire `GPS` (_waypoints_, _tracks_ et _track_points_). _Les fichiers GPX sont en WGS84._  

Superposer la trace issue du fichier GPS monthaut2.gpx et la carte topo de la zone concernée : dalle Scan25 de Saint-Guilhem-Le-Désert (répertoire : `SCAN25`, fichier   `SC25_TOPO_0740_6300_L93_E100.jp2` et `SC25_TOPO_0740_6300_L93_E100.jp2`).

Définir le SCR du projet : EPSG:2154 (Lambert 93).

Cliquez sur la trace GPS avec l'outil _Identifier_ : dans _(Dérivé)_, relevez la longueur sur l'ellipsoïde.

Avec l'outil _"Exporter/ajouter des colonnes de géométrie"_ (dans la boîte à outils de traitement : _Géotraitement QGIS > Outils de table d'un vecteur_), mesurez la longueur totale de la trace  en mètres avec le _SCR du projet_.

Dans la couche obtenue, relevez la valeur du champ _length_ : il s'agit de la longueur de la trace projetée en Lambert 93.

## Indicatrices de Tissot, Pseudo-Mercator et extension QuickMapServices

Dans un nouveau projet, ouvrez ces couches incluses dans le fichier GeoPackage `data/NaturalEarth/NaturalEarth2020.gpkg` :

- `ne_50m_admin_0_countries`
- `ne_50m_coastline`
- `ne_50m_geographic_lines`
- `ne_50m_graticules_30`

Utilisez l'outil _Identifier_ sur la couche `ne_50m_geographic_lines` et la longueur indiquée dans _(Dérivé)_ pour relever :

- la longueur de l'Equateur
- la longueur des Tropiques
- la longueur des Cercles Polaires

!!!note
	Les indicatrices de Tissot sont des disques de surface égale utilisables pour mesurer les déformations liées aux projections. Dans le fichier `data/NaturalEarth/tissot.shp`, les disques ont un rayon de 640 km.

Superposer les indicatrices de Tissot et le quadrillage à 30°  (fichier `ne_50m_graticules_30.shp` et `tissot.shp`.

Utiliser le mécanisme de projection à la volée pour afficher successivement la carte du monde avec les 3 projections suivantes :

1. World Robinson / ESPG:54030
2. World Winkel-Tripel / EPSG:54042
3. Pseudo-mercator / EPSG:3857

Laisser le projet en Pseudo-Mercator. Il s'agit d'un système de coordonnées couramment utilisé en webmapping ; installer l'extension "QuickMapServices" et ajoutez le fond de carte OpenStreetMap.

Avec l'outil _"Exporter/ajouter des colonnes de géométrie"_  (cf. TP précédent), calculer la surface des indicatrices le système Pseudo-Mercator (SCR du projet) pour quantifier la distortion de surface des cercles.

## TP Transformation de données raster dans un nouveau système de coordonnées

Ouvrez une des dalles NASADEM du Liban (WGS84) et convertissez-la dans le système de coordonnées UTM 36N  (EPSG:32636) avec une résolution de 30 mètres par pixel. Pour cela, utilisez l'outil _"Projection(Warp)"_ accessible depuis la  boîte à outils  _GDAL > Projections_

!!!note
	Nous verrons plus en détail les reprojections de données raster dans la **partie 5**.
