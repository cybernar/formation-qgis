# TP partie 5. Données raster.

## TP Calculs sur un MNT

Dans un nouveau projet, ouvrez les couches vectorielles suivantes :

- `COMMUNE.shp` dans le répertoire `data\IGN_ETALAB\ADMIN-EXPRESS-COG`
- `CoursEau_76_Occitanie.shp` dans le répertoire `data\IGN_ETALAB\BD_Carthage`
- `CLC12_RLRMP_RGF.shp` dans le répertoire `data\Occitanie\CLC12`

Pour visualiser les différents types d'occupation du sol avec les couleurs standard de Corine Land Cover, ouvrez les propriétés de la couche et chargez le fichier de style `CLC12.sld` dans le dossier `FichiersLegende`.

Ouvrez le raster inclus dans le répertoire `data\IGN_ESR\BD_ALTI_25M` :

- `BDALTIV2_25M_FXX_0725_6300_MNT_LAMB93_IGN69.asc`

Enregistrez le projet sous le nom `TP5.qgz`, et créez dans `data` un nouveau répertoire `TP5` pour les fichiers générés dans les exercices suivant. 

### Conversion entre formats de raster

La BD ALTI de l'IGN est distribuée au format [Arc/Info ASCII Grid](https://gdal.org/drivers/raster/aaigrid.html) (extension `.asc`). Les dalles de la BD ALTI pour la métropole sont en Lambert 93, mais le système de coordonnées n'est pas spécifié au niveau du fichier (remarquez le point d'interrogation à droite du nom de la couche).

Vous allez dans ce premier exercice convertir le fichier raster `BDALTIV2_25M_FXX_0725_6300_MNT_LAMB93_IGN69.asc` au format GeoTIFF, en spécifiant le système de coordonnées pour le fichier de sortie. Utilisez l'outil _Convertir_, dans *GDAL / Conversion raster*, avec les options suivantes :

- Couche en entrée = `BDALTIV2_25M_FXX_0725_6300_MNT_LAMB93_IGN69`
- Ecraser la projection = `IGNF:LAMB93` (ou `EPSG:2154`)
- Paramètres avancés / profil = *Défaut*
- Type de données en sortie = utiliser type de la couche en entrée
- Fichier sortie converti = *Enregistrer vers un fichier* > `TP5/0725_6300_MNT.tif`

Vous avez converti le raster au format **GeoTIFF, sans compression**. Comparez la taille du fichier `0725_6300_MNT.tif` obtenu, avec le fichier d'origine `.asc`. Puis essayez de nouveau l'outil *Convertir* avec les mêmes paramètres que précédemment, mais avec ma méthode de compression **Deflate** :

- Paramètres avancés / profil = *Compression élevée*
- Fichier sortie converti = *Enregistrer vers un fichier* > `0725_6300_MNT_Deflate.tif`

### Analyse de terrain

A partir du MNT, vous allez calculer la **pente du terrain en degrés** avec l'outil *Pente*, dans *GDAL / Analyse raster*, et les options suivantes :

- Couche source = `0725_6300_MNT.tif`
- Couche sortie pente = *Enregistrer vers un fichier* > `0725_6300_pente.tif`

Pour mieux faire ressortir les zones escarpées, changez la symbologie de la couche et appliquez sur la couche `0725_6300_pente` un rendu de type 'Pseudo-couleur bande unique', avec par exemple la palette **RdBu (inversée)** et les 5 bornes suivantes : **0, 10, 20 , 40, 65**.

![Pente][1]

!!!tip "Astuce"
	Pour obtenir un rendu transparent avec le fond de carte IGN : 
	**(1)** afficher la couche WMTS "Pyramide ScanExpress, légende Niveaux Gris L93" (_voir partie 1 de la formation pour la connexion WMTS_) puis 
	**(2)** dans les propriétés du raster `0725_6300_pente` > Symbologie > Rendu de la couleur, choisir *Mode de fusion = Multiplier*.

Toujours à partir du MNT, calculez l'orientation du terrain, avec l'outil *Exposition* dans *GDAL / Analyse raster* et les options suivantes :

- Couche source = `0725_6300_MNT.tif`
- Couche sortie pente = *Enregistrer vers un fichier* > `TP5/0725_6300_orientation.tif`

Dans le fichier raster en sortie, les valeurs des pixels correspondent à l'azimuth en degrés (0° = nord, 90° = est, etc.) 

Enregistrez le projet.

![Orientation][2]

## TP Rastérisation et combinaison de raster

### Rastérisation de la Couche CLC12

Dans cet exercice vous allez convertir en raster le shapefile `CLC12_RLRMP_RGF.shp`, en prenant comme zone de travail l'étendue du raster `0725_6300_MNT.tif`, et avec la même résolution (25 mètres). Dans les pixels du nouveau raster, on doit retrouver le code CLC niveau 3. 

Ouvrez les propriétés de la couche `CLC12_RLRMP_RGF` et inspectez les caractéristiques des champs. Le champ "CODE_12" est de type texte. Or, pour convertir le shapefile en raster, il faut que le champ qui porte les valeurs soit de type numérique. Nous allons donc convertir les codes en entier dans un nouveau champ.

Ouvrez la calculatrice de champ, créez un nouveau champ "INTCODE_12" de type *Entier*, de longueur *10*, avec l'expression suivante : `to_int("CODE_12")`. Cliquez sur OK et enregistrez.

Avec l'outil *Rastérisation (vecteur vers raster)* dans *GDAL / Conversion vecteur* et les options suivantes, lancez la rastérisation :

- couche en entrée = `CLC12_RLRMP_RGF`
- champ à utiliser pour la valeur des pixels = "INTCODE_12"
- unités du raster résultat = *Unités géoréférencées* (remarque : c'est pour pouvoir spécifier une résolution en mètres dessous)
- résolution horizontale et verticale = *25*
- emprise du résultat = *724987.5,749987.5,6275012.5,6300012.5 [EPSG:2154]* (cliquez sur *Utiliser l'emprise de la couche* > `0725_6300_MNT`)
- affecter une valeur nulle = *-99* (remarque : c'est la valeur des pixels pour *Absence de données*)
- type de données en sortie = *Int16* (valeurs entières)
- couche sortie rastérisé = *Enregistrer vers un fichier* > `CLC12_Rasterise.tif`

![Orientation][3]

### Rastérisation de la Couche CoursEau

Nous allons rastériser les cours d'eau, avec comme valeur pour les pixels : **1** si ils sont traversés par un cours d'eau, sinon **Absence de données**. Cette fois-ci, on utilise donc  une valeur constante et plus  un attribut.

Avec l'outil *Rastérisation (vecteur vers raster)* dans *GDAL / Conversion vecteur*, spécifiez les options suivantes et lancez la rastérisation :

- couche en entrée = `CoursEau_76_Occitanie`
- Valeur fixe à utiliser pour les pixels = *1*
- unités du raster résultat = *Unités géoréférencées* (pour pouvoir spécifier une résolution en mètres dessous)
- résolution horizontale et verticale = *25*
- emprise du résultat = *724987.5,749987.5,6275012.5,6300012.5 [EPSG:2154]* (cliquez sur *Utiliser l'emprise de la couche* > `0725_6300_MNT`)
- affecter une valeur nulle = *0* (c'est la valeur des pixels pour *Absence de données*)
- type de données en sortie = *Byte* (valeurs entières entre 0 et 255)
- couche sortie rastérisé = *Enregistrer vers un fichier* > `CoursEau_Rasterise.tif`

Enregistrez le projet.

![Orientation][4]

### Calculatrice raster

A l'aide de la calculatrice raster, vous allez combiner les couches raster *CLC12* et *Orientation* pour rechercher les zones de type 'Forêts de feuillus' (code = 311), sur des terrains orientés entre le sud-est et le sud-ouest (azimuth compris entre 135° et 225°).

Ouvrez la calculatrice par le menu *Raster / Calculatrice raster*, et définissez le fichier en sortie :  

- Couche en sortie = `terrain_S_P5.tif`
- Format = GeoTIFF

Pour l'expression, les 3 prédicats suivants articulés avec l'opérateur booléen 'AND' donneront en sortie des pixels *0 (faux)* ou *1 (vrai)*.

- Expression = 
`"CLC12_Rasterise@1" = 311  AND "0725_6300_orientation@1" >= 135 AND "0725_6300_orientation@1" <= 225`

En appliquant sur le raster un rendu de type "Palette / valeurs uniques" et en n'affichant que la classe de valeur=1, vous obtiendrez une carte de cette apparence :

![Calculette][5]

Enregistrez le projet.

### Raster de distance

A partir du raster `CoursEau_Rasterise.tif` précédemment créé, calculez un nouveau raster `dist_courdo.tif` dans lequel chaque pixel portera la distance par rapport au cours d'eau le plus proche. Vous utiliserez l'outil _Proximité_ (dans *GDAL / Analyse raster*) et les options suivantes :

- couche en entrée = `CoursEau_Rasterise.tif`
- liste des valeurs de pixel = *1*
- unités de distance = *Coordonnées référencées*
- distance maximale = laisser 0 (c'est à dire pas de distance max)
- valeur nulle = *-99*
- type de données en sortie = *Float32*
- fichier sortie proximité = `dist_courdo.tif`

![Proximite][6]

Enregistrez le projet.

## TP Extraction de données raster

Restez dans le même projet et ouvrez le fichier `monthaut2.gpx` dans le répertoire `Autres_donnees`. Affichez les *waypoints* uniquement.

### Installation de l'extension Point Sampling Tool

Ouvrez le gestionnaire d'extensions de QGIS et installez l'extension 'Point Sampling Tool'. **L'outil est accessible sous la forme d'un nouveau bouton _Point Sampling Tool_ dans la barre d'outils _Extensions_.**

Cette extension permet d'extraire, pour une couche de points, les valeurs des rasters et des couches polygonales à la position des points. Pour que cela fonctionne, il faut que **toutes les couches partagent le même système de coordonnées**.

### Extraction de données raster dans un fichier de points

Sélectionnez manuellement les 5 waypoints situés sur l'emprise du raster `0725_6300_MNT.tif`. Avec un clic-droit sur la couche + *Exporter / Sauvegarder les entités sélectionnées sous*,  exportez les 5 points sélectionnées dans un nouveau shapefile `waypoints_monthaut_l93.shp` (*SCR = EPSG:2154*).

Dans le panneau des couches, rendez visibles les couches suivantes : `0725_6300_MNT` (raster altitude), `0725_6300_pente` (raster pente), `0725_6300_orientation` (raster azimut), `CLC12_RLRMP_RGF` (polygones CLC), `COMMUNE` (polygones communes), 
 `dist_courdo` (raster distance).

Ouvrez l'outil _Point Sampling Tool_, choisissez la couche de points `waypoints_monthaut_l93` en entrée, et un nouveau shapefile en sortie : `waypoints_monthaut_PST.shp`. Pour les champs et bandes d'où extraire les valeurs, sélectionnez dans la liste avec clic + touche CTRL :

- waypoints_monthaut_l93:ele 
- waypoints_monthaut_l93:name
- waypoints_monthaut_l93:desc
- CLC12_RLRMP_RGF:CODE_12
- COMMUNE:INSEE_COM
- COMMUNE:NOM_COM
- 0725_6300_MNT:bande1
- 0725_6300_pente:bande1
- 0725_6300_orientation:bande1
- dist_courdo:bande1

![point sampling tool][7]

!!!tip "Astuce"
	Pour afficher des étiquettes avec plusieurs attributs comme dans la carte ci-dessus, utilisez une expression avec la fonction de concaténation et avec des retours à la ligne, par exemple : 

		concat( 'NAME=' ,  "name" ,  '\nCOM=' , left("NOM_COM",9) , '\nDist.eau=' , round("CoursEau_p") , ' m.\nPente=' , round("pente",1) , ' °\nAzim=' , round("orientatio") , '° \nAlti=' , round("0725_6300_",1), ' m.' ) 

	Configurez les étiquettes avec un arrière-plan et des connecteurs. Utilisez l'outil de déplacement dans la barre d'outils _Etiquettes_ pour changer leur position.

Enregistrez le projet.

### Statistiques zonales

Dans cet exercice nous allons calculer les statistiques relatives à l'altitude pour les communes de l'arrondissement de Lodève, du moins celles qui sont incluses dans le raster `0725_6300_MNT` : pour chaque commune, nous calculerons altitude moyenne, minimale et maximale.

Sélectionnez les entités de `COMMUNE` avec cette expression : `"INSEE_DEP" = '34' AND  "INSEE_ARR" = '2'`. Enregistrez les entités sélectionnées dans un nouveau shapefile `COMMUNE_D34_A2.shp` (clic-droit + *Exporter / Sauvegarder les entités sélectionnées sous*).

Pour chaque entité de la nouvelle couche, calculer les valeurs agrégées avec l'outil _Analyse Raster / Statistiques de zone_.

- Couche raster = `0725_6300_MNT`
- Couche vecteur = `COMMUNE_D34_A2`
- Préfixe = _
- Statistiques à calculer = Compte, Moyenne, Min, Max

Les 4 nouveaux champs sont ajoutés à la couche `COMMUNE_D34_A2`. 
Inspectez le résultat dans la table attributaire. Que se passe-t-il pour les communes qui sont à la fois dedans et en dehors du raster (comme Puéchabon), et pour celles qui sont complétement en dehors (comme Argelliers) ?

Enregistrez le projet.

## TP Statistiques focales

### Statistiques de voisinage avec Grass

Avec l'outil *r.neighbors* dans *GRASS GIS 7 / Raster*, calculer dans un nouveau raster `CLC12_neighbours.tif` le nombre de type d'occupations du sol différents dans un rayon de 200 m autour de chaque pixel (fenêtre de voisinage circulaire 17x17). Utiliser les paramètres suivants :

- input = `CLC12_Rasterise.tif`
- neighborhood operation = *diversity*
- neighborhood size = *17*
- use circular neighborhood = _OUI_
- output layer = `CLC12_neighbours.tif`

Le mode d'emploi de la commande est décrit dans le manuel en ligne Grass 7 : <https://grass.osgeo.org/grass78/manuals/r.neighbors.html>

Un autre exemple d'utilisation, avec le même outil et les mêmes données en entrée, pourrait être de chercher l'occupation du sol la plus représentée dans un rayon de 200 m autour de chaque pixel (neighborhood operation = `mode`).

Enregistrez le projet.

## TP Création de fond de carte à partir d'un MNT

Dans ce dernier exercice, vous allez créer un fond de carte du Liban à partir de 4 rasters NASADEM téléchargés sur <https://earthdata.nasa.gov/>.

Dans un nouveau projet, ouvrez les 4 rasters suivants dans le répertoire `data\NASADEM` : 

- n33e035.hgt
- n33e036.hgt
- n34e035.hgt
- n34e036.hgt

Ouvrez la couche `ne_10m_admin_0_countries` (frontières) dans le fichier GeoPackage `data/NaturalEarth/NaturalEarth2020.gpkg`.

Zoomez sur le Liban et définissez comme SCR du projet (*Projet / Propriétés* > SCR) le système 'WGS84 / UTM36N (EPSG:32636'. 

Enregistrez le projet sous le nom `TP5_Liban.qgz`.

### Etape 1 : assembler les rasters

Créez un raster unique à partir des 4 fichiers `.hgt` avec l'outil *Fusionner*, dans *GDAL / Divers rasters* :

- couches en entrée = les 4 rasters *n33e035, n33e036, n34e035, n34e036*
- type de données en sortie = *Int16* ou *Float32* (remarque: les rasters en entrée sont de type Int16)
- fichier sortie fusionnée = `DEM_assemblage.tif`

### Etape 2 : projeter le raster en UTM

Le raster assemblé précédemment, tout comme les 4 dalles NASADEM, est en WGS84. Nous allons maintenant le projeter dans le système UTM, fuseau 36N (celui du Liban) avec l'outil *Projection (gdalwarp)*, dans *GDAL / Projections rasters* :

- couche en entrée = `DEM_assemblage.tif`
- SCR destination = EPSG:32636
- méthode de réechantillonage = *Bilinéaire*
- valeur Nodata pour le fichier en sortie = *-9999*
- résolution fichier en sortie = *30*
- type de données en sortie = *Float32*
- étendu du fichier en sortie = laisser blanc - ou bien *Sélectionner l'emprise depuis le canevas* et dessiner un rectangle autour du Liban
- fichier sortie = `DEM_Liban_UTM36N.tif`

Un MNT dans un système de coordonnées projeté métrique permet d'utiliser les outils raster tels que la création d'ombrage (voir ci-dessous), le calcul de pente.

### Etape 3 : définir un rendu adapté à la topographie

Dans les propriétés de la couche `DEM_Liban_UTM36N.tif`, onglet Symbologie, définir un rendu de type _Pseudo-couleur bande unique_ avec *Interpolation = Discret* et 7 classes d'altitude :

- bleu jusqu'à 0m
- vert foncé pour 1 à 500m
- vert clair pour 501 à 1000 m
- jaune pour 1001 à 1500 m
- marron clair pour 1501 à 2000 m
- marron foncé pour 2001 à 2500 m
- gris clair pour + de 2500 m

Enregistrez ce style comme "style par défaut" (fenêtre _Propriétés_, bouton _Styles_), ce qui génèrera un nouveau fichier `DEM_Liban_UTM36N.qml`

!!!tip "Astuce"
	Pour les couleurs applicables à une carte topographique, vous pouvez trouver de l'inspiration de cette page : <https://en.wikipedia.org/wiki/Wikipedia:WikiProject_Maps/Conventions/Topographic_maps>. 

	Dans le répertoire `Autres_donnees`, vous trouverez également 2 fichiers de palettes `topo_couleurs_8.gpl` et `topo_couleurs_30.gpl` (respectivement 8 et 30 couleurs) inspirés de la page précédente, à importer dans QGIS via le _Gestionnaire de symboles_

### Etape 4 : créer un ombrage transparent

Avec l'outil *Ombrage* dans *GDAL / Analyse raster*, créez un nouveau raster `Ombrage_Liban_UTM36N.tif` :

- couche élévation entrée = `DEM_Liban_UTM36N.tif`
- azimut = 300
- angle = 40
- couche ombrage sortie = `Ombrage_Liban_UTM36N.tif`

Ouvrez les propriétés de la couche `Ombrage_Liban_UTM36N` et dans l'onglet *Transparence*, modifier le paramètre *Opacité* (par exemple à 60%). Dans l'onglet *Symbologie* vous pouvez aussi modifier le mode de fusion = 'Multiplier'.

![Carte Liban][8]

[1]: img/pente_stguilhem.png 
[2]: img/orientation_stguilhem.png 
[3]: img/rasterisation_clc.png
[4]: img/rasterisation_courdo.png
[5]: img/calc_feuillus_sud2.png
[6]: img/distance_courdo.png
[7]: img/point_sampling_tool.png
[8]: img/carte_liban.png