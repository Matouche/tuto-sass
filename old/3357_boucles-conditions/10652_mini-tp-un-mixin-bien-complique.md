# Prérequis
Passons maintenant à la pratique, et j'ai décidé de vous laisser un peu libres. Vous vous souvenez du mixin `tablette` que nous avions codés il y a quelques chapitres ? Je vous avais suggéré de faire un mixin par appareil (mobile, ordinateur de bureau, etc.). Il y a mieux. Nous allons créer un unique mixin `breakpoint` qui accepte un argument (le nom de l'appareil) et qui génère la *media-query* correspondante.
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

# La solution
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

# Un peu mieux…
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