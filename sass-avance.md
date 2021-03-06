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

Dans le prochain chapitre, on se lance dans un sujet passionnant, les conditions !

## Les conditions

Entrons maintenant dans le royaume des *conditions*. Comme c'est un concept assez complexe à expliquer, je vous propose d'étudier d'abord le principe dans un premier temps, puis de voir son utilité dans un deuxième temps.

### Les bases

Tout d'abord, il faut savoir que avec Sass on peut effectuer des tests. Par exemple, on peut vérifier que la variable `$truc` vaut 42. Pour cela on utilise l'opérateur `==`, comme ceci :
```scss
$truc == 42
```

Ce test a pour résultat un booléen. Ce booléen vaudra `true` si le test est positif (si `$truc` vaut bien 42) ou `false` si le test est négatif (si `$truc` ne vaut pas 42). Il existe six opérateurs différents pour faire des tests :


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

Deux trois choses à savoir (qui peuvent sembler évidentes à certains d'entre vous) :
* Les directives `@else` et `@else if` sont optionnelles.
* On peut utiliser autant de `@else if` que l'on veut après un `@if`, il ne peut y a voir qu'un `@else`.
* Le `@else` se place toujours en dernier, après le/les `@else if` s'il y en a.
* Il y a une grande différence entre plusieurs `@if` d'affilée et un `@if` suivi de plusieurs `@else if`. Dans le premier cas, Sass évalue chaque condition individuellement. Dans le second, si l'une des conditions est remplie, alors Sass ne regardera pas les suivantes.

Voilà pour la théorie, voyons maintenant à quoi ça sert dans la pratique…

### Mini-TP, un mixin bien compliqué

#### Prérequis
Passons maintenant à la pratique, et j'ai décidé de vous laisser un peu libres. Vous vous souvenez des mixins « *media-queries* » que nous avions codés il y a quelques chapitres ? Je vous avais suggéré de faire un mixin par appareil (mobile, ordinateur de bureau, etc.). Il y a mieux. Nous allons créer un unique mixin `breakpoint` qui accepte un argument (le nom de l'appareil) et qui génère la *media-query* correspondante.³
Voici par exemple ce que j’attends :
```scss
\\SCSS :
p{
  @include breakpoint(mobile){
    font-size: 16px;
  }
}
\\Ce qui donne en CSS :
@media screen and (max-width: 640px) {
  p {
    font-size: 16px;
}}
```

Je veux que votre mixin accepte au moins `mobile`, `tablette` et `ordi` en argument. Si on donne une valeur différente, Sass doit afficher un message d'erreur et ne pas générer de code CSS. Pour cela, vous devez utiliser la directive `@error` dont nous n'avons pas encore parlé, qui s'emploie ainsi :

```scss
@error "L'argument $appareil ne peut valoir que mobile, tablette ou ordi.";
```

Voilà, vous savez tout, à vous de jouer !

#### La solution
Fini ? Je vous laisse comparer votre solution avec la mienne. Si vous bloquez, vous pouvez toujours regarder un petit extrait puis ré-essayer par vous-même.
[[secret]]
| ```scss
| @mixin breakpoint($appareil){
|   @if $appareil == mobile{
|     @media screen and (max-width: 640px){
|       @content;
|     }
|   }
|   @else if $appareil == phablette{
|     @media screen and (min-width: 640px){
|       @content;
|     }
|   }
|   @else if $appareil == tablette{
|     @media screen and (min-width: 800px){
|       @content;
|     }
|   }
|   @else if $appareil == ordi{
|     @media screen and (min-width: 1024px){
|       @content;
|     }
|   }
|   @else{
|       @error "L'argument $appareil ne peut valoir que mobile, phablette, tablette ou ordi.";
|       //Cette directive affiche une erreur et bloque la conversion en CSS.
|       //Elle est utile pour vérifier si un argument a une valeur cohérente.
|   }
| }
| ```

#### Un peu mieux…
Comme tout TP, ce mixin n'est pas parfait. En effet, il est généralement déconseillé de choisir des *breakpoints* (points d'arrêts) spécifiques à un appareil. De nouveaux écrans apparaissent chaque année, plus petits (smartwatches) ou plus grands (phablettes, télévisions connectées). Il est donc recommandé de définir ses *breakpoints* en fonction du contenu.

Adaptons notre mixin pour qu'il utilise des variables plutôt que des largeurs d'écran fixes :

[[secret]]
| ```scss
| $bp-S: 640px !default;
| $bp-M: 800px !default;
| $bp-L: 1024px !default;
|
| @mixin breakpoint($bp){
|   @if $bp == xsmall{
|     @media screen and (max-width: $bp-S){
|       @content;
|     }
|   }
|   @else if $bp == small{
|     @media screen and (min-width: $bp-S){
|       @content;
|     }
|   }
|   @else if $bp == medium{
|     @media screen and (min-width: $bp-M){
|       @content;
|     }
|   }
|   @else if $bp == large{
|     @media screen and (min-width: $bp-L){
|       @content;
|     }
|   }
|   @else{
|       @error "L'argument $bp ne peut valoir que xsmall, small, medium et large.";
|   }
| }
| ```

Ainsi, on ne pense plus en terme d'appareils, mais de présentation de la page, de la plus étroite à la plus large. Les variables ont des valeurs par défaut, mais qui sont faites pour être modifiées selon le projet (il faut faire des tests pour déterminer les bons *breakpoints*, par exemple en utilisant [ish.](http://bradfrost.com/demo/ish)).

Si vous enregistrez ce code dans une feuille partielle à part, vous pourrez ensuite l'importer dans chacune de vos feuilles de styles, en changeant auparavant les valeurs par défaut (c'est tout l'intérêt de `!default`) :

```scss
$bp-S: 500px;
$bp-M: 980px;
$bp-L: 1250px;
@import 'partials/breakpoint';
```

C'est tout pour ce TP, il y a sans doute d'autres améliorations possibles. Pour approfondir, vous pouvez regarder du côté de [Breakpoint Sass (en)](http://breakpoint-sass.com/), qui propose un mixin comme le nôtre (mais en plus complet).

### En résumé

* La directive @if permet d'indiquer qu'un bloc de propriétés ne doit être appliqué que si une certaine condition est remplie. On l'utilise ainsi :
```scss
@if $une_variable == "quelque chose"{
    color: red;
}
```
* Les directives `@else if` et `@else` permettent de créer des structures conditionnelles plus complexes. Elles sont facultatives et signifient respectivement « Sinon Si » et « Sinon ».

### Pour s'entraîner

Pourquoi ne pas modifier un peu notre mixin `button` ? Immaginons un instant un mixin qui n'accepte qu'un seul argument `$style` ? Si `$style` vaut `"light"`, alors on obtient un bouton avec un fond clair, s'il vaut `$dark` on obtient un bouton au fond sombre.


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

#### Énoncé

Je vous propose de finir ce chapitre avec un petit exercice où les boucles nous seront particulièrement utiles : l'utilisation d'une **sprite d'images**. Une sprite, si vous ne savez pas ce que c'est, c'est une grande image sur laquelle on regroupe toutes les icônes, tous les smilleys, tous les pictogrammes utilisés dans un design. Comme souvent, le but recherché est de réduire la charge pour le serveur et le temps de chargement pour les visiteurs (plutôt que d'effectuer une trentaine de requêtes sur les images, on n'en a plus qu'une seule à effectuer). Ensuite, on se débrouille avec des `background-position` pour sélectionner la bonne image. Comme c'est assez ch… fatigant à effectuer manuellement, il existe des outils clef-en-main qui générent directment la *sprite* et le code CSS correspondant. Mais je vous propose de partir sur quelque chose de simple et de tout faire avec une boucle. Le but est d'obtenir ceci :

![Résultat de l'exercice](img/boutons_sprite.png)

Voici le code HTML (il n'a rien d'exceptionnel) :

```html
<a class="button icon-settings">Settings</a>
<a class="button icon-watch">Watch</a>
<a class="button icon-power">Power</a>
<a class="button icon-fullscreen">Full Screen</a>
<a class="button icon-reduce">Reduce</a>
```

Voici notre sprite :

![Mini sprite d'icones](img/sprite.png)

Et voici les styles de base de nos boutons (ce n'est pas franchement le sujet de l'exercice) :

```scss
.button{
    font-family: sans-serif;
    border: none;
    padding: 10px 10px 10px 52px; // on laisse de la place à gauche
    background-color: #fe0;
    position: relative; // pour pouvoir placer l'icône en absolute à l'intérieur
    display: inline-block;
    height: 32px;
    line-height: 32px;

    &:hover {
        background-color: lighten(#fe0, 20%); //c'est plus sympa, non ?
    }
}
```

Maintenant, à vous de jouer ! Il suffit de chipoter un peu avec `.button::before`, une boucle et `background-position` et le tour est joué.

#### Correction

Voici ma correction de l'exercice :

[[secret]]
| ```scss hl_lines="27-34"
| .button{
|     font-family: sans-serif;
|     border: none;
|     padding: 10px 10px 10px 52px; // on laisse de la place à gauche
|     background-color: #fe0;
|     position: relative; // pour pouvoir placer l'icône en absolute à l'intérieur
|     display: inline-block;
|     height: 32px;
|     line-height: 32px;
|
|     &:hover {
|         background-color: lighten(#fe0, 20%); //c'est plus sympa, non ?
|     }
|
|     &::before{
|         content: ' ';
|         display: block;
|         position: absolute;
|         top :10px;
|         left: 10px;
|         width: 32px;
|         height: 32px;
|         background-image: url(sprite.png);
|     }
| }
|
| $icons: 'settings', 'watch', 'power', 'fullscreen', 'reduce';
|
| @for $i from 1 through 5{
|     .icon-#{nth($icons, $i)}::before{
|         $pos: -32px * ($i - 1); //On commence à 0 et on finit à 32*4px
|         background-position-x: $pos;
|     }
| }
| ```

J'ai surligné la partie qui nous intéresse vraiment. On a donc une liste qui contient les noms des icônes dans le bon ordre et une boucle `@for` qui parcourt la liste (avec la fonction `nth()`), insère le nom dans le sélecteur (avec une interpolation qui va bien) et la valeur de l'indice `$i` sert à calculer la position de la sprite. On aurait aussi pu parcourir la liste directement avec une boucle `@each` et se servir de la fonction (`index()`) pour retrouver la valeur de `$i`, mais ça me semblait moins évident.

Voilà pour ce petit exercice. Comme dit précédemment, on préfèrera souvent utiliser un outil externe[^generateur] qui nous génère la sprite directement et le code (S)CSS qui va avec. En effet, on avit ici des icônes de la même taille allignées, ce qui est rarement le cas en conditions réelles, où il s'agit de gérer des sprites plus complexes. Voici par exemple celle utilisée par votre site préféré :

![La sprite de Zeste de Savoir](https://zestedesavoir.com/static/images/sprite.f651d381c5ba.png)

[^generateur]: Si vous êtes intéressé, je vous invite à regarder du côté de [Stitches](https://draeton.github.io/stitches/).

### En résumé

+ La boucle `@for` est utile pour **répéter plusieurs fois un même code en changeant uniquement un nombre** et s'utilise ainsi : `@for $i from 1 through 4{...}`.
+ La boucle `@each` permet de **répéter un même bout de code en parcourant une liste d'items**, on s'en sert comme ceci : `@each $aliment in frites, tomate, yaourt, raviolis{...}`.

Je ne vous ai volontairement pas parlé de la boucle `@while`, qui me semble moins utile que les deux autres. Sachez que c'est une boucle qui se répète tant qu'une condition est vraie. Si vous pensez en avoir besoin un jour, je vous invite à consulter [la doc Sass](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_12).


## Et maintenant ?
Nous arrivons tout doucement à la fin de ce tutoriel. Dans ce dernier chapitre (un peu fourre-tout, je dois bien l'admettre), il sera question du type map, de bibliothèques externes et des différentes implémentations de Sass.

### Le type map

Avant de vous lâcher dans la nature, je m'en voudrait de ne pas évoquer le type map. Je n'ai pas eu l'occasion de vous en parler jusqu'à présent, mais il s'agit du dernier arrivé (depuis Sass 3.3) parmi les types de données. Une map correspond à ce que l'on retrouve dans certains langages de programmation sous le nom de *tableau associatif*, de *hash*, ou encore de *dictionnaire*. On peut voir une map comme une liste dont chaque item serait une paire clef-valeur. Comme c'est tout de suite plus simple avec un exemple, je vous propose de voir comment on pourrait s'en servir pour les couleurs de notre feuille de styles :

```scss
$colors: (
  base: #ff0,
  base-hover: #ff4,
  dark: #222,
  dark-hover: #444,
  light: #eee,
  ultra-light: #fff
);
```

[[information]]
|Notez que j'ai écrit toutes les valeurs en dur pour cet exemple, mais que j'aurais clairement pu utiliser les fonctions `darken()` et `lighten()` pour en calculer certaines.

Comme vous pouvez le voir, une map se définit entre parenthèses, les paires étant séparées par des virgules.

Une fois notre map ainsi constituée, on peut s'en servir avec la fonction `map-get()` ainsi :

```scss
.client{
  ...
  background-color: map-get($colors, dark);
  color: map-get($colors, ultra-light);
  ...
}
```

En terme d'organisation, on se retrouve avec une seule variable pour regrouper toutes nos couleurs, au lieu de la demi-douzaine auparavant, c'est donc plutôt une bonne chose. Cependant, la syntaxe `map-get()` est sacrément lourde à utiliser. C'est pourquoi, certains recommandent d'utiliser une fonction *helper* pour raccourcir celle-ci :

```scss
@function color($name) {
  @return($colors, $name);
}

.client{
  ...
  background-color: color(dark);
  color: color(ultra-light);
  ...
}
```

Simple, clair, organisé. On peut d'ailleurs faire des choses assez cools avec un tel système, pour supporter plusieurs « variantes » d'une même couleur via des sous-map. Si vous voulez en savoir plus, je vous invite à consulter [cet article de Erskine Design](http://erskinedesign.com/blog/friendlier-colour-names-sass-maps/).

En dehors des couleurs, le type map peut aussi se révéler utile pour toute variable de configurations devant contenir des valeurs qui sont utilisées par plusieurs fonction d'une même bibliothèque, par exemple. Mais c'est une bibliothèque, au juste ?

### Utiliser des bibliothèques externes

Depuis le début de ce tutoriel, je n'arrête pas de vous dire que Sass évite de se répéter. C'est tellement vrai que des gens ont créé pour nous des **bibliothèques**, regroupant *mixins* et *fonctions* prêts à l'emploi. Je propose de vous en présenter deux-trois ici, sans entrer dans les détails, car leur documentation est souvent très accueillante.

#### Neat

![Neat](img/neat.png)

**Neat** nous facilite la vie lorsqu'on doit créer des grilles fluides. Comme il s'agit d'une tâche récurrente, et qui nécessite des calculs parfois complexes, c'est l'exemple parfait de ce qu'une bibliothèque Sass peut nous offrir. Pour l'installer, la première chose à faire est d'utiliser `gem` :

```
gem install neat
```

Ensuite, déplacez-vous dans le dossier qui accueille les fichiers SCSS de votre projets et entrez la commande :

```
neat install
```

Cela devrait créer un sous-dossier "neat". Pour utiliser Bourbon, il ne vous plus qu'à l'importer, comme n'importe quel *partial* :

```scss
@import "neat/neat";
```

Neat propose principalement deux mixins : `grid-container` et `grid-column`. Voici un petit exemple pour mieux comprendre :

```html
<div class="conteneur">
  <div class="simple">Lorem ipsum...</div>
  <div class="simple">Lorem ipsum...</div>
  <div class="double">Lorem ipsum...</div>
</div>
```

```scss
@import "neat/neat";

.conteneur {
  @include grid-container;
}
.simple {
  @include grid-column(3);
}
.double {
  @include grid-column(6);
}
```

On obtient ceci :

![Un exemple de grille utilisant Neat](img/exemple-neat.png)

Par défaut, Neat utilise une grille de douze colonnes, séparées par des gouttières de 20 pixels. On peut changer cela, en utilisant une map :

```
grid: (
columns: 4,
gutter: 20px);

.conteneur {
  @include grid-container;
}
.simple {
  @include grid-column(1, $grid);
}
.double {
  @include grid-column(2, $grid);
}
```

Je ne développe pas plus au sujet de Neat, mais je vous invite à [lire la documentation qui est très bien faite](http://neat.bourbon.io/examples/). Sachez qu'on peut aussi se servir de Neat pour créer des grilles *responsive*.

#### Bourbon

![Bourbon](img/bourbon.png)

Les auteurs de Neat se sont avant tout fait connaître avec **Bourbon**, une autre bibliothèque Sass. Au départ, il s'agissait d'un bibliothèque de *mixins* gérant les préfixes propriétaires (préfixes requis par les navigeurs sur certaines propriétés CSS3). Par exemple, un mixin `animation` permettait d'obtenir toutes les variantes préfixées de la propriété `animation`. **Ce n'est plus le cas.** Tout d'abord, parce que l'évolution des navigateurs rend l'utilisation de tels préfixes de moins en moins nécessaire, et ensuite parce des outils comme [Autoprefixer](https://github.com/postcss/autoprefixer), intervenant après Sass, on remplacé les bibliothèques.

Aujourd'hui, Bourbon propose néanmoins plusieurs mixins, fonctions et variables prédéfinis qui peuvent vous permettre de gagner du temps. Il faut le voir comme un couteau-suisse pour Sass.

L'installation se déroule de la même manière que Neat : d'abord `gem install bourbon` puis `bourbon install` afin de pouvoir l'importer avec `@import bourbon/bourbon;`.

Ça ne sert pas à grand chose que je vous liste tout ce que Bourbon propose, mais sachez qu'on y trouve un mixin pour générer des trianges, différentes courbes de timing pour les animations, des listes de polices de caractères fréquentes, etc. La documentation est de toute façon très bien faite, et [je vous invite à vous y référer](bourbon.io/docs/).

#### Susy3

![Susy3](img/susy.png)

**Susy** est un autre système de grille fluide, très chouette à utiliser. L'approche est un peu différente, Susy ne proposant pas de mixins tous prêts mais des fonctions qui effectuent les calculs de largeur à notre place. L'idée est de ne pas imposer une méthode (Neat utilise des flottants dans ces mixins), afin de pouvoir utiliser, si on le souhaite, les derières nouveautés de CSS à ce sujet, *flexbox* et le module de grille de CSS[^css-grid]. La [page de présentation](http://oddbird.net/susy/) est un bon endroit pour commencer.

[^css-grid]: Comment ! Vous ne connaissez pas le [CSS Grid module](https://la-cascade.io/css-grid-layout-guide-complet/) ? Mais c'est le futur, les amis !

### RubySass, LibSass, DartSass

Avant de se quitter, j'aimerai vous parler un peu des différentes implémentations de Sass. Jusqu'à présent, nous avons utilisé RubySass, c'est-à-dire l'implémentation de Sass écrite en Ruby. Il s'agit de l'édition originale et officielle de Sass (au moment où j'écris ces lignes nous en sommes à la version 3.5). Elle a l'avantage d'être assez facile à utiliser via la commande `sass --watch` qui accepte pas mal de paramètres. D'ailleurs, à ce sujet, saviez-vous que `sass --watch` accepte un paramètre `-t` qui permet de décider quel  d'intentation doivent avoir les fichiers CSS compilés ? `t` peut prendre la valeur *nested* (sa valeur par défaut), *compressed*, *compact* ou *expanded* (les noms parlent d'eux-même):

```
sass --watch sass:stylesheets -t compressed
```

Cependant, RubySass présente l'inconvénient d'être assez lent. À notre échelle ce n'est pas un véritable problème, mais ça peut le devenir sur de gros projets. C'est pour cela qu'est apparue LibSass, un portage de Sass en C/C++. LibSass est pensée pour être extrêmement rapide mais ne propose pas, de base, une interface en ligne de commande, comme RubySass. À la place, on a la possibilité de faire appel à LibSass à partir d'un grand nombre de langages de programmation. Cela peut sembler un peu compliqué, mais c'est ce qui va permettre par exemple d'appeler Sass de puis Javascript. Je ne choisis évidemment pas cet exemple par hasard. Vous avez peut-être déjà entendu parlé de Node.js, qui permet d'exécuter du code Javascript côté serveur. Pour plusieurs raisons, la première étant que les développeurs front-end connaissent bien Javascript, Node est aussi devenu l'outil standard pour scripter toutes les actions répétitives du front-end comme la minification du JS et du CSS, l'utilisation d'Autoprefixer, la génération des sprites et vous l'aurez compris, la compilation Sass. C'est pourquoi l'implementation la plus populaire de LibSass reste node-sass.

Je ne vais pas développer ici l'automatisation des tâches du front-end avec Node, déjà parce que je ne connais pas bien ce sujet, et ensuite parce qu'il est suffisemment vaste pour qu'un cours entier lui soit destiné. Si RubySass était particulièrement pratique pour apprendre à se servir de Sass, node-sass est donc quant à lui l'outil idéal en préprod.

[[question]]
|Mais, n'est-ce pas problématique pour les développeurs de Sass de devoir maintenir RubySass et LibSass en même temps ?

C'est une excellente question, à se demander si on ne vous l'a pas soufflée. :-°  
En effet, le fait de devoir maintenir deux versions n'est pas sans inconvéniants. Les nouvelles fonctionnalités apparaissent d'abord dans RubySass, et il faut attendre pour qu'elles soient disponibles dans LibSass. En plus, il existe, ou il a existé, des comportements légèrements différents entre les deux implémentations.

C'est ainsi qu'à la mi-2016, la développeuse Natalie Weizenbaum a annoncé [le lancement de DartSass](http://sass.logdown.com/posts/1022316-announcing-dart-sass), destiné à être la nouvelle implémentation officielle de Sass, remplaçant à le long terme RubySass. Le choix du langage Dart, soutenu par Google, n'est pas anodin. Dart permet une exécution rapide, proche de la vélocité de LibSass, tout en restant pratique pour implémenter de nouvelles fonctionnalités pour les développeurs de Sass, comme l'était Ruby. Surtout, Dart peut être exporté vers Javascript, facilitant l'intégration avec Node. À l'heure où j'écris ces lignes, DartSass est encore en beta, mais il semble bien que ce soit là que se trouve le futur de ce qui est désormais, je l'espère, votre préprocesseur favori. :)

### Conclusion

Chapitre assez chargé, n'est-ce pas ? On a vu comment se servir du type map, on a regardé du côté des bibliothèques externes et on a même évoqué (rapidement) LibSass. Avant de se quitter, je vous laisse avec quelques bonnes adresses pour se tenir au courant des nouveautés auour de Sass. En plus du [blog officiel de l'équipe qui développe Sass](http://sass.logdown.com/), je vous conseille de jeter régulièrement un oeil du côtés de sites spécialisés comme [SitePoint](https://www.sitepoint.com/?s=Sass), [The Treehouse](http://blog.teamtreehouse.com/?s=Sass) ou [CSS Tricks](https://css-tricks.com/?s=Sass). Ces trois là sont de vraies mines d'or. À l'époque où j'ai débuté l'écriture de ce tutoriel, [The Sass Way](https://thesassway.com) était la référence en termes d'articles. Malheureusement, il ne semble pas avoir été mis à jour depuis 2015 (oui, ce tutoriel a mis pas mal de temps à voir le jour :-°). Ce qui me permet de bien insister sur le fait que l'écosystème Sass est en constant changement. Si Sass reste à peu près stable, les bibliothèques qui l'entourent ne cessent de changer et d'évoluer. Un article qui a 2 ans peut donc être déjà obsolète. Ah, et pour finir, [n'oubliez pas la doc !](http://sass-lang.com/documentation/file.SASS_REFERENCE.html)

## Conclusion

Et voilà, vous êtes arrivés à la fin de ce tuto ! Je n'exclus pas quelques changements par la suite, si j'ai le temps d'améliorer certains exemples, ou si certaines infos deviennent obsolètes (et cela risque d'arriver assez vite). S'il vous reste des interrogations, n'hésitez pas à les poster sur le forum Web. Pour finir, je remercie évidemment toutes les personnes qui ont suivi ce tuto en beta et m'ont apporté leurs conseils.
