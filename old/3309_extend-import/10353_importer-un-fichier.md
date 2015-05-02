Vous connaissez certainement la directive CSS `@import` qui permet
d’importer une feuille de style. Cela permet de diviser ses styles en
plusieurs fichiers pour mieux s’organiser. Cependant, cette directive
augmente le nombres de requêtes vers le serveur et ralentit donc le
chargement de la page. Sass améliore le fonctionnement de `@import` en
ajoutant directement le contenu des feuilles importées. Ainsi, le
fichier final regroupera l’ensemble des règles et il n’y aura qu’une
seule requête. Vous voulez tester ? Créez un fichier nommé
`titres.scss` dans le dossier `sass` de votre projet. A
l’intérieur, nous allons styliser nos titres :

```scss
//Fichier titres.scss
h1{font-size: 2.5rem;}
h2{font-size: 2rem;}
h3{font-size: 1.5rem;}
```

Maintenant, on peut l’importer dans notre fichier `style.scss` :
```scss
//Fichier style.scss
@import 'titres';
```

Comment cela, j’ai oublié l’extension ! Sass n’a pas besoin de
l’extension pour importer le fichier (hop, 5 caractères économisés ;-)).
Si vous regardez dans le fichier `stylesheets/style.css`, vous
verrez que `@import 'titres';` a bien été remplacé par le contenu du
fichier `titres.scss`.