Passons maintenant à la boucle `@each`, qui est très puissante lorsqu'il s'agit de *parcourir* une liste.
Reprenons l'exemple précédent que j'ai légèrement modifié :
```css
#bloc-frites {
  background-image: url("image-frites.png");
}
#bloc-tomate {
  background-image: url("image-tomate.png");
}
#bloc-yaourt {
  background-image: url("image-yaourt.png");
}
#bloc-raviolis {
  background-image: url("image-raviolis.png");
}
```

Comme vous voyez, j'ai remplacé les nombres (dans les ID et URL) par des chaînes de caractères. Comment automatiser tout ça avec Sass ? Tout d'abord, stockons ces chaînes de caractères dans une liste :
```scss
$aliments: frites, tomate, yaourt, raviolis;
```

Ensuite, on va *parcourir* la liste : à chaque itération on a accès à un seul item de la liste. On pourrait utiliser une boucle `@for` et la fonction `nth()` comme ceci :
```scss
@for $i from 1 through 4{
  $aliment: nth($aliments, $i);
  #bloc-#{$aliment}{
    $image: 'image-' + $aliment + '.png';
    background-image: url($image);
  }
}
```

Cependant, ce code est assez long, et pas forcément très clair. C'est pourquoi Sass propose une boucle dédiée aux listes, la boucle `@each` :
```scss hl_lines="1"
@each $aliment in $aliments{
  #bloc-#{$aliment}{
    $image: 'image-' + $aliment + '.png';
    background-image: url($image);
  }
}
```

La première ligne se lit ainsi : la variable `$aliment` vaudra tour à tour chaque valeur contenue dans la liste `$aliments`. On obtient donc le même code que précédemment.

Vous vous rappelez des listes multidimensionnelles ? Elles sont aussi parcourables avec `@each`, c'est ce qu'on appelle *l'affectation multiple*. À chaque itération, on peut accéder à tous les éléments d'une des « sous-listes », comme ceci : 
```scss hl_lines="1"
@each $titre, $fonte in (h1, 2.5rem), (h2, 2rem), (h3, 1.5rem){
  #{$titre}{
    font-size: $fonte;
  }
}
```

Le code CSS généré est le suivant :
```css
h1 {
  font-size: 2.5rem;
}
h2 {
  font-size: 2rem;
}
h3 {
  font-size: 1.5rem;
}
```

Personnellement, je trouve plus simple et plus lisible d'utiliser la fonction `zip()` :
```scss hl_lines="3"
$titres: h1, h2, h3;
$fontes: 2.5rem, 2rem, 1.5rem;
@each $titre, $fonte in zip($titres, $fontes){
  #{$titre}{
    font-size: $fonte;
  }
}
```
Voilà, c'est tout pour `@each`, même si nous en reparlerons dans le prochain chapitre.