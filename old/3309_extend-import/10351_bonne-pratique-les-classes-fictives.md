*« Classe fictive »* est la meilleure traduction que j’ai trouvé pour
l’anglais « placeholder selector ». Partons d’un exemple : un blog
avec des posts de type texte ou photo. Il y aura deux classes pour les
posts, `.texte` et `.photo`, qui auront des propriétés en communs, que
nous mettront dans une classe `.post` dont elles hériteront. Voici le
code :

```scss
.post{
  width: 40rem;
}
.texte{
  @extend .post;
}
.photo{
  @extend .post;
}
```

Ce code ne pose pas de problème, sauf que l’on va créer une classe
`.post` qui ne servira qu’à Sass (pas utilisée dans le HTML).

C’est là que les « classes fictives » entrent en jeu. Elles
fonctionnent comme une classe ou un ID, mais commencent par `%` (au lieu
de `#` ou `.`) et ne sont utilisables que pour l’héritage (elles
n’apparaissent pas dans le fichier `.css`). Transformons
`.post` en `%post` :

```scss hl_lines="1 5 8"
%post{
  width: 40rem;
}
.texte{
  @extend %post;
}
.photo{
  @extend %post;
}
```

Comme ceci, le code CSS sera alors :

```css hl_lines="1"
.texte, .photo{
  width: 40rem;
}
```

Plus de trace de notre classe fictive !

On aurait pu aussi créer un *mixin* nommé `post`. Mais dans ce cas là,
le code du mixin aurait été répété dans le bloc `.texte` ET dans le bloc
`.photo`, alors que, en utilisant une « classe fictive », le code
n’apparait qu’une seule fois. Dans ce cas là, l’utilisation d’un mixin
alourdit le code CSS, ce qui pose des problèmes de performances et de
lisibilité (si vous voulez travaillez avec quelqu’un n’utilisant pas
Sass, par exemple). Bref, si vous n’avez pas besoin d’utiliser des
arguments ou `@content`, préférez une « classe fictive » à un mixin.