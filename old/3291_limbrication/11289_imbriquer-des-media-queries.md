Depuis quelque temps et la démocratisation du Responsive Web Design (RWD pour les intimes), les media-queries ont le vent en poupe. Justement, saviez-vous que Sass permet d'imbriquer la directive `@media` ? C'est tout simple :
```scss hl_lines="3"
body{
  width: 360px;
  @media screen and (min-width: 480px){
    width: 480px;
  }
}
```
Cela donnera en CSS :
```css
body {
  width: 360px;
}
@media screen and (min-width: 480px) {
  body {
    width: 480px;
  }
}
```

Voilà, pas grand chose à dire là-dessus, mais c'est très utile pour regrouper toutes les règles s'appliquant à un même élément dans le même bloc.