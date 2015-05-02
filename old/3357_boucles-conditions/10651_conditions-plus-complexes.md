Passons à des choses plus complexes… On veut dire ceci à Sass :
```text
SI $var == 'bidule'
  Appliquer color: red;
SINON
  Appliquer color: blue;
```
La directive Sass pour dire « SINON » est `@else`. Elle s'utilise ainsi :
```scss
p{
  @if $var == 'bidule'{
    color: red;
  }
  @else{
    color: blue;
  }
}
```
Donc, si on a `$var: 'chose';` alors le code généré sera :
```css
p {
  color: blue;
}
```

Encore plus compliqué. On veut que Sass comprenne ceci :
```text
SI $var == 'bidule'
  Appliquer color: red;
SINON SI $var == 'machin'
  Appliquer color: green;
SINON
  Appliquer color: blue;
```
Pour dire « SINON SI » à Sass, on va employer `@else if`, comme cela :
```scss
p{
  @if $var == 'bidule'{
    color: red;
  }
  @else if $var == 'machin'{
    color: green;
  }
  @else{
    color: blue;
  }
}
```
Voilà pour la théorie, voyons maintenant à quoi ça sert…