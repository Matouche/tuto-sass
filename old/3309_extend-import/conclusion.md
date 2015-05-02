# En résumé

+ Pour qu'un élément X *hérite des propriétés* d'un élément Y, on utilise la directive `@extend`. Sass ajoutera le sélecteur X à chaque fois qu'apparait le sélecteur Y. 
+ Les *« classes fictives »* (qui commencent par `%`) n'apparaissent pas dans le fichier `.css` car elles ne servent qu'à l'héritage. Elles génèrent moins de code qu'un mixin.
+ On ne peut pas hériter d'un *sélecteur « imbriqué »*. Si un élément « imbriqué » hérite d'un autre élément, Sass appliquera la *fusion des sélecteurs*.
+ Sass améliore la directive `@import` (qui permet d'*importer une feuille de style*) en ajoutant directement le code dans le fichier généré (ce qui réduit le nombre de requêtes). On a pas besoin de préciser l'extension du fichier.
+ Une *feuille partielle* ne sert qu'à être importée dans une autre feuille de style. Son nom commence par un `_`.