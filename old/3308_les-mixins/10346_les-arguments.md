Notre mixin `joli-hover` fonctionne plutôt bien, mais est difficilement paramétrable. En effet, si vous avez besoin d'un élément de couleur orange qui passe à l'orange foncé au survol, tout va bien. Mais si vous voulez pouvoir choisir des couleurs différentes à chaque utilisation du mixin, il va falloir modifier `joli-hover` pour qu'il accepte des *arguments*.

Un argument, c'est une variable à l'intérieur d'un mixin. Première étape : pour ajouter des arguments, on écrit leurs noms à la suite du nom du mixin, entre parenthèses :

```scss
@mixin joli-hover($normal, $hover)
```

Ici, notre mixin a deux arguments : `$normal` et `$hover`. Ils contiendront respectivement la couleur normale et celle au survol.

La deuxième étape consiste à utiliser nos arguments dans le mixin. On s'en sert comme des variables, on écrit leur nom là où l'on souhaite utiliser leur valeur :

```scss hl_lines="2 4"
@mixin joli-hover($normal, $hover){
  color: $normal;
  &:hover{
    color: $hover;
  }
}
```

Il ne nous reste plus qu'à modifier notre code lors de l'utilisation du mixin `joli-hover` pour indiquer les valeurs des arguments. Pour cela, on écrit les valeurs entre parenthèses, après le nom du mixin :

```scss hl_lines="2"
a{
  @include joli-hover(skyblue, blue);
}
```

Ici, l'argument `$normal` vaut `skyblue` et l'argument `$hover` vaut `blue`. Le CSS généré sera donc le suivant :
```css
a{
  color: skyblue;
} 
a:hover{
  color: blue;
}
```

Améliorons notre mixin en donnant des *valeurs par défaut* aux arguments. Ainsi, lorsqu'on inclura le mixin, on pourra (ou non) attribuer une valeur aux attributs. Si l'on ne le fait pas, c'est la valeur par défaut qui sera utilisée par Sass. Voici comme on attribue des valeurs par défaut aux arguments d'un mixin :

```scss
@mixin joli-hover($normal: orange, $hover: orangered)
```

On a donné par défaut à `$normal` la valeur `orange` et à `$hover` la valeur `orangered`. Maintenant, on peut (ou non) préciser les valeurs des arguments. Voyez plutôt :

```scss hl_lines="2"
a{
  @include joli-hover(skyblue, blue);
}
h1{
  @include joli-hover; //Pas d'arguments.
  //Sass utilise les valeurs par défaut. 
}
```

Et voilà ! Dans le cas des liens, Sass utilise les valeurs données lors de l'inclusion. Par contre, dans le cas des titres 1, ce sont les valeurs par défaut qui sont employées. Notez que si nous avions seulement précisé une valeur, elle aurait été attribuée à `$normal` (le premier argument) et `hover` aurait reçu la valeur par défaut.