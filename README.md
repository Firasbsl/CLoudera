# Déploiement et Configuration de Cloudera avec Ansible

Ce projet utilise Ansible pour automatiser le déploiement et la configuration de Cloudera Manager et de ses services associés sur un cluster. Le processus est divisé en deux étapes principales : l'installation de Cloudera Manager et des agents, et la configuration des services via l'API de Cloudera Manager.

## Prérequis

Avant de commencer, assurez-vous de disposer des éléments suivants :

- **Ansible** installé sur la machine d'administration.
- **Accès réseau** aux serveurs spécifiés dans le fichier d'inventaire.
- **Identifiants Cloudera** pour l'accès à l'API (les identifiants par défaut sont `admin` pour l'utilisateur et `admin` pour le mot de passe).

## Fichier d'Inventaire (`inventory.ini`)

Le fichier d'inventaire définit les groupes et les hôtes de votre cluster. Voici un exemple de configuration :

```ini
[cloudera_manager]
192.168.0.1 

[cloudera_agents]
192.168.0.2 
192.168.0.3 

[edge]
192.168.0.4 
