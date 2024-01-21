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
Below is the details on the above file

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
```

- `apiVersion`: Specifies the version of the Kubernetes API. In this case, it's using the `apps/v1` API version for Deployments.
- `kind`: Defines the type of Kubernetes resource, and in this case, it's a Deployment.
- `metadata`: Contains metadata about the Deployment, including its name.

```yaml
spec:
  replicas: 2
```

- `spec`: Describes the desired state for the Deployment.
- `replicas: 2`: Specifies that the Deployment should have 2 replicas, meaning it will create and maintain two instances (pods) of the application.

```yaml
  selector:
    matchLabels:
      app: mysql
```

- `selector`: Specifies how the Deployment identifies which pods to manage.
- `matchLabels`: Specifies that the pods managed by this Deployment should have labels matching the specified criteria.
- `app: mysql`: The label key-value pair that the Deployment uses to select pods. Pods with the label `app: mysql` will be managed by this Deployment.

```yaml
  template:
    metadata:
      labels:
        app: mysql
```

- `template`: Describes the pod template that will be used to create new pods.
- `metadata`: Contains metadata for the pod template.
- `labels`: Specifies labels for pods created from this template.
- `app: mysql`: The label key-value pair assigned to pods created from this template.

```yaml
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

- `spec`: Describes the specification of the pod template.
- `containers`: Specifies the containers within the pod.
- `name: mysql-container`: The name assigned to the container.
- `image: mysql:latest`: The Docker image used for the container, in this case, the latest version of the MySQL image.
- `env`: Specifies environment variables for the container.
  - `name: MYSQL_ROOT_PASSWORD`: Sets the root password for MySQL.
  - `name: MYSQL_DATABASE`: Specifies the name of the MySQL database to be created.

In summary, this Kubernetes Deployment configuration defines a desired state where two replicas of a MySQL database are managed. Each replica is created from a pod template, and the MySQL container within each pod is configured with specific environment variables, including the root password and the name of the database to be created.


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
