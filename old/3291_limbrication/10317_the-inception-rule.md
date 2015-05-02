L'imbrication, c'est bien. Mais il ne faut pas en abuser. Vous allez être vite tentés de copier la structure du fichier HTML dans la feuille de style. Comme ceci (je n'écris pas le code entier, mais vous devriez comprendre l'idée) :

```
body{
  #global{
    header{
  nav{
        ul{
          &.sous-menu{
            li{
              a{
                &:hover{...}
              }
            }
          }
        }
      }
    }
  }
}
```

En CSS, on obtient alors le code suivant : 
```css
body #global header nav ul.sous-menu li a:hover{
  ...
}
```

Vous trouvez ça lisible ? Moi pas ! En plus d'être très laid, ce code pose des problèmes de performances à cause des sélecteurs à ralonge. Mais, surtout, ce code n'est pas du tout maintenable ! Si on modifie un peu le HTML, la feuille de style doit forcément être modifiée. Autant revenir au temps où on stylisait directement en HTML !

C'est là qu'intervient ce que l'on appelle *« The Inception Rule »*. La règle est la suivante : *n'imbriquez pas plus de quatre niveaux différents*. Cette règle peut vous sembler contraignante, mais je vous invite à la suivre. Elle vous empêchera de faire des feuilles de style trop spécifiques, et elles seront donc plus facilement maintenables. On peut parfois dépasser cette limite, comme lorsqu'on ajoute de l'intéraction (avec `:hover` par exemple). Mais essayez le plus possible de ne pas trop allonger vos sélecteurs. Il y a souvent moyen de faire plus court.