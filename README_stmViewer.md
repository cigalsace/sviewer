# stmViewer

stmViewer propose une extension au sviewer de geOrchestra développé par GéoBretagne permettant de visualiser une "story map".
Chaque carte de la story map est composé de trois éléments:
- Un titre
- Un commentaire stocké dans un fichier HTML
- Un WMC définissant la carte à afficher

La story map est définie dans le fichier `customConfig.js` de la façon suivante.

```js
stm: [
    {
        title: 'First map',
        wmc_url: 'wmc/map1.wmc',
        html_url: 'wmc/map1.html'
    }, {
        title: 'Second map',
        wmc_url: 'wmc/map2.wmc',
        // wmc_url: '14cc2d9249fba1e87068adc1e9528ffe', // Possibilité de spécifier un WMC à partir de son identifiant geOrchestra
        html_url: 'wmc/map2.html'
    }
]
```

Si la variable stm est définie, un bouton supplémentaire s'affiche dans le sViewer en haut à droite.
Il permet de charger la première carte. Des boutons de navigation sous le commentaire de la carte permet de passer à la carte suivante ou revenir en arrière.

Pour une démonstration: cf. https://www.cigalsace.org/tools/stmViewer/
