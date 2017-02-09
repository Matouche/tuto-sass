# Plus loin avec Sass
 Dans cette seconde partie, nous allons nous intéresser à quelques aspects un peu plus techniques de Sass.
 Nous parlerons d'héritage, des conditions, des boucles et nous essairons le framework Bourbon et la bibliothèque Susy qui étendent un peu plus les capacités de Sass.

## L'héritage avec @extend
Pour inaugurer cette seconde partie consacrée à des sujets un peu plus avancés concernant Sass, j'ai décidé de vous parler de la directive @extend. L'héritage n'est pas une notion complexe en soit, mais je n'ai pas souhaité vous en parler avant car on l'utilise assez peu, et que son utilisation est de plus en plus déconseillée. Cependant, je m'en voudrait de ne pas l'évoquer pour autant. On va donc voir en quoi cela consiste, et on verra ensuite pourquoi il est recommandé de s'en passer.

### Il est où le grisbi ?
Rassurez-vous, votre grande-tante est encore en vie ! Ici, ce sont des sélecteurs qui héritent. L’**héritage** est la posibilité pour un élément d'*hériter* des propriétés d’un autre élément. Prenons par exemple les *cartes* de notre page Web (représentant un produit, une étape de production ou un avis de client). Elles ont toutes des propriétés en commun, regroupées dans la classe `.card`, mais ont aussi un certain nombre de propriétés différentes, qui sont réparties entre les classes `.product`, `.step` et `.client`. On peut dire que, conceptuellement, une élément `.product`, `.step` ou `.client` hérite des propriétés de la classe `.card`.

Pour l'instant, cet héritage est écrit en dur dans le HTML :chaque élément a deux classes. Si jamais on oublie de donner la classe `card` a un élément `product`, il ne recevra pas tous les styles nécessaires pour son bon affichage. Sass nous permet de gérer cela autrement, à l'intérieur de la feuille de style, avec la directive `@extend`. Cette dernière permet de dire qu'un élément X hérite forcément des propriétés d'un élément Y :

```scss hl_lines="8,15,18"
.card{
    ...
    img{
        ...
    }
}
.product{
    @extend .card;
    min-height: 20rem;
    background-color: #fff;
    color: #222;
    ...
}
.step{
    @extend .card;
}
.client{
    @extend .card;
    background-color: #222;
    color: #fff;
    ...
}
```

Qu'est-ce que cela va donner en CSS ? En fait Sass va, tout simplement, placer les sélecteurs qui héritent partout où il trouve le sélecteur `.card`. Ainsi, tous les blocs de propriétés concernant `.card` seront appliqués aussi à `.product`, `.step` et `.client` :
```css
.card, .product, .step, .client {
    ...
}
.card img, .product img, .step img, .client img {
    ...
}
```

Et voilà, on n'a donc plus besoin de la classe `card` dans notre HTML, vu que l'héritage est déjà géré dans le CSS. Un nouvel élément a lui aussi besoin de ressembler à un carte ? Pas de souci, il suffit de le faire hériter lui aussi.

Il est important de noter qu'on peut spécifier n’importe quel sélecteur **simple** après `@extend`. Il ne peut donc pas s'agir d'un *sélecteur imbriqué* du type `X Y`, `X>Y`, `X+Y` et `X~Y` (les deux derniers étant respectivement les sélecteurs d’adjacence directe et indirecte). Bon, d'un autre côté, si vous souhaitez que `.bidule` hérite de `.machin>p+div~.truc`, c'est que vous avez l'esprit légèrement tordu.

[[information]]
| Encore plus fort : on peut faire des héritages en chaîne, «en cascade» :
|
| ```scss hl_lines="5 9"
| .message{
| border: 2px solid black;
| }
| .alerte{
|     @extend .message;
|     color: red;
| }
| .danger{
|     @extend .alerte;
|     font-size: xx-large;
| }
| ```
| La classe `.danger` hérite de la classe `.alerte` qui hérite de la classe `.message` !

###Les placeholders (un nouvel eldorado ?)
Maintenant que nos classes héritent de `.card`, on a vu qu'on n'avait plus besoin de mentionner `card` dans notre HTML. À vrai dire, la classe `.card` n'est utile que pour Sass, pour faire notre héritage. Depuis sa version 3.2, Sass inclut donc un nouveau type de sélecteurs, les *placeholders*, ou *« `@extend`-only selectors »*. Un placeholder est une classe qui ne sert qu'à l'héritage, et qui disparaitra durant la compilation. Pour transformer une classe en placeholder, ce n'est pas bien compliqué : il suffit de remplacer le `.` par un `%`. Ainsi, dans notre exemple :

```scss hl_lines="1, 8,15,18"
%card{
    ...
    img{
        ...
    }
}
.product{
    @extend %card;
    min-height: 20rem;
    background-color: #fff;
    color: #222;
    ...
}
.step{
    @extend %card;
}
.client{
    @extend %card;
    background-color: #222;
    color: #fff;
    ...
}
```

Et le résultat en CSS ? La même chose qu'auparavant, mais sans `.card` :

```css
.product, .step, .client {
    ...
}
.product img, .step img, .client img {
    ...
}
```

On se retrouve donc avec un placeholder, qui n'apparait que dans le fichier SCSS, dont héritent les classes que l'on utilise réellement dans le HTML.

###Bonne pratique : Faut-il oublier @extend ?

[[question]]
| Mais, n'aurait-on pas pu utiliser un mixin à la place ?

Héhé, excellente question, chers lecteurs, que de nombreuses personnes se sont posées avant vous. Il est vrai qu'à chaque fois qu'on utilise l'héritage (ici avec le placeholder `%card`), on pourrait aussi faire appel à un mixin (le mixin `card`). À l'inverse, on aurait aussi pu gérer nos boutons avec l'héritage de placeholders plutôt qu'avec des mixins : on n'a pas accès aux arguments, mais on aurait pu créer un placeholder `%button`, dont héritent deux placeholders `%light-button` et `dark-button`, dont héritent les différentes classes de boutons de notre design. Le résultat est en apparence le même, cependant, on a quelques différences entre les 2 techniques :

* Il y a des répétitions dans le fichier CSS généré avec des mixins, pas avec l'héritage.
* On obtient des sélecteurs plus longs avec l'héritage.
* On ne peux évidemment pas utiliser d'arguments avec l'héritage.
* L'héritage pose problème à l'intérieur de media-queries, pas les mixins.
* On obtient parfois des comportements assez innatendus avec l'héritage.

Concernant le premier point, on a vu dans le chapitre sur les mixins que ce n'était pas un réel problème, vu que l'algorithme de *gzip* gère très bien les répétitions. Concernant le deuxième point, il peut, à grande échelle, influer sur le temps de chargement de la page, mais c'est probablement peu visible[^imprecis]. Le troisième point peut poser problème concernant la réutilisabilité du code : un placeholder n'est pas paramétrable, il n'a donc pas vraiment vocation à être réutilisé dans plusieurs projets. Les deux derniers points sont plus complexes, et carrément problématiques. En fait, pour les comprendre, il faut garder en mémoire qu'un placeholder, **ce n'est techniquement pas la même chose qu'un mixin**, leur fonctionnement étant différent.

[^imprecis]: Encore que... il faudrait vérifier, je suis loin d'être spécialiste du domaine.

#### Héritage & Media-queries

En effet, lorsqu'une classe `.x` hérite d'une classe `.y`, Sass va ajouter le sélecteur de `.x` partout où il trouvera`.y`, et il ne fera rien d'autre. Ainsi, imaginons le code suivant :

```scss
%agrume{
    color:red;
}
.mandarine{
    @media (min-width: 720px){
        @extend %agrume;
    }
}
```

Il est important qu vous compreniez que ce code ne peut pas fonctionner. On pourait s'attendre à obtenir ceci :

```css
@media (min-width: 720px) {
    .mandarine {
        color:red;
    }
}
```

Mais ça, c'est ce qui se passerait si on avait utilisé un mixin. Avec un placeholder, c'est très différent : Sass cherche où il peut trouver le sélecteur `%agrume` pour le remplacer par le sélecteur `.mandarine`. Sauf qu'il voit bien que l'élément `.mandarine` est à l'intérieur d'une media-query, alors que `%agrume` est à l'extérieur de celle-ci. Il ne va pas déplacer l'élément `%agrume` à l'intérieur de la media-query, parce que ce n'est pas comme cela que fonctionne l'héritage. On a donc droit à une erreur de compilation. En bref, l'héritage et les media-queries ne font pas bon ménage.

#### Comportements imprévus
Concernant le dernier point, il ya plusieurs choses à savoir. En effet, on pense, à tord, que l'héritage est équivalent à l'utilisation des mixins. Ce qui, régulièrement, peut générer du code supperflu et imprévu.

Tout d'abord, rappelez-vous : je vous ai dit dans la première partie qu'on pouvait donner n'importe quel sélecteur simple à @extend. Cela veut dire que, théoriquement, un élément peut hériter d'une balise HTML comme `a`, `strong`, `h1`, etc. Mais ce serait plutôt une mauvaise idée. En effet, il est probable que ces balises existent dans différents contextes sur votre page : dans votre bannière, dansles différentes sections, dans une barre latérale, dans le pied de page... Si la classe `.bidon` hérite d'une de ces balises, Sass insèrera son nom partout où apparait la balise en question, dans tous les contextes différents où la balise est utilisée. Mais, est-ce vraiment ce que l'on souhaite : la classe `.bidon` se trouve-t-elle réellement dans chacun de ces contextes ? Parce que, si ce n'est pas le cas, on aura créé des sélecteurs qui n'ont aucune raison d'exister.

Par ailleurs, n'oubliez pas que `@extend` ne déplace pas le code. Petit exemple :

```scss
.mandarine{
    @extend %mere;
    color : red;
}
%mandarine{
    color: blue;
}
```

De quelle couleur est le texte de l'élément `.mandarine` ? Rouge ? Loupé, il est bleu, parce que la propriété du placeholder `%agrume` est située en-dessous dans la feuille de style et écrase donc la précédente.

Parlons aussi un peu de la *fusion des sélecteurs*. Regardez cet exemple :

```scss hl_lines="1 4"
#div1 #div2 em{
  @extend strong;
}
#div3 #div4 strong{
  font-weight: bold;
}
```

Comment Sass va-t-il traduire ceci ? J’ai mis en évidence les deux séquences à fusionner. Comme elles n'ont pas d’éléments
en commun, Sass créera deux nouveaux sélecteurs : le premier avec les parents de `strong` avant les parents de
`em` et le second avec les parents de `em` avant les parents de `strong`. Voici donc le résultat :

```css hl_lines="2-3"
#div3 #div4 strong,
#div3 #div4 #div1 #div2 em,
#div1 #div2 #div3 #div4 em{
  font-weight: bold;
}
```

Maintenant, que se passe-t-il lorsque les éléments parents ont au moins un élément en commun ? Faisons le test avec cet exemple :

```scss hl_lines="1 4"
#commun #div1 em{
  @extend strong;
}
#commun #div2 strong{
  font-weight: bold;
}
```

Comme vous le voyez, les sélecteurs ont l’élément `#commun` en commun. Sass, durant la traduction en CSS, fusionnera les deux `#commun` :

```css hl_lines="2-3"
#commun #div2 strong,
#commun #div2 #div1 em,
#commun #div1 #div2 em{
  font-weight: bold;
}
```

Là encore, on peut se demander si tous ces sélecteurs générés sont bien utiles, s'ils ciblent bien tous des éléments qui existent réellement dans la page, parce que si ce n'est pas le cas, cela ralentira inutilement l'affichage de la page, ce qui, sur des terminaux peu puissants (GSMs d'entrée de gamme par exemple), se fera sentir.

#### On oublie l'héritage ?

Je pense que cette section assez théorique devrait vous avoir fait comprendre que l'héritage a de nombreux effets secondaires, alors que les mixins n'ont que des avantages, et sont paramétrables. Plusieurs articles publiés sur des sites spécialisés par des personnes autrement plus qualifiées que moi-même recommandent de proscrire `@extend`, tout simplement. Après, vous faites ce que vous voulez, vous êtes des grandes personnes. ;)

### En résumé
* L’héritage est la posibilité pour un élément d’**hériter des propriétés CSS d’un autre élément**.
* Les **placeholders** sont des sélecteurs dont le nom commence par le symbole `%` et dont peuvent hériter d'autres éléments. Ils ne servent d'ailleurs qu'à cela car ils n'apparaissent pas ensuite dans le code CSS généré.
* L'héritage amène parfois à **des résultats innatendus** et présente de sérieuses limitations. On lui préfère souvent les **mixins**.

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


|Opérateur| Exemple     | Test positif si…             |
----------|-------------|------------------------------|
|**==**   |`$truc == 42`|`$truc` égal 42               |
|**!=**   |`$truc != 42`|`$truc` différent de 42       |
|<        |`$truc < 42` |`$truc` inférieur strict à 42 |
|\>       |`$truc > 42` |`$truc` supérieur strict à 42 |
|<=       |`$truc <= 42`|`$truc` inférieur ou égal à 42|
|\>=      |`$truc >= 42`|`$truc` supérieur ou égal à 42|

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
