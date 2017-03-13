# paramsViewer

ParamsViewer propose une extension au sviewer de geOrchestra développé par GéoBretagne permettant de renseigner des valeurs de paramètres pour filtrer l'affichage des données selon des champs de la table attributaire.

URL d'exemple: 
- https://www.cigalsace.org/tools/paramsViewer/0.03/index.html?layers=GRANDEST:commune_actuelle_params_pop[pop_min:2000;pop_max:15000]


## Objectif du paramsViewer

Permettre de préciser la valeur d'un ou plusieurs paramètres dans le sviewer pour interagir avec une couche de données (ex.: filtrer les objets à afficher).


## Contraintes et choix d'une solution

Les prérequis pour le choix d'une solution ont été les suivants:
- Doit fonctionner même si le paramètre à faire varier dépend d'une table attributaire liée et n'est pas directement stocké dans la table contenant la géométrie.
- Doit pouvoir être paramétrable via l'URL pour faciliter la réutilisation du sviewer.
- Doit permettre une mise à jour dynamique du layer à partir de l'interface utilisateur.
- Doit offrir une interface générée à la volée, facile d'accès et d'utlisation.


## Analyse rapide des possibilités d'implémentations envisagées:

### 1. CQL_FILER:

Mise en oeuvre:
- Créer une vue dans la base de données PostGresSQL/PostGIS
- Parser le CQL_FILTER précisé dans l'URL pour générer interface avec liste des paramètres
- Faire varier le paramètre CQL_FILTER à la volée dans le sviewer et recherger les layers correspondants

Avantages: 
- le sviewer supporte déjà le CQL_FILER dans l'URL
- possibilité de définir plusieurs requêtes différentes à partir d'un même layer Geoserver.

Principale difficulté: 
- nécessité de parser la requête CQL_FILER pour identifier les paramètres et valeurs passée, puis modifier les valeurs à la volée => pas de solution simple trouvée sur ce dernier point à ce stade.

### 2. SQL VIEW:

Mise en oeuvre:
- Créer une vue SQL paramétrable dans Géoserver
- Indiquer les paramètres dans l'URL et leur valeur par defaut
- Générer la liste des paramètre dans l'interface utilisateur
- Faire varier à la volée le paramètre VIEWPARAMS et recharger les layers correspondants

Avantages:
- Simplicité de mise en oeuvre (au regard notamment de la solution CQL_FILER), via l'utilisation notamment de l'interface Geoserver pour définir les vues

Difficultés:
- Nécessite la mise en place d'une vue SQL paramétrée adaptée (ne s'applique pas à des données de type shapefile notamment)


## Solution retenue: 

Utilisation de couches (layers) de type "SQL View" et de requêtes paramétrées ("VIEWPARAMS") de Geoserver (cf. http://docs.geoserver.org/stable/en/user/data/database/sqlview.html [EN] et http://geoserver.geo-solutions.it/edu/fr/adding_data/add_sqllayers.html [FR]).


## Utilisation:

Préciser le nom du paramètre et au besoin une valeur par défaut entre crochets après le nom de la couche dans l'URL.
Exemple: `layers=MonNameSpace:MonLayerName[param:defaultValue]`

La valeur par defaut peut être omise pour ne pas appliquer de valeur au paramètre. Dans ce cas, c'est la valeur par defaut définie dans Geoserver qui est utilisée.
Exemple: `layers=MonNameSpace:MonLayerName[param:]`

Plusieurs paramètres peuvent être précisés, sur différentes couches.
Exemple: `layers=MonNameSpace:MonLayerName[param1:defaultValue1;param2:],MonNameSpace:MonLayerName2[param3:]`

L'application génère automatiquement, dans la boîte de dialogue de recherche (icone "loupe"), un champ de saisie pour chaque paramètre.
Il suffit alors de saisir la valeur souhaitée pour un paramètre et de valider.

**A noter:** si une couche de fonds n’est pas opérationnelle, cela bloque le bon fonctionnement de la mise à jour des paramètres. 
