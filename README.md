# Intégration de Keycloak avec PHP

Ce guide explique comment intégrer Keycloak à une application PHP pour gérer l'authentification et l'autorisation des utilisateurs.

## Prérequis

- Un serveur Keycloak opérationnel sur le port 8090
- PHP installé localement ou sur un serveur, configuré pour écouter sur le port 8080
- Composer pour la gestion des dépendances PHP

## Étape 1 : Installation de Keycloak

Téléchargez et installez Keycloak depuis [le site officiel](https://www.keycloak.org/downloads.html). Suivez les instructions pour démarrer le serveur sur le port 8090.

## Étape 2 : Configuration de Keycloak

### Création d'un Realm

1. Connectez-vous à la console d'administration de Keycloak à l'adresse `http://192.168.1.100:8090`.
2. Cliquez sur `Add realm`.
3. Nommez-le (ex. `MyRealm`) et confirmez avec `Create`.

### Création d'un Client

1. Allez à `Clients` dans le menu de gauche et cliquez sur `Create`.
2. Entrez `my_php_app` comme `Client ID`.
3. Sélectionnez `public` pour `Access Type`.
4. Configurez `Valid Redirect URIs` pour utiliser une adresse IP spécifique (ex. `http://192.168.1.5:8080/*`).
5. Cliquez sur `Save`.

### Ajout d'un Utilisateur (Optionnel)

1. Allez à `Users`, puis `Add user`.
2. Remplissez les détails et définissez un mot de passe sous `Credentials`.

## Étape 3 : Installation des dépendances PHP

Utilisez Composer pour installer le client OAuth2 :

```bash
composer require league/oauth2-client
```
## Étape 4 : Création du script PHP

Créez un fichier index.php :

```php
<?php
require_once 'vendor/autoload.php';

use League\OAuth2\Client\Provider\GenericProvider;

$provider = new GenericProvider([
    'clientId'                => 'my_php_app',
    'clientSecret'            => '',
    'redirectUri'             => 'http://192.168.1.5:8080/',
    'urlAuthorize'            => 'http://192.168.1.100:8090/auth/realms/MyRealm/protocol/openid-connect/auth',
    'urlAccessToken'          => 'http://192.168.1.100:8090/auth/realms/MyRealm/protocol/openid-connect/token',
    'urlResourceOwnerDetails' => 'http://192.168.1.100:8090/auth/realms/MyRealm/protocol/openid-connect/userinfo'
]);

if (!isset($_GET['code'])) {
    $authorizationUrl = $provider->getAuthorizationUrl();
    $_SESSION['oauth2state'] = $provider->getState();
    header('Location: ' . $authorizationUrl);
    exit;
} elseif (empty($_GET['state']) || ($_GET['state'] !== $_SESSION['oauth2state'])) {
    unset($_SESSION['oauth2state']);
    exit('Invalid state');
} else {
    $accessToken = $provider->getAccessToken('authorization_code', ['code' => $_GET['code']]);
    echo 'Access Token: ' . $accessToken->getToken();
}
```

## Étape 5 : Exécution et Test

- Lancez le serveur PHP sur un port spécifique (ex. `php -S 192.168.1.5:8080` dans le répertoire de votre projet).
- Accédez à `http://192.168.1.5:8080/` dans votre navigateur.
- Vous serez redirigé vers Keycloak pour la connexion.
- Utilisez les informations d'identification de l'utilisateur de test pour vous connecter :
  - **Username:** koffi
  - **Password:** ing2024

## Commentaire sur l'utilisation de `localhost`

- Pour un développement et des tests locaux, remplacez `192.168.1.5` par `localhost` et ajustez les ports comme nécessaire.

## Conclusion

Vous avez maintenant une application PHP capable de s'authentifier via Keycloak en utilisant les informations de connexion d'un utilisateur de test. Adaptez les paramètres et les configurations selon les besoins spécifiques de votre projet.

