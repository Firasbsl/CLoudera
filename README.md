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

```
## Commandes pour Exécuter les Playbooks
1. Installation de Cloudera Manager et des Agents
Exécutez la commande suivante pour installer Cloudera Manager et les agents sur les serveurs définis dans le fichier d'inventaire :

```cloudera.yml
ansible-playbook -i inventory.ini cloudera.yml
```
2. Configuration des Services Cloudera
Une fois Cloudera Manager installé, exécutez le playbook install.yml pour configurer les services via l'API de Cloudera :
```install.yml
ansible-playbook -i inventory.ini install.yml
```
## Détails des Playbooks

### Playbook `cloudera.yml`

Le playbook `cloudera.yml` installe Cloudera Manager et les agents sur les serveurs suivants :

- **`cloudera_manager`** : Serveur principal où Cloudera Manager sera installé.
- **`cloudera_agents`** : Serveurs où les agents Cloudera seront installés.

#### Étapes :

1. **Installation des paquets requis** : `curl`, `wget`, `openjdk-8-jdk`, et `ntp`.
2. **Téléchargement et ajout du dépôt Cloudera** : Télécharge le fichier de liste de dépôts et ajoute la clé GPG.
3. **Mise à jour du cache `apt`** : Assure que les sources sont à jour.
4. **Installation des paquets Cloudera** : Installe les démons et les agents Cloudera.
5. **Démarrage et activation des services** : Assure que les services Cloudera sont démarrés et activés au démarrage.

### Playbook `install.yml`

Le playbook `install.yml` configure les services Cloudera via l'API de Cloudera Manager sur le serveur `cloudera_manager`.

#### Étapes :

1. **Attente du démarrage du serveur Cloudera Manager** : Vérifie que le serveur est en cours d'exécution sur le port `7180`.
2. **Liste des services existants** : Utilise l'API REST pour lister les services déjà configurés.
3. **Affichage des services existants** : Affiche les services récupérés via l'API pour vérification.
4. **Configuration des services Cloudera** : Configure les services suivants via un appel POST à l'API :
   - **HDFS** (Système de fichiers distribué Hadoop)
   - **YARN** (Gestionnaire de ressources)
   - **HIVE** (Data warehouse pour Hadoop)
   - **HUE** (Interface utilisateur pour Hadoop)
   - **SPARK** (Framework de calcul distribué)
   - **ZOOKEEPER** (Coordination de services distribués)
   - **KAFKA** (Système de messagerie distribué)
   - **TEZ** (Moteur de calcul distribué)
   - **HIVE_ON_TEZ** (Exécution de HIVE sur TEZ)
5. **Vérification de la réponse de l'API** : Affiche la réponse pour confirmer que la configuration a été appliquée.

## Conclusion

En suivant ces instructions, vous pourrez déployer et configurer Cloudera Manager et ses services associés sur votre cluster de manière automatisée. Pour accéder à l'interface graphique de Cloudera Manager, ouvrez votre navigateur à l'adresse [http://<IP-serveur>:7180](http://<IP-serveur>:7180).
