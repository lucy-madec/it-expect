# Cahier de Recettes – Cinetech
  
- **Projet** : Cinetech (bibliothèque films/séries – TMDb)

---

## A. Tests fonctionnels / E2E

### Connexion utilisateur (identifiants valides)
**Type** : Fonctionnel / E2E  
**Objectif** : Vérifier qu’un utilisateur enregistré peut se connecter avec des identifiants valides et accéder à la page d’accueil.  
**Prérequis** :  
- Un compte utilisateur existant  
- L’utilisateur est **déconnecté**
**Données de test** :  
- Email : `mail@example.com`  
- Mot de passe : `Motdepasse!234`  
**Étapes** :  
1) Aller sur la page **Connexion** depuis l'icone de profil.  
2) Saisir l’email et le mot de passe valides.  
3) Cliquer sur **Se connecter**.  
**Résultat(s) attendu(s)** :  
- Redirection vers la **page d’accueil**.  
- Le **nom d’utilisateur** s’affiche **sous le logo** dans le header. 

---

### Barre de recherche avec autocomplétion (films/séries)
**Objectif** : Vérifier que la recherche propose des suggestions (autocomplétion) et que la sélection d’un item ouvre la page détail correspondante.  
**Prérequis** :  
- Clé **TMDb** configurée et connectivité réseau OK.  
- Page contenant la **barre de recherche** accessible.  
**Données de test** :  
- Terme de recherche : `inception`  
- Élément attendu dans la liste : `Inception` avec affiche de film  
**Étapes** :  
1) Cliquer dans la barre de recherche.  
2) Taper `inception`.  
3) Attendre l’apparition de la **liste déroulante** d’autocomplétion.  
4) Cliquer sur l’entrée `Inception` dans la liste.  
**Résultat(s) attendu(s)** :  
- La liste d’autocomplétion affiche au moins une suggestion pertinente contenant **“Inception”**.  
- Après le clic sur la suggestion, navigation vers la **page détail** correspondante (`/movies/{id}` ou `/tv/{id}`).  
- La page détail affiche le **titre** et les **métadonnées** du contenu (genre, pays d’origine, synopsis).  

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
