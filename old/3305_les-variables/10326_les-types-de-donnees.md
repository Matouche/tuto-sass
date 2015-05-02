Les variables contiennent donc des données. Sass nous permet d'utiliser *6 types de données différents*. Voyons plus en détail à quoi ils correspondent.

On ouvre la danse avec les *nombres*. Les nombres peuvent ne pas avoir d'unité (par exemple `255`) ou en avoir une (par exemple `1em` ou `16px`). On peut s'en servir pour contenir la fonte des caractères, la largeur ou la hauteur d'un bloc, la dimensions des marges, etc. Bref, c'est simple à utiliser :

```scss hl_lines="1 3"
$largeur: 800px;
body{
  width: $largeur;
}
```

Viennent ensuite les *chaînes de caractères* (le texte). Les chaînes de caractères peuvent être encadrée par des guillemets (par exemple `"images/logo.png"` ou `'font.ttf'`) ou non (par exemple `bold`). Là encore, ce n'est pas bien compliqué :

```scss hl_lines = "1 3"
$alignement: justify;
body{
  text-align: $alignement;
}
```

Revenons rapidement sur le premier type que nous avons utilisé : les *couleurs*. Les couleurs peuvent être nommées (par exemple `yellow`), ou écrite sous une forme hexadécimale (rappelez-vous `#ffff00`), mais aussi sous la forme RGB ou RGBA (par exemple `rgb(255,255,0)` ou `rgba(255,255,0,0.5)`). Les variables sont très utiles pour les couleurs :

```scss hl_lines = "1 3"
$couleur-texte: #777777;
body{
  color: $couleur-texte;
}
```

Continuons avec les *listes*. Les listes sont des ensembles de données, séparées par des espaces (par exemple `10px 10px 0 0`) ou des virgules (par exemple `"Courier New",  Arial, sans-serif`). Elles sont donc utiles pour certaines propriétés (`margin`, `padding`, `font-family`, ...) comme dans ce cas :

```scss hl_lines="1 4"
$arrondis: 1em 0 1em 0;
body{
  background-color: orange;
  border-radius: $arrondis;
}
```

Les deux derniers types sont le type *null* et le type *booléen*. On définit une variable de type null ainsi : `$variable: null;` Comme son nom l'indique, une variable de type null ne contient rien. Les booléens quant à eux peuvent avoir deux valeurs : `true` (Vrai) et `false` (Faux). Ces deux types de données vous seront utiles lorsque vous utiliserez des conditions. Mais actuellement, il est normal que vous n'en voyez pas l'intérêt.