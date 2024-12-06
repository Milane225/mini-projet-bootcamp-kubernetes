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

```
mini-projet-bootcamp-kubernetes/
├── deployment-mysql.yml             # Deployment MySQL
├── deployment-wordpress.yml         # Deployment WordPress
├── namespace.yml                    # Création du NameSpace
├── pv.yml                           # Provisionnement du PersistentVolume (PV)
├── pvc.yml                          # Association du PersistentVolume (PV) au PersistentVolumeClaims (PVC)
├── service-cluster-ip-mysql.yml     # Service MySQL (ClusterIP)
├── service-nodeport-wordpress.yml   # Service WordPress (NodePort)
├── README.md                        # Documentation du projet
````


## Déploiement

## 1- Lancement et vérification du namespace
### ==> Lancement du namespace
```
kubectl apply -f namespace.yml
```
### ==> Vérification du namespace
```
kubectl get namespace
```

## 2- Provisionnement du PersistentVolume (PV)
### ==> Provisionnement du PersistentVolume (PV)
```
kubectl apply -n mini-projet-wordpress -f pv.yml
```
### ==> Vérification du PersistentVolume (PV)
````
kubectl get pv -n mini-projet-wordpress -o wide
````
````
kubectl get pv -n mini-projet-wordpress pv -o wide
````
````
kubectl describe pv -n mini-projet-wordpress pv
````

## 3- Association du PersistentVolume (PV) au PersistentVolumeClaims (PVC)
### ==> Association du PersistentVolume (PV) au PersistentVolumeClaims (PVC)
```
kubectl apply -n mini-projet-wordpress -f pvc.yml
```
### ==> Vérification du PersistentVolumeClaims (PVC)
````
kubectl get pvc -n mini-projet-wordpress -o wide
````
````
kubectl get pvc -n mini-projet-wordpress pvc -o wide
````
````
kubectl describe pvc -n mini-projet-wordpress pvc
````

## 4- Création du Deployment MySQL
### ==> Création du Deployment MySQL
```
kubectl apply -n mini-projet-wordpress -f deployment-mysql.yml
```
### ==> Vérification du Deployment MySQL
````
kubectl get deploy -n mini-projet-wordpress -o wide
````

## 5- Création du Service MySQL (ClusterIP)
### ==> Création du Service MySQL (ClusterIP)
```
kubectl apply -n mini-projet-wordpress -f service-cluster-ip-mysql.yml
```
### ==> Vérification du Service MySQL (ClusterIP)
````
kubectl get services -n mini-projet-wordpress
````
````
kubectl get svc -n mini-projet-wordpress
````

## 6- Création du Deployment WordPress
### ==> Création du Deployment WordPress
```
kubectl apply -n mini-projet-wordpress -f deployment-wordpress.yml
```
### ==> Vérification du Deployment WordPress
````
kubectl get deploy -n mini-projet-wordpress -o wide
````

## 7- Création du Service WordPress (NodePort)
### ==> Création du Service WordPress (NodePort)
```
kubectl apply -n mini-projet-wordpress -f service-nodeport-wordpress.yml
```
### ==> Vérification du Service WordPress (NodePort)
````
kubectl get services -n mini-projet-wordpress
````
````
kubectl get svc -n mini-projet-wordpress
````

## 8- Suppressions
### ==> suppression du Service WordPress (NodePort)
```
kubectl delete -n mini-projet-wordpress -f service-nodeport-wordpress.yml
```
### ==> suppression du Service MySQL (ClusterIP)
```
kubectl delete -n mini-projet-wordpress -f service-cluster-ip-mysql.yml
```
### ==> suppression du Deployment WordPress
```
kubectl delete -n mini-projet-wordpress -f deployment-wordpress.yml
```
### ==> suppression du Deployment MySQL
```
kubectl delete -n mini-projet-wordpress -f deployment-mysql.yml
```
### ==> suppression du PersistentVolumeClaims (PVC)
```
kubectl delete -n mini-projet-wordpress -f pvc.yml
```
### ==> suppression du PersistentVolume (PV)
```
kubectl delete -n mini-projet-wordpress -f pv.yml
```
### ==> suppression du namespace
```
kubectl delete -n mini-projet-wordpress -f namespace.yml
```
