L'*imbrication de règles* est une fonctionnalité fondamentale de Sass. Elle s'inscrit vraiment dans le principe « Don't Repeat Yourself ».

Observez le début du fichier `style.scss` :
```scss hl_lines="1 4"
header {
  background-color: skyblue;
}
header h1 {
  color: orange;
  font-style: italic;
  font-size: 20px;
}

```
On répète deux fois `header`. Ce n'est pas un problème à cette échelle là, mais imaginez que nous utilisions des classes au nom compliqué, situées à l'intérieur d'autres classes au nom compliqué. On n'a pas forcément envie de répéter plusieurs fois les mêmes sélecteurs. Avec Sass, ce code peut être écrit de la manière suivante en utilisant l'imbrication de règles :

```scss
header {
  background-color: skyblue;
  h1 {
    color: orange;
    font-style: italic;
    font-size: 20px;
  }
}
```

Avec l'imbrication, nous n'avons écrit `header` qu'une seule fois et nous avons mis le bloc `h1` *à l'intérieur* du bloc `header`. Sass comprend lors de la traduction en CSS que les règles ne doivent concerner que les balises `<h1>` situées à l'intérieur d'une balise `<header>`. On obtient bien la même chose qu'avec le code précédent. Cependant, vous remarquez que l'on a regroupé toutes les règles concernant le header dans un même bloc. Ainsi, le code est plus lisible, car il est mieux *organisé*.

Sachez aussi que l'on peut imbriquer des propriétés CSS. Par exemple, nous pouvons mettre dans un même bloc les propriétés `font-size` et `font-style`, comme ceci (faites bien attention au deux-points après `font`) :

```scss hl_lines="3"
h1 {
  color: orange;
  font: {
    style: italic;
    size: 20px;
  }
}
```