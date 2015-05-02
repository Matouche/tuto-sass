Un préprocesseur qu’est-ce que c’est ? Voyons voir ce que nous dit Wikipédia
:

> En informatique, un préprocesseur est un programme qui procède à
> des transformations sur un code source, avant l’étape de traduction
> proprement dite (compilation ou interprétation).
Source: Wikipédia

Vous n’y voyez toujours pas grand chose ? Je vais tenter de vous expliquer
ça plus simplement. Un préprocesseur est un programme jouant le rôle d’une
moulinette : on lui donne du code source, et il génère du code source, modifié.
Dans le cas d’un préprocesseur CSS, on donne du code à la moulinette et elle
nous sert des fichiers CSS qui pourront être compris par un navigateur.

Sass est un préprocesseur CSS qui génère (on dit aussi compile) du code CSS à partir de fichiers
contenant du code spécifique à Sass. Ces fichiers ont l’extension `.scss`.

Je ne vois pas à quoi cela peut servir…

En fait, un préprocesseur rajoute des fonctionnalités pour optimiser son code, pour le rendre plus lisible et plus facile à modifier ultérieurement.

Je vous donne un exemple : vous vous rendez compte qu'une propriété doit être modifiée. Oui, sauf que votre fichier CSS fait plus de 600 lignes. Comment la retrouver ? Surtout si vous avez l'habitude de rajouter vos modifications à la fin du fichier, sans aucune organisation !

La devise de de Sass pourrait être "Don't Repeat Yourself" (ne vous répétez pas). Pour cela, Sass ajoute des fonctionalités qui permettent de mieux gérer son code. Quelques exemples :

+ Vous avez une valeur (dimension, couleur,…) qui se répète souvent dans votre feuille de style ? Stockez-la dans une variable !
+ Vous en avez marre de répéter les mêmes sélecteurs légèrement modifiés des dizaines de fois ? Alors l'imbrication de règles est faite pour vous.
+ Vous pensez que votre code pourrait être divisé en plusieurs fichiers ? Vous pouvez importer des fichiers qui seront ensuite insérés par Sass dans la feuille de style finale.
+ Vous répétez souvent un même groupe de propriétés ? Avec les mixins, vous n'aurez qu'à les écrire une seule fois.

Et ceci n'est qu'un petit aperçu de ce qui vous attend dans ce tutoriel !