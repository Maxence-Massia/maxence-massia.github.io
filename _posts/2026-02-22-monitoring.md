---
title: "Monitoring – Prometheus & Grafana"
date: 2026-02-22 10:05:00 +0100
categories: [Projets]
tags: [Monitoring, Prometheus, Grafana, Linux]
---

# 📊 Projet Monitoring

Dans le cadre de l’infrastructure M2L, j’ai mis en place une solution de supervision afin de surveiller l’état des machines et des services.

## 🖥️ Installation

Le monitoring est hébergé sur une machine virtuelle Linux située sur le serveur **PVE1**.

J’ai installé :

- Prometheus (collecte des métriques)
- Grafana (visualisation)
- Node Exporter sur la machine Linux supervisée

## 📈 Données surveillées

- CPU
- RAM
- espace disque
- charge système
- réseau

Ces métriques sont collectées par Prometheus et affichées dans Grafana via des dashboards.

## ⏳ Évolution prévue

Une extension du projet est envisagée afin de superviser :

- les équipements réseau (switch, routeur, point d’accès)
- les serveurs

Cette partie est actuellement en réflexion.
