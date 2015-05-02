La *portée des variables* est un problème assez technique, que les programmeurs connaissent bien. Ne partez pas en courant ! Je vais essayer d'être le plus clair possible.

Jusqu'à présent, nous déclarions nos variables au début du code. Cependant, on peut aussi déclarer une variable à l'intérieur d'un bloc, comme ceci :

```scss
header{
  $largeur: 600px;
  width: $largeur; // 600px
}
```

Cependant, attention ! Cette variable n'est utilisable que dans ce bloc. On dit que c'est une variable *locale*. Par opposition, les variables *globales* (que l'on déclare en dehors d'un bloc) sont utilisables dans toute la feuille de style. Si vous tentez de l'utiliser dans un autre bloc, vous aurez alors droit à une jolie erreur :

> Syntax error: Undefined variable: "$largeur".
Source: Sass énervé

Cela nous autorise à créer plusieurs variables différentes avec le même nom, selon le bloc on l'on se situe :

```scss
header{
  $largeur: 600px;
  width: $largeur; // 600px
}
article{
  $largeur: 400px;
  width: $largeur; // 400px
}
```

Dans ce code, il y a une variable `$largeur` dans le bloc `header` et une autre variable `$largeur` dans le bloc `article`.

Pour finir, sachez que les variables globales peuvent être modifiées à l'intérieur d'un bloc. Regardez le code suivant :

```scss
$fonte: 12px;
p{
  $fonte: 16px;
  font-size: $fonte; // 16px
}
img{
  font-size: $fonte; // 16px
}
```

Ici, on a modifié la variable `$fonte` à l'intérieur du bloc `p`. Par la suite, c'est la nouvelle valeur qui est utilisée, même dans un autre bloc (ici `img`).

Voilà, vous savez tout sur les différentes portées (globale et locale) des variables.