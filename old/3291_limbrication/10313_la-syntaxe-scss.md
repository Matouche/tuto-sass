La syntaxe par défaut de Sass est le *SCSS* (pour « Sassy CSS »). On retrouve dans ce nom les lettres CSS. En effet, la syntaxe SCSS se base sur la syntaxe du CSS. En fait, tout code CSS est compatible SCSS. Cela veut dire que, si vous avez déjà des fichiers CSS, vous n'avez qu'à leur mettre l'extension `.scss` pour qu'il soient lisibles par Sass ! Vous pourrez ensuite les « améliorer » (ou plutôt les ré-organiser) en utilisant les fonctionnalités propre à Sass. De plus, si vous travaillez avec quelqu'un ne connaissant pas Sass, tout son travail en CSS sera directement importable dans un fichier `.scss`, sans aucune modification. Elle est pas belle, la vie ?

Pour commencer, je vous propose de créer un nouveau fichier, dont le contenu sera le suivant :
```scss
header {
  background-color: skyblue;
}
header h1 {
  color: orange;
  font-style: italic;
  font-size: 20px;
}
p {
  font-size: 16px;
}
p.introduction {
  font-size: 18px;
}
```

Enregistrez-le dans le dossier `sass` de votre projet sous le nom `style.scss`. Comme vous le voyez, je ne vous ai pas menti : c'est du CSS pur. Nous verrons par la suite comment utiliser l'imbrication pour mieux organiser ce code.

Mais, tout d'abord, voyons comment demander à Sass de transformer nos fichiers `.scss` en CSS.