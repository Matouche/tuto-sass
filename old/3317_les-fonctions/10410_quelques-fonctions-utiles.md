Commençons tout d'abord par voir quelques fonctions très utiles que Sass nous sert sur un plateau. Je les ai classées en différentes catégories.

# Pour définir une couleur

Vous avez sans doute déjà utilisé `rgb()`, `rgba()`, `hsl()` ou `hsla()` pour définir une couleur dans une feuille de style. Sass considère que ce sont des fonctions qui renvoient une couleur. Cela veut dire qu'il les convertira automatiquement au format le plus adapté dans le fichier final.
[[information]]
| Si vous ne connaissez pas le modèle HSL (Teinte Saturation Lumière), allez donc voir par [ici](http://docs.webplatform.org/wiki/css/color#HSL_and_HSLA_notation) ou par [là](http://fr.wikipedia.org/wiki/Teinte_saturation_lumi%C3%A8re).

*[HSL]: Hue Saturation Lightness

# Pour jouer avec les nombres

De toutes les fonctions que Sass propose concernant les nombres, je pense que `percentage($number)` et `round($number)` sont les deux seules que vous devez retenir.

La première demande un nombre sans unité et retourne un pourcentage, comme dans cet exemple :
```scss
div{
  width: percentage(0.5); //50%
}
```  
Cependant, cela devient vraiment intéressant si vous avez des valeurs en pixels (tirées d'une maquette par exemple), car vous n'avez plus besoin de faire les calculs vous-mêmes :
```scss
div{
  width: percentage(200px/960px); //20,8333% (vous aviez prévu de le faire de tête ?)
}
```

Enfin, la fonction `round()` renvoie l'arrondi à l'unité du nombre donné. Très utile lorsque vous faites des divisions impliquant des pixels :
```scss
small{
  font-size: round(15px/2); //8px (7.5px aurait été absurde)
}
```