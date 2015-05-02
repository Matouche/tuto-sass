Je ne vous ai pas encore tout dit ! Les arguments vous réservent encore quelques surprises.

Tout d'abord, sachez que, lorsque l'on inclue un mixin on peut attribuer des valeurs à nos arguments en utilisant leurs noms. Ces exemples valent mieux qu'un long discours :

```scss
@include joli-hover($normal: blue, $hover: skyblue);
@include joli-hover($hover: skyblue, $normal: blue);
@include joli-hover($hover: skyblue);
```

La première ligne vous montre cette nouvelle syntaxe. Cela ressemble un peu à l'attribution de valeur par défaut. On peut changer l'ordre des arguments, comme dans la deuxième ligne, sans que Sass n'y voit aucun inconvénient. La troisième ligne est la plus intéressante : on donne une valeur à `$hover` (le deuxième argument), sans préciser la valeur du premier argument du mixin (qui prend du coup la valeur par défaut). Cela n'aurait pas été possible avec la syntaxe classique.

Nommer explicitement nos arguments à l'inclusion est plus long à écrire mais rend le code plus lisible. Si vous revenez plus tard sur votre code, vous n'aurez pas besoin de vous rappeler de l'ordre des arguments. Vous saurez précisément à quel argument correspond chaque valeur.

Dans certains cas, nous ne pouvons pas savoir exactement combien d'arguments seront donnés. C'est le cas avec les polices de caractères : il est souvent recommandé de donner une liste de polices pour des raisons de compatibilité. Si notre mixin a pour argument une liste de polices, alors nous devons dire à Sass qu'un nombre variable d'argument(s) est attendu.

Pour y voir plus clair, je vous propose de prendre comme exemple le mixin `gros` :

```scss
@mixin gros{
  font:{
    size: 20px;
    family: "Times New Roman", Times, serif;
  }
}
```

Nous allons le modifier pour pouvoir choisir la/les police(s) en utilisant des arguments. Pour indiquer à Sass que l'on attend une liste d'arguments, on doit ajouter `...` au nom de l'argument qui contiendra la liste, comme ceci :

```scss hl_lines="1 4"
@mixin gros($polices...){
  font:{
    size: 20px;
    family: $polices;
  }
}
```

L'argument `$polices` contient la liste des valeurs données comme arguments au mixin. On s'en sert ensuite comme une simple variable de type "liste".

Voilà, notre mixin fonctionne désormais ! Vous pouvez l'utiliser comme ceci, par exemple :

```scss
@include gros(Arial, Helvetica, sans-serif);
```
Le code généré sera :

```css
font-size: 20px;
font-family: Arial, Helvetica, sans-serif;
```