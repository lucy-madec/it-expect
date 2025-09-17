## 1 - Périmètre & priorités

Fonctionnalités couvertes :
- Authentification (login)
- Recherche (autocomplete → page détail)
- Page détail (film/série)
- Favoris (ajout/suppression)

Priorités :
- **P1** : Authentification, Recherche, Favoris
- **P2** : Page détail

## 2 - Types de tests par fonctionnalité

| Fonction  | Unitaires              | Intégration                | Fonctionnels / E2E                  | Sécurité                     |
|-----------|------------------------|----------------------------|-------------------------------------|------------------------------|
| Auth      | —                      | Login API ↔ stockage token | Connexion valides                   | Accès /favorites sans login  |
| Recherche | Mapper TMDb → modèle   | UI ↔ API TMDb ↔ rendu liste| Barre recherche → page détail       | Inputs nettoyés (anti-injection) |
| Détail    | Formatage dates/notes  | Fetch détail ↔ rendu       | Affichage titre/autres détails liés à l’œuvre | —                            |
| Favoris   | —                      | Ajout favoris              | Parcours ajout/suppression favoris  | Route protégée (via login)   |

## 3 - Environnements & données de test

### 3.1 Environnements
- **Local** : Application MVC (front en templates + contrôleurs) + Backend/API + Base de données démarrés en local.
- **Préproduction** : Environnement copie de la prod pour tester avant mise en prod. (Si non dispo : tests réalisés en local.)

### 3.2 Secrets / API
- Clé **TMDb** stockée dans un fichier de configuration non versionné (ex. `.env`), jamais commitée.

### 3.3 Comptes de test
- Utilisateur standard : `user@test.com` / `Motdepasse!234`

### 3.4 Données connues (références)
- Film vitrine : **Inception** (TMDb `id=27205`) pour valider recherche, détail et favoris.

### 3.5 Jeux de données / Fixtures
- Dossier `tests/data/` contenant des échantillons de données pour les tests (JSON).
- Exemples :
  - `tmdb_movie_inception.json` : réponse TMDb simplifiée (mock).
  - `movie_mapped_inception.json` : modèle interne attendu après mapping.

### 3.6 Reset / Seed (avant intégration & E2E)
- **Reset** : réinitialiser la base (vider les tables) avant les tests.
- **Seed** : insérer l’utilisateur de test et les données minimales.

# 4 - Outils & structure

## 4.1 Outils de test
- **PHPUnit** : outil standard PHP pour les **tests unitaires** (fonctions/classes isolées) et **d’intégration** (contrôleurs/API + base).
- **Fonctionnels / E2E** : pour l’instant **manuels** (suivre le cahier de recettes dans le navigateur).
- **Sécurité** : tests **négatifs** avec PHPUnit (ex. route protégée sans session → 401/redirect).

## 4.2 Installation (PHPUnit)
Installer via Composer (en dev) :
```bash
composer require --dev phpunit/phpunit ^10
```

Lancer les tests :
```bash
vendor/bin/phpunit
```

## 4.3 Structure des dossiers
```
/tests
  /unit           ← tests unitaires (mappers, helpers…)
  /integration    ← tests intégration (API ↔ DB ↔ contrôleurs)
/data            ← fixtures (JSON, SQL, etc.)
phpunit.xml.dist ← configuration PHPUnit
```

## 4.4 Scripts Composer (confort)
Ajouter dans `composer.json` (section "scripts") :
```json
{
  "scripts": {
    "test": "phpunit",
    "test:unit": "phpunit --testsuite Unit",
    "test:integration": "phpunit --testsuite Integration"
  }
}
```

## 4.5 Configuration PHPUnit minimale
Créer `phpunit.xml.dist` à la racine :
```xml
<?xml version="1.0" encoding="UTF-8"?>
<phpunit bootstrap="tests/bootstrap.php"
         colors="true"
         beStrictAboutTestsThatDoNotTestAnything="false">
  <testsuites>
    <testsuite name="Unit">
      <directory>tests/unit</directory>
    </testsuite>
    <testsuite name="Integration">
      <directory>tests/integration</directory>
    </testsuite>
  </testsuites>
</phpunit>
```

## 4.6 Bootstrap PHPUnit
Créer `tests/bootstrap.php` :
```php
<?php
require __DIR__ . '/../vendor/autoload.php';
```

## 4.7 Conventions de nommage
- 1 fichier = 1 cas de test (ou un petit groupe proche).
- Noms lisibles, par ex. :
  - `tests/unit/MapTmdbMovieTest.php`
  - `tests/integration/AddFavoriteTest.php`