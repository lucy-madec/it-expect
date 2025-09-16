# Cahier de Recettes – Cinetech

- **Version** : 1.0  
- **Date** : 2025-09-16  
- **Projet** : Cinetech (bibliothèque films/séries – TMDb)  
- **Environnement** : `local` (http://localhost:3000 ou 5173), API backend up, clé TMDb configurée

---

## Légende des statuts
- **OK** = Conforme / **KO** = Non conforme / **BLK** = Bloquant / **NA** = Non applicable

---

## Modèle de cas de test (à réutiliser)
**ID** : `TC-XXX-YYY`  
**Type** : Unitaire / Intégration / Fonctionnel / E2E  
**Objectif** : …  
**Prérequis** : …  
**Données de test** : …  
**Étapes** :  
1) …  
2) …  
**Résultat(s) attendu(s)** : …  
**Critères d’acceptation** :  
- …  
**Résultat obtenu** : (à renseigner pendant l’exécution)  
**Observations** : …  
**Statut** : OK / KO / BLK / NA  

---

## A. Tests fonctionnels / E2E

### TC-FUNC-001 — Connexion utilisateur (identifiants valides)
**Type** : Fonctionnel / E2E  
**Objectif** : Vérifier qu’un utilisateur enregistré peut se connecter avec des identifiants valides et accéder à la page d’accueil.  
**Prérequis** :  
- Un compte utilisateur existe en base (ex. `lucy@example.com` / `Motdepasse!234`).  
- L’utilisateur est **déconnecté** (aucun token actif dans le storage).  
- Backend/API et base de données démarrés.  
**Données de test** :  
- Email : `lucy@example.com`  
- Mot de passe : `Motdepasse!234`  
**Étapes** :  
1) Ouvrir la page **/login**.  
2) Saisir l’email et le mot de passe valides.  
3) Cliquer sur **Se connecter**.  
**Résultat(s) attendu(s)** :  
- Redirection vers la **page d’accueil**.  
- Le **nom d’utilisateur** s’affiche **sous le logo** dans le header.  
- Un **token** (session/JWT) est stocké (storage/cookie) et la session est active.  
**Critères d’acceptation** :  
- Aucune **erreur** visible (toast/alert) après soumission.  
- Rechargement de la page conserve la session (toujours connecté).  
- Le menu utilisateur affiche les actions pertinentes (ex. *Déconnexion*).  
**Résultat obtenu** : _à compléter_  
**Observations** : _à compléter_  
**Statut** : OK / KO / BLK / NA  

---

### TC-FUNC-002 — Barre de recherche avec autocomplétion (films/séries)
**Type** : Fonctionnel / E2E  
**Objectif** : Vérifier que la recherche propose des suggestions (autocomplétion) et que la sélection d’un item ouvre la page détail correspondante.  
**Prérequis** :  
- Clé **TMDb** configurée et connectivité réseau OK.  
- Page contenant la **barre de recherche** accessible.  
**Données de test** :  
- Terme de recherche : `inception`  
- Élément attendu dans la liste : `Inception (2010)` (film)  
**Étapes** :  
1) Cliquer dans la barre de recherche.  
2) Taper `inception`.  
3) Attendre l’apparition de la **liste déroulante** d’autocomplétion.  
4) Cliquer sur l’entrée `Inception (2010)` dans la liste.  
**Résultat(s) attendu(s)** :  
- La liste d’autocomplétion affiche au moins une suggestion pertinente contenant **“Inception”**.  
- Après le clic sur la suggestion, navigation vers la **page détail** correspondante (`/movies/{id}` ou `/tv/{id}`).  
- La page détail affiche le **titre** et les **métadonnées** du contenu (date, note, synopsis).  
**Critères d’acceptation** :  
- Pas d’erreur réseau affichée pour une connexion valide.  
- L’URL contient un **id** valide (> 0).  
- Un **chargement** (spinner/skeleton) est visible si la requête prend >300 ms.  
**Résultat obtenu** : _à compléter_  
**Observations** : _à compléter_  
**Statut** : OK / KO / BLK / NA  

---

## B. Idées de compléments immédiats (même catégorie)
> Tu pourras cloner TC-FUNC-002 et adapter :  
- **TC-FUNC-003** — Recherche : aucun résultat (affiche “Aucun résultat” et état vide).  
- **TC-FUNC-004** — Recherche : accents/variantes (ex. `Amélie` vs `Amelie`).  
- **TC-FUNC-005** — Sécurité : empêcher l’injection (saisie `"<script>"` n’injecte rien, input échappé/sanitizé).

---

## C. Pistes pour les autres catégories (quand tu voudras les ajouter)
- **Unitaires** : parsers/mappers TMDb → modèle interne, formatage des dates/notes, utilitaires de pagination.  
- **Intégration** : route `/api/search` ↔ service TMDb ↔ mapping ↔ rendu composant Liste.  
- **Sécurité** : durée et validation des **JWT**, middleware “auth required” sur routes sensibles.  
- **Performance** : temps de rendu Home < 2 s (cold), recherche < 800 ms à 10 requêtes concurrentes.

---

👉 **Note** : Ce fichier est une version corrigée/structurée de ton cahier existant, avec des champs mesurables et des critères d’acceptation concrets. Tu peux dupliquer le modèle pour atteindre la cible “~5 tests par type”.
