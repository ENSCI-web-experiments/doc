# Aide mémoire HTML

Éléments de base du langage HTML (listes non exhaustives, voir [ici](https://fr.flossmanuals.net/initiation-html5/introduction/) par exemple pour une introduction plus complète ou [là](https://developer.mozilla.org/fr/docs/Web/Guide/HTML/HTML5) pour une référence plus pointue).

## Structure de base d'un fichier HTML

```html
<!doctype html>
<html lang="fr">

  <!-- En-tête et méta-données -->
  <head>
    <meta charset="utf-8">
    <meta name="description" content="Exemple de page HTML">
    <meta name="author" content="Someone">
    
    <title>Titre de la page</title>
    
    <!-- Intégration des feuilles de styles -->
    <link rel="stylesheet" href="css/styles.css">
  </head>
  
  <!-- Corps de page -->
  <body>
    <h1>Titre de la page</h1>
    <p class="intro">Introduction</p>
    <h2>Première partie</h2>
    <p>Premier paragraphe</p>
    
    <!-- Intégration des fichiers JavaScript -->
    <script src="js/scripts.js"></script>
  </body>
</html>
```

## Balises de type bloc

- `<div></div>` : Division (générique)
- `<h1></h1>` : Titre de page (une seule balise `<h1>` par page)
- `<h2></h2>` à `<h6></h6>` : Sous-titres
- `<p></p>` : Paragraphe de texte
- `<pre></pre>` : Texte préformaté
- `<hr>` : Ligne horizontale

## Balises de type en-ligne

- `<span></span>` : Division (générique)
- `<a href="adresse_cible">texte</a>` : lien vers une adresse cible
- `<em></em>` : Emphase (italique)
- `<strong></strong>` : Emphase forte (gras)
- `<code></code>` : Texte préformaté
- `<img src="adresse_image" alt="texte_alternatif" width="x" height="y">` : Image
- `<br>` : Saut de ligne

## Balises de type liste

- `<ul></ul>` : Liste non ordonnée (à puce)
- `<ol></ol>` : Liste ordonnée (numérotée)
- `<li></li>` : Item de liste

```html
<ul>
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3 introduisant une sous-liste :
    <ul>
      <li>Sous-item 1</li>
      <li>Sous-item 2</li>
    </ul>
  </li>
</ul>
```

## Balises de type tableau

- `<table></table>` : Tableau
- `<caption></caption>` : Légende du tableau
- `<thead></thead>` : En-tête du tableau
- `<tbody></tbody>` : Corps du tableau
- `<tr></tr>` : Ligne de tableau
- `<td></td>` : Cellule de tableau
- `<th></th>` : Cellule d'en-tête de tableau

```html
<table>
  <caption>Exemple de tableau</caption>
  <thead>
    <tr>
      <th colspan="2">L'en-tête (qui recouvre 2 colonnes)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Le corps du tableau</td>
      <td>avec deux colonnes</td>
    </tr>
    <tr>
      <td>Et...</td>
      <td>deux lignes</td>
    </tr>
  </tbody>
</table>
```