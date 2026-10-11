# Conteneurisation d'une application LAMP avec Docker

Ce projet reprend l'application PHP du projet Ansible et la fait tourner dans des conteneurs Docker. Le serveur web Apache + PHP et la base MySQL sont lancés ensemble avec Docker Compose. Le détail de ma démarche est dans [JOURNAL.md](JOURNAL.md).

## Fichiers

- `Dockerfile` : construit l'image PHP 8.2 avec Apache et y copie `index.php`
- `docker-compose.yml` : lance le serveur web et la base MySQL 8.0
- `index.php` : page de test qui affiche la version de PHP
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

Tester la page dans le navigateur :
