# TP partie 4B. Outils de traitement sur les données vectorielles.

CEFE-CNRS, mars 2020

Cyril Bernard (cyril.bernard@cefe.cnrs.fr)


## TP Traitements

Dans un nouveau projet, ouvrez la couche suivante incluse dans le répertoire `data\IGN_ETALAB\ADMIN-EXPRESS-COG` :

- `DEPARTEMENT.shp`

Ouvrez la couche suivante incluse dans le répertoire `data\Occitanie\CLC12` : 

- `CLC12_RLRMP_RGF.shp`

    Pour visualiser les différents types d'occupation du sol avec les couleurs standard de Corine Land Cover, ouvrez les propriétés de la couche et chargez le fichier de style `CLC12.sld` dans le dossier `FichiersLegende`.

Ouvrez les couches suivantes incluses dans le répertoire `data\Occitanie\Region` :

- `trace-du-reseau-autoroutier-doccitanie.shp`
- `dreal-occitanie-mats-eoliens-en-occitanie.csv`

    Affichez la couche des mâts éoliens en Occitanie (fichier `.csv`) en tant que couche _Texte Délimité_, avec le codage UTF-8. Attention, les colonnes contenant les coordonnées sont "xcoord" et "ycoord" (SCR = Lambert 93, EPSG:2154)

### Création d'un GeoPackage

Dans ce TP, nous vous proposons d'enregistrer toutes les couches issues des traitements dans une base de données GeoPackage (un fichier `.gpkg`), plutôt que générer de multiples shapefiles.

Créez le fichier `tp4_exercices.gpkg` dans le répertoire `data`, en exportant la couche des mâts éoliens : clic droit + *Exporter / Sauvegarder les entités sous ...* avec les options suivantes :

- format = `GeoPackage`
- nom de fichier = `tp4_exercices.gpkg` (bouton *Parcourir* pour choisir l'emplacement du fichier)
- nom de la couche = `Mâts éoliens`
- SCR = `EPSG:2154`

### Zone tampon

Dans cet exercice nous créérons une zone tampon de 5000 m. autour de l'autoroute A61. 

D'après les propriétés de la couche `trace-du-reseau-autoroutier-doccitanie`, quel est son système de coordonnées ? 

Convertissez cette couche dans le système Lambert 93, tout en l'exportant dans le fichier GeoPackage  avec clic droit + *Exporter / Sauvegarder les entités sous ...* et les options suivantes :

- format = `GeoPackage`
- nom de fichier = `tp4_exercices.gpkg` (bouton *Parcourir* pour chercher le fichier)
- nom de la couche = `Réseau autoroutier`
- SCR = `EPSG:2154`

Ouvrez la table attributaire de la nouvelle couche `tp4_exercices Réseau autoroutier`. Le champ *num_route* contient le nom des routes : à l'aide d'une expression, sélectionnez les tronçons de l'autoroute A61 

Créez la zone tampon de 5000 m avec l'outil **Tampon**, et les options suivantes :

- Couche source = `Réseau autoroutier`
- Entités sélectionnées uniquement = `OUI`
- Distance = `5000`
- Nb segments = `10`
- Style d'extrémité = `Rond`
- Regrouper le résultat = `OUI`
- Fichier sortie = *Enregistrer vers fichiers Geopackage > tp4_exercices.gpkg > nom couche = Tampon A61 5000m*

![Tampon A61][1]

Pourquoi est-il nécessaire d'exporter la couche _Réseau autoroutier_ en Lambert 93 pour créer la zone tampon ? (voir la réponse dans le cours sur les systèmes de coordonnées)

Combien y a t-il d'éoliennes dans la zone tampon de 5000 m autour de l'A61 ?

### Matrice de distance

Pour chaque mât éolien, calculez la distance
avec les 2 mâts éoliens les plus proches, avec l'outil *Matrice de distance* et les options suivantes :

- couche de points en entrée, et couche de points cible = `tp4_exercices Mâts éoliens`
- identifiant pour la couche en entrée, et pour la couche cible = `id_mat`
- type de matrice en sortie = `Matrice de distance linéaire (N*k+3)`
- Utiliser uniquement les points cibles les plus proches (k) = **`2`**
- Fichier sortie = *Enregistrer vers fichiers Geopackage > tp4_exercices.gpkg > nom couche = Calcul Eoliennes 2 voisins*

Inpectez le résultat : cliquez avec l'outil *Identifier* sur quelques points dans la couche de sortie. Que remarquez-vous ? Quel est le type de géométrie de cette nouvelle couche (cf Propriétés de la couche, et Source) ?

### Grille

Dans cet exercice, vous allez créer un maillage hexagonale de 1km de côté sur l'étendue du département de l'Aude.

Dans la couche `DEPARTEMENT`, sélectionnez le département de l'Aude et zoomez dessus (*clic-droit + Zoomer sur la sélection*).

Avec l'outil *Créer une grille*, utilisez les options suivantes puis lancez la création :

- Type de grille = `hexagonale`
- Etendue de la grille = Utiliser l'emprise du canevas
- Espacement horizontal et vertical = `5000`
- SCR de la grille = EPSG:2154
- Grille en sortie = *Enregistrer vers fichiers Geopackage > tp4_exercices.gpkg > nom couche = Grille Aude 5km*

Ensuite, pour supprimer les polygones en dehors du département de l'Aude, procédez de cette manière : avec l'outil *Sélection par localisation*, sélectionnez les hexagones qui intersecte le département. Inversez la sélection. Passez en mode *Mise à jour*, supprimez les entités sélectionnées, enregistrez les modifications et quittez le mode *Mise à jour*.

![Grille Aude ][2]

### Compter les points dans les polygones

Avec l'outil *Compter les points dans les polygones*, compter le nombre d'éoliennes dans chaque maille hexagonale. Enregistrez le résultat dans une nouvelle couche `Calcul nb éoliennes` du fichier GeoPackage `tp4_exercices.gpkg`.

### Somme des longueurs de lignes

Avec l'outil *Somme des longueurs de lignes*, compter la longueur totale de tronçons autoroutiers dans chaque maille hexagonale. Enregistrez le résultat dans une nouvelle couche `Calcul long autoroutes` du fichier GeoPackage `tp4_exercices.gpkg`.

### Analyse de superposition

Dans cet exercice, vous calculerez, à partir des données Corine Land Cover 2012 et de la grille hexagonale précédemment créée, le % de zones classées en "Forêts et milieux semi-naturels" (code=3xx) dans chaque maille.

Commencez par sélectionner, dans la couche `CLC12_RLRMP_RGF`, les polygones correspondant aux zones dont le code commence par '3' avec *Sélection par expression*. Vous pouvez par exemple utiliser l'expression : *"CODE_12" LIKE '3%'*, ou bien l'expression :  *regexp_match(  "CODE_12" , '^3')* 

Exportez les entités sélectionnées (clic droit + *Exporter / Sauvegarder les entités sélectionnés sous...*) dans une nouvelle couche `CLC12 Forets Milieux SemiNat` du fichier GeoPackage `tp4_exercices.gpkg`. 

Avec l'outil *Analyse de superposition*, calculez % de zones classées en "Forêts et milieux semi-naturels" :

- Couche source = `Grille Aude 5km`
- Couches de superposition = `CLC12 Forets Milieux SemiNat`
- Couche en sortie = `Calcul P ForetsSemiNat`

![Aude CLC12 3xx][3]

### Intersection + tableau croisé dynamique avec Group Stats

Dans cet exercice, nous déterminons la surface occupée dans un rayon de 2 km autour de chaque parc éolien, par chaque type d'occupation du sol.

Dans le gestionnaire d'extensions, recherchez et installez l'extension _Group Stats_ qui nous aidera à agréger les surfaces par nom de parc éolien et par code d'occupation.

Créez une zone tampon de 2000 m autour de chaque mât éolien avec l'outil **Tampon**, et les options suivantes :

- Couche source = `Mâts éoliens`
- Entités sélectionnées uniquement = `NON`
- Distance = `2000`
- Nb segments = `10`
- Style d'extrémité = `Rond`
- Regrouper le résultat = **`NON`**
- Fichier sortie = *Enregistrer vers fichier Geopackage > tp4_exercices.gpkg > nom couche = Tampon Eol 2000m*

Regroupez les zones tampons qui appartiennent au même parc éolien avec l'outil *Regrouper*. Utilisez les options suivantes :

- Couche source = `Tampon Eol 2000m`
- champs de regroupement = *id_parc, n_parc*
- couche sortie regroupée = *Enregistrer vers fichier Geopackage > tp4_exercices.gpkg > nom couche = Calcul ParcEol 2000m*

![Regroup parcs eoliens 2000m][4]

Avec l'outil *Intersection*, calculer l'intersection des couches `Tampon Eol 2000m` et `CLC12_RLRMP_RGF` dans une nouvelle couche `Calcul Inter Eol CLC12`.

- couche source = `CLC12_RLRMP_RGF` 
- couche superposition = `Tampon Eol 2000m`
- champ d'entrée (source) à conserver = *ID, CODE_12*
- champ (superposition) à conserver = *id_parc, n_parc*
- couche sortie intersection = *Enregistrer vers fichier Geopackage > tp4_exercices.gpkg > nom couche = Calcul Inter ParcEol CLC12*

Dans le menu _Vecteur_, ouvrez l'outil _Group Stats_.

- Dans la liste **Couches**, choisir `Calcul Inter ParcEol CLC12`. 
- Dans **Colonnes**, faire glisser `CODE_12`
- Dans **Lignes**, faire glisser `id_parc` et `n_parc`
- Dans **Valeurs**, faire glisser `Surface` et `somme`

Cliquer sur *Calculer*. Copier le tableau avec le menu *Données / Copie dans le presse-papiers*, et coller dans Microsoft Excel ou LibreOffice Calc. 

[1]: img/tp4_tampon.png 
[2]: img/tp4_grille.png 
[3]: img/aude_3xx.png
[4]: img/parc_eol_2000m.png