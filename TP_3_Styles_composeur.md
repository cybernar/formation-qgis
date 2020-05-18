# TP partie 3. Exploration et présentation des données, création de cartes avec QGIS

## TP Symbologie + mise en page avec NaturalEarth

Ouvrez ces couches incluses dans le fichier GeoPackage `data/NaturalEarth/NaturalEarth2020.gpkg` :

- `ne_50m_admin_0_countries` (Pays du monde)
- `ne_50m_populated_places_simple` (Villes)
- `ne_50m_geographic_lines` (Equateur + tropiques + cercles polaires)

### 1.1. Rendu de type "Catégorisé"

Ouvrez la table attributaire de la couche `ne_50m_admin_0_countries` (Pays) et jetez un coup d'oeil aux valeurs dans la colonne "INCOME_GRP". Les pays sont déjà classés selon 5 catégories. Créez une carte du monde selon le revenu moyen :

- Dans les propriétés de la couche, définir une symbologie de type **Catégorisé**, basé sur la colonne "INCOME_GRP". Classer les valeurs, supprimer la classe _Toutes les autres valeurs_,  choisir la palette _Viridis_. 
- Astuce pour un plus joli rendu : pour toutes les classes, définir des bordures blanches. Ne pas modifier chaque classe une par une. Modifier l'ensemble des classes en cliquant sur le triangle à droite de _Symbole_, et _Configurer le symbole_. Puis, dans le définition du symbole, cliquer sur _Remplissage simple_ et modifier la couleur de la bordure. Astuce 2 : configurer le projet avec la projection **World Robinson (EPSG:54030)**.
- Toujours dans les propriétés de la couche, mais dans _Source_, changer le nom de la couche : **Countries by Income (GDP)**
- Enregistrer le projet sous le nom `World_2020.qgz`

### 1.2. Mise en page 

Créez une nouvelle mise en page nommée *gpd*, avec la carte précédente (pays selon le revenu moyen). 

- Sur une page A4 portrait, ajouter la carte. Régler la taille de la carte de manière à ne pas montrer l'Antarctique, et laisser de la place à gauche. 
- Ajouter la légende à gauche. Cocher la case *Ne montrer que les entités à l'intérieur de la carte liée* dans les propriétés de la légende pour n'afficher que les couches visibles (celle du revenu par pays). Toujours dans les propriétés de la légende, choisir une police de caractères (pour *Etiquettes élément* et pour *Titre de sous-groupe*)
- Astuce pour un plus joli rendu : dans les propriétés de l'élément carte, cocher *Arrière-plan* et choisir une couleur gris clair pour l'arrière-plan.
- Exportez la carte au format image `.png` (résolution 300 dpi) 
- Enregistrer le projet

![NE Countries Income][1]

### 2.1. Rendu de type "Gradué"

Ouvrez la table attributaire de la couche `ne_50m_populated_places_simple`. La colonne *pop_max* contient la population actuelle des 1249 lieux et villes. Créez une carte des principales villes selon la population :

- Ouvrir une seconde fois la couche `ne_50m_admin_0_countries` (pays) et appliquer pour cette couche un rendu de type *Symbole unique* avec le symbole nommé 'Land'.
- Dans les propriétés de la couche, définir une symbologie de type **Gradué**, basé sur la colonne *pop_max*. Classifier les entités en 6 classes, avec les bornes suivantes : **100000 - 1000000 - 2500000 - 5000000 - 10000000 - 20000000 - Inf** (vous pouvez aussi essayer les ruptures de Jenks). Pour l'ensemble des classes, choisir un symbole ponctuel rose avec une transparence de 60 %. Le paramètre _Méthode_ doit être réglé sur 'Taille', avec une taille de symbole de 0,5 à 4 mm.
- Modifier les étiquettes pour la légende : "0.1 to 1 M", "1 M to 2.5 M", etc. (voir carte ci-dessous)


### 2.2. Mise en page 

Créez une nouvelle mise en page nommée *pop_places*, avec la carte précédente (pays selon le revenu moyen). 

!!!tip "Astuce"
	Vous pouvez aussi **dupliquer la mise en page précédente** et la renommer, ce qui vous permettra de gagner du temps.

- Sur une page A4 portrait, ajouter la carte.
- Ajouter la légende à gauche. Dans les propriétés de l'objet légende,  désactiver *Mise à jour auto* pour retirer manuellement la couche des pays, qui n'a pas à apparaître ici.
- Exportez la carte au format image `.png` (résolution 300 dpi) 
- Enregistrer le projet

![NE Places pop][2]

### 3. Symboles proportionnels à la taille de la population

Créez une carte des principales villes selon la population, mais au lieu de faire 6 classes avec 6 tailles de symbole comme précédemment, ce sera des cercles vraiment proportionnels à la population.

- Définir une symbologie de type Symbole Unique, avec un point rose, et une transparence de 60%. Faire varier la taille du symbole selon le champ *pop_max* : ouvrir l'assistant, appliquer pour les valeurs de 50000 à 40M, une taille de symbole de 0,5 à 5 mm (plutôt que le diamètre, choisir la surface proportionnelle à la valeur de *pop_max*).
- Enregistrer le projet

## TP Symbologie + mise en page Occitanie

Dans un nouveau projet, ouvrez les couches suivantes incluses dans le répertoire `data\IGN_ETALAB\ADMIN-EXPRESS-COG` :

- `CHEF-LIEU.shp`
- `COMMUNE_CARTO.shp`
- `REGION_CARTO.shp`

Ouvrez les couches suivantes incluses dans le répertoire `data\SNCF` :

- `liste-des-gares.csv` (couche de type *Texte délimité*)
- `formes-des-lignes-du-rfn.shp`

### 1. Symbologies de type "Symbole unique"

Représentez les régions (couche `REGION_CARTO`) en gris clair, avec des bordures blanches

### 2. Style de type "Catégorisé"

#### Chefs-lieux

Classez les chefs-lieux avec des symboles ponctuels, selon leur statut administratif. Sur la couche `CHEF-LIEU` définissez une symbologie ponctuelle basée sur l'attribut *STATUT* avec les caractéristiques suivantes :

- Symbole *carré*, bleu, taille 4mm, pour STATUT='Préfecture de région'
- Symbole *carré*, marron, taille 2mm, pour STATUT='Préfecture'
- ne pas représenter les autres catégories

Renommer la couche : *Chefs-lieux*

!!!note "Remarque"
	Vous pouvez créer un effet graphique en ajoutant à chaque symbole ponctuel un **effet d'ombrage**. Cependant, cette option consomme des ressources graphiques et **ralentit l'affichage**.

#### Réseau ferré

Classez les lignes de la couche `formes-des-lignes-du-rfn` selon les catégories dans la colonne *libellé*. 

Affichez uniquement les lignes exploitées, avec un symbole linéaire gris foncé, taille 0.6 mm.

Renommez la couche : *Réseau ferré*

### 3. Style de type "Gradué"

Vous allez affecter une couleur aux **communes** suivant leur **densité de population**. 

Sur la couche `COMMUNE_CARTO`, créez une symbologie basée sur l'expression suivante : *POPULATION / ($area / 1000000)*

Définissez 4 classes par la méthode des ruptures de Jenks, avec un dégradé de l'orange au rouge (_OrRd_).

Puis, modifiez manuellement les 4 classes avec les bornes suivantes :

- de 0 à 20 habitants
- de 20 à 50 habitants
- de 50 à 100 habitants
- \+ de 100 habitants

Renommez la couche : *Densité en hab./km2*

### 4. Etiquettes basées sur des règles

Sur la couche `CHEF-LIEU` définissez 2 classes d'étiquettes basées sur les règles suivantes :

- pour STATUT='Préfecture de région', définir des étiquettes avec la police Bahnschrift, en lettres capitales, avec un arrière-plan blanc. 
- si STATUT='Préfecture', définir des étiquettes avec la police Bahnschrift, en lettres capitales, avec un tampon blanc.

Placer les étiquettes avec l'option Position *Autour du point* et une distance de 1 à 2 mm. 

**Enregistrez le projet** sous le nom `Occitanie_SNCF_2020.qgz`

### 5. Mise en page

Créez une nouvelle mise en page sur une page A4 portrait avec :

- en haut de la page, un titre : *Réseau ferré en Occitanie*
- à droite, une carte centrée sur l'Occitanie à l'échelle 1/2 000 000
- à gauche, une légende avec les couches précédemment définies
- en bas de la carte, une barre d'échelle avec 2 divisions de 50 km.
- en bas de la carte, une flèche du nord
- une mention des sources, sous la carte, qui doit indiquer l'organisme ou le site, et la date de téléchargement.

**Enregistrez le projet**

Enfin, vous allez ajouter à gauche, sous la légende, une carte miniature de la France avec l'emprise de l'Occitanie.

Procédez dans cet ordre :

- dans les propriétés de la carte principale (*Carte1*), cocher *Verrouillez les couches*
- revenir dans la vue principale de QGIS : désafficher toutes les couches, sauf `REGION_CARTO`
- dans la mise en page, ajouter un nouvel objet carte : *Carte2*, avec la métropole.
- dans les propriétés de la carte miniature, options **Aperçu**, ajouter un aperçu avec **Cadre de carte** = *Carte1*

Exporter la carte au format image `.png`, puis au format PDF.

**Enregistrez le projet**

Voici une "suggestion de présentation" réalisée avec QGIS 3.10.

![Occitanie SNCF][4]

## TP Styles sur données raster

### 1. Symbologie Raster

Le raster IGN **BD Alti 500** montre les altitudes de la métropole à une résolution de 500 m.

Dans un nouveau projet, ouvrez le raster `MNT500_L93_FRANCE.ASC` dans `BDALTI`. Créez et appliquez un style de type "valeur
discrétisée" (Type de rendu = _Pseudo-couleur_ / Interpolation = _Discret_) avec 6 classes d’altitude :

- bleu pour 0m
- vert foncé pour 1 à 200m
- vert clair pour 201 à 500 m
- jaune pour 500 à 1000 m
- marron pour 1000 à 2000 m
- gris clair pour + de 2000 m

Enregistrez ce style comme "style par défaut" (fenêtre _Propriétés_, bouton _Styles_) dans un fichier `MNT500_L93_FRANCE.qml`

!!!tip "Bon à savoir"
	Pour les couleurs applicables à une carte topographique, vous pouvez trouver de l'inspiration de cette page : <https://en.wikipedia.org/wiki/Wikipedia:WikiProject_Maps/Conventions/Topographic_maps>. 

	Dans le répertoire `Autres_donnees`, vous trouverez également 2 fichiers de palettes `topo_couleurs_8.gpl` et `topo_couleurs_30.gpl` (respectivement 8 et 30 couleurs) inspirés de la page précédente, à importer dans QGIS via le _Gestionnaire de symboles_

### 2. Mise en page

Créez une mise en page avec la carte à gauche, et une légende *Altitude* à droite.

Dans les propriétés de la carte, options **Grille**, ajoutez une grille avec les coordonnées en degrés. Donnez les caractéristiques suivantes à la grille : *Type = Continue*, *SCR = EPSG:4326* (pour une grille en WGS84 sur cette carte en Lambert 93), *intervalle = 5°*, *Afficher les coordonnées*, *Format = décimal avec suffixe*.

![BD ALTI 500][5]

[1]: img/world_income_gdp.png 
[2]: img/world_pop_place.png 
[3]: img/world_pop_place2.png
[4]: img/occitanie_sncf2.png
[5]: img/france_alti.png