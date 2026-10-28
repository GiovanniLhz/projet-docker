Journal de Bord du Projet Kubernetes
Orchestration de conteneurs avec Kubernetes (MicroK8s) et résolution d'incidents système
1. Architecture et Ingestion Kubernetes
Déploiement d'un cluster léger via MicroK8s sur l'environnement Ubuntu Server (`192.168.56.101`).
Utilisation de `kubectl` pour administrer, créer et inspecter les objets du cluster.
Séparation des rôles : Un bloc pour les Pods (les conteneurs) et un bloc pour le réseau (le service).
---
2. Déchiffrage et Configuration (`web-deployment.yaml`)
Le Déploiement (`Deployment`) :
`apiVersion: apps/v1` : Indique à l'API de Kubernetes d'utiliser la version 1 de la catégorie "apps" pour comprendre et exécuter le déploiement.
`kind: Deployment` : Construit l'objet Deployment qui va gérer et maintenir nos Pods en vie.
`metadata:` / `name: mon-site-web-deployment` : Sert à identifier et nommer le déploiement.
`spec:` : Les caractéristiques techniques qui donnent les ordres sur ce que le Deployment doit faire.
`replicas: 2` : Lance et maintient 2 copies identiques (les Pods) en même temps pour assurer la haute disponibilité.
`selector:` & `matchLabels:` : Critère de sélection pour que Kubernetes reconnaisse les Pods grâce au badge `app: mon-site-web`.
`template:` : Le moule réutilisable qui fabrique les Pods (ce qui évite de réécrire toute la configuration).
`containers:` : Décrit le conteneur à l'intérieur du Pod :
`name: nginx` : Le nom du conteneur.
`image: nginx:latest` : Télécharge la dernière image officielle de Nginx depuis le site officiel (Docker Hub) avec ses dépendances.
`containerPort: 80` : Le port sur lequel Nginx écoute à l'intérieur du Pod (le port web par excellence).
Le Service Réseau (`Service - NodePort`) :
`apiVersion: v1` & `kind: Service` : Sélectionne la catégorie Service pour créer un point d'entrée réseau (un "standardiste").
`metadata:` / `name: mon-site-web-service` : Nom unique donné au Service.
`type: NodePort` : Ouvre une porte externe sur notre VM pour intercepter les requêtes venant de l'extérieur.
`selector:` / `app: mon-site-web` : Permet au Service de cibler et d'envoyer le trafic uniquement vers les bons Pods.
`ports:` :
`port: 80` : Le port interne du Service.
`targetPort: 80` : La cible où le trafic est redirigé (le port 80 du conteneur Nginx).
`nodePort: 30080` : Le port magique externe (compris entre 30000 et 32767) utilisé pour envoyer une requête vers la VM : `http://192.168.56.101:30080`.
---
3. Résolution d'Incident SysAdmin (Disque & LVM)
Diagnostic des Pods en `Pending` : Utilisation de `kubectl describe pod` pour identifier un Taint de sécurité lié à une pression disque (`disk-pressure`).
Nettoyage Docker : Suppression des images et du cache inutilisés via `sudo docker system prune -a -f`.
Extension de partition LVM : Prise de conscience que l'installateur Ubuntu réserve par défaut la moitié de l'espace disque. Utilisation de `lvextend -l +100%FREE` et `resize2fs` pour faire passer la partition principale de 12 Go à 23 Go et éliminer définitivement la saturation.
---
4. Validation & Contrôle
Application : `microk8s kubectl apply -f web-deployment.yaml` pour appliquer la configuration.
Test CLI :
Vérification des Pods en `Running` (`1/1`) via `microk8s kubectl get pods`.
Contrôle du Service via `microk8s kubectl get services`.
Test Navigateur : Validation visuelle directe sur l'hôte via `http://192.168.56.101:30080` (affichage de la page "Welcome to nginx!").
