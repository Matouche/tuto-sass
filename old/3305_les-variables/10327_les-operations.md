Avec les variables, nous pouvons faire des calculs ! Sass propose 5 opérations :

+ l'*addition* avec le symbole `+` ;
+ la *soustraction* avec le symbole `-` ;
+ la *multiplication* avec le symbole `*` ;
+ la *division* avec le symbole `/` ;
+ le *modulo* (le reste de la division euclidienne) avec le symbole `%`.

C'est très simple à utiliser avec des nombres :

```scss
$var: 15px;
p{
  font-size: $var + 5px; // = 20px
  width: $var * (5+5) - 50px; // = 100px 
}
```

Faites cependant attention à la division. Si vous écrivez juste `font-size: 40px/2;`, Sass ne fera pas la division (il laissera ce code dans le CSS). Pour qu'il fasse le calcul, vous devez ajouter des parenthèses : `font-size: (40px/2);`. Ce problème ne se pose pas si vous utilisez des variables (`$var/2`) dans la division, ou si le calcul est composé de plusieurs opérations (`40px/2 + 5px;`).

On peut aussi effectuer des calculs avec des couleurs ! Regardez ce code :

```scss
p{
  color: #555555 + #111111; // = #666666
  color: #333333 * 2; // = #666666
  color: #676767 - 1; // = #666666
}
```

Dans la pratique, on effectue rarement des calculs sur des couleurs. Nous verrons plus tard que pour modifier des couleurs, il existe des *fonctions* spécialisées.