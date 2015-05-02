La dernière chose dont je souhaite vous parler va vous sembler peut-être anecdotique. Pourtant cette technique peut vous être utile et je souhaite donc vous la montrer rapidement. Il s'agit ici de passer à un mixin des propriétés qui seront rajoutées au code généré.

Un exemple vaut mille mots, regardez donc le mixin `joli-hover` que j'ai un peu modifié :

```scss hl_lines="5"
@mixin joli-hover($normal, $hover){
  color: $normal;
  &:hover{
    color: $hover;
	@content;
  }
}
```
Vous avez sans doute remarqué que j'ai ajouté `@content`. Sass remplacera cette ligne par les propriétés que l'on passera au mixin.

Pour passer des propriétés à notre mixin, on doit les ajouter entre accolades (après les arguments) :

```scss hl_lines="3"
a{
  @include joli-hover(skyblue, blue){
    text-decoration: none;
  }
}
```

Désormais, `text-decoration: none;` va remplacer `@content;` dans le code généré :

```css hl_lines="6"
a{
  color: skyblue;
}
a:hover {
  color: blue;
  text-decoration: none;
}
```

L'avantage, c'est que vous pouvez passer autant de propriétés que vous voulez entre les accolades, et elles seront inclues à la place de `@content`. Si vous ne mettez pas d'accolades, Sass ignorera `@content`.