# Journal de Bord Projet Évolutif Sciensano

## Session 2 : Conteneurisation et Orchestration Multi-Conteneurs avec Docker

### 1. Structure & Configuration de base

* Écriture du fichier Web dynamique `index.php` pour valider le serveur PHP.
* Création du fichier de construction `Dockerfile` basé sur `php:8.2-apache`.
* Transfert automatique du code source vers `/var/www/html/index.php`.
* Exposition du port HTTP `80` au niveau de l'image.

### 2. Premier test du conteneur Web tout seul

* Build de l'image Docker locale `projet-docker-web`.
* Déploiement du conteneur Web avec remappage de port vers l'hôte (`8080:80`).
* Traitement du conflit de port `8080` en coupant les conteneurs isolés via `docker stop`.
* Validation du fonctionnement via le terminal avec `curl http://localhost:8080`.

### 3. Orchestration Multi-Services (`docker-compose.yml`)

* Rédaction du fichier `docker-compose.yml` liant le service Web et le service Base de Données.
* Configuration du service Web :
  * Volume de développement dynamique `./:/var/www/html`.
  * Dépendance de démarrage `depends_on: db`.
* Configuration du service Base de Données (`mysql:8.0`) :
  * Variables d'environnement : `MYSQL_ROOT_PASSWORD` et création de `sciensano_db`.
* Mappage des ports d'accès : `8080:80` (Web) et `3306:3306` (MySQL).

### 4. Validation & Contrôle Qualité

* Lancement unifié de la stack en arrière-plan via `docker-compose up -d`.
* Vérification CLI : `curl http://localhost:8080` (Code HTTP 200 / Rendu PHP valide).
* Vérification BDD : Contrôle de la création de `sciensano_db` via `docker exec`.
* Rendu Navigateur : Validation visuelle sur l'hôte via `http://localhost:8080`.