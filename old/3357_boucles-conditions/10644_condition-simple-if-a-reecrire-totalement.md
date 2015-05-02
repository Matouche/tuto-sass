Nous entrons maintenant dans le domaine des *conditions*. Celles-ci ne vous seront utiles que dans des cas très particuliers, lorsque vous créez des mixins qui doivent être ré-utilisables sur plusieurs projets, par exemple. En gros, une condition est un bloc dont les propriétés ne doivent être applique que sile suivant :
> SI `$truc` est égal à `42`, on applique ces propriétés.

Pour dire SI à Sass, on utilise la directive `@if` suivie du test à effectuer( égalité, inégalité, supériorité ou infériorité) :

+---------+-------------------+----------------------------------+
|Opérateur| Exemple           | Signification                    |
+=========+===================+==================================+
|==       |`@if $truc == 42{}`| Si `$truc` égal 42               |
+---------+-------------------+----------------------------------+
|!=       |`@if $truc != 42{}`| Si `$truc` différent de 42       |
+---------+-------------------+----------------------------------+
|<        |`@if $truc < 42{}` | Si `$truc` inférieur strict à 42 |
+---------+-------------------+----------------------------------+
|\>       |`@if $truc > 42{}` | Si `$truc` supérieur strict à 42 |
+---------+-------------------+----------------------------------+
|<=       |`@if $truc <= 42{}`| Si `$truc` inférieur ou égal à 42|
+---------+-------------------+----------------------------------+
|\>=      |`@if $truc >= 42{}`| Si `$truc` supérieur ou égal à 42|
+---------+-------------------+----------------------------------+

Pour que voyiez mieux l'utilité de la chose, je vous propose un petit exemple. Reprenons pour cela notre mixin `joli-hover` (vous vous en souvenez ?) :
```css
@mixin joli-hover($normal, $hover){
  color: $normal;
  &:hover{
    color: $hover;
  }
}
```
Maintenant, que se passe-t-il lorsqu'on donne la même valeur à `$normal` et à `$hover` ? Notre bloc `:hover` ne sert plus à rien, on peut l'enlever pour économiser trois lignes (n'est-ce pas fantastique !). Il faut donc vérifier si `$normal` est différent de `$hover` avant d'appliquer une couleur au survol.