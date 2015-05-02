# En résumé

+ Un mixin est un *bout de code réutilisable*, que l’on crée avec la directive `@mixin` suivie du nom du mixin et du code entre accolades.
+ Pour *inclure un mixin*, on doit utiliser la directive `@include` suivie du nom du mixin.
+ On peut rendre un mixin paramétrable avec des *arguments*, des variables à l'intérieur du mixin, que l'on ajoute entre parenthèses.
+ On peut *nommer les arguments* lors de l'inclusion pour un code plus lisible, et on peut indiquer à Sass que l'on attend un *nombre variable d'arguments* en ajoutant `...`.
+ Pour *passer un bloc de contenu* à un mixin, on doit l'ajouter entre accolades. Dans le code généré, le bloc de contenu viendra remplacer la directive `@content`. C'est très utile avec les media-queries.

Ce chapitre était très dense, entrainez-vous pour voir si vous êtes bien au point avec les mixins. Le chapitre suivant sera, je l'espère, moins difficile.