This workshop is ideal for participants familiar with basic Kubernetes concepts who want practical experience managing complex applications.

---

### **Workshop Title:**
Deploying and Managing Multi-Container Applications on Kubernetes

---

### **Overview:**
This hands-on workshop teaches participants how to deploy a multi-container application in Kubernetes, expose it with a service, monitor logs, and troubleshoot common issues. By the end, participants will have the skills to deploy complex applications and manage their lifecycle on a Kubernetes cluster.

---

### **Workshop Objectives:**

1. **Understand Multi-Container Deployment**: Learn how to define and deploy multi-container applications in Kubernetes.
2. **Service Exposure**: Set up and configure services to expose applications to internal or external users.
3. **Monitoring Logs and Application Health**: Learn how to use `kubectl` commands to monitor application logs and health.
4. **Troubleshooting Skills**: Practice diagnosing and resolving common issues in multi-container deployments.

---

### **Prerequisites:**

- Basic understanding of Kubernetes resources and `kubectl` commands.
- Access to a Kubernetes cluster (via Minikube, a managed Kubernetes service, or a lab-provided cluster).
- Installed `kubectl` and text editor (VS Code, Vim, etc.).

---

### **Agenda:**

#### **Part 1: Introduction to Multi-Container Applications in Kubernetes**

1. **Overview of Multi-Container Patterns**
   - Sidecar, Ambassador, and Adapter patterns
   - Benefits of using multi-container pods (e.g., shared resources, closely coupled containers)

2. **Workshop Application Preview**
   - Brief overview of the sample application to be deployed (e.g., Nginx as a web server and Redis as a cache in the same pod)

---

#### **Part 2: Creating a Multi-Container Pod Specification**

1. **Writing a YAML Deployment for Multi-Container Pod**
   - Define a pod with two containers (e.g., Nginx and Redis) in the same pod to enable container communication.
   - Example YAML structure:

     ```yaml
     apiVersion: v1
     kind: Pod
     metadata:
       name: multi-container-pod
     spec:
       containers:
       - name: nginx-container
         image: nginx
         ports:
         - containerPort: 80
       - name: redis-container
         image: redis
         ports:
         - containerPort: 6379
     ```

2. **Applying the Configuration**
   - Use `kubectl apply` to create the pod:
     ```bash
     kubectl apply -f multi-container-pod.yaml
     ```

3. **Verifying the Deployment**
   - Check pod status and container readiness:
     ```bash
     kubectl get pods
     kubectl describe pod multi-container-pod
     ```

---

#### **Part 3: Exposing the Application**

1. **Creating a Service to Expose the Multi-Container Pod**
   - Create a Service YAML file to expose the Nginx container.
   - Example YAML structure:

     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: nginx-service
     spec:
       selector:
         app: multi-container-pod
       ports:
       - protocol: TCP
         port: 80
         targetPort: 80
       type: LoadBalancer
     ```

2. **Applying and Testing the Service**
   - Apply the Service configuration:
     ```bash
     kubectl apply -f nginx-service.yaml
     ```
   - Verify service availability and get external IP:
     ```bash
     kubectl get services
     ```
   - Access the application via the external IP to confirm it’s running.

---

#### **Part 4: Monitoring Logs and Health**

1. **Checking Container Logs**
   - Use `kubectl logs` to check logs for each container:
     ```bash
     kubectl logs <pod-name> -c nginx-container
     kubectl logs <pod-name> -c redis-container
     ```

2. **Real-Time Log Monitoring**
   - Use `kubectl logs -f` for real-time log monitoring:
     ```bash
     kubectl logs -f <pod-name> -c nginx-container
     ```

3. **Container Resource Monitoring**
   - Use `kubectl top` to monitor resource usage:
     ```bash
     kubectl top pod <pod-name>
     ```

---

#### **Part 5: Troubleshooting Common Issues**

1. **Common Multi-Container Issues**
   - Pod scheduling issues (e.g., insufficient resources)
   - Network connectivity between containers

2. **Diagnosing Pod Failures**
   - Describe the pod to inspect events and errors:
     ```bash
     kubectl describe pod <pod-name>
     ```

3. **Identifying Resource Constraints**
   - Adjust resource requests and limits to troubleshoot OOM errors or CPU throttling:
     ```yaml
     resources:
       requests:
         memory: "64Mi"
         cpu: "250m"
       limits:
         memory: "128Mi"
         cpu: "500m"
     ```

4. **Debugging Network Connectivity**
   - Use `kubectl exec` to access one container and attempt to ping or curl the other container’s localhost IP and port.

5. **Rolling Updates and Rollbacks**
   - Practice updating container images in a multi-container pod and observe Kubernetes rolling out changes.
   - Roll back in case of issues:
     ```bash
     kubectl rollout undo deployment/<deployment-name>
     ```

---

#### **Part 6: Workshop Challenge - Deploy and Manage a Custom Multi-Container Application**

In this challenge, participants will:

1. Deploy a multi-container application of their choice (e.g., WordPress with a MySQL database container).
2. Configure and expose it as a LoadBalancer service.
3. Monitor its logs and resource usage.
4. Troubleshoot any deployment or connectivity issues.

Participants can use provided YAML templates or create their configurations from scratch, reinforcing the skills gained during the workshop.

---

### **Wrap-Up and Q&A**

- **Recap Key Learnings**
- **Q&A Session for Participant Queries**
- **Additional Resources**
  - Kubernetes Documentation
  - `kubectl` Cheatsheets for Advanced Operations

---

### **Post-Workshop Assignment:**

Encourage participants to deploy a more complex application (e.g., an application with a front-end, back-end, and database) in a multi-container setup, monitor it, and resolve any deployment issues. Participants can document their experience to reflect on troubleshooting steps and findings.

---

This hands-on workshop combines guided learning with a practical challenge, building participants’ skills and confidence in deploying, monitoring, and managing multi-container applications on Kubernetes. Let me know if you’d like additional details for any specific sections!
