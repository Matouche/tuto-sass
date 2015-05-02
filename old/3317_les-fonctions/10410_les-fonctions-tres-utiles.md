Commençons tout d'abord par voir quelques fonctions très utiles que Sass nous sert sur un plateau. Je les ai classées en différentes catégories.

# Pour définir une couleur

Vous avez sans doute déjà utilisé `rgb()`, `rgba()`, `hsl()` ou `hsla()` pour définir une couleur dans une feuille de style. Sass considère que se sont des fonctions qui renvoient une couleur. Cela veut dire qu'il les convertira automatiquement au format le plus adapté dans le fichier final (hexadécimal pour les couleurs opaques et RGBA pour les autres).
[[information]]
| Si vous ne connaissez pas le modèle HSL (Teinte Saturation Lumière), allez donc voir par [ici](http://docs.webplatform.org/wiki/css/color#HSL_and_HSLA_notation) ou par [là](http://fr.wikipedia.org/wiki/Teinte_saturation_lumi%C3%A8re).

*[HSL]: Hue Saturation Light

# Pour modifier une couleur

Attention, ça se corse, car Sass propose beaucoup de fonctions pour modifier les couleurs. Commençons par celles qui changent radicalement de couleur :
```css
complement($color);
grayscale($color);
mix($color1, $color2);
```
Les deux premières prennent une couleur pour seul argument :
+ `invert()` renvoie l'inverse de la couleur donnée (ex: #ff0000 devient #00ffff),
+ `grayscale()`revoie la nuance de gris correspondant à la couleur (ex: #ff0000 devient #808080).

Enfin, la fonction `mix()`prend deux couleurs, et renvoie le mélange des deux :
```css
h1{
  color: mix(#ff0000, #0000ff);//Renvoie #7f007f
  //Rouge + Bleu = Violet
}
```
A noter que `mix()`accepte un troisième argument optionnel en pourcentage, `weight`, qui indique s'il faut plus de la première ou de la deuxième couleur.

Continuons avec des fonctions qui proposent des modifications de Lumière, de Saturation ou d'Opacité :
```css
//Lumière
lighten($color, $amount);
darken($color, $amount);
//Saturation
saturate($color, $amount);
desaturate($color, $amount);
//Opacité
opacify($color, $amount);
transparentize($color, $amount);
```
Nous avons déjà vu `darken()` et, comme vous pouvez le constater, toutes ces fonctions demandent une couleur et un « pourcentage d'efficacité ». Je pense que les noms sont assez explicites, donc plutôt que de vous décrire tout en détail, je vous propose plutôt un joli schéma bilan.