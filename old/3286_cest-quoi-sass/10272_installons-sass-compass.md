Convaincus ? Alors lançons nous dans l'installation de Sass.

Mais avant tout, il faut que vous disposiez de Ruby. Pourquoi ? Parce que Sass est programmé en Ruby et qu'il nécessite donc l'interpréteur Ruby pour fonctionner. Sous Linux, vous pouvez l'installer avec votre gestionnaire de paquets favoris. Sous Mac OS X, vous pouvez sauter cette étape : Ruby est installé par défaut.

Enfin, sous Windows, vous pouvez télécharger l'installateur depuis [le site officiel](http://rubyinstaller.org/downloads/). Durant l'installation, pensez à cocher la case "Add Ruby executables to your PATH".

Maintenant, nous pouvons installer Sass et Compass avec `gem` (le gestionnaire de paquets inclus avec Ruby). Rassurez-vous, ce n'est pas difficile. Vous devez seulement lancer un terminal (pour Windows, lancez le programme `cmd` depuis le menu Démarrer) et entrer : `gem install compass` (amis Unixiens, n'oubliez pas `sudo` !).

Vous vous demandez pourquoi on installe Compass directement ? Et bien, comme cela, `gem` installe Compass et Sass (car il sait que Compass a besoin de Sass pour fonctionner). Et j'ai l'honneur de vous dire que votre installation de Sass et Compass est terminée ! Si, si !