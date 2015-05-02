La syntaxe par défaut de Sass est le *SCSS* (pour « Sassy CSS »). On retrouve dans ce nom les lettres CSS. En effet, la syntaxe SCSS se base sur la syntaxe du CSS. En fait, tout code CSS est compatible SCSS. Cela veut dire que, si vous avez déjà des fichiers CSS, vous n'avez qu'à leur mettre l'extension `.scss` pour qu'il soient lisibles par Sass ! Vous pourrez ensuite les « améliorer » (ou plutôt les ré-organiser) en utilisant les directives propres à Sass. De plus, si vous travaillez avec quelqu'un ne connaissant pas Sass, tout son travail en CSS sera directement importable dans un fichier `.scss`, sans aucune modification. Elle est pas belle, la vie ?

Voici par exemple ce à quoi ressemble un fichier SCSS :
[[secret]]
| ```scss
| @import "compass/reset";
| 
| $fg: #333;
| $bg: #FFF;
| 
| body{
|   min-width: 320px;
|   max-width: 960px;
|   color: $fg;
|   background-color: $bg;
| }
| 
| header{
|   background-color: $fg;
|   h1{
|     color: $bg;
|     text-align: center; 
|   }
|   .auteur{
|     text-align: right;
|     &::before{
|       content:'— ';
|     }
|   }
| }
| 
| @each $title, $size in (h1: 2rem, h2: 1.5rem, h3: 1.2rem) {
|   #{$title} {
|     font-size: $size;
|   }
| }
| ```

Vous ne comprenez pas tout, c'est normal ! Si vous avez déjà fait de la programmation, certaines structures doivent vous sembler familières.

Ce code ne peut évidemment pas être compris par un navigateur. Il manque une étape, celle de la **compilation**. Sass compile les fichiers SCSS (il les fait passer à la moulinette) et il génère des fichiers CSS classiques, compréhensibles par un navigateur.

**Schéma très moche que je referais bientôt :**
![Sass compile les fichiers SCSS pour obtenir des fichiers CSS](/media/galleries/848/c6fc3bbc-2c27-4863-9e30-67c4284ba65c.png.960x960_q85.png)

En sortie, on obtient donc un beau code CSS, comme celui-ci :
[[secret]]
| ```css
| html, body, div, span, applet, object, iframe,
| h1, h2, h3, h4, h5, h6, p, blockquote, pre,
| a, abbr, acronym, address, big, cite, code,
| del, dfn, em, img, ins, kbd, q, s, samp,
| small, strike, strong, sub, sup, tt, var,
| b, u, i, center,
| dl, dt, dd, ol, ul, li,
| fieldset, form, label, legend,
| table, caption, tbody, tfoot, thead, tr, th, td,
| article, aside, canvas, details, embed,
| figure, figcaption, footer, header, hgroup,
| menu, nav, output, ruby, section, summary,
| time, mark, audio, video {
|   margin: 0;
|   padding: 0;
|   border: 0;
|   font: inherit;
|   font-size: 100%;
|   vertical-align: baseline;
| }
| 
| html {
|   line-height: 1;
| }
| 
| ol, ul {
|   list-style: none;
| }
| 
| table {
|   border-collapse: collapse;
|   border-spacing: 0;
| }
| 
| caption, th, td {
|   text-align: left;
|   font-weight: normal;
|   vertical-align: middle;
| }
| 
| q, blockquote {
|   quotes: none;
| }
| q:before, q:after, blockquote:before, blockquote:after {
|   content: "";
|   content: none;
| }
| 
| a img {
|   border: none;
| }
| 
| article, aside, details, figcaption, figure, footer, header, hgroup, main, menu, nav, section, summary {
|   display: block;
| }
| 
| body {
|   min-width: 320px;
|   max-width: 960px;
|   color: #333;
|   background-color: #FFF;
| }
| 
| header {
|   background-color: #333;
| }
| header h1 {
|   color: #FFF;
| }
| header .auteur {
|   text-align: right;
| }
| header .auteur::before {
|   content: '— ';
| }
| 
| h1 {
|   font-size: 2rem;
| }
| 
| h2 {
|   font-size: 1.5rem;
| }
| 
| h3 {
|   font-size: 1.2rem;
| }
| ```