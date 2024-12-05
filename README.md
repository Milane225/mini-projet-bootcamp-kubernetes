# Déploiement de WordPress avec des Manifests Kubernetes

Ce projet consiste à déployer une application WordPress en utilisant exclusivement des manifests Kubernetes, sans utiliser HELM.

## Objectifs du projet

- Créer un namespace pour isoler l'espace de travail
- Monter des volumes **PV** et **PVC** pour persister les données
- Déployer une base de données MySQL à l'aide d'un **Deployment** qui va utiliser le volume **PVC**.
- Exposer le pod MySQL à l'aide d'un service de type **ClusterIP**.
- Déployer WordPress avec les variables d'environnement nécessaires pour se connecter à la base de données MySQL.
- Utiliser un **volume persistant** pour stocker les données de WordPress dans le répertoire `/data` du nœud.
- Exposer l'interface frontend de WordPress à l'aide d'un service de type **NodePort**.

## Prérequis

- Un cluster Kubernetes fonctionnel (dockerlabs, Minikube ou un cluster cloud).
- `kubectl` installé et configuré pour interagir avec votre cluster.
- Accès à une image Docker de WordPress et de MySQL sur un registre public ou privé.

## Structure du projet

```plaintext
mini-projet-bootcamp-wordpress/
├── deployment-mysql.yaml             # Deployment MySQL
├── deployment-wordpress.yaml         # Deployment WordPress
├── namespace.yaml                    # Création du NameSpace
├── pv.yaml                           # Provisionnement du PersistentVolume (PV)
├── pvc.yaml                          # Association du PersistentVolume (PV) au PersistentVolumeClaims (PVC)
├── service-cluster-ip-mysql.yaml     # Service MySQL (ClusterIP)
├── service-nodeport-wordpress.yaml   # Service WordPress (NodePort)
├── README.md                         # Documentation du projet
