# En résumé
+ La boucle `@for` est utile pour **répéter plusieurs fois un même code en changeant uniquement un nombre** et s'utilise ainsi : `@for $i from 1 through 4{...}`.
+ La boucle `@each` permet de **répéter un même bout de code en parcourant une liste d'items**, on s'en sert comme ceci : `@each $aliment in frites, tomate, yaourt, raviolis{...}`.
+ On peut **faire des tests** sur des variables (avec == et !=) et n'appliquer certaines règles que **si une condition est respectée** en utilisant `@if`.
+ On peut faire des **conditions plus complexes** avec `@else if`(sinon si) et `@else` (sinon).

Je ne vous ai volontairement pas parlé de la boucle `@while` dont l'utilité est plus que limitée. Si vous pensez en avoir besoin un jour, je vous invite à consulter [la doc Sass](http://sass-lang.com/documentation/file.SASS_REFERENCE.html#_12).

Dans le prochain chapitre, il sera question d'un type de donnée qui a récemment fait son apparition et qui est prometteur, Map.