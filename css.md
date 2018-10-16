# Aide mémoire CSS

Éléments de base du langage CSS (listes non exhaustives, voir [ici](https://fr.wikibooks.org/wiki/Le_langage_CSS) par exemple pour une introduction plus complète ou [là](https://developer.mozilla.org/fr/docs/Web/CSS) pour une référence plus pointue).

N'hésitez pas à tester "en live" avec un outil tel [CodePen](https://codepen.io/).

## Structure de base d'un fichier CSS

```css
sélecteur-1 {
  propriété-1: valeur-1;
  propriété-2: valeur-2;
  propriété-n: valeur-n;
}

sélecteur-2 {
  propriété-1: valeur-1;
  propriété-2: valeur-2;
  propriété-n: valeur-n;
}
```

## Les sélecteurs

`*` : Sélecteur universel, par exemple _"tous les éléments ont une taille de caractère de 14 pixels"_ :

```css
* {
  font-size: 14px;
}
```

`nom_de_balise` : Sélecteur de balise, par exemple _"tous les textes des liens sont de couleur bleu et tous les titres de premier niveau ont un fond rouge"_ :

```css
a {
  color: blue;
}

h1 {
  background: red;
}
``` 

`balise_mère balise_descendante` : Sélecteur de descendance, par exemple _"tous les textes avec emphase dans un paragraphe ont la couleur verte"_ :

```css
p em {
  color: green;
}
```

```html
<p>
  <em>Ce texte sera en vert</em>
  <strong>
    <em>Ce texte sera en vert aussi</em>
  </strong>
</p>
```

`balise_mère > balise_fille` : Sélecteur d'enfants directs, par exemple _"tous les textes avec emphase au premier niveau d'un paragraphe ont la couleur verte"_ :

```css
p > em {
  color: green;
}
```

```html
<p>
  <em>Ce texte sera en vert</em>
  <strong>
    <em>Mais pas celui-ci</em>
  </strong>
</p>
```

`balise + balise_suivante` : Sélecteur d'adjacence, par exemple _"le paragraphe suivant un titre de second niveau sera écrit en gras"_ :

```css
h2 + p {
  font-weight: bold;
}
```

```html
<h1>Titre 1</h1>
<p>Paragraphe normal</p>
<h2>Titre 2</h2>
<p>Ce paragraphe sera en gras</p>
```

`sélecteur_1, sélecteur_2` : Regroupement de sélecteurs, par exemple _"les titres de niveau 1 à 3 sont écrits en bleu, ceux de niveau 4 à 6 en rouge"_ :

```css
h1, h2, h3 {
  color: blue;
}

h4, h5, h6 {
  color: red;
}
```

`.nom_de_classe` : Sélecteur de classe, par exemple _"tous les éléments ayant la classe 'bordure' ont une bordure jaune"_ :

```css
.bordure {
  border: 1px solid yellow;
}
```

```html
<h1 class="bordure">Titre avec bordure jaune</h1>
<p>Paragraphe normal</p>
<h2>Titre normal</h2>
<p class="bordure">Paragraphe avec bordure jaune</p>
```

`#identifiant` : Sélecteur d'identifiant, par exemple _"l'élément ayant l'identifiant 'invisible' sera invisible"_ :

```css
#invisible {
  display: none;
}
```

```html
<h1>Titre 1</h1>
<p>Paragraphe normal</p>
<h2>Titre 2</h2>
<p id="invisible">Paragraphe invisible</p>
```

`balise:état` : Sélecteur d'état, par exemple _"au survol de la souris, le texte des éléments de classe 'survol' devient rouge"_ :

```css
.survol:hover {
  color: red;
}
```

**Note** : tous les types de sélecteurs peuvent être librement combinés entre eux (balise, descendance, regroupement, classe, identifiant, etc.) 

## Les unités

### Unités absolues

`in` (inch – 1in = 2.54cm), `cm`, `mm`, `pt` (point – 1pt = 0.0138in), `pc` (pica - 1pc = 12 pt), `px` (pixel – 1px = 0.75pt)

```css
h1 { margin: 0.5in; }
h2 { line-height: 3cm; }
h3 { word-spacing: 4mm; }
h4 { font-size: 12pt; }
h5 { font-size: 1pc; }
h6 { font-size: 12px; }   
```

### Unités relatives

`em` (la taille de caractères en cours), `ex` (la hauteur de la police de caractère en cours)

Par exemple _"la hauteur de ligne des paragraphes sera égale à 2.5 fois la taille du texte"_ (ici on aura donc une hauteur de ligne de 40px) :

```css
p {
    font-size: 16px;
    line-height: 2.5em;
}
```

### Le pourcentage

`%` : suivant la propriété considérée, l'élément prendra une dimension proportionnelle à la dimension analogue de son élément parent (l'élément qui le contient), par exemple _"le bloc d'identifiant 'demi' aura pour largeur la moitié de celle de l'élément qui le contient"_ :

```css
#demi {
  width: 50%;
}
```

On utilise généralement les unités `px`, `em` et `%`, les autres étant surtout réservées à l'impression (sachant que de nouvelles unités sont régulièrement mises à disposition dans les navigateurs).

## Les couleurs

Il existe à l'origine 16 couleurs de base prédéfinies : `aqua`, `black`, `blue`, `fuchsia`, `gray`, `green`, `lime`, `maroon`, `navy`, `olive`, `purple`, `red`, `silver`, `teal`, `white`, et `yellow`. Mais de [nombreuses autres](https://www.w3.org/TR/css-color-3/#svg-color) sont généralement disponibles dans la plupart des navigateurs.

La plupart du temps, une couleur est donnée au format RGB (rouge, vert, bleu) :

- `#rrggbb` où `rr`, `gg` et `bb` sont des nombres au [format hexadécimal](https://fr.wikipedia.org/wiki/Syst%C3%A8me_hexad%C3%A9cimal) entre `00` et `ff` (par exemple, du vert moyen, `#00cc00`)
- `#rgb` (même exemple : `#0c0`)
- `rgb(x, x, x)` où x est un entier entre 0 et 255 (même exemple : `rgb(0, 204, 0)`)
- `rgba(x, x, x, a)` comme `rgb()` mais avec une valeur de transparence entre 0 et 1 (même exemple, mais transparent de moitié : `rgba(0, 204, 0, 0.5)`)

Par exemple _"les blocs de classe 'translucide' ont un fond bleu transparent à 50%"_ :

```css
div.translucide {
  background-color: rgba(0, 0, 255, 0.5);
}
```

Il existe aussi la couleur `transparent`.

## Propriétés de boîte

- `margin` : Marge extérieure 
- `margin-top` : Marge de haut
- `margin-right` : Marge de droite
- `margin-bottom` : Marge de bas
- `margin-left` : Marge de gauche

```css
div {
  margin: 5px;
}

/* ou */

div {
  margin: 5px 5px 5px 5px;
}

/* ou */

div {
  margin-top: 5px;
  margin-right: 5px;
  margin-bottom: 5px;
  margin-left: 5px;
}
```

```css
div {
  margin: 5px 10px 15px 20px;
}

/* ou */

div {
  margin-top: 5px;
  margin-right: 10px;
  margin-bottom: 15px;
  margin-left: 20px;
}
```

-----

- `padding` : Espacement (ou marge) intérieur
- `padding-top` : Espacement de haut
- `padding-right` : Espacement de droite
- `padding-bottom` : Espacement de bas
- `padding-left` : Espacement de gauche

-----

- `border` : Bordure
- `border-top` : Bordure de haut
- `border-right` : Bordure de droite
- `border-bottom` : Bordure de bas
- `border-left` : Bordure de gauche
- `border-width` : Taille de bordure
- `border-color` : Couleur de la bordure
- `border-style` : [Style de ligne](https://developer.mozilla.org/fr/docs/Web/CSS/border-style) de la bordure

```css
div {
	border: 1px solid red;
}

/* ou */

div {
	border-width: 1px;
	border-style: solid;
	border-color: red;
}
```

-----

- `width` : Largeur de l'élément (si pourcentage : par rapport à la largeur de l'élément parent)
- `height` : Hauteur de l'élément (si pourcentage : par rapport à la hauteur de l'élément parent)

-----

- `color` : Couleur de texte, de soulignement et de bordure par défaut
- `background` : Fond du bloc
- `background-color` : Couleur de fond du bloc
- `background-image` : Image de fond du bloc
- `background-repeat` : Répétition de l'image de fond
- `background-position` : Position de l'image de fond

```css
.deco {
  background: url(../images/image.png) center no-repeat;
}

/* ou */

.deco {
  background-image: url(../images/image.png);
  background-position: center;
  background-repeat: no-repeat;
}
```

-----

- `font-family` : Police(s) de caractères – `serif`, `sans-serif`, `cursive`, `fantasy`, `monospace` ou toute autre police disponible pour le navigateur
- `font-style` : Style de police – `normal`, `italic` ou `oblique`
- `font-variant` : Variation de la police – `normal` ou `small-caps`
- `font-weight` : Épaisseur de la police – `normal`, `bold`, `bolder`, `lighter` ou une valeur de 100 à 900
- `font-size` : Taille de la police – `xx-small`, `x-small`, `small`, `medium`, `large`, `x-large`, `xx-large` ou taille relative ou absolue
- `text-decoration` : Décoration du texte – `none`, `underline`, `overline`, `line-through`, `blink`
- `text-transform` : Transformation du texte – `none`, `capitalize`, `uppercase`, `lowercase`
- `text-align` : Alignement horizontal du texte – `left`, `right`, `center`, `justify`

-----

- `display` : Mode d'affichage des blocs
 - `block` : en tant que bloc (une ligne est insérée avant et après le bloc)
 - `inline` : en ligne
 - `list-item` : en tant qu'élément de liste
 - `none` : non affiché
 
-----

- `list-style-type` : Type de puce ou de numérotation des listes (s'applique aux éléments `ul`, `ol` et `li`, et ceux avec `display: list-item;`)
 - `disc` : disque
 - `circle` : cercle
 - `square` : carré
 - `decimal` : 1 2 3 4 ...
 - `lower-roman` : i ii iii iv ...
 - `upper-roman` : I II III IV ...
 - `lower-alpha` : a b c d ...
 - `upper-alpha` : A B C D ...
 - `none` : pas de puce