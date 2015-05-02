Vous avez sans doute remarqué que, dans un design, certaines valeurs reviennent souvent. Par exemple, il y a certaines couleurs qui sont utilisées par de nombreux éléments du design. Observez ce code :

```css hl_lines="2 5 8 12"
header{
  background-color: #ff8000;
}
nav{
  border: solid 2px #ff8000;
}
a{
  color: #ff8000;
  text-decoration: underline;
}
strong{
  color: #ff8000;
  font-weight: bold;
}
```

Comme vous le voyez, la couleur `#ff8000` revient très souvent. Maintenant, imaginez que vous souhaitez changer cette couleur. Vous devez alors effectuer un rechercher-remplacer dans toute votre feuille de style. Mais il y a bien plus simple, vous pouvez utiliser une *variable* !

Une variable, c'est un peu comme une enveloppe. On stocke à l'intérieur une valeur (la couleur `#ff8000` dans notre exemple) et on met une étiquette (c'est le nom de la variable). Regardez ce code :

```scss hl_lines="1 4 7 10 14"
$macouleur: #ff8000; //Déclaration de la variable

header{
  background-color: $macouleur;
}
nav{
  border: solid 2px $macouleur;
}
a{
  color: $macouleur;
  text-decoration: underline;
}
strong{
  color: $macouleur;
  font-weight: bold;
}
```

Au début du code, on *déclare* la variable `$macouleur` (les noms de variable commencent tous par le symbole `$`) et on lui donne la valeur `#ff8000`. Ensuite, lorsqu'on en a besoin, on écrit juste le nom de la variable et Sass va chercher la valeur correspondante. Maintenant, pour changer la couleur, on n'a qu'à modifier la déclaration de la variable.

Vous remarquez que j'ai écris dans le code un *commentaire* précédé  par `//`. C'est un commentaire spécifique à Sass, qui n'apparaitra pas dans le fichier `.css`.

Actuellement vous n'en verrez pas forcément l’intérêt, mais je souhaite tout de même vous parler de `!default`. Lorsque vous attribuez une valeur à une variable, il est possible que cette variable existe déjà. Dans ce cas là, la nouvelle valeur remplace l'ancienne. C'est le comportement par défaut. Pour que la variable garde sa valeur d'origine si elle existe déjà, alors il faut ajouter `!default`, comme ceci :

```scss hl_lines="2"
$macouleur: orange;
$macouleur: skyblue !default;
h1{
  color: $macouleur; //color: orange;
}
```

Je n'insiste pas trop là-dessus, car je ne vois pas d'application concrète à votre niveau. Sachez juste que cela existe.