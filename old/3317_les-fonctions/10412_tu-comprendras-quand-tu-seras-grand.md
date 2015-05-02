Intéressons-nous maintenant à six fonctions concernant les listes. Dans l'immédiat, elles ne vous seront d'aucune utilité. Cependant, à partir du prochain chapitre, consacré aux *conditions & boucles*, elles seront indispensables.

# La notion d'index
Dans une liste, chaque élément a un index unique. Il s'agit tout bêtement de sa position dans la liste : le premier a l'index 1, le deuxième a l'index 2, etc. Prenons par exemple la liste suivante :
```scss
$font-family: "Source Sans Pro", Helvetica, Arial, sans-serif;
```

On obtient donc les couples Index-Valeur suivants :
+---------------------------------------------------+
| La liste $font-family                             |
+======+=================+=========+=====+==========+
| Index| 1               | 2       | 3   | 4        |
+------+-----------------+---------+-----+----------+
|Valeur|"Source Sans Pro"|Helvetica|Arial|sans-serif|
+------+-----------------+---------+-----+----------+

La fonction `nth($list, $n)` permet de récupérer une valeur de la liste `$list` en donnant son index `$n` :
```scss hl_lines="4-6"
div{
  $border: 1px solid black;
  border:{
    width: nth($border, 1); //1px
    style: nth($border, 2); //solid
    color: nth($border, 3); //black
  }
}
```

La fonction `index($list, $value)` a le comportement exactement inverse. Elle renvoie l'index d'une valeur `$value` dans la liste `$list`. Si la valeur donnée n'existe pas dans la liste, la fonction renvoie `null`.
```scss
index(1px solid black, solid); //2
index(("Source Sans Pro", Helvetica, Arial, sans-serif), "Times New Roman"); //null
```

Enfin, lorsqu'on travaille avec une liste, on a souvent besoin de connaître le nombre d'éléments qu'elle contient. Pour cela, on utilise la fonction `length($list)` :
```scss
$font-family: "Source Sans Pro", Helvetica, Arial, sans-serif;
length($font-family); //4
nth($font-family, length(font-family)); //sans-serif
```
Comme vous le voyez ligne 3, on peut obtenir le dernier élément d'une liste en combinant `nth()` et `length()`.

# Ajouter et assembler
Les trois fonctions que je vais vous présenter maintenant renvoient une liste. La première est `append($list, $val)` et sert à ajouter un élément à une liste :
```scss hl_lines="3"
$font-list: "Source Sans Pro", Helvetica, Arial;
p{
  font-family: append($font-list, sans-serif);
  // "Source Sans Pro", Helvetica, Arial, sans-serif
}
```

La deuxième est `join($list1, list2)` et assemble deux listes en une seule :
```scss hl_lines="3"
$liste1: 50px 2em;
$liste2: 10px 2em;
$liste3: join($liste1, $liste2);
footer{
  padding: $liste3; // 50px 2em 10px 2em
}
```
[[information]]
| Sachez que `append()` et `join()` acceptent un troisième argument optionnel : `$separator`. Il permet de choisir le séparateur entre les éléments de la liste générée (entrez `comma` si vous voulez des virgules ou `space` si vous voulez des espaces). Par défaut, Sass choisit le séparateur de la/les listes passées en argument(s).

La troisième et dernière fonction est assez particulière mais très puissante. La fonction `zip($lists...)` accepte un ensemble de listes qu'elle va assembler en une unique *liste multidimensionnelle* (une liste de listes).
Pour comprendre son fonctionnement, je vous propose d'essayer vous-même ce petit exemple :
```scss hl_lines="5"
$proprietes: color background-color width;
$durees: 1s 2s 10s;
$timing: linear ease-in-out ease;
div{
  transition: zip($proprietes, $durees, $timing);
}
```
Le code généré par Sass sera le suivant :
```css hl_lines="2"
div {
  transition: color 1s linear, background-color 2s ease-in-out, width 10s ease;
}
```
Comme vous pouvez le constater, `zip()` a renvoyé une liste contenant 3 listes, qui contiennent chacune un élément de chaque liste passée en argument.

Pour modéliser cela on peut faire un tableau à double entrée :
+------+----------------+-----+
|color |background-color|width|
+------+----------------+-----+
|1s    |2s              |10s  |
+------+----------------+-----+
|linear|ease-in-out     |ease |
+------+----------------+-----+

Les listes passées en arguments sont représentées par les lignes tandis que les listes renvoyées sont représentées par les colonnes.