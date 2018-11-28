# Interface Text

Exemple de base disponible sur CodePen :

- https://codepen.io/LePhasme/pen/RqYWKJ
- https://codepen.io/LePhasme/pen/mQGeao
- https://codepen.io/LePhasme/pen/LXgLOL

Il s'agit de l'exposition via une interface WebSocket du module [Natural](https://github.com/NaturalNode/natural).

## Prérequis HTML

```html
<!doctype html>
<html lang="fr">

  <!-- ... -->

  <!-- Corps de page -->
  <body>
  
    <!-- ... -->
    
    <!-- Intégration de la connexion au serveur dédié : -->
    <script src="https://io-io-io.io:3093/socket.io/socket.io.js"></script>
    
  </body>
</html>
```

## API JavaScript

### Connexion au serveur

```javascript
var server = io.connect('https://io-io-io.io:3093')
```

### Récupération des tokens : `tokens`

#### Envoi de la requête

Paramètres :

- `text` : texte à découper en tokens (**obligatoire**)
- `lang` : langue du texte (`fr` ou `en`, par défaut `en` - _facultatif_)

```javascript
server.emit('tokens', {
      text: 'Hello world!',
      lang: 'en'
})
```

#### Réception de la requête :

Résultat :

- `data.tokens` : la liste des tokens
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('tokens', function(result) {
    var tokens = result.tokens
    // ...
})
```

Exemple (https://codepen.io/LePhasme/pen/RqYWKJ) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on clique sur le bouton "Submit"...
$('#change').on('click', function () {
    // On lance une requête "tokens" sur le texte saisi dans le champ, considéré comme étant en français :
    server.emit('tokens', {
        text: $('#keyword').val(),
        lang: 'fr'
    })
})

// Quand on récupère un résultat de recherche...
server.on('tokens', function(result) {
    var tokens = result.tokens
    // ...on parcourt tous les tokens un par un...
    for (var i = 0; i < tokens.length; i++) {
        // ...pour en afficher le contenu dans la console :
        console.log(tokens[i])
        // Ainsi que dans la page :
        $('<pre>' + tokens[i] + '</pre>').appendTo('body')
    }
})
```

### Récupération du sentiment : `sentiment`

#### Envoi de la requête

Paramètres :

- `text` : texte à analyser (**obligatoire**)
- `lang` : langue du texte (`fr` ou `en`, par défaut `en` - _facultatif_)

```javascript
server.emit('sentiment', {
      text: 'I like this!',
      lang: 'en'
})
```

#### Réception de la requête :

Résultat :

- `data.sentiment` : le "sentiment" entre -1 et 1
- `data.tokens` : les tokens (voir `tokens`)
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('sentiment', function(result) {
    var sentiment = result.sentiment
    // ...
})
```

Exemple (https://codepen.io/LePhasme/pen/mQGeao) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on clique sur le bouton "Submit"...
$('#change').on('click', function () {
    // On lance une requête "tokens" sur le texte saisi dans le champ, considéré comme étant en français :
    server.emit('sentiment', {
        text: $('#keyword').val(),
        lang: 'fr'
    })
})

// Quand on récupère un résultat...
server.on('sentiment', function(result) {
    var sentiment = result.sentiment
    var tokens = result.tokens
    // ...on parcourt tous les tokens un par un...
    for (var i = 0; i < tokens.length; i++) {
        // ...pour en afficher le contenu dans la console :
        console.log(tokens[i])
        // Ainsi que dans la page :
        $('<pre>' + tokens[i] + '</pre>').appendTo('body')
    }
    // ...et le "sentiment":
    console.log(sentiment)
    if (sentiment > 0) {
      $('<pre class="sentiment positif">' + sentiment + '</pre>').appendTo('body')
    } else {
      $('<pre class="sentiment negatif">' + sentiment + '</pre>').appendTo('body')      
    }
})
```

### Récupération des données phonétiques : `phonetics`

#### Envoi de la requête

Paramètres :

- `text` : texte à analyser (**obligatoire**)

```javascript
server.emit('phonetics', {
      text: 'I like this!'
})
```

#### Réception de la requête :

Résultat :

- `data.phonetics` : les termes phonétique
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('phonetics', function(result) {
    var phonetics = result.phonetics
    // ...
})
```

Exemple (https://codepen.io/LePhasme/pen/LXgLOL) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on clique sur le bouton "Submit"...
$('#change').on('click', function () {
    // On lance une requête "phonetics" sur le texte saisi :
    server.emit('phonetics', {
        text: $('#keyword').val()
    })
})

// Quand on récupère un résultat...
server.on('phonetics', function(result) {
    var phonetics = result.phonetics
    $('#tweets').empty()
    phonetics.forEach(function (phonetic) {
     $('<pre>' + phonetic + '</pre>').appendTo('#tweets')
    })
})
```
