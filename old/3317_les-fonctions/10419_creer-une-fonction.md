[[question]]
| Je cherche une fonction très particulière que tu n'as pas présenté, comment faire ?
Il faut tout d'abord vérifier si cette fonction n'existe pas déjà. En effet, je n'ai pas la place (et ce serait sans intérêt) de décrire toutes les fonctions que Sass propose ici. Allez donc faire un tour sur [la page de la documentation consacrée aux fonctions](http://sass-lang.com/documentation/Sass/Script/Functions.html). Il y en a pour tous les goûts.

Si vous pensez avoir besoin d'une fonction qui n'existe pas encore, il va falloir la créer. Prenons pour exemple une fonction chargée de calculer la largeur d'une grille de mise en page, constituée de colonnes séparées par des gouttières. Elle aura trois arguments :

+ le nombre de colonnes `$n`
+ la largeur d'une colonne `$colonne`
+ la largeur d'un gouttière `$gouttiere`

Le nombre de gouttières est forcément égal à `$n - 1`. Voici donc le code de notre fonction :
```scss
@function largeur-grille($n, $colonne, $gouttiere){
  @return $n * $colonne + ($n - 1) * $gouttiere;
}
```

Comme vous le voyez, la syntaxe est proche de celle pour créer un mixin, à ceci près que l'on utilise la directive `@function` et que l'on doit appeler `@return` pour renvoyer le résultat de la fonction.

Maintenant, on peut utiliser notre fonction comme n'importe quelle autre :
```scss hl_lines="2"
div{
  width: largeur-grille(12, 60px, 20px); //940px
}
```

Sachez enfin que, comme pour les mixins, on a la possibilité de donner des valeurs par défauts aux arguments :
```scss hl_lines="1"
@function largeur-grille($n: 12, $colonne: 60px, $gouttiere: 20px){
  @return $n * $colonne + ($n - 1) * $gouttiere;
}
```