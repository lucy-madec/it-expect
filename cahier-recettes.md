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

## B. Test Unitaire

### Mapper TMDb à un Modèle interne (Film)
**Type** : Unitaire
**Objectif** : Vérifier que la fonction de mapping transforme correctement un objet TMDb `movie` en modèle interne `Movie`.
**Prérequis** :
- Fonction `mapTmdbMovieToMovieModel(input)` disponible.
- Environnement de test (ex. Jest/PhpUnit) prêt.
**Données de test** :
`{"id":27205,"title":"Inception","vote_average":8.4,","overview":"A mind-bending thriller...","poster_path":"/qmDpIHrmpJINaRKAfWQfftjCdyi.jpg","genre_ids":[28,878,12]}`
**Etapes**:
1. Appeler `mapTmdbMovieToMovieModel(input)` avec l'objet ci-dessus.
**Résultats attendus**:
- Retourne un objet :
    - `id = 27205`
    - `title = "Inception"`
    - `rating = 8.4` _(float 0-10)_
    - `overview` non vide
    - `posterUrl` construit correctement _(ex.`https://image.tmdb.org/t/p/w500/qmDpIH...jpg`)_
    - `genres` non vide (_id ou nom_)

---

## C. Test Integration

### Ajout en favoris
**Type**: Intégration
**Objectif**: Vérifier que le clic sur ⭐ côté UI envoie a requête d’ajout, que la persistance fonctionne, et que l’UI se met à jour.
**Prérequis** :
- Utilisateur **connecté**
- Un film visible (ex. `movieId = 27205`).
**Données de test** :
- `movieId = 27205` (Inception)
**Etapes**:
1. Ouvrir **/movies/27205**
2. Cliquer sur ⭐ **Ajouter aux favoris**.
3. Ouvrir **/favorites**.
**Résultats attendus**:
- L'icône ⭐ passe en **actif(blanche à jaune)**.
- "Inception" est présent dans la page **/favorites**.

---

## D. Test Sécurité / Fonctionnel

### Accès non autorisé à /favorites
**Type**: Sécurité / Fonctionnel
**Objectif**: S’assurer que l’accès à une page protégée demande une authentification et qu’aucune donnée ne fuite.
**Prérequis** :
- **Aucune** session active
**Données de test** :
- URL protégée : `/favorites`
**Etapes**:
1. Ouvrir **/favorites** en étant **déconnecté**.
2. Observer la redirection et/ou le message.
**Résultats attendus**:
- Redirection vers **/login**.