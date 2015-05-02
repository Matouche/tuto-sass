Notre fichier `titres.scss` pose un problème : il est lui aussi
traduit à part et transformé en un fichier `titres.css` par Sass.
Pourtant, ce fichier ne sert à rien puisque nous voulons juste inclure
le code dans `style.scss`.

Pour éviter cela, nous devons le transformer en *feuille partielle* ou
« partial » en anglais. Une feuille partielle est un feuille de style
qui ne sert qu’à être incluse dans d’autre feuilles de styles. Pour
transformer une feuille de style en feuille partielle, il suffit de la
renommer en ajoutant un `_` (tiret bas, ou underscore) devant :
`titres.scss` devient donc `_titres.scss`. C’est tout ! Vous
n’avez même pas besoin de changer `@import 'titres';` car Sass devine
qu’il faut rajouter un `_` (encore un caractère économisé !).

Personnellement, j’ai l’habitude (et je ne suis pas le seul) de créer un
sous-dossier `partials` dans le dossier `sass`, comme cela,
les feuilles partielles sont à part. Je les importe ensuite comme ceci
(ce code est tiré d’un de mes fichiers) :

```css
@import 'partials/defaut';
@import 'partials/figure';
@import 'partials/aside';
@import 'partials/slideshow';
```