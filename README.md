# Consignes
1. Tronc commun minimum => tout ce qu'on a déjà fait ensemble mais avec une autre thématique que les livres et les auteurs :
* Plusieurs entités
    * Propriétés de plusieurs types dont deux types de relations différentes
    * Getters et setters
    * Repository
* Génération du schema de base de données / Migrations
* Contenu par défaut
    * Factory (Foundry)
    * Fixture / Command
* Plusieurs Controllers / Routes dont au minimum la page d'accueil de l'appli
* Une liste d'entités avec pagination + les pages de détail d'une entité (par exemple `/book/{id}`)
 
2. Une fonctionnalité avancée supplémentaire, au choix :
* Formulaire d'ajout et d'édition de contenu d'une entité (Form, AbstractType, templates twig associés, Validation Constraints, route dédiée...)
* User et session d'authentification (Security Bundle, UserInterface, formulaire de login, Factory...) : soit un user fixe défini dans la fixture, soit si vous avez envie de bosser encore plus, un mécanisme d'ajout d'utilisateur en front (= formulaire de création d'utilisateur)
 
3. Une mini documentation dans le fichier Readme.md pour me lister les routes et fonctionnalités existantes sur le projet.

# Documentation


## 1. Architecture et organisation

### 1.1. Structure principale

* `src/Controller/` : Contrôleurs de l’application, gèrent les requêtes HTTP et renvoient les réponses.
* `src/Entity/` : Entités Doctrine représentant les tables de la base de données.
* `src/Form/` : Formulaires Symfony utilisés pour la saisie utilisateur (ex: inscription).
* `src/Security/` : Gestion de l’authentification et sécurité.
* `templates/` : Vues Twig.
* `config/packages/security.yaml` : Configuration du système de sécurité.

---

## 2. Fonctionnalités principales

### 2.1. Authentification

* **Inscription** (`/register`) :
  Utilisation d’un formulaire personnalisé permettant de créer un utilisateur avec les champs :

  * Nom (`name`)
  * Email (`mail`) - utilisé comme identifiant
  * Mot de passe (avec hashage)
  * Date de naissance (`birthdayDate`)
  * Code postal (`code_postal`)

* **Connexion** (`/login`) :
  Authentification avec email et mot de passe.
  Implémentation de la fonctionnalité "Se souvenir de moi" (remember\_me).
  Utilisation d’un authentificateur personnalisé (`AppCustomAuthenticator`).

* **Déconnexion** (`/logout`) :
  Route gérée automatiquement par Symfony.

---

### 2.2. Gestion des utilisateurs

* Entité `User` avec les propriétés :

  * id, name, mail, birthdayDate, code\_postal, password, roles, etc.
* Rôle par défaut `ROLE_USER` garanti.
* Validation unique sur l’email.

---

### 2.3. Pages principales

* **Page d’accueil** (`/`) :
  Affiche un tableau de bord ou liens vers les sections principales : magasins, vigil, login, inscription.

* **Pages Magasins, Vigils** :
  Il n'y a pas de listes affichés. Problème de version entre foundry et symfony, différence entre ce qu'il me renvoie et le nom des fonctions que l'on peut ou non utiliser selon la version

---

## 3. Sécurité

* Configuration dans `config/packages/security.yaml` :

  * Gestion des hashers automatiques (`auto`).
  * Provider en mémoire pour les tests et utilisateurs enregistrés.
  * Firewall `main` configuré avec l’authentificateur personnalisé.
  * Activation de `remember_me` avec cookie sécurisé.

* Authenticator `AppCustomAuthenticator` :

  * Récupération des champs du formulaire (mail, password, ...).
  * Redirection après succès vers `app_home`.
  * Gestion des erreurs d’authentification.

---

## 4. Base de données

* Utilisation de Doctrine ORM.
* La colonne mail est unique.
* Les données sensibles comme le mot de passe sont hashées avant persistance.

## 5. Instructions d’installation

1. Cloner le dépôt.
2. Installer les dépendances avec Composer :

   ```bash
   composer install
   ```
3. Configurer la base de données dans `.env`.
4. Créer la base et exécuter les migrations :

   ```bash
   php bin/console doctrine:database:create
   php bin/console doctrine:migrations:migrate
   ```
5. Lancer le serveur de développement :

   ```bash
   symfony server:start
   ```
6. Accéder à `http://localhost:8000` pour tester.

---

## 6. Utilisation

* Inscription via `/register`.
* Connexion via `/login`.
* Après connexion, accès à la page d’accueil protégée.
