This workshop is designed for beginners to intermediate users, focusing on practical commands and tasks they can immediately apply.

---

### **Workshop Title:**  
Mastering Kubernetes with `kubectl`: A Hands-on Workshop

---

### **Overview:**
This hands-on workshop is designed to provide participants with practical experience in managing Kubernetes clusters using the `kubectl` command-line tool. By the end of the workshop, participants will understand core `kubectl` commands, how to interact with cluster resources, and how to manage applications running in Kubernetes.

---

### **Workshop Objectives:**

1. **Understand the Basics**: Familiarize participants with `kubectl` and Kubernetes components.
2. **Practical Skill Development**: Teach essential `kubectl` commands and workflows to manage Kubernetes resources.
3. **Application Management**: Enable participants to create, update, and delete applications on a Kubernetes cluster.
4. **Troubleshooting and Monitoring**: Show participants how to troubleshoot applications using `kubectl` and review resource health.

---

### **Prerequisites:**

- Basic knowledge of containerization (Docker)
- Kubernetes cluster access (via Minikube, a managed Kubernetes service, or a lab-provided cluster)
- Installed `kubectl` tool on the local machine

---

### **Agenda:**

#### **Part 1: Introduction to `kubectl`**

1. **What is `kubectl`?**
   - Overview of the command-line tool
   - Key benefits and functionalities

2. **Configuring `kubectl` to connect to a Kubernetes Cluster**
   - Setting up `kubectl` config file
   - Using `kubectl config` commands (e.g., `kubectl config get-contexts`, `kubectl config use-context`)
   - Switching between multiple clusters

---

#### **Part 2: Core `kubectl` Commands and Resource Management**

1. **Exploring Cluster Resources**
   - List nodes in the cluster:
     ```bash
     kubectl get nodes
     ```
   - Describe nodes and view details:
     ```bash
     kubectl describe node <node-name>
     ```

2. **Creating and Managing Pods**
   - Deploy a simple pod using a YAML file:
     ```yaml
     # pod.yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: demo-pod
     spec:
       containers:
       - name: demo-container
         image: nginx
     ```
   - Apply the configuration:
     ```bash
     kubectl apply -f pod.yaml
     ```
   - Inspect and delete the pod:
     ```bash
     kubectl get pods
     kubectl describe pod demo-pod
     kubectl delete pod demo-pod
     ```

3. **Working with Deployments**
   - Create a deployment:
     ```bash
     kubectl create deployment demo-deployment --image=nginx
     ```
   - Scaling and updating a deployment:
     ```bash
     kubectl scale deployment demo-deployment --replicas=3
     kubectl set image deployment/demo-deployment nginx=nginx:1.21
     ```
   - Rolling back a deployment:
     ```bash
     kubectl rollout undo deployment/demo-deployment
     ```

---

#### **Part 3: Service and Networking Management**

1. **Exposing Applications**
   - Expose a deployment as a service:
     ```bash
     kubectl expose deployment demo-deployment --type=LoadBalancer --port=80
     ```
   - View service details:
     ```bash
     kubectl get services
     ```

2. **Interacting with Pods and Services**
   - Executing commands inside a pod:
     ```bash
     kubectl exec -it <pod-name> -- /bin/bash
     ```
   - Forwarding ports to access applications locally:
     ```bash
     kubectl port-forward <pod-name> 8080:80
     ```

---

#### **Part 4: Troubleshooting and Debugging**

1. **Monitoring Resource Usage**
   - Viewing logs of a pod:
     ```bash
     kubectl logs <pod-name>
     ```
   - Following logs for real-time updates:
     ```bash
     kubectl logs -f <pod-name>
     ```

2. **Diagnosing Issues**
   - Viewing resource events:
     ```bash
     kubectl get events --sort-by='.metadata.creationTimestamp'
     ```
   - Describing problematic pods:
     ```bash
     kubectl describe pod <pod-name>
     ```
   - Using `kubectl top` to monitor resource usage:
     ```bash
     kubectl top nodes
     kubectl top pods
     ```

---

#### **Part 5: Managing Configurations and Secrets**

1. **Creating and Applying ConfigMaps**
   - Create a ConfigMap:
     ```bash
     kubectl create configmap demo-config --from-literal=key=value
     ```
   - Reference ConfigMap in a deployment

2. **Handling Secrets**
   - Create a Secret:
     ```bash
     kubectl create secret generic demo-secret --from-literal=username=admin --from-literal=password=secret
     ```
   - Reference Secret in a deployment

---

#### **Part 6: Automation with `kubectl`**

1. **Using Labels and Selectors**
   - Adding labels to resources:
     ```bash
     kubectl label pod <pod-name> environment=production
     ```
   - Selecting resources with labels:
     ```bash
     kubectl get pods -l environment=production
     ```

2. **Using `kubectl apply` and `kubectl delete` for GitOps**
   - Applying configuration files to automate deployments
   - Managing versions of YAML files for continuous integration

---

### **Part 7: Workshop Challenge - Deploy and Monitor a Sample Application**

Participants will deploy a sample multi-container application, expose it, monitor logs, and troubleshoot any issues. This section reinforces their learning and tests their `kubectl` skills in a realistic scenario.

---

### **Wrap-Up and Q&A**

- **Recap Key Commands and Best Practices**
- **Q&A Session for Participant Queries**
- **Additional Resources**
  - Kubernetes Documentation
  - `kubectl` Cheatsheets and community resources

---

### **Post-Workshop Assignment:**

Assign participants to create a Kubernetes deployment of a sample app of their choice, scaling it, exposing it, and monitoring it using `kubectl` commands learned during the workshop.

---

This workshop outline provides practical, hands-on training to build confidence in using `kubectl` for managing Kubernetes resources and handling real-world scenarios. Let me know if you’d like to expand any sections further!
