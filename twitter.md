# Interface Twitter

Exemple de base disponible sur [CodePen](https://codepen.io/LePhasme/pen/EdOKZX).

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
});
```

#### Réception de la requête :

Résultat :

- `data` : la liste des tweets
- `keyword` : le texte recherché
- `error` : `true` ou `false` (si `error` vaut `true`, alors le résultat contient une propriété `reason` décrivant l'erreur)

```javascript
server.on('search', function(result) {
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
});
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

### Récupération des personnes abonnées : `followers`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)

```javascript
server.emit('followers', {
      name: 'LePhasme'
});
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

### Récupération des personnes suivies : `following`

#### Envoi de la requête

Paramètres :

- `name` : nom ("screen name") de l'utilisateur (**obligatoire**)

```javascript
server.emit('following', {
      name: 'LePhasme'
});
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

### Récupération des données de l'utilisateur à partir de son identifiant : `user`

#### Envoi de la requête

Paramètres :

- `id` : identifiant numérique de l'utilisateur (**obligatoire**)

```javascript
server.emit('user', {
      id: 17386789
});
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
});
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
