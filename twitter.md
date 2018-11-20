# Interface Twitter

Exemple de base disponible sur [CodePen](https://codepen.io/LePhasme/pen/EdOKZX).

Il s'agit d'un sous-ensemble de l'[API Twitter officielle](https://developer.twitter.com/en/docs/api-reference-index).

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

### Recherche de tweets : `search`

#### Envoi de la requête

Paramètres :

- `keyword` : mot(s) à chercher (4 caractères minimum - **obligatoire**)
- `since` : date de début de recherche au format `aaaa-mm-jj` (par défaut `2018-01-01` - _facultatif_)
- `count` : nombre de tweets à retourner (entre 1 et 100, par défaut 10 - _facultatif_)

```javascript
server.emit('search', {
      keyword: 'text',
      since: '2017-01-25',
      count: 10
})
```

#### Réception de la requête :

Résultat :

- `data.statuses` : la liste des tweets
- `keyword` : le texte recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('search', function(result) {
    var tweets = result.data.statuses
    // ...
})
```

Exemple (https://codepen.io/LePhasme/pen/YRYyrN) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on est connecté...
server.on('connected', function () {
    // ...on émet un requête de recherche sur le mot-clé "text" :
    server.emit('search', {
        keyword: 'text',
        since: '2017-01-25',
        count: 10
    })
})

// Quand on récupère un résultat de recherche...
server.on('search', function(result) {
    var tweets = result.data.statuses
    // ...on parcourt tous les tweets un par un...
    for (var i = 0; i < tweets.length; i++) {
        // ...pour en afficher le texte dans la console :
        console.log(tweets[i].text)
        // Ainsi que dans la page :
        $('<p>' + tweets[i].text + '</p>').appendTo('body')
    }
})
```

Les principales propriétés d'un tweet sont :

- `created_at`: date de création
- `entities.hashtags` : liste des hashtags associés
- `entities.media` : liste des médias associés
- `entities.user_mentions` : liste des utilisateurs "tagués" dans le tweet
- `entities.urls` : liste des liens associés
- `favorite_count` : nombre de "likes"
- `id` et `id_str` : identifiant du tweet
- `lang` : langage du tweet
- `retweet_count` : nombre de "retweets"
- `source` : logiciel utilisé pour composer le tweet
- `text` : contenu du tweet
- `user.id` et `user.str_id` : identifiant de l'auteur
- `user.name` : nom complet de l'auteur
- `user.screen_name` : nom d'utilisateur de l'auteur
- `user.location` : localisation de l'auteur

### Recherche de d'images : `search-images`

#### Envoi de la requête

Paramètres :

- `keyword` : mot(s) à chercher (4 caractères minimum - **obligatoire**)
- `since` : date de début de recherche au format `aaaa-mm-jj` (par défaut `2018-01-01` - _facultatif_)
- `count` : nombre de tweets à retourner (entre 1 et 100, par défaut 10 - _facultatif_)

```javascript
server.emit('search-images', {
      keyword: 'text',
      since: '2017-01-25',
      count: 10
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des images
- `keyword` : le texte recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('search-images', function(result) {
    var images = result.data
    // ...
})
```

Exemple (https://codepen.io/LePhasme/pen/yQpYZd) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on est connecté...
server.on('connected', function () {
    // ...on émet un requête de recherche d'images sur le mot-clé "text" :
    server.emit('search-images', {
        keyword: 'text',
        since: '2017-01-25',
        count: 10
    })
})

// Quand on récupère un résultat de recherche...
server.on('search-images', function(result) {
    var images = result.data
    // ...on parcourt toutes les images une par une...
    for (var i = 0; i < images.length; i++) {
        // ...pour en afficher les informations dans la console :
        console.log(images[i])
        // Ainsi que le contenu dans la page :
        $('<img src="' + images[i].secured + '" width="100px">').appendTo('body')
    }
})
```

Les principales propriétés d'une image sont :

- `normal` : son adresse "normale" (protocole `http` non sécurisé)
- `secured` : son adresse sécurisée (protocole `https`, à utiliser dans CodePen par exemple)
- `tweet.id` : l'identifiant du tweet associé
- `tweet.text` : le texte du tweet associé
- `user.id` : l'identifiant de l'utilisateur associé
- `user.name` : le nom de l'utilisateur associé

### Récupération des données complètes des personnes abonnées : `followers-full`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)

```javascript
server.emit('followers-full', {
      name: 'LePhasme'
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des personnes abonnées
- `name` : le nom recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('followers-full', function(result) {
    var followers = result.data
    // ...
})
```

### Récupération des données complètes des personnes suivies : `following-full`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)

```javascript
server.emit('following-full', {
      name: 'LePhasme'
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des personnes suivies
- `name` : le nom recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('following-full', function(result) {
    var following = result.data
    // ...
})
```

### Récupération des données de l'utilisateur à partir de son identifiant : `user`

#### Envoi de la requête

Paramètres :

- `id` : identifiant numérique de l'utilisateur (**obligatoire**)

```javascript
server.emit('user', {
      id: 17386789
})
```

#### Réception de la requête :

Résultat :

- `data` : les données de l'utilisateur
- `id` : l'identifiant recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('user', function(result) {
    var user = result.data
    // ...
})
```

### Récupérer les tweets d'un utilisateur : `user-tweets`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)
- `count` : nombre de tweets à retourner (entre 1 et 100, par défaut 10 - _facultatif_)

```javascript
server.emit('user-tweets', {
      name: 'LePhasme',
      count: 20
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des tweets
- `name` : le nom de l'utilisateur
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('user-tweets', function(result) {
    var tweets = result.data
    // ...
})
```

### Abonnemant à un fil "live" : `subscribe`

#### Envoi de la requête

Paramètres :

- `keyword` : filtre (4 caractères minimum - **obligatoire**)

```javascript
server.emit('subscribe', {
      keyword: 'text'
})
```

#### Réception de la requête :

Résultat :

- `data` : le nouveau tweet
- `keyword` : le texte du filtre
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('subscribe', function(result) {
    var tweet = result.data
    // ...
})
```

### Récupération des identifiants des personnes abonnées : `followers`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)

```javascript
server.emit('followers', {
      name: 'LePhasme'
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des _identifiants_ des personnes abonnées
- `name` : le nom recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('followers', function(result) {
    var followers = result.data
    // ...
})
```

### Récupération des identifiants des personnes suivies : `following`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)

```javascript
server.emit('following', {
      name: 'LePhasme'
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des _identifiants_ des personnes suivies
- `name` : le nom recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('following', function(result) {
    var following = result.data
    // ...
})
```
