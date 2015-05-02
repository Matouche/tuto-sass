Je souhaite revenir un peu sur les *chaînes de caractères* et, plus précisément, je veux vous parler de la *concacténation* et de l'*interpolation*. Rassurez-vous, derrière ces noms barbares se cachent deux fonctionnalités très simples à utiliser de Sass.

La *concaténation*, c'est le fait de mettre plusieurs chaînes de caractères bout à bout. Pour concaténer, on utilise tout simplement le symbole `+`, comme ceci :

```scss hl_lines="3"
$fin: space;
p{
  font-family: mono + $fin;// = monospace
}
```

Cependant, je pense que l'*interpolation* vous sera plus utile. L'interpolation permet d'insérer le contenu d'une variable là où vous le souhaitez. Vous aimeriez insérer une variable dans un sélecteur ou dans le nom d'une propriété ? Avec l'interpolation, c'est possible ! On utilise la syntaxe `#{...}`, comme dans cet exemple :

```scss hl_lines="2"
$intro: ".introduction";
p#{$intro}|{ // p.introduction
  font-size: 2em;
}
```

Dans certains cas, utiliser l'interpolation vous sera très utile (pour ne modifier qu'un bout du sélecteur par exemple). Alors, retenez bien sa syntaxe.