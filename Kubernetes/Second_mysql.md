Creating a Kubernetes deployment for MySQL with persistent data storage and exposing ports involves several steps.  and Kubernetes setup.

### Step 1: Set up a Kubernetes Cluster

1. **Install Minikube (for local development) or use a cloud provider (like Google Kubernetes Engine, Azure Kubernetes Service, or Amazon EKS).**
   
   ```bash
   # Example for Minikube
   minikube start
   ```

### Step 2: Create Persistent Volume (PV) and Persistent Volume Claim (PVC)

2. **Define a Persistent Volume (mysql-pv.yaml):**

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  hostPath:
    path: "/Users/arpan/Raw_data/mysql1"
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```

   ```bash
   kubectl apply -f mysql-pv.yaml
   ```

3. **Define a MySQL Deployment (mysql-deployment.yaml):**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: admin
            - name: MYSQL_DATABASE
              value: "mydatabase"
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-persistent-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-persistent-storage
          persistentVolumeClaim:
              claimName: mysql-pvc
```

   ```bash
   kubectl apply -f mysql-deployment.yaml
   ```

### Step 4: Expose MySQL Service

4. **Define a MySQL Service (mysql-service.yaml):**

   ```yaml
   apiVersion: v1
   kind: Service
   metadata:
     name: mysql-service
   spec:
     selector:
       app: mysql
     ports:
       - protocol: TCP
         port: 3306
         targetPort: 3306
     type: LoadBalancer
   ```

   ```bash
   kubectl apply -f mysql-service.yaml
   ```

   This will expose MySQL on an external IP. You can access it using this external IP.

### Step 5: Monitor and Manage

5. **Check the status and logs of your MySQL pod:**

   ```bash
   kubectl get pods
   kubectl logs <pod_name>
   ```

6. **Stop and delete the Kubernetes cluster (if needed):**

   ```bash
   minikube stop
   minikube delete
   ```

###  You can access the MySQL pod using the `kubectl exec` command. Here's an example:

```bash
kubectl exec -it <mysql-pod-name> -- mysql -u root -p
```
