---
title: "Installation technique : Prometheus & Grafana"
date: 2026-03-06 10:00:00 +0100
categories: [Projets]
tags: [Monitoring, Linux, Prometheus, Grafana]
---

## 🛠️ Procédure d'installation du monitoring

Ce document détaille les étapes techniques que j'ai suivies pour mettre en place la solution de supervision sur ma machine virtuelle **PVE1**.

---

### 1. Installation de Prometheus
Pour commencer, j'ai récupéré Prometheus directement depuis le site officiel pour avoir la dernière version stable.

* **Téléchargement et extraction :** Utilisation de `wget` pour charger l'archive, puis extraction avec la commande `tar`.
* **Organisation du système :** J'ai déplacé le dossier extrait vers `/etc/` afin de respecter l'arborescence Linux et d'assurer une meilleure gestion des fichiers de configuration.
* **Mise en service :** Comme le logiciel ne démarre pas tout seul, j'ai créé un fichier de service système (`prometheus.service`). Cela me permet de gérer Prometheus avec la commande `sudo service prometheus start` et de garantir qu'il se relance automatiquement si la VM redémarre.

### 2. Configuration de Node Exporter
Node Exporter est le module qui permet à Prometheus de récupérer les informations réelles de la machine (CPU, RAM, etc.).

* **Installation :** J'ai suivi la même méthode (téléchargement et extraction).
* **Liaison avec Prometheus :** J'ai dû modifier le fichier de configuration principal de Prometheus (`prometheus.yml`) pour ajouter un **job_name**. J'y ai renseigné l'adresse IP de ma machine et le port utilisé par Node Exporter pour que les données remontent correctement.

### 3. Visualisation avec Grafana Enterprise
Grafana est l'outil qui sert à afficher les données sous forme de graphiques.

* **Installation :** J'ai installé la version **Grafana Enterprise** en suivant la documentation officielle. Comme pour Prometheus, j'ai configuré un service pour qu'il soit toujours actif au démarrage.
* **Connexion des données :** Dans l'interface web de Grafana, j'ai ajouté une "Data Source" de type Prometheus en entrant l'URL de mon serveur.
* **Tableau de bord :** J'ai ensuite choisi et configuré un dashboard complet. Je peux maintenant surveiller en un coup d'œil toutes les performances et l'état de santé de ma machine virtuelle.

---


### 🔗 Retour au projet
👉 [Revenir à la présentation générale du projet]({{ site.baseurl }}/posts/monitoring/)
