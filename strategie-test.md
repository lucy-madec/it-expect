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
