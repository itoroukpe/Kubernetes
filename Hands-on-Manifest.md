This workshop is designed for participants with basic Kubernetes knowledge, aiming to deepen their skills in defining application configurations and managing deployments.

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

#### **Part 2: Deploying a Stateful Application**

**Objective:** Deploy a database application (e.g., MySQL) using a `StatefulSet`.

1. **Overview and Concepts (5 minutes)**
   - Stateful applications: Require persistent storage and unique network identifiers
   - Use cases for `StatefulSets` and need for `PersistentVolume` (PV) and `PersistentVolumeClaim` (PVC)

2. **Setting Up Persistent Storage (15 minutes)**
   - Create `PersistentVolume` (PV) and `PersistentVolumeClaim` (PVC):
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
         path: "/mnt/data/mysql"  # For Minikube; change for cloud environment
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
           storage: 5Gi
     ```

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
               value: "yourpassword"
             volumeMounts:
             - name: mysql-persistent-storage
               mountPath: /var/lib/mysql
       volumeClaimTemplates:
       - metadata:
           name: mysql-persistent-storage
         spec:
           accessModes: ["ReadWriteOnce"]
           resources:
             requests:
               storage: 5Gi
     ```

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

Feel free to adjust the time or depth for each section based on the audience's familiarity with Kubernetes. Let me know if you’d like to refine or expand any specific area!
