# Journal de Bord du  Projet Docker

## Conteneuriser un serveur Apache, son interpréteur PHP et une page de test en php 

### 1. Structure & Configuration de base (Le Dockerfile)

* **`FROM php:8.2-apache`** : Récupère l'image officielle de PHP avec Apache sur le Docker Hub pour créer une image personnalisée et utilisable.
* **Création du `index.php`** : Écriture d'un fichier PHP de test pour valider le bon fonctionnement du serveur web Apache et de son interpréteur PHP.
* **`COPY index.php /var/www/html/index.php`** : Permet de copier le fichier `index.php` de la VM vers l'intérieur du conteneur. Par défaut, Apache cherche ses fichiers web à cet emplacement (`/var/www/html/`) pour les afficher.
* **`EXPOSE 80`** : Indique que le conteneur est configuré pour recevoir du trafic sur le port 80, qui est le port par défaut utilisé par Apache.

---

### 2. Le cheminement d'une requête vers le conteneur

1. **Envoi de la requête** : On envoie une requête vers le serveur (depuis un navigateur ou via la commande `curl`) sur l'adresse `http://192.168.56.101:8080`.
2. **Réception sur la VM** : La machine hôte (la VM Ubuntu) reçoit la requête sur le port **`8080`**.
3. **Redirection par Docker** : Le *Docker Daemon* (le moteur Docker) surveille ce port `8080` et redirige le trafic vers le port interne du conteneur (le port `80`).
4. **Interprétation et Réponse** : Le service Apache à l'intérieur du conteneur lit le fichier `index.php`, le fait interpréter par PHP, et renvoie la page web en réponse.

---

### 3. Orchestration Multi-Services (`docker-compose.yml`)

* **`version: '3.8'`** : Spécifie la version de la syntaxe utilisée pour le fichier Docker Compose.
* **`services:`** : Permet de déclarer l'ensemble des conteneurs du projet (ici, `web` et `db`).
* **Configuration du service `web`** :
  * **`build: .`** : Construit l'image locale en se basant sur le `Dockerfile` situé dans le dossier courant.
  * **`ports: "8080:80"`** : Fait la liaison entre le port alternatif `8080` de la VM et le port `80` du conteneur.
  * **`volumes: .:/var/www/html`** : Synchronise en direct le dossier de la VM avec le dossier du conteneur. Ainsi, toute modification du fichier `index.php` est prise en compte instantanément, sans avoir à reconstruire l'image.
  * **`depends_on: db`** : Assure que le conteneur de la base de données démarre en premier, avant le conteneur web.
* **Configuration du service Base de Données (`db`)** :
  * **`image: mysql:8.0`** : Télécharge l'image officielle de MySQL 8.0 depuis le Docker Hub.
  * **`environment:`** : Configure automatiquement les paramètres au premier lancement (mot de passe root `test1234` et création de la base de données `sciensano_db`).
  * **`ports: "3306:3306"`** : Ouvre le port d'écoute standard de MySQL pour permettre les échanges de données.

---

### 4. Validation & Contrôle

* **Lancement** : `docker compose up -d` pour démarrer toute l'architecture en arrière-plan.
* **Test CLI** : `curl http://192.168.56.101:8080` (vérifie le code HTTP 200 et la bonne interprétation du PHP).
* **Test Navigateur** : Validation visuelle directe sur l'hôte via `http://192.168.56.101:8080`.
