
# Intégration de Keycloak avec PHP

Ce guide détaille les étapes pour intégrer Keycloak à une application PHP pour gérer l'authentification et l'autorisation des utilisateurs.

## Prérequis

- Un **serveur Keycloak** opérationnel
- PHP installé localement ou sur un serveur
- **Composer** pour la gestion des dépendances PHP

## Étape 1 : Installation de Keycloak

Téléchargez et installez Keycloak depuis [le site officiel](https://www.keycloak.org/downloads.html). Suivez les instructions pour démarrer le serveur.

## Étape 2 : Configuration de Keycloak

### Création d'un Realm

1. Connectez-vous à la console d'administration de Keycloak.
2. Cliquez sur `Add realm`.
3. Nommez-le (ex. `MyRealm`) et confirmez avec `Create`.

### Création d'un Client

1. Allez à `Clients` dans le menu de gauche et cliquez sur `Create`.
2. Entrez `my_php_app` comme `Client ID`.
3. Sélectionnez `public` pour `Access Type`.
4. Ajoutez `http://localhost:8080/*` à `Valid Redirect URIs`.
5. Cliquez sur `Save`.

### Ajout d'un Utilisateur (Optionnel)

1. Allez à `Users`, puis `Add user`.
2. Remplissez les détails et définissez un mot de passe sous `Credentials`.

## Étape 3 : Installation des dépendances PHP

Utilisez Composer pour installer le client OAuth2 :

```bash
composer require league/oauth2-client
