Observez ce code :
```scss hl_lines="3 8"
@mixin tablette{
  @media (max-width: 950px){
    @content;
  }
}

h1{
  font-size: 3.5rem;
  @include tablette{
    font-size: 2.5rem; 
  }
}
```
Deux détails à remarquer :

+ le mixin `tablette` contient la directive `@content`, que nous n'avons pas encore vue ;
+ l'inclusion du mixin est suivie d'un bloc de code entre accolades.

En fait, lorsque Sass voit un `@content` à l'intérieur d'un mixin, il s'attend à ce qu'on lui passe un bloc de code lors de l'inclusion. Ce bloc de code va prendre la place de la directive `@content` partout où elle est présente dans le mixin inclus. Dans notre exemple, le code CSS généré sera :
```css
h1 {
  font-size: 3.5rem;
}
@media (max-width: 950px) {
  h1 {
    font-size: 2.5rem;
  }
}

```
Comme vous pouvez le constater, `@content` a effectivement été remplacé par `font-size: 2.5rem`.

Si on avait passé plusieurs règles à notre mixin, elles auraient toutes été insérées. On aurait même pu ajouter un niveau d'imbrication :
```scss
h1{
  @include tablette{
    strong{
      color:blue;
}}}
```

Je pense que vous avez compris le principe. C'est très pratique pour les media-queries. Plutôt que de les répéter plusieurs fois, on peut créer un mixin par appareil (mobile, phablette, tablette, écran large,etc.). Ainsi, le code devient plus clair : pour chaque élément (menu, en-tête, article, etc.), on établit, à l'intérieur de son bloc, les styles adaptés à chaque taille d'écran.