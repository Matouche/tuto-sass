Il me reste à vous parler encore de quelques comportements plus ou moins
complexes de Sass. En premier lieu, vous savez déjà que l’on peut
spécifier n’importe quel sélecteur après `@extend`. N’importe lequel ?
Pas tout à fait. En effet, on ne peut pas actuellement indiquer un
*sélecteur « imbriqué »*, c’est-à-dire qu’on ne peut pas utiliser les
sélecteurs de type `X Y`, `X>Y`, `X+Y` et `X~Y` (les deux derniers sont
des sélecteurs d’adjacence directe et indirecte).

Continuons notre exploration par la *fusion des sélecteurs*. Regardez
cet exemple :

```scss hl_lines="1 4"
#div1 #div2 em{
  @extend strong;
}
#div3 #div4 strong{
  font-weight: bold;
}
```

Comment Sass va-t-il traduire ceci ? J’ai mis en évidence 
les deux séquences à fusionner. Comme elles n'ont pas d’éléments 
en commun, Sass créera deux nouveaux sélecteurs :
le premier avec les parents de `strong` avant les parents de
`em` et le second avec les parents de `em` avant les parents
de `strong`. Voici donc le résultat :

```css hl_lines="2-3"
#div3 #div4 strong, 
#div3 #div4 #div1 #div2 em, 
#div1 #div2 #div3 #div4 em{
  font-weight: bold;
}
```

Maintenant, que se passe-t-il lorsque les éléments parents ont au
moins un élément en commun ? Faisons le test avec cet exemple :

```scss hl_lines="1 4"
#commun #div1 em{
  @extend strong;
}
#commun #div2 strong{
  font-weight: bold;
}
```

Comme vous le voyez, les sélecteurs ont l’élément `#commun` en commun.
Sass, durant la traduction en CSS, fusionnera les deux `#commun` :

```css hl_lines="2-3"
#commun #div2 strong, 
#commun #div2 #div1 em, 
#commun #div1 #div2 em{
  font-weight: bold;
}
```

Voilà, c’est tout pour la fusion de sélecteurs.