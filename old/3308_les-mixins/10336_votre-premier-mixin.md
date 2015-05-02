Je vous propose de partir d'un exemple concret : des liens. Regardez le code SCSS suivant :

```scss
a{
  color: orange; 
  &:hover{
    color: orangered;
  }
}
```

On change de couleur au survol, tout simplement.

Venons-en aux *mixins*. Un mixin est un bout de code réutilisable. Dans notre exemple, nous appliquons à nos liens plusieurs styles. Avec Sass, nous pouvons "enfermer" ces styles dans un mixin, puis, lorsqu'on en aura besoin, on aura plus qu'à indiquer le nom du mixin à utiliser. Un mixin fonctionne comme une *macro* : une macro est un ensemble d'instructions à effectuer, un mixin est un ensemble de styles à appliquer à un élément. Dans les deux cas, l'intérêt est la ré-utilisabilité (pas sûr que ce mot existe :-D), on évite de se répéter ("Don't Repeat Yourself !", je vous rappelle...).

Mettons nos styles dans un mixin. Pour cela on utilise la directive `@mixin`, comme ceci :

```scss
@mixin joli-hover{
  color: orange; 
  &:hover{
    color: orangered;
  }
}
```

On écrit d'abord `@mixin`, puis le nom du mixin, et enfin, les styles composant le mixin (entre accolades). Dans notre cas, notre mixin s'appelle `joli-hover`. Félicitations, vous avez créé votre premier mixin !