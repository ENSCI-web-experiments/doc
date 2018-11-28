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

### Récupération des tendances : `trends`

#### Envoi de la requête

Paramètres :

- `id` : identifiant WOEID du lieu, voir http://www.woeidlookup.com/ (par défaut 1 = Global - _facultatif_)

```javascript
server.emit('trends', {
      id: 1
})
```

#### Réception de la requête :

Résultat :

- `data` : la liste des tendances
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('trends', function(result) {
    var trends = result.data
    // ...
})
```

Exemple (https://codepen.io/LePhasme/pen/xQyqdj) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on clique sur le bouton "Submit"...
$('#change').on('click', function () {
    // On lance une requête "trends" sur la zone géographique saisie (1 = global, 23424819 = France, cf. http://www.woeidlookup.com) :
    server.emit('trends', {
        id: parseInt($('#keyword').val(), 10)
    })
})

// Quand on récupère un résultat...
server.on('trends', function(result) {
    var trends = result.data
    $('#tweets').empty()
    trends.forEach(function (trend) {
      if (trend.tweet_volume === null) {
          // Pas de nombre de tweets connu :
          $('<p class="unknown">' + trend.name + '</p>').appendTo('#tweets')
      } else {
          // Nombre de tweets connu :
          $('<p class="known">' + trend.name + ' (' + trend.tweet_volume + ' tweets)' + '</p>').appendTo('#tweets')
      }
    })
})
```

Les principales propriétés d'une tendance sont :

- `name`: le nom de la tendance (ou hashtag)
- `tweet_volume` : nombre de tweets associés (peut valoir `null` = inconnu)
- `url` : adresse de la tendance sur le site de Twitter

### Recherche de tweets : `search`

#### Envoi de la requête

Paramètres :

- `keyword` : mot(s) à chercher (4 caractères minimum - **obligatoire**), par exemple (tiré de https://developer.twitter.com/en/docs/tweets/search/guides/standard-operators.html) :
    - `watching now` : containing both “watching” and “now”. This is the default operator
    - `"happy hour"`: containing the exact phrase “happy hour”
    - `love OR hate` : containing either “love” or “hate” (or both)
    - `beer -root` : containing “beer” but not “root”
    - `#haiku` : containing the hashtag “haiku”
    - `from:interior` : sent from Twitter account “interior”
    - `list:NASA/astronauts-in-space-now` : sent from a Twitter account in the NASA list astronauts-in-space-now
    - `to:NASA` : tweet authored in reply to Twitter account “NASA”
    - `@NASA` : mentioning Twitter account “NASA”
    - `politics filter:safe` : containing “politics” with Tweets marked as potentially sensitive removed
    - `puppy filter:media` : containing “puppy” and an image or video
    - `puppy -filter:retweets` : containing “puppy”, filtering out retweets
    - `puppy filter:native_video` : containing “puppy” and an uploaded video, Amplify video, Periscope, or Vine
    - `puppy filter:periscope` : containing “puppy” and a Periscope video URL
    - `puppy filter:vine` : containing “puppy” and a Vine
    - `puppy filter:images` : containing “puppy” and links identified as photos, including third parties such as Instagram
    - `puppy filter:twimg` : containing “puppy” and a pic.twitter.com link representing one or more photos
    - `hilarious filter:links` : containing “hilarious” and linking to URL
    - `puppy url:amazon` : containing “puppy” and a URL with the word “amazon” anywhere within it
    - `superhero since:2015-12-21` : containing “superhero” and sent since date “2015-12-21” (year-month-day)
    - `puppy until:2015-12-21` : containing “puppy” and sent before the date “2015-12-21”
    - `movie -scary :)` : containing “movie”, but not “scary”, and with a positive attitude
    - `flight :(` : containing “flight” and with a negative attitude
    - `traffic ?` : containing “traffic” and asking a question
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

Exemple (https://codepen.io/LePhasme/pen/yQpYZd et https://codepen.io/LePhasme/pen/jQeawR) :

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

**ATTENTION : cette requête peut prendre beaucoup de temps à s'exécuter avant de retourner un résultat !**

Exemple (https://codepen.io/LePhasme/pen/MzrKBj) :

```javascript
// Connexion au serveur :
var server = io.connect('https://io-io-io.io:3093')

// Quand on est connecté...
server.on('connected', function () {
    // ...on émet un requête de followers sur "LePhasme" :
    server.emit('followers-full', {
        name: 'LePhasme'
    })
})

// Quand on récupère un résultat...
server.on('followers-full', function(result) {
    var followers = result.data
    console.log(result)
    // ...on parcourt tous les followers un par un...
    for (var i = 0; i < followers.length; i++) {
        // ...pour en afficher les informations dans la console :
        console.log(followers[i])
        // Ainsi que dans la page :
        $('<p>' + followers[i].name + '</p>').appendTo('body')
    }
})
```

Les principales propriétés d'un follower sont les mêmes que celles d'un utilisateur :

- `created_at` : date de création du compte
- `description` : texte de description du profil
- `followers_count` : nombre de followers
- `following` : `true` si le follower suit l'utilisateur, `false` sinon 
- `friends_count` : nombre d'abonnements
- `id` et `id_str` : identifiant de l'utilisateur
- `lang` : langage de l'utilisateur
- `location` : localisation de l'utilisateur
- `name` : nom complet
- `profile_background_image_url` : adresse "normale" de l'image de fond du profil
- `profile_background_image_url_https` : adresse sécurisée de l'image de fond du profil
- `profile_image_url` : adresse "normale" de l'image de profil
- `profile_image_url_https` : adresse sécurisée de l'image de profil
- `screen_name` : nom d'utilisateur
- `status` : dernier tweet publié (voir la requête `search` pour les propriétés d'un tweet)
- `statuses_count` : nombre de tweets publiés
- `url` : adresse personnelle associée au compte de l'utilisateur

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

Exemple : mêmes principes et nature de résultats que `followers-full`.

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

Pour les propriétés d'un utilisateur, voir les résultats de la requête `followers-full`.

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
