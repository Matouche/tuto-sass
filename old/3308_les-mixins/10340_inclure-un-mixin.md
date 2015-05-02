Pour l'instant, votre mixin ne sert à rien. Il faut désormais l'inclure dans le bloc concernant nos liens. Pour cela, on se sert de la directive `@include` suivie du nom du mixin, comme cela :

```scss hl_lines="2"
a{
  @include joli-hover;
}
```

Maintenant, Sass, saura qu'il faudra inclure le mixin `joli-hover` avant de traduire en CSS. Le résultat final est bien celui attendu :

```css
a{
  color: orange;
} 
a:hover{
  color: orangered;
}
```

On peut inclure plusieurs mixins à la fois, comme dans cet exemple (remarquez l'imbrication de propriétés) :

```scss hl_lines="8 9"
@mixin gros{
  font:{
    size: 20px;
    family: "Times New Roman", Times, serif;
  }
}
h1{
  @include joli-hover;
  @include gros;
}
```

On a inclus deux mixins dans le bloc `h1` : `joli-hover` et `gros`.
Sass génerera le CSS suivant :

```css
h1{
  color: orange;
  font-size: 20px;
  font-family: "Times New Roman", Times, serif;
}
h1:hover{
  color: orangered;
}
```

Je pense que vous comprenez maintenant ce que je veux par "ré-utilisabilité". Notre mixin `joli-hover` est utilisé deux fois (pour les liens et les titres 1), mais nous n'avons pas eu besoin de recopier plusieurs fois son contenu. En plus, en donnant des noms explicites à vos mixins, vous rendez votre code plus lisible (mon `h1` a un `joli-hover` et est `gros`).