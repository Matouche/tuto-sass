Une fois n'est pas coutume, commençons par examiner un exemple. Regardez ce code CSS :
```css
#bloc1 {
  background-image: url("image1.png");
}
#bloc2 {
  background-image: url("image2.png");
}
#bloc3 {
  background-image: url("image3.png");
}
#bloc4 {
  background-image: url("image4.png");
}
```

Comme vous le voyez, on a répété quatre fois le même code, avec un léger changement à chaque fois. Ici, on a juste modifié un nombre (plus précisément on a ajouté 1 à chaque fois).

Observez maintenant ce code SCSS :
```scss hl_lines="1"
@for $i from 1 through 4{
  #bloc#{$i}{
    $image: 'image' + $i + '.png';
    background-image: url($image);
  }
}
```
Ce code a le même effet que le premier. On utilise la boucle `@for` qui va répéter le bloc de code, en changeant la valeur de `$i` à chaque itération (ou répétition de la boucle). Cette variable (appelée indice de la boucle) vaudra tour à tour 1, 2, 3 et 4 (tous les entiers entre 1 et 4, `from 1 through 4`). Remarquez que ici, l'interpolation et la concaténation sont très utiles.

Deux petites choses à savoir aussi sur la boucle `@for` (mais dont l'utilité est limitée) :

- on peut compter à l'envers : `@for $i from 4 through 1` (`$i` vaudra 4, 3, 2, puis 1).
- on peut remplacer `through` par `to` : dans ce cas, Sass n'inclut pas la dernière valeur (dans notre exemple, `$i` vaudra 1, 2 puis 3 mais pas 4).