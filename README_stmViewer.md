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
        content_url: 'wmc/map1.html'
    }, {
        title: 'Second map',
        wmc_url: 'wmc/map2.wmc',
        content_url: 'wmc/map2.html'
        // content_url: '14cc2d9249fba1e87068adc1e9528ffe'
    }
]
```

Si la variable stm est définie, un bouton supplémentaire s'affiche dans le sViewer.
Il permet de charger la première carte. Des boutons de navigation sous le commentaire de la carte permet de passer à la carte suivante ou revenir en arrière.
