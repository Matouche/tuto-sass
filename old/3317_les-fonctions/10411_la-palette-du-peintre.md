Voyons maintenant quelques unes des très nombreuses fonctions à notre disposition pour modifier les couleurs. 

# Inverse, Nuance de gris et Mélange
Commençons par celles qui changent radicalement une couleur :
```scss
invert($color)
grayscale($color)
mix($color1, $color2)
```
Les deux premières prennent une couleur pour seul argument :

+ `invert()` renvoie l'inverse ou négatif de la couleur donnée (ex: #ff0000 devient #00ffff),
+ `grayscale()`renvoie la nuance de gris correspondant à la couleur (ex: #ff0000 devient #808080).

Enfin, la fonction `mix()`prend deux couleurs, et renvoie le mélange des deux :
```scss
h1{
  color: mix(#ff0000, #0000ff);//Renvoie #7f007f
  //Rouge + Bleu = Violet
}
```
A noter que `mix()`accepte un troisième argument optionnel en pourcentage, `$weight`, qui indique s'il faut plus de la première ou de la deuxième couleur.

# Lumière, Saturation, Opacité
Continuons avec des fonctions qui proposent des modifications de lumière, de saturation ou d'opacité :
```scss
//Lumière
lighten($color, $amount) //plus clair
darken($color, $amount) //plus sombre

//Saturation
saturate($color, $amount) //plus saturé
desaturate($color, $amount) //moins saturé

//Opacité
opacify($color, $amount) //plus opaque
transparentize($color, $amount) //moins opaque
```
Nous avons déjà vu `darken()` et, comme vous pouvez le constater, toutes ces fonctions demandent une couleur et un « pourcentage d'efficacité » de la transformation. Je pense que les noms sont assez explicites, donc plutôt que de vous décrire tout en détail, je vous propose plutôt un joli schéma bilan :

![Schéma-bilan des fonctions qui modifient une couleur  ($amount: 30%)](/media/galleries/848/41f4769a-d494-44d9-a0b9-3601179a71b2.png.960x960_q85.png)