This workshop is designed for participants with basic Kubernetes knowledge, aiming to deepen their skills in defining application configurations and managing deployments.
---
3. **Writing the StatefulSet Manifest (15 minutes)**
   - Create a new file: `stateful-app.yaml`
   - Define a `StatefulSet` for MySQL:
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  serviceName: "mysql"
  replicas: 1
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports:
        - containerPort: 3306
          name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "yourpassword" # Replace with a secure password
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
volumeClaimTemplates:
  - metadata:
      name: mysql-persistent-storage
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 5Gi

```
---

### **Workshop Title:** 
**Deploying Stateful and Stateless Applications on Kubernetes**

### **Workshop Overview:**
This workshop will guide participants through the process of writing Kubernetes manifests for deploying both stateful and stateless applications. Attendees will learn the difference between stateful and stateless deployments, best practices for each, and how to leverage Kubernetes objects like Deployments, StatefulSets, PersistentVolumes (PVs), and PersistentVolumeClaims (PVCs).

### **Target Audience:**
Developers, DevOps Engineers, and System Administrators with basic knowledge of Kubernetes concepts such as Pods, Deployments, and Services.

---

### **Workshop Outline:**

#### **1. Introduction (15 minutes)**
   - Overview of Kubernetes manifests and configuration files
   - Difference between stateless and stateful applications
   - Key Kubernetes objects for managing state: Deployments, StatefulSets, PVs, PVCs

#### **2. Workshop Goals and Requirements (10 minutes)**
   - Workshop goals: Deploy both a stateless and a stateful application using Kubernetes manifests
   - Requirements: A running Kubernetes cluster (e.g., Minikube, Kind, or managed cluster), `kubectl` installed, text editor

#### **3. Setting Up the Environment (15 minutes)**
   - Ensure all participants have access to a Kubernetes cluster
   - Verify `kubectl` connectivity with `kubectl get nodes`

---

### **Hands-on Exercises:**

#### **Part 1: Deploying a Stateless Application**

**Objective:** Deploy a simple stateless web application using a Kubernetes `Deployment`.

1. **Overview and Concepts (5 minutes)**
   - Stateless applications: Typically front-end or web services, no need to retain data between sessions
   - Best fit for Kubernetes `Deployments`

2. **Writing the Manifest (15 minutes)**
   - Create a new file: `stateless-app.yaml`
   - Define a `Deployment` for a sample web application (e.g., `nginx` or a basic Node.js app):
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: stateless-app
     spec:
       replicas: 3
       selector:
         matchLabels:
           app: stateless-app
       template:
         metadata:
           labels:
             app: stateless-app
         spec:
           containers:
           - name: web
             image: nginx:latest
             ports:
             - containerPort: 80
     ```
This manifest file, `stateless-app.yaml`, defines a **Kubernetes Deployment** for a stateless application. The deployment ensures high availability and scalability of the application by creating and managing multiple replicas of the application pods.

### **Explanation of the Manifest**

1. **`apiVersion: apps/v1`**  
   - Specifies the API version of the Kubernetes object. In this case, `apps/v1` is used for the Deployment object.

2. **`kind: Deployment`**  
   - Specifies the type of Kubernetes resource being created. Here, it is a **Deployment**, which manages the lifecycle of application pods.

3. **`metadata:`**  
   - Contains metadata about the Deployment.
   - `name: stateless-app`: Assigns the Deployment the name `stateless-app`.

4. **`spec:`**  
   - The `spec` section defines the desired state of the Deployment.

   - **`replicas: 3`**:  
     Specifies the number of pod replicas to run. Kubernetes ensures there are always 3 pods running for this Deployment.

   - **`selector:`**  
     Identifies which pods are managed by this Deployment. The selector matches pods with the label `app: stateless-app`.

   - **`template:`**  
     Describes the pod template, which defines how the pods created by this Deployment should look.

     - **`metadata:`**  
       - Contains labels for the pod. The label `app: stateless-app` associates the pods with the Deployment.
     
     - **`spec:`**  
       - Defines the pod specifications, including its containers.
     
       - **`containers:`**  
         - Lists the containers to be created within each pod.

         - **`- name: web`**:  
           Names the container `web` for easier identification.

         - **`image: nginx:latest`**:  
           Specifies the container image to use. In this case, it uses the latest version of the official `nginx` image from a container registry like Docker Hub.

         - **`ports:`**  
           - Specifies the ports exposed by the container.
           - **`- containerPort: 80`**:  
             Exposes port `80` inside the container. This is the default HTTP port for `nginx` to serve web traffic.

---

### **What This Manifest Does**
- Creates a **Deployment** named `stateless-app`.
- Ensures **3 replicas** of the pod are always running.
- Each pod runs a container with the `nginx:latest` image and exposes port `80` for HTTP traffic.
- Kubernetes monitors and maintains the desired state (e.g., if a pod crashes, it will automatically recreate it).

---


### **Key Benefits of This Deployment**
1. **High Availability**: Ensures multiple replicas of the app are running for fault tolerance.
2. **Scalability**: You can easily scale the application by changing the `replicas` count and reapplying the manifest.
3. **Stateless Nature**: Each pod is independent and does not maintain a persistent state, making it ideal for load-balanced applications. 

---
3. **Deploying and Testing (10 minutes)**
   - Apply the manifest: `kubectl apply -f stateless-app.yaml`
   - Verify the deployment: `kubectl get deployments`
   - Expose the deployment using a `Service` for external access:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: stateless-service
     spec:
       selector:
         app: stateless-app
       ports:
         - protocol: TCP
           port: 80
           targetPort: 80
       type: LoadBalancer
     ```
   - Apply and verify service accessibility
  ---
This manifest file defines a **Kubernetes Service** that exposes the `stateless-app` Deployment to external traffic, allowing users to access the application from outside the cluster.

---

### **Explanation of the Manifest**

1. **`apiVersion: v1`**  
   - Specifies the API version for the resource. For a **Service**, `v1` is the appropriate API version.

2. **`kind: Service`**  
   - Indicates the type of Kubernetes resource being created. In this case, it's a **Service**, which is used to expose and route traffic to the pods.

3. **`metadata:`**  
   - Contains metadata about the Service.
   - **`name: stateless-service`**: Assigns the Service the name `stateless-service`. This is how the Service will be referred to within the cluster.

4. **`spec:`**  
   - Defines the desired state and configuration of the Service.

   - **`selector:`**  
     - Specifies how the Service finds the pods it routes traffic to.  
     - **`app: stateless-app`**: Matches pods with the label `app: stateless-app`, ensuring the Service routes traffic to the `stateless-app` pods created by the Deployment.

   - **`ports:`**  
     - Configures the ports for the Service.
     - **`protocol: TCP`**: Specifies the communication protocol (default is TCP).
     - **`port: 80`**: This is the port on which the Service will be exposed to the external world.
     - **`targetPort: 80`**: The port on the pods (containers) where the traffic will be routed. This corresponds to the `containerPort: 80` in the Deployment.

   - **`type: LoadBalancer`**  
     - Specifies the Service type.
     - **`LoadBalancer`**: Exposes the Service externally by provisioning a load balancer in the cloud environment (e.g., AWS, GCP, Azure). This allows external users to access the application through the load balancer's IP or hostname.

---

### **How It Works**
1. **Selector Mechanism**:  
   - The `selector` (`app: stateless-app`) ensures that traffic is routed to the pods created by the `stateless-app` Deployment.

2. **Port Mapping**:  
   - Requests arriving at the Service's `port: 80` are forwarded to the containers' `targetPort: 80`.

3. **External Access**:  
   - The `type: LoadBalancer` creates a cloud provider-managed load balancer. The load balancer assigns an external IP or DNS name, making the application accessible from outside the Kubernetes cluster.

---

### **Steps to Use This Manifest**
1. Save the manifest to a file named `stateless-service.yaml`.
2. Apply the manifest to the cluster:
   ```bash
   kubectl apply -f stateless-service.yaml
   ```
3. Check the Service to get the external IP address:
   ```bash
   kubectl get services
   ```

   Example output:
   ```
   NAME               TYPE           CLUSTER-IP      EXTERNAL-IP    PORT(S)        AGE
   stateless-service  LoadBalancer   10.100.200.1    35.200.120.5   80:30080/TCP   1m
   ```
   - **`EXTERNAL-IP`**: The public IP or DNS name assigned by the load balancer.

4. Access the application by visiting the external IP in your browser:
   ```
   http://<EXTERNAL-IP>
   ```

---
To expose the port using a **NodePort** service instead of a **LoadBalancer**, you need to modify the `type` field in the Service manifest to `NodePort`. Here's the updated manifest:

---

### NodePort Service Manifest for `stateless-service`

```yaml
apiVersion: v1
kind: Service
metadata:
  name: stateless-service
spec:
  selector:
    app: stateless-app
  ports:
    - protocol: TCP
      port: 80         # Service port exposed to external clients
      targetPort: 80    # Port on the pod to forward traffic to
      nodePort: 30080   # Fixed NodePort (optional; Kubernetes will assign one if omitted)
  type: NodePort
```

---

### Explanation of Changes:
1. **`type: NodePort`**:
   - Changed the service type to **NodePort** to expose the service on a port of the Kubernetes nodes.

2. **`nodePort: 30080`**:
   - Specifies a fixed NodePort to use for exposing the service. If omitted, Kubernetes assigns a random port in the range 30000–32767.

3. **`port` vs `targetPort`**:
   - **`port: 80`**: The port on the service where external traffic is received.
   - **`targetPort: 80`**: The port on the container in the pod where the service forwards traffic.

---

### Steps to Apply the Manifest:
1. Save the updated manifest to a file, e.g., `stateless-service.yaml`.
2. Apply the manifest:
   ```bash
   kubectl apply -f stateless-service.yaml
   ```

3. Verify the service:
   ```bash
   kubectl get services
   ```

   Example output:
   ```
   NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)          AGE
   stateless-service  NodePort    10.96.123.456    <none>        80:30080/TCP     2m
   ```

---

### Access the Application:
1. Use the **Node IP** of any worker node in the cluster.
2. Combine it with the **NodePort** (e.g., `30080`).

Example URL:
```
http://<Node-IP>:30080
```

This configuration ensures the application is exposed to external traffic through a NodePort service on all Kubernetes worker nodes.
---
### **Benefits of This Service Configuration**
1. **External Accessibility**: Enables users to access the application from outside the Kubernetes cluster.
2. **Load Balancing**: Distributes incoming traffic across the available pod replicas, ensuring high availability and reliability.
3. **Dynamic Pod Management**: Automatically updates routing when pods are added or removed, as long as they match the `selector`.

---

### **Use Cases**
- Exposing stateless web applications (e.g., Nginx, Node.js) to the internet.
- Providing external users access to services running in a Kubernetes cluster.
- Simplifying traffic distribution and scaling through the use of a cloud-managed load balancer.
  ---
   

#### **Part 2: Deploying a Stateful Application**

**Objective:** Deploy a database application (e.g., MySQL) using a `StatefulSet`.

1. **Overview and Concepts (5 minutes)**
   - Stateful applications: Require persistent storage and unique network identifiers
   - Use cases for `StatefulSets` and need for `PersistentVolume` (PV) and `PersistentVolumeClaim` (PVC)

2. **Setting Up Persistent Storage (15 minutes)**
   - Create `PersistentVolume` (PV) and `PersistentVolumeClaim` (PVC): mysql-pv.yaml
     ```yaml
     apiVersion: v1
     kind: PersistentVolume
     metadata:
       name: mysql-pv
     spec:
       capacity:
         storage: 5Gi
       accessModes:
         - ReadWriteOnce
       hostPath:
         path: "/mnt/k0s-data/mysql"  # Update this path to match the directory on your K0s node
     ---
    
### PersistentVolumeClaim (PVC) Configuration: mysql-pvc.yaml
```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 5Gi
```

---

### Steps to Apply in K0s:
1. Save the configurations into separate YAML files:
   - `mysql-pv.yaml` for the **PersistentVolume**.
   - `mysql-pvc.yaml` for the **PersistentVolumeClaim**.

2. Apply the configurations to your K0s cluster:
   ```bash
   kubectl apply -f mysql-pv.yaml
   kubectl apply -f mysql-pvc.yaml
   ```

3. Verify that the PV and PVC are created and bound:
   ```bash
   kubectl get pv
   kubectl get pvc
   ```

   Example output:
   ```
   NAME        CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM           STORAGECLASS   REASON   AGE
   mysql-pv    5Gi        RWO            Retain           Bound    default/mysql-pvc   standard               10m
   ```

   The **STATUS** should be `Bound` for both the PV and PVC.

---

### Notes for K0s:
- **`hostPath`**: The specified path (`/mnt/k0s-data/mysql`) must exist on the node running K0s. Ensure the directory is created with the necessary permissions:
   ```bash
   mkdir -p /mnt/k0s-data/mysql
   chmod 777 /mnt/k0s-data/mysql
   ```

- For a multi-node K0s setup, you should use a storage solution like NFS, GlusterFS, or a cloud-based storage provider for shared access, instead of `hostPath`, as `hostPath` is node-specific.

This configuration enables persistent storage for applications in K0s by using local paths or extending it for distributed setups with storage solutions.

---
3. **Writing the StatefulSet Manifest (15 minutes)**
   - Create a new file: `stateful-app.yaml`
   - Define a `StatefulSet` for MySQL:
     ```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: mysql
spec:
  selector:
    matchLabels:
      app: mysql
  serviceName: "mysql"
  replicas: 1
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:5.7
        ports:
        - containerPort: 3306
          name: mysql
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: "yourpassword" # Replace with a secure password
        volumeMounts:
        - name: mysql-persistent-storage
          mountPath: /var/lib/mysql
  volumeClaimTemplates:
  - metadata:
      name: mysql-persistent-storage
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 5Gi

     ```
---

### Headless Service Manifest
Save this as `mysql-service.yaml`:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: mysql
spec:
  ports:
  - port: 3306
    targetPort: 3306
  clusterIP: None # Creates a headless service
  selector:
    app: mysql
```

---

### Notes:
- The `volumeClaimTemplates` ensures that each replica gets its own PVC dynamically.
- Ensure that the previously created `PersistentVolume` and `PersistentVolumeClaim` are in the same namespace as this StatefulSet to ensure correct binding.
- Replace `yourpassword` in `MYSQL_ROOT_PASSWORD` with a secure value.

This configuration ensures persistent storage for MySQL in your **K0s** cluster while leveraging StatefulSets to manage the stateful nature of the application.
---
4. **Deploying and Testing (10 minutes)**
   - Apply the StatefulSet: `kubectl apply -f stateful-app.yaml`
   - Verify pod creation and persistent storage with `kubectl get pods`, `kubectl get pvc`
   - Test database connectivity within the cluster (e.g., `kubectl exec -it mysql-0 -- mysql -uroot -p`)

#### **4. Cleaning Up Resources (5 minutes)**
   - Delete the created resources: `kubectl delete -f stateless-app.yaml -f stateful-app.yaml`
   - Optionally delete the PV and PVC.

---

### **5. Q&A and Troubleshooting (10 minutes)**
   - Address any challenges participants faced during deployment
   - Common troubleshooting tips for `StatefulSets` and `Deployments`

### **6. Wrap-Up and Additional Resources (5 minutes)**
   - Recap key differences and use cases for stateless vs. stateful applications
   - Share links to Kubernetes documentation and tutorials for further learning

---

### **Workshop Outcome:**
By the end of this workshop, participants will be able to:
- Write and deploy Kubernetes manifests for both stateless and stateful applications.
- Understand the role of `PersistentVolumes` and `PersistentVolumeClaims` in stateful applications.
- Use Kubernetes `Deployment` and `StatefulSet` objects to manage different types of application workloads.

---


