# En résumé
+ Une *fonction* demande des arguments, fait des calculs et renvoie un résultat.
+ Pour *créer une fonction*, on utilise une syntaxe proche de celle pour créer un mixin, mais on emploie la directive `@function` et on appelle `@return` pour renvoyer un résultat.

+-----------------------------------------------------------------------------------------------------+
|Mémo des fonctions utiles                                                                            |
+---------+---------------------------------------------+---------------------------------------------+
|Catégorie|Nom(Arguments)                               | Résultat                                    |
+=========+=============================================+=============================================+
|Nombres  |`percentage($number)`                        |Le pourcentage correspondant au nombre       |
+         +---------------------------------------------+---------------------------------------------+
|         |`round($number)`                             |L'arrondi à l'unité                          |
+---------+---------------------------------------------+---------------------------------------------+
|Couleurs |`rgb($red, $green, $blue)`                   |La couleur demandée                          |
+         +---------------------------------------------+---------------------------------------------+
|         |`rgba($red, $green, $blue, $alpha)`          |La couleur demandée                          |
+         +---------------------------------------------+---------------------------------------------+
|         |`hsl($hue, $saturation, $lightness)`         |La couleur demandée                          |
+         +---------------------------------------------+---------------------------------------------+
|         |`hsla($hue, $saturation, $lightness, $alpha)`|La couleur demandée                          |
+         +---------------------------------------------+---------------------------------------------+
|         |`invert($color)`                             |La couleur inverse (négatif)                 |
+         +---------------------------------------------+---------------------------------------------+
|         |`grayscale($color)`                          |La nuance de gris correspondant à la couleur |
+         +---------------------------------------------+---------------------------------------------+
|         |`mix($color1, $color2)`                      |Le mélange des deux couleurs                 |
+         +---------------------------------------------+---------------------------------------------+
|         |`lighten($color, $amount)`                   |La couleur plus claire (selon le %)          |
+         +---------------------------------------------+---------------------------------------------+
|         |`darken($color, $amount)`                    |La couleur plus foncée (selon le %)          |
+         +---------------------------------------------+---------------------------------------------+
|         |`saturate($color, $amount)`                  |La couleur plus saturée (selon le %)         |
+         +---------------------------------------------+---------------------------------------------+
|         |`desaturate($color, $amount)`                |La couleur moins saturée (selon le %)        |
+         +---------------------------------------------+---------------------------------------------+
|         |`opacify($color, $amount)`                   |La couleur plus opaque (selon le %)          |
+         +---------------------------------------------+---------------------------------------------+
|         |`transparentize($color, $amount)`            |La couleur moins opaque (selon le %)         |
+---------+---------------------------------------------+---------------------------------------------+
|Listes   |`nth($list, $n)`                             |La valeur dans la liste à l'index `$n`       |
+         +---------------------------------------------+---------------------------------------------+
|         |`index($list, $value)`                       |L'index de la valeur dans la liste           |
+         +---------------------------------------------+---------------------------------------------+
|         |`length($list)`                              |Le nombre d'éléments dans la liste           |
+         +---------------------------------------------+---------------------------------------------+
|         |`append($list, $val, $separator: auto)`      |La liste avec la valeur ajoutée à la fin     |
+         +---------------------------------------------+---------------------------------------------+
|         |`join($list1, $list2, $separator: auto)`     |Les deux listes assemblées en une seule      |
+         +---------------------------------------------+---------------------------------------------+
|         |`zip($lists...)`                             |La liste multidimensionnelle tirée des listes|
+---------+---------------------------------------------+---------------------------------------------+