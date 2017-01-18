Rassurez-vous, votre grande-tante est encore en vie ! Ici, ce sont des
classes CSS qui héritent. L’*héritage de classe* est le fait qu’un
élément peut *hériter* des propriétés d’un autre élément. Par exemple,
on peut décider que la classe `.alerte` hérite de toutes les propriétés
appliquées à la classe `.message`. On peut le faire directement dans le
HTML, en attribuant les deux classes à notre élément :

```html
<p class="message alerte">Au secours !</p>
```

Cependant, cette technique est lourde et vous ne devez jamais oublier de
rajouter la classe `.message`.

Sass vous permet de dire que X élément hérite forcément des propriétés
de l’élément Y. Pour cela, on utilise la directive `@extend`, comme ceci :

```scss hl_lines="5"
.message{
  border: 2px solid black;
}
.alerte{
  @extend .message;
  color: red;
}
```

La classe `.alerte` hérite donc des propriétés de la classe `.message`
(bon, elle hérite seulement d’une propriété, mais c’est pour l’exemple
;-)).

Comment ça marche ? Eh bien, c’est très simple, Sass rajoute `.alerte`
partout où apparait `.message`. Le code CSS généré sera donc :

```css hl_lines="1"
.message, .alerte{
  border: 2px solid black;
}
.alerte{
  color: red;
}
```

Plus complexe : `.message` apparait plusieurs fois dans le fichier (je
n’ai pas fait l’imbrication pour que vous compreniez bien, mais sachez
que cela marcherait aussi) :

```scss
.message{
  border: 2px solid black;
}
.message:hover{
  font-style: italic;
}
```

La classe `.alerte` sera ajoutée par Sass dans chaque sélecteur contenant
`.message`:

```css hl_lines="1 4"
.message, .alerte{
  border: 2px solid black;
}
.message:hover, .alerte:hover{
  font-style: italic;
}
```

Encore plus fort : on peut faire des héritages multiples, “en cascade”.
Regardez le code suivant :

```scss hl_lines="5 9"
.message{
  border: 2px solid black;
}
.alerte{
  @extend .message;
  color: red;
}
.danger{
  @extend .alerte;
  font-size: xx-large;
}
```

La classe `.danger` hérite de la classe `.alerte` qui hérite de la
classe `.message` ! On obtient donc logiquement ce code CSS :

```css hl_lines="1 4"
.message, .alerte, .danger{
  border: 2px solid black;
}
.alerte, .danger{
  color: red;
}
.danger{
  font-size: xx-large;
}
```
