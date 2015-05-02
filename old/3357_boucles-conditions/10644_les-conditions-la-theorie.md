Entrons maintenant dans le royaume des *conditions*. Comme c'est un concept assez complexe à expliquer, je vous propose d'étudier d'abord le principe dans un premier temps, puis de voir son utilité dans un deuxième temps.

Tout d'abord, il faut savoir que avec Sass on peut effectuer des tests. Par exemple, on peut vérifier que la variable `$truc` vaut 42. Pour cela on utilise l'opérateur `==`, comme ceci :
```scss
$truc == 42
```

Ce test a pour résultat un booléen (enfin une utilité). Ce booléen vaudra `true` si le test est positif (si `$truc` vaut bien 42) ou `false` si le test est négatif (si `$truc` ne vaut pas 42). Il existe six opérateurs différents pour faire des tests :
+---------+-------------+------------------------------+
|Opérateur| Exemple     | Test positif si…             |
+=========+=============+==============================+
|**==**   |`$truc == 42`|`$truc` égal 42               |
+---------+-------------+------------------------------+
|**!=**   |`$truc != 42`|`$truc` différent de 42       |
+---------+-------------+------------------------------+
|<        |`$truc < 42` |`$truc` inférieur strict à 42 |
+---------+-------------+------------------------------+
|\>       |`$truc > 42` |`$truc` supérieur strict à 42 |
+---------+-------------+------------------------------+
|<=       |`$truc <= 42`|`$truc` inférieur ou égal à 42|
+---------+-------------+------------------------------+
|\>=      |`$truc >= 42`|`$truc` supérieur ou égal à 42|
+---------+-------------+------------------------------+

J'ai mis les deux premiers en gras car ce sont les seuls que j'ai déjà utilisé dans la pratique. Les autres sont, à mon humble avis, plus anecdotiques.

Intéressons-nous maintenant à la directive `@if`. Elle attend une condition à évaluer (le plus souvent un booléen) et un bloc de code entre accolades. Elle s'utilise comme ceci :
```scss hl_lines="2"
p{
  @if $condition {
    color: red;
  }
}
```
La condition à évaluer est ici `$condition` :

- si `$condition` vaut `true` ou n'importe quoi autre que `false` ou `null`, alors le bloc de code sera inséré :
```css
p{
  color: red;
}
```
- si `$condition` vaut `false` ou `null` alors Sass ignorera le bloc de code.

Comme vous devriez le deviner, on peut remplacer `$condition` par l'un des tests vus précédemment :
```scss
$var: 'bidule';
p{
  @if $var == 'bidule'{
    color: red;
  }
  @if $var == 'machin'{
    color: blue;
  }
}
```
Le code généré sera :
```css
p{
  color: red;
}
```