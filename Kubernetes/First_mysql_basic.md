# Kubernetes-tutorial

Creating a Kubernetes cluster and deploying MySQL pods involves several steps. Below are the basic steps to set up a simple Kubernetes cluster with two MySQL pods. Note that this is a basic example, and for production use, additional considerations, such as security, data persistence, and configurations, should be taken into account.

### Prerequisites:
1. **Kubernetes Installed:**
   - Ensure that you have Kubernetes installed. You can use Minikube for a local development cluster or any other Kubernetes distribution suitable for your environment.

2. **kubectl Installed:**
   - Make sure you have `kubectl` installed, which is the Kubernetes command-line tool.

### Steps to Create a Kubernetes Cluster with MySQL Pods:

1. **Start Kubernetes Cluster (Using Minikube):**
   ```bash
   minikube start
   ```

2. **Create MySQL Deployment:**
   Create a YAML file (`mysql-deployment.yaml`) with the following content:
   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: mysql-deployment
   spec:
     replicas: 2
     selector:
       matchLabels:
         app: mysql
     template:
       metadata:
         labels:
           app: mysql
       spec:
         containers:
         - name: mysql-container
           image: mysql:latest
           env:
           - name: MYSQL_ROOT_PASSWORD
             value: "password"
           - name: MYSQL_DATABASE
             value: "mydatabase"
   ```

   Apply the deployment:
   ```bash
   kubectl apply -f mysql-deployment.yaml
   ```

3. **Expose MySQL Deployment:**
   Expose the MySQL deployment to make it accessible within the cluster:
   ```bash
   kubectl expose deployment mysql-deployment --port=3306
   ```

4. **View Pods:**
   View the running MySQL pods:
   ```bash
   kubectl get pods
   ```

   Ensure that the pods are in a `Running` state before proceeding.

5. **Access MySQL Pods:**
   You can access the MySQL pods using port forwarding:
   ```bash
   kubectl port-forward pod-name 3306:3306
   ```
   Replace `pod-name` with the name of one of the MySQL pods.

6. **Connect to MySQL:**
   You can now connect to MySQL using a MySQL client. For example:
   ```bash
   mysql -h 127.0.0.1 -P 3306 -u root -p
   ```

### Steps to Stop the Kubernetes Cluster:

To stop the Kubernetes cluster (using Minikube), you can run:
```bash
minikube stop
```

This command stops the local Kubernetes cluster. You can start it again later using `minikube start`.

### Cleanup:
If you want to delete the resources created during this example, you can use the following command:
```bash
kubectl delete deployment mysql-deployment
```
