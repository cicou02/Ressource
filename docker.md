# 🐳 Guide Complet des Commandes Docker

Ce document est une référence exhaustive (Cheat Sheet) des commandes Docker, classées par catégories pour faciliter votre flux de travail. Bien que Docker possède de très nombreuses options, ce guide couvre toutes les commandes et sous-commandes principales.

---

## 📦 1. Gestion des Conteneurs (Containers)

Les conteneurs sont les instances en cours d'exécution de vos images. L'API moderne privilégie la syntaxe `docker container [COMMANDE]`, mais la syntaxe courte reste la plus utilisée.

### Cycle de vie
* **Créer et démarrer** : `docker run [OPTIONS] IMAGE [CMD]`
  * *-d* : mode détaché (arrière-plan).
  * *-p host:container* : mapping de port.
  * *-v host:container* : montage de volume.
  * *--name nom* : nommer le conteneur.
  * *--rm* : supprimer le conteneur à son arrêt.
* **Créer (sans démarrer)** : `docker create [OPTIONS] IMAGE`
* **Démarrer un conteneur arrêté** : `docker start CONTENEUR`
* **Arrêter proprement** : `docker stop CONTENEUR`
* **Redémarrer** : `docker restart CONTENEUR`
* **Mettre en pause** : `docker pause CONTENEUR`
* **Reprendre (sortir de pause)** : `docker unpause CONTENEUR`
* **Tuer (arrêt forcé)** : `docker kill CONTENEUR`
* **Attendre l'arrêt d'un conteneur** : `docker wait CONTENEUR` (renvoie le code de sortie)
* **Supprimer un conteneur arrêté** : `docker rm CONTENEUR`
* **Supprimer un conteneur en cours d'exécution** : `docker rm -f CONTENEUR`

### Informations et Interactivité
* **Lister les conteneurs actifs** : `docker ps` ou `docker container ls`
* **Lister TOUS les conteneurs (actifs et inactifs)** : `docker ps -a`
* **Afficher les logs** : `docker logs [-f] CONTENEUR` (*-f pour suivre en direct*)
* **Inspecter (détails JSON)** : `docker inspect CONTENEUR`
* **Exécuter une commande dans un conteneur actif** : `docker exec -it CONTENEUR COMMANDE` (ex: `bash` ou `sh`)
* **S'attacher à un conteneur en cours** : `docker attach CONTENEUR`
* **Afficher les processus internes** : `docker top CONTENEUR`
* **Afficher les statistiques (CPU, RAM, etc.)** : `docker stats [CONTENEUR]`
* **Afficher le mapping des ports** : `docker port CONTENEUR`
* **Copier des fichiers (Hôte <-> Conteneur)** : `docker cp CHEMIN_LOCAL CONTENEUR:CHEMIN_CONTENEUR` (et inversement)
* **Renommer un conteneur** : `docker rename ANCIEN_NOM NOUVEAU_NOM`
* **Mettre à jour les ressources d'un conteneur** : `docker update --cpu-shares 512 -m 300M CONTENEUR`

---

## 🖼️ 2. Gestion des Images (Images)

Les images sont les modèles à partir desquels les conteneurs sont créés. (Syntaxe moderne : `docker image [COMMANDE]`).

### Récupération et Création
* **Construire une image (depuis un Dockerfile)** : `docker build -t NOM:TAG CHEMIN` (ex: `docker build -t mon-app:1.0 .`)
* **Télécharger une image (depuis le registre)** : `docker pull IMAGE[:TAG]`
* **Créer une image à partir d'un conteneur modifié** : `docker commit CONTENEUR NOM_IMAGE:TAG`
* **Ajouter un tag à une image locale** : `docker tag IMAGE_ID NOM_IMAGE:TAG`
* **Pousser une image vers un registre** : `docker push NOM_IMAGE:TAG`

### Inspection et Nettoyage
* **Lister les images locales** : `docker images` ou `docker image ls`
* **Inspecter une image** : `docker inspect IMAGE`
* **Voir l'historique de création (couches)** : `docker history IMAGE`
* **Supprimer une image** : `docker rmi IMAGE`
* **Supprimer les images non utilisées (dangling)** : `docker image prune`

### Import / Export
* **Sauvegarder une image dans un fichier tar** : `docker save -o fichier.tar IMAGE`
* **Charger une image depuis un fichier tar** : `docker load -i fichier.tar`
* **Exporter le système de fichiers d'un conteneur** : `docker export -o fichier.tar CONTENEUR`
* **Importer un système de fichiers comme image** : `docker import fichier.tar IMAGE:TAG`

---

## 🌐 3. Gestion des Réseaux (Networks)

Les réseaux permettent aux conteneurs de communiquer entre eux de manière isolée.

* **Lister les réseaux** : `docker network ls`
* **Créer un réseau** : `docker network create NOM_RESEAU`
* **Inspecter un réseau** : `docker network inspect NOM_RESEAU`
* **Connecter un conteneur à un réseau** : `docker network connect NOM_RESEAU CONTENEUR`
* **Déconnecter un conteneur d'un réseau** : `docker network disconnect NOM_RESEAU CONTENEUR`
* **Supprimer un réseau** : `docker network rm NOM_RESEAU`
* **Supprimer tous les réseaux inutilisés** : `docker network prune`

---

## 💾 4. Gestion des Volumes (Volumes)

Les volumes sont le mécanisme recommandé pour persister les données générées et utilisées par les conteneurs Docker.

* **Lister les volumes** : `docker volume ls`
* **Créer un volume** : `docker volume create NOM_VOLUME`
* **Inspecter un volume** : `docker volume inspect NOM_VOLUME`
* **Supprimer un volume** : `docker volume rm NOM_VOLUME`
* **Supprimer tous les volumes inutilisés** : `docker volume prune`

---

## ⚙️ 5. Système et Registre

Commandes générales pour gérer votre environnement Docker et vos connexions.

* **Afficher la version de Docker** : `docker version`
* **Afficher les informations du système (conteneurs, stockage, etc.)** : `docker info`
* **Se connecter à un registre (ex: Docker Hub)** : `docker login`
* **Se déconnecter d'un registre** : `docker logout`
* **Rechercher une image sur le Docker Hub** : `docker search TERME`
* **Voir l'utilisation de l'espace disque** : `docker system df`
* **Écouter les événements en temps réel du serveur Docker** : `docker events`

### 🧹 Nettoyage Global (Prudence !)
* **Supprimer tous les éléments inutilisés (conteneurs arrêtés, réseaux, images pendantes)** : `docker system prune`
* **Supprimer TOUT ce qui n'est pas utilisé (incluant les images sans conteneur actif et les volumes)** : `docker system prune -a --volumes`

---

## 🐙 6. Docker Compose (Orchestration locale)

Docker Compose permet de définir et de gérer des applications multi-conteneurs via un fichier `docker-compose.yml`. 
*(Note : La commande moderne est `docker compose` au lieu de `docker-compose`)*

* **Démarrer tous les services en arrière-plan** : `docker compose up -d`
* **Arrêter et supprimer les conteneurs, réseaux, et volumes** : `docker compose down` (*ajouter `-v` pour les volumes*)
* **Construire ou reconstruire les services** : `docker compose build`
* **Démarrer les services existants** : `docker compose start`
* **Arrêter les services** : `docker compose stop`
* **Redémarrer les services** : `docker compose restart`
* **Lister les conteneurs du projet** : `docker compose ps`
* **Afficher les logs de tous les services** : `docker compose logs -f`
* **Exécuter une commande dans un service** : `docker compose exec SERVICE COMMANDE`