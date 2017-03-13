# paramsViewer

ParamsViewer propose une extension au sviewer de geOrchestra d�velopp� par G�oBretagne permettant de renseigner des valeurs de param�tres pour filtrer l'affichage des donn�es selon des champs de la table attributaire.

URL d'exemple: 
- https://www.cigalsace.org/tools/paramsViewer/0.03/index.html?layers=GRANDEST:commune_actuelle_params_pop[pop_min:2000;pop_max:15000]


## Objectif du paramsViewer

Permettre de pr�ciser la valeur d'un ou plusieurs param�tres dans le sviewer pour interagir avec une couche de donn�es (ex.: filtrer les objets � afficher).


## Contraintes et choix d'une solution

Les pr�requis pour le choix d'une solution ont �t� les suivants:
- Doit fonctionner m�me si le param�tre � faire varier d�pend d'une table attributaire li�e et n'est pas directement stock� dans la table contenant la g�om�trie.
- Doit pouvoir �tre param�trable via l'URL pour faciliter la r�utilisation du sviewer.
- Doit permettre une mise � jour dynamique du layer � partir de l'interface utilisateur.
- Doit offrir une interface g�n�r�e � la vol�e, facile d'acc�s et d'utlisation.


## Analyse rapide des possibilit�s d'impl�mentations envisag�es:

### 1. CQL_FILER:

Mise en oeuvre:
- Cr�er une vue dans la base de donn�es PostGresSQL/PostGIS
- Parser le CQL_FILTER pr�cis� dans l'URL pour g�n�rer interface avec liste des param�tres
- Faire varier le param�tre CQL_FILTER � la vol�e dans le sviewer et recherger les layers correspondants

Avantages: 
- le sviewer supporte d�j� le CQL_FILER dans l'URL
- possibilit� de d�finir plusieurs requ�tes diff�rentes � partir d'un m�me layer Geoserver.

Principale difficult�: 
- n�cessit� de parser la requ�te CQL_FILER pour identifier les param�tres et valeurs pass�e, puis modifier les valeurs � la vol�e => pas de solution simple trouv�e sur ce dernier point � ce stade.

### 2. SQL VIEW:

Mise en oeuvre:
- Cr�er une vue SQL param�trable dans G�oserver
- Indiquer les param�tres dans l'URL et leur valeur par defaut
- G�n�rer la liste des param�tre dans l'interface utilisateur
- Faire varier � la vol�e le param�tre VIEWPARAMS et recharger les layers correspondants

Avantages:
- Simplicit� de mise en oeuvre (au regard notamment de la solution CQL_FILER), via l'utilisation notamment de l'interface Geoserver pour d�finir les vues

Difficult�s:
- N�cessite la mise en place d'une vue SQL param�tr�e adapt�e (ne s'applique pas � des donn�es de type shapefile notamment)


## Solution retenue: 

Utilisation de couches (layers) de type "SQL View" et de requ�tes param�tr�es ("VIEWPARAMS") de Geoserver (cf. http://docs.geoserver.org/stable/en/user/data/database/sqlview.html [EN] et http://geoserver.geo-solutions.it/edu/fr/adding_data/add_sqllayers.html [FR]).


## Utilisation:

Pr�ciser le nom du param�tre et au besoin une valeur par d�faut entre crochets apr�s le nom de la couche dans l'URL.
Exemple: `layers=MonNameSpace:MonLayerName[param:defaultValue]`

La valeur par defaut peut �tre omise pour ne pas appliquer de valeur au param�tre. Dans ce cas, c'est la valeur par defaut d�finie dans Geoserver qui est utilis�e.
Exemple: `layers=MonNameSpace:MonLayerName[param:]`

Plusieurs param�tres peuvent �tre pr�cis�s, sur diff�rentes couches.
Exemple: `layers=MonNameSpace:MonLayerName[param1:defaultValue1;param2:],MonNameSpace:MonLayerName2[param3:]`

L'application g�n�re automatiquement, dans la bo�te de dialogue de recherche (icone "loupe"), un champ de saisie pour chaque param�tre.
Il suffit alors de saisir la valeur souhait�e pour un param�tre et de valider.

**A noter:** si une couche de fonds n�est pas op�rationnelle, cela bloque le bon fonctionnement de la mise � jour des param�tres.