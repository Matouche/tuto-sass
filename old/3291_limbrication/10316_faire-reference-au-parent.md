Il me reste encore à vous expliquer comment *faire référence au parent*. Regardez la deuxième partie de notre fichier `style.scss` :

```scss
p {
  font-size: 16px;
}
p.introduction {
  font-size: 18px;
}
```

Comment utiliser l'imbrication ici ? Vous pensez que le code suivant marchera ?

```scss
p{
  font-size: 16px;
  .introduction{
    font-size: 18px;
  }
}
```

En réalité, on obtiendra le sélecteur `p .introduction` (notez l'espace), qui sélectionne les éléments ayant la classe `.introduction` situés à l'intérieur d'un paragraphe, au lieu de sélectionner les paragraphes ayant la classe `.introduction`.

Comment faire alors ? Eh bien, en utilisant le sélecteur `&`, qui fait référence au parent. Ainsi, ce code fonctionnera :

```css
p{
  font-size: 16px;
  &.introduction{
    font-size: 18px;
  }
}
```

Compris ? On peut aussi s'en servir avec une *pseudo-classe* (`&:hover` sélectionne le parent au survol) ou même le mettre à la fin(`section &` sélectionne le parent lorsqu'il est contenu dans une balise `<section>`). Je vous l'avais dit, c'est magique Sass !