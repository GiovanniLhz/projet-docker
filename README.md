# Conteneurisation d'une application LAMP avec Docker

L'objectif de ce projet est de conteneuriser une page PHP servie par Apache et de la déployer sur ma VM Ubuntu via Docker Compose, accompagnée d'une base de données MySQL prête à l'emploi. 

Contrairement au projet Ansible où les services étaient installés directement sur le système de la VM (port 80), les conteneurs permettent ici d'isoler l'application et ses dépendances en exposant le service web sur le port `8080` de la VM. Le détail de ma démarche est dans [JOURNAL.md](JOURNAL.md).

## Fichiers

- `Dockerfile` : construit l'image PHP 8.2 avec Apache et y copie `index.php`
- `docker-compose.yml` : lance le serveur web et la base de donnée MySQL 8.0
- `index.php` : page de test pour vériffier le bon fonctionnement du projet 
- `JOURNAL.md` : mon journal de bord

## Lancer le projet

Démarrer les conteneurs :

```bash
docker compose up -d --build
```

Vérifier que tout tourne :

```bash
docker compose ps
```
