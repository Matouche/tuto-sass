Si vous avez un peu écouté en Maths au collège et/ou au lycée, vous devriez savoir ce qu'est une fonction mathématique. Du coup j'ai ressorti mon cahier de 3^e^ :
> Définition : Une fonction est un procédé par lequel on associe à un nombre donné (initial) un seul nombre correspondant.

Pour Sass, c'est un peu la même chose : on passe des données à la fonction (l'équivalent du nombre initial en Maths), elle fait ses calculs, et elle *renvoie* le résultat. Les données initiales, appelées arguments (comme pour les mixins) peuvent être des nombres, des couleurs, des chaînes de caractères, des listes, etc. tout comme le résultat.

[[information]]
| Si, en plus d'écouter en Maths, vous avez déjà fait de la programmation, non seulement vous êtes formidable, mais en plus vous devriez savoir ce qu'est une fonction.

Un exemple ? Je vous propose d'essayer la fonction `darken`, inclue avec Sass, qui permet d'assombrir une couleur :

![La fonction darken prend deux arguments (#ff0000 et 10 %) et renvoie #cc0000.](/media/galleries/848/b2d407f4-5ad5-4d20-a872-3536888a1531.png.960x960_q85.png)

Elle prend deux arguments : `$color`, la couleur de départ, et `$amount` le pourcentage d'assombrissement. Elle renvoie la couleur assombrie du pourcentage demandé. On utilise la fonction `darken` ainsi :
```scss hl_lines="2"
header{
  background-color: darken(#ff0000, 10%);
}
```  
Comme attendu, ce code donne le CSS suivant :

```css hl_lines="2"
header{
  background-color: #cc0000;
}
``` 

Comme vous pouvez le remarquer, cette syntaxe est assez proche de celle utilisée par les mixins (les arguments entre parenthèses), sauf qu'on n'utilise pas `@include`. D'ailleurs, vous pouvez, comme pour les mixins, nommer vos arguments :
```scss
header{
  background-color: darken($color: #ff0000, $amount: 10%);
}
``` 

Sass met à votre disposition tout un tas de fonctions très utiles, que nous allons voir plus en détail dans la suite de ce chapitre.