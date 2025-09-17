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

| Fonction        | Unitaires                        | Intégration                      | Fonctionnels / E2E                        | Sécurité                            | Perf |
|-----------------|----------------------------------|----------------------------------|------------------------------------------|-------------------------------------|------|
| Auth            | —                                | Login API ↔ stockage token       | Connexion valides       | Accès /favorites sans login | —    |
| Recherche       | Mapper TMDb → modèle | UI ↔ API TMDb ↔ rendu liste | Barre recherche → page détail | Inputs nettoyés (anti-injection)    | —    |
| Détail          | Formatage dates/notes            | Fetch détail ↔ rendu             | Affichage titre/autres détails lié à l'oeuvre               | —                                   | —    |
| Favoris         | —                                | Ajout favoris   | Parcours ajout/suppression favoris        | Route protégée (via login)          | —    |
