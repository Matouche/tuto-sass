# Plus loin avec Sass
 Dans cette seconde partie, nous allons nous intéresser à quelques aspects un peu plus techniques de Sass.
 Nous parlerons des fonctions, des conditions, des boucles et nous essairons le framework Bourbon et la bibliothèque Susy qui étendent un peu plus les capacités de Sass.

## L'héritage avec @extend
Je vous souhaite la bienvenue dans le dernier chapitre de cette première partie.
### Il est où le grisbi ?
Rassurez-vous, votre grande-tante est encore en vie ! Ici, ce sont des classes CSS qui héritent. L’*héritage de classe* est le fait qu’un élément puisse *hériter* des propriétés d’un autre élément. Je m'explique

### Plus loin avec l'héritage
####Classes fictives
####Comportements étranges
### Bonne pratique : Mixins vs @extend (ou pourquoi vous devriez oublier @extend)
### En résumé
Dans le prochain chapitre, on se lance dans un sujet passionnant, les boucles !

## Les boucles

Une fois n'est pas coutume, on va partir d'un exemple issu de notre fil rouge. Regardez le code suivant :

```scss
#presentation{
    background-image: url('img/bg-presentation.jpg');
}
#production{
    background-image: url('img/bg-production.jpg');
}
#contact{
    background-image: url('img/bg-contact.jpg');
}
```

Vous remarquez qu'on a répété trois fois le même code en ne changeant qu'un mot à chaque fois.

Stockons les différentes variantes dans une liste :

```scss
$sections : "presentation", "production", "contact";
```

Et maintenant, comment fait-on ? On utilise une boucle, évidemment !

### La boucle @for

Un boucle permet de répéter un certain nombre de fois un même bout de code, en changeant une valeur à chaque fois. Il existe 3 familles de boucles dans le langage SCSS (nous n'en verrons que deux). La première, c'est la boucle @for. Regardez plutôt le code suivant :

```scss
@for $i from 1 through 3{
    #section-#{$i}{
        background-image: url('img/bg-' + $i + '.png');
    }
}
```
On obtient le CSS suivant :

```scss
#section-1 {
    background-image: url("img/bg-1.png");
}
#section-2 {
    background-image: url("img/bg-2.png");
}
#section-3 {
    background-image: url("img/bg-3.png");
}
```

Comme vous pouvez le voir, la boucle `@for` a répété trois fois le bloc de code, en changeant la valeur de `$i` à chaque itération (ou répétition de la boucle). Cette variable (appelée indice de la boucle) à pris tour à tour  les valeurs 1, 2 puis 3 (tous les entiers entre 1 et 3, `from 1 through 3`). Remarquez que ici, l'interpolation et la concaténation m'ont été très utiles.

Bon, c'est très bien tout cela, mais on n'a pas obtenu le code du départ. Alors oui, on pourrait changer les ids de nos sections dans le HTML et les noms de nos images, mais ce serait de la triche. Non, on va plutôt utiliser notre liste `$sections`, et une fonction que nous avons vu il y a peu : `nth()`. Et il faut bien dire qu'elle ne révèle toute son utilité qu'avec les boucles :

```scss
$sections : "presentation", "production", "contact";
@for $i from 1 through length($sections){
    #section-#{nth($section, $i)}{
        background-image: url('img/bg-' + nth($section, $i) + '.png');
    }
}
```

Et voilà ! On obtient à nouveau un code qui se sert des noms de images et de nos ids. Remarquez que, pour plus de fléxibilité, j'ai utilisé `length()` pour indiquer la fin de la boucle. Ainsi, si on a une nouvelle section à ajouter, on aura juste à modifier la liste et la boucle sera toujours valable.

[[information]]
|Deux petites choses à savoir aussi sur la boucle `@for` (mais dont l'utilité est limitée) :
|
|- on peut compter à l'envers : `@for $i from 4 through 1` (`$i` vaudra 4, 3, 2, puis 1).
|- on peut remplacer `through` par `to` : dans ce cas, Sass n'inclut pas la dernière valeur (dans notre exemple, `$i` vaudra 1 puis 2 mais pas 3).

### La boucle @each

La boucle @for est formidable. Mais il y a encore mieux. Parce que, sachant que les boucles sont la plupart du temps utilisées pour parcourir des listes, comme nous l'avons fait, les concepteurs de Sass ont ajouté à notre attirail une autre boucle, dédiée spécifiquement à cette utilisation. La boucle @each, comme son nom l'indique, se répète pour CHAQUE item d'une liste. Elle s'utilise comme ceci :

```scss
$sections : "presentation", "production", "contact";
@each $section in $sections{
    #section-#{$section}{
        background-image: url('img/bg-' + nth($section, $i) + '.png');
    }
}
```

La première ligne se lit ainsi : la variable `$section` vaudra tour à tour chaque valeur contenue dans la liste `$sections`. On obtient donc le même code que précédemment.

Vous vous rappelez des listes multidimensionnelles ? Elles sont aussi parcourables avec `@each`, c'est ce qu'on appelle *l'affectation multiple*. À chaque itération, on peut accéder à tous les éléments d'une des « sous-listes », comme ceci :

```scss hl_lines="1"
// Ce bout de code permet de gérer en un rien de temps le rythme vertical de
// vos titres.
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

Voilà, c'est tout pour `@each`, même si nous en reparlerons lorsqu'il sera question des *maps*.

### Mini TP : Gestion d'une *sprite*

Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

### En résumé

+ La boucle `@for` est utile pour **répéter plusieurs fois un même code en changeant uniquement un nombre** et s'utilise ainsi : `@for $i from 1 through 4{...}`.
+ La boucle `@each` permet de **répéter un même bout de code en parcourant une liste d'items**, on s'en sert comme ceci : `@each $aliment in frites, tomate, yaourt, raviolis{...}`.

Je ne vous ai volontairement pas parlé de la boucle `@while` dont l'utilité est plus que limitée. Si vous pensez en avoir besoin un jour, je vous invite à consulter [la doc Sass](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_12).

Dans le prochain chapitre, on parlera des conditions.

## Les conditions

Entrons maintenant dans le royaume des *conditions*. Comme c'est un concept assez complexe à expliquer, je vous propose d'étudier d'abord le principe dans un premier temps, puis de voir son utilité dans un deuxième temps.

### Principes de base

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

### @else, @else if…

Passons à des choses plus complexes… On veut dire ceci à Sass :
```text
SI $var == 'bidule'
  Appliquer color: red;
SINON
  Appliquer color: blue;
```
La directive Sass pour dire « SINON » est `@else`. Elle s'utilise ainsi :
```scss
p{
  @if $var == 'bidule'{
    color: red;
  }
  @else{
    color: blue;
  }
}
```
Donc, si on a `$var: 'chose';` alors le code généré sera :
```css
p {
  color: blue;
}
```

Encore plus compliqué. On veut que Sass comprenne ceci :
```text
SI $var == 'bidule'
  Appliquer color: red;
SINON SI $var == 'machin'
  Appliquer color: green;
SINON
  Appliquer color: blue;
```
Pour dire « SINON SI » à Sass, on va employer `@else if`, comme cela :
```scss
p{
  @if $var == 'bidule'{
    color: red;
  }
  @else if $var == 'machin'{
    color: green;
  }
  @else{
    color: blue;
  }
}
```
Voilà pour la théorie, voyons maintenant à quoi ça sert dans la pratique…

### Mini-TP, un mixin bien compliqué
