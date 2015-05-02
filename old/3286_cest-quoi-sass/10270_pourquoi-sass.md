En effet, pourquoi rajouter une couche ? Après tout, le langage CSS est assez simple pour être codé à la main. En fait, un préprocesseur rajoute des fonctionnalités pour optimiser son code, pour le rendre plus lisible et plus facile à modifier ultérieurement.

Je vous donne un exemple : vous vous rendez compte qu'une propriété doit être modifiée. Oui, sauf que votre fichier CSS fait plus de 600 lignes. Comment la retrouver ? Surtout si vous avez l'habitude de rajouter vos modifications à la fin du fichier, sans aucune organisation !

La devise de de Sass pourrait être *"Don't Repeat Yourself"*  (ne vous répétez pas). Pour cela, Sass ajoute des fonctionalités qui permettent de mieux gérer son code. Quelques exemples :

* Vous avez une valeur (dimension, couleur,...) qui se répète souvent dans votre feuille de style ? Stockez-la dans une *variable* !
* Vous en avez marre de répéter les mêmes sélecteurs légèrement modifiés des dizaines de fois ? Alors *l'imbrication de règles* est faite pour vous.
* Vous pensez que votre code pourrait être divisé en plusieurs fichiers ? Vous pouvez *importer des fichiers* qui seront ensuite insérés par Sass dans la feuille de style finale.
* Vous répétez souvent un même groupe de propriétés ? Avec les *mixins*, vous n'aurez qu'à les écrire une seule fois.

Et ceci n'est qu'un petit aperçu de ce qui vous attend dans ce tutoriel !