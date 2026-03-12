---
title: "Installation technique : Prometheus & Grafana"
date: 2026-03-06 10:00:00 +0100
categories: [Projets]
tags: [Monitoring, Linux, Prometheus, Grafana]
---

## 🛠️ Procédure d'installation du monitoring

Ce document détaille les étapes techniques que j'ai suivies pour mettre en place la solution de supervision sur ma machine virtuelle **PVE1**.

---

# Rapport de Projet : Monitoring Infrastructure avec Prometheus & Grafana

## 1. Objectif du projet
L'objectif principal est la mise en place d'une solution de **monitoring centralisée** pour surveiller en temps réel l'état de mes infrastructures (CPU, mémoire vive, stockage et flux réseau). Ce projet permet d'obtenir une vision précise des performances de ma machine virtuelle tournant sur l'hyperviseur **Proxmox (PVE1)** ainsi que de mon **routeur**.

---

## 2. Installation et configuration de Prometheus

* **Méthode d'installation :** J'ai installé Prometheus via les dépôts officiels Ubuntu. Cette approche est avantageuse car elle inclut nativement les composants essentiels comme `alertmanager` et `node_exporter`.
* **Gestion du service :** L'installation crée automatiquement un service **systemd** (`systemctl`). Cela garantit que Prometheus se lance automatiquement au démarrage du serveur, assurant une collecte de données ininterrompue à chaque ouverture de terminal ou redémarrage.

---

## 3. Collecte des données avec Node Exporter

Pour que le serveur puisse analyser les métriques, j'utilise l'agent **Node Exporter** sur chaque machine cible.

* **Sur la VM :** Le collecteur est actif et communique les données système de ma VM Proxmox.
* **Sur le Routeur :** J'ai installé un agent `node_exporter` sur le routeur pour récupérer ses informations spécifiques.
* **Configuration du serveur :** J'ai modifié le fichier `prometheus.yml` pour ajouter un nouveau `job_name` appelé `router_node`. J'y ai renseigné l'adresse IP et le port spécifique du routeur pour qu'il soit reconnu comme source de données.

---

## 4. Installation sécurisée de Grafana

L'installation de Grafana a été réalisée via le **dépôt officiel** pour garantir la sécurité et la simplicité des mises à jour :

1.  **Authentification :** Installation d'une clé GPG pour sécuriser et valider les paquets.
2.  **Dépôt :** Ajout du dépôt officiel Grafana aux sources du système.
3.  **Installation :** Installation de la version Enterprise via `apt`.
4.  **Service :** Comme pour Prometheus, Grafana est configuré en tant que service système pour un démarrage automatique.

---

## 5. Visualisation et Dashboard

* **Liaison des données :** Dans l'interface web de Grafana, j'ai ajouté Prometheus comme **Data Source** en renseignant l'adresse IP de mon serveur.
* **Résultat final :** J'ai configuré un tableau de bord (Dashboard) dynamique. Il me permet de visualiser toutes les statistiques et performances de ma VM et de mon routeur sous forme de graphiques clairs, précis et très complets.

---


### 🔗 Retour au projet
👉 [Revenir à la présentation générale du projet]({{ site.baseurl }}/posts/monitoring/)
