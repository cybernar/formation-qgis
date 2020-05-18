# TP partie 4A. Données vectorielles : requêtes et jointure, édition.

## TP Requêtes, jointures

Dans un nouveau projet, ouvrez les couches suivantes incluses dans le répertoire `data\IGN_ETALAB\ADMIN-EXPRESS-COG` :

- `COMMUNE.shp`
- `DEPARTEMENT.shp`

Ouvrez les couches suivantes incluses dans le répertoire `data\IGN_ETALAB\BD_Carthage` :

- `CoursEau_76_Occitanie.shp`
- `PlanEau_76_Occitanie.shp`

!!!note
	Tous ces shapefiles sont codés en UTF-8. Veillez à ouvrir les données avec l'encodage UTF-8 pour que les toponymes s'affichent correctement.

Enregistrez le projet sous le nom `tp4.qgz` dans `data`.

Créez un nouveau répertoire `TP4` dans `data`. Les fichiers générés dans les exercices suivants seront enregistrés dans ce nouveau répertoire.

### Sélection par expression 

Ouvrez la table d'attributs de la couche `DEPARTEMENT`.

Sélectionnez les départements dont le code "INSEE_REG" est '76' (code de la région Occitanie). Enregistrez les entités sélectionnées dans une nouvelle couche : `DEPARTEMENT_Occitanie.shp` avec *Clic-droit + Exporter / Sauvegarder les entités sélectionnées sous ...*

### Filtre d'entités

Ouvrez les propriétés de la couche `CoursEau_76_Occitanie`. Dans l'onglet _Champ_ examinez les propriétés des champs "Classe" et "NomEntiteH" : quel est leur type ? quelle est leur longueur max ? Dans l'onglet _Source_, cliquez sur le bouton _Constructeur de requêtes_ en bas à droite pour filtrer les données : appliquez une expression pour **n'afficher que les cours d'eau de classe = '1' et '2'**. Testez l'expression pour voir le nombre de cours d'eau concernés et validez.

### Requêtes spatiales

Dans la couche `CoursEau_76_Occitanie`, sélectionnez (manuellement, ou à l'aide d'une expression) **le fleuve Aude**. Puis, à l'aide de l'outil **Sélection par localisation**, sélectionnez les communes traversées par ce cours d'eau. Combien y a t-il de communes concernées ?

![Jointure Spatiale Aude][1]

Exportez la liste des communes intersectées dans un nouveau fichier : *Clic-droit + Exporter / Sauvegarder les entités sélectionnées sous ...*. Utilisez le format `ODS` de manière à pouvoir l’ouvrir avec un tableur. Bien spécifier *Type de géométrie = Aucune géométrie* dans les options de sortie.

### Jointure spatiale

Dans cet exercice, vous allez déterminer pour chaque plan d'eau, dans quel département il est situé. Pour simplifier, on ne prendra pas en compte les plans d'eau à cheval sur plusieurs départements, mais uniquement ceux inclus dans 1 seul département.

Ouvrez l'outil *Jointure par localisation*. A la couche source `PlanEau_76_Occitanie`, joignez la couche `DEPARTEMENT_Occitanie` avec les options suivantes :
- prédicat = *à l'intérieur*
- champs à ajouter = "INSEE_DEP" et "NOM_DEP_M"
- type de jointure = *uniquement les attributs de la 1ère entité localisée*
- ne pas supprimer le enregistrements non-joints. 

Le chemin et le nom du fichier de sortie doivent être spécifiés, par exemple : `TP4\plando_dept_joinspat.shp`.

Inspectez le résultat : la couche en sortie a-t-elle le même nombre d'entités que la couche `PlanEau_76_Occitanie` en entrée ? Combien de plans d'eau ne sont pas à l'intérieur d'1 seul département ?

Enregistrez le projet.

## TP Jointure attributaire, calculatrice de champs

### Jointures attributaires

Ouvrez dans un tableur (LibreOffice Calc ou Microsoft Excel) la base *Comparateur de communes* de l'INSEE : `base-cc-comparateur.xls` (répertoire: `INSEE`). Le but de cet exercice est d'importer ce fichier dans QGIS et de le joindre à la couche `COMMUNE`.

Pour que les données des communes soient exploitables dans QGIS, il faut que la 1ère ligne du tableau contiennent les noms des colonnes ("CODGEO", "LIBGEO", "REG", "DEP", etc.). Dans votre tableur, supprimer les 5 premières lignes de la feuille COM. Enregistrez la feuille modifiée dans un nouveau fichier `com-comparateur.ods` (format `.ods`, ou `.xlsx`, ou `.csv`, ou `.txt`),  et **fermer le tableur**.

Dans QGIS, ouvrir la feuille `COM` du classeur `com-comparateur.ods`.

Vous allez joindre les données INSEE à la couche 
`COMMUNE`. Les champs qui nous intéressent pour la suite de l'exercice sont "P16_LOG" et "P16_RSECOCC" (nombre total de logement, et nombre de résidences secondaires ou occasionnelles en 2016 par commune). Ouvrez les propriétés de la couche `COMMUNE`, ajoutez une jointure avec la table `COM` basée sur les champs "CODGEO" et "INSEE_COM". Dans les champs joints, choisissez : "P16_LOG" et "P16_RSECOCC" et comme préfixe : *'j'*. Ouvrez la table attributaire de la couche `COMMUNE` et vérifiez que les 2 champs joints se trouvent bien à droite.

Exportez la couche `COMMUNE` avec la jointure sous le nom : `commune_logemt.shp` (*clic droit + Exporter / Sauvegarder les entités sous...*) 

Enregistrez le projet.

### Calculatrice de champs

Ouvrez la table attributaire de la couche `commune_logemt.shp` précédemment créée. Passez en mode mise à jour, ouvrez la calculatrice de champ et calculer dans une nouvelle colonne *"p_rsec"* le pourcentage de résidences secondaires et logements
occasionnels en 2016 (utiliser les
champs "P16_RSECOCC" et "P16_LOG" pour le calcul). Enregistrez et quittez le mode mise à jour.

Ensuite, réalisez une cartographie du pourcentage de résidences secondaires et logements occasionnels dans les communes de l'Occitanie : classer les
pourcentages avec *Jolies ruptures* (5 classes dans l'exemple ci-dessous) et utiliser le dégradé de couleurs de votre choix.

Enregistrez le projet.

![Occitanie % Résidences Secondaires][2]

## TP Création de couches, outils de numérisation

Dans cette exercice, vous allez vectoriser les formations végétales d'après une carte de 1972 scannée et géoréférencée.

Dans un nouveau projet, ouvrez la couche `Mission_Cevennes_Jonte2.tif` incluse dans le répertoire `data\Autres_donnees`. 

Quel est le système de coordonnées de ce raster ?

### Création de shapefiles

Créer un nouveau shapefile `Form_Vegetales.shp` de type *Polygone*, dans le SCR *EPSG:2154*, et avec les attributs suivants : `code` (texte, longueur=2), `libelle` (texte, longueur=40).

### Outils de numérisation

Dans la couche `Form_Vegetales.shp`, numériser une vingtaine de polygones d'après la carte scannée `Mission_Cevennes_Jonte2.tif`. Dans la barre d'outils *Accrochage*, utilisez **Activer l’accrochage** et **Activer le tracé** pour numériser des polygones contigus tout en joignant parfaitement leurs bords. A chaque polygone numérisé, saisissez le code (vous pouvez aussi définir une symbologie de type *Catégorisé* comme ci-dessous, avec une transparence de 60 %, pour visualiser en couleur le code de chaque polygone). 

Enregistrer les polygones puis mesurer leur surface en m2 dans un nouveau champ `surf_ha` de type décimal, avec la fonction *$area*.

![Outils numérisation][3]

[1]: img/joinspat.png 
[2]: img/occitanie_pressecocc.png 
[3]: img/numerisation.png