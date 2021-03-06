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

Enregistrez-le dans le dossier `sass` de votre projet sous le nom `style.scss`. Comme vous le voyez, il s'agit pour l'instant de CSS pur. Nous verrons par la suite comment utiliser l'imbrication pour mieux organiser ce code.

Comme nous avons créé notre projet avec Compass, nous pouvons aussi le compiler avec Compass. Pour cela, vous devez vous placer à l'intérieur du dossier de votre projet (pour nous, c'est le dossier `test-sass`). Ouvrez un terminal dans ce dossier (sous Windows 7, ||Maj|| + Clic-Droit et choisir *« Ouvrir une fenêtre de commandes ici »*). Compass propose deux commandes pour ce que l'on veut faire.

La première commande est `compass compile`. Si vous l'entrez, vous devriez voir s'afficher `create stylesheets/style.css`. Comme vous pouvez vous en douter, cette ligne indique que le fichier `style.css` a été généré. Vous pouvez vérifier dans le dossier `stylesheets`, le fichier est là.

Cependant, il y a mieux. J'ai l'honneur de vous présenter la commande magique `compass watch` ! Si vous entrez cette commande, alors Compass « écoutera » le projet. Cela veut dire que dès que vous aurez fait une modification sur un fichier `.scss`, alors Compass génèrera automatiquement le fichier `.css`. Vous n'aurez qu'à actualiser votre navigateur pour voir les modifications. Franchement, que demander de plus !