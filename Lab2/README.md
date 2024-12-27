# Lab2: Déploiement d'une Application Nginx sur Kubernetes

### Préparation du Système

Bienvenue à Ubuntu 22.04.4 LTS (GNU/Linux 5.15.0-116-generic x86\_64).

- Documentation : [https://help.ubuntu.com](https://help.ubuntu.com)
- Management : [https://landscape.canonical.com](https://landscape.canonical.com)
- Support : [https://ubuntu.com/pro](https://ubuntu.com/pro)

#### Informations Système (au 26 Dec 2024, 17:08 UTC)

- Charge du système : 0.93
- Utilisation de la partition root : 19.8% de 30.34GB
- Utilisation de la mémoire : 23%
- Utilisation de la swap : 0%
- Processus actifs : 183
- Utilisateurs connectés : 0
- IPv4 pour eth0 : 10.0.2.29
- IPv6 pour eth0 : fd00::a00:27ff\:fec8:9864



---

## Étapes de Déploiement

### Étape 1 : Création d'un Pod

1. Création d'un fichier `mypod.yaml` :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers:
  - name: nginx-container
    image: nginx:latest
    ports:
    - containerPort: 80
```

2. Application du fichier :

```bash
kubectl apply -f mypod.yaml
```

3. Vérification de l'état des pods :

```bash
kubectl get pods
```

### Étape 2 : Création d'un ReplicaSet

1. Création d'un fichier `myreplicaset.yaml` :

```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      labels:
        app: my-nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

2. Application du fichier :

```bash
kubectl apply -f myreplicaset.yaml
```

3. Vérification de l'état des ReplicaSets et des pods :

```bash
kubectl get replicasets
kubectl get pods
```

### Étape 3 : Création d'un Deployment

1. Création d'un fichier `mydeployment.yaml` :

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-nginx
  template:
    metadata:
      labels:
        app: my-nginx
    spec:
      containers:
      - name: nginx-container
        image: nginx:latest
        ports:
        - containerPort: 80
```

2. Application du fichier :

```bash
kubectl apply -f mydeployment.yaml
```

3. Vérification de l'état des deployments et des pods :

```bash
kubectl get deployments
kubectl get pods
```

### Étape 4 : Création d'un Service

1. Création d'un fichier `myservice.yaml` :

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  type: NodePort
  selector:
    app: my-nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
    nodePort: 30001
```

2. Application du fichier :

```bash
kubectl apply -f myservice.yaml
```

3. Vérification de l'état des services :

```bash
kubectl get services
```

### Étape 5 : Accès à l'Application

1. Récupération de l'adresse IP du noeud :

```bash
hostname -I
```

2. Accès à l'application via l'URL suivante :

```
http://<node-ip>:30001/
```

---

**Exemple d'URL :**
Si l'adresse IP du noeud est `192.168.70.118`, accès à :

```
http://192.168.70.118:30001/
```

**Félicitations !** Votre application Nginx est déployée avec succès sur Kubernetes.

