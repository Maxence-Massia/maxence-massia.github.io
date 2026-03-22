---
title: "Projet Monitoring : Prometheus & Grafana"
date: 2026-03-06 10:00:00 +0100
categories: [Projets]
tags: [Monitoring, Linux, Prometheus, Grafana]
---

# Projet Monitoring

Dans le cadre de l’infrastructure M2L, j’ai mis en place une solution de supervision afin de surveiller l’état des machines et des services.

---

# Rapport de Projet : Monitoring Infrastructure avec Prometheus & Grafana

## 1. Objectif du projet
L'objectif principal est la mise en place d'une solution de **monitoring centralisée** pour surveiller en temps réel l'état de mes infrastructures (CPU, mémoire vive, stockage et flux réseau). Ce projet permet d'obtenir une vision précise des performances du routeur, Switch, Point accès.

![Schéma de l'architecture Monitoring](/assets/img/Projet/interface.png)

---

## 2. Installation et configuration de Prometheus

* **Méthode d'installation :** J'ai installé Prometheus via les dépôts officiels Ubuntu. Cette approche est avantageuse car elle inclut nativement les composants essentiels comme `alertmanager` et `node_exporter`.
* **Gestion du service :** L'installation crée automatiquement un service **systemd** (`systemctl`). Cela garantit que Prometheus se lance automatiquement au démarrage du serveur, assurant une collecte de données ininterrompue à chaque ouverture de terminal ou redémarrage.

![Statut du service Prometheus](/assets/img/Projet/systemctl.png)

---

## 3. Collecte des données avec Node Exporter

Pour que le serveur puisse analyser les métriques, j'utilise l'agent **Node Exporter** sur chaque machine cible sauf AP et Switch.

* **Sur la VM :** Le collecteur est actif et communique les données système de ma VM Proxmox.
* **Sur le Routeur :** J'ai installé un agent `node_exporter` sur le routeur pour récupérer ses informations spécifiques.
* **Configuration du serveur :** J'ai modifié le fichier `prometheus.yml` pour ajouter un nouveau `job_name` appelé `router_node`. J'y ai renseigné l'adresse IP et le port spécifique du routeur pour qu'il soit reconnu comme source de données.

![Configuration prometheus.yml](/assets/img/Projet/prometheus.png)

---

## 4. Installation de Grafana

L'installation de Grafana a été réalisée via le **dépôt officiel** pour garantir la sécurité et la simplicité des mises à jour :

1.  **Authentification :** Installation d'une clé GPG pour sécuriser et valider les paquets.
2.  **Dépôt :** Ajout du dépôt officiel Grafana aux sources du système.
3.  **Installation :** Installation de la version Enterprise via `apt`.
4.  **Service :** Comme pour Prometheus, Grafana est configuré en tant que service système pour un démarrage automatique.

---

## 5. Visualisation et Dashboard

* **Liaison des données :** Dans l'interface web de Grafana, j'ai ajouté Prometheus comme **Data Source** en renseignant l'adresse IP de mon serveur.
* **Résultat final :** J'ai configuré un tableau de bord (Dashboard) dynamique. Il me permet de visualiser toutes les statistiques et performances de ma VM et de mon routeur sous forme de graphiques clairs, précis et très complets.

![Dashboard Grafana Node Exporter](/assets/img/Projet/CPU.png)

---

## 6. Supervision Réseau avancée via SNMP (Switch & AP)

Pour intégrer les équipements réseaux (Switch Cisco/HP et Point d'Accès Wifi), j'ai mis en place le protocole **SNMP** (Simple Network Management Protocol).

* **Configuration des équipements :** Activation du service SNMP sur le Switch et l'AP avec la création d'une communauté dédiée nommée `team3`.
* **Mise en place de SNMP Exporter :** * Utilisation d'un **générateur de configuration** pour créer le fichier `snmp.yml` (gestion des MIBs).
    * Modification du fichier pour y injecter la communauté `team3`.
* **Configuration de Prometheus :** Ajout de deux jobs spécifiques (`snmp-switch` et `snmp-ap`) dans le `prometheus.yml`. J'ai configuré les cibles (IP) et spécifié les modules pour que Prometheus sache quel profil de scan utiliser.

![Cibles SNMP dans Prometheus](/assets/img/Projet/Status.png)

En cliquant sur le lien de la cible (Target) dans Prometheus, on accède directement à la page des métriques brutes générées par l'exportateur. 
![Métriques brutes reçues de l'AP](/assets/img/Projet/ap.png)

* **Visualisation Grafana :** Importation d'un dashboard spécialisé pour SNMP Exporter.
    * **Métriques clés surveillées :** Uptime des équipements, statut des ports (Up/Down), taux de transfert (Input/Output Traffic) et erreurs sur les interfaces.

![Dashboard Grafana SNMP final](/assets/img/Projet/ApSwitch.png)

  ---
