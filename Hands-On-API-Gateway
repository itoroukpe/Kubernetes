This workshop will guide participants through setting up K0, deploying an API Gateway, and managing API traffic within the Kubernetes cluster.

---

### **Workshop Title:**
Deploying API Gateway in Kubernetes Using K0 – Zero Friction Kubernetes Tool

---

### **Overview:**
This workshop introduces K0, a tool designed to simplify Kubernetes cluster management and reduce friction in deploying applications. Participants will learn how to leverage K0 to set up and deploy an API Gateway in a Kubernetes environment, allowing seamless management of API traffic for various applications.

---

### **Workshop Objectives:**

1. **Understanding K0 Basics**: Familiarize participants with the K0 tool and its core functionalities.
2. **Setting Up API Gateway**: Deploy an API Gateway using K0 in a Kubernetes cluster.
3. **Managing API Traffic**: Configure the API Gateway to route requests and apply traffic policies.
4. **Best Practices**: Learn security, scaling, and monitoring techniques for API Gateways in Kubernetes.

---

### **Prerequisites:**

- Basic understanding of Kubernetes and API Gateways
- Access to a Kubernetes cluster (using Minikube or a managed Kubernetes service)
- `kubectl` and K0 installed on the local machine

---

### **Agenda:**

#### **Part 1: Introduction to K0 and API Gateway Concepts**

1. **Introduction to K0**
   - Overview of K0, its features, and benefits for Kubernetes management.
   - Explanation of the “zero-friction” approach to deploying applications.

2. **Overview of API Gateway in Kubernetes**
   - What is an API Gateway and its role in a microservices architecture.
   - Benefits of using an API Gateway for traffic routing, load balancing, and authentication.

---

#### **Part 2: Setting Up the Kubernetes Environment**

1. **Initializing K0**
   - Setting up K0 to manage your Kubernetes cluster.
   - Initializing a new Kubernetes environment using K0:
     ```bash
     k0 init
     ```

2. **Configuring K0 to Access the Cluster**
   - Connect K0 to an existing Kubernetes cluster:
     ```bash
     k0 connect
     ```
   - Verify the connection by listing cluster nodes and namespaces:
     ```bash
     k0 list nodes
     k0 get namespaces
     ```

3. **Understanding K0 Commands for Cluster Management**
   - Brief walkthrough of essential K0 commands for cluster administration.
   - Explanation of how K0 simplifies multi-cluster management.

---

#### **Part 3: Deploying the API Gateway Using K0**

1. **Preparing the API Gateway Deployment**
   - Overview of the API Gateway deployment and service requirements.
   - Create a configuration file (e.g., `api-gateway.yaml`) with necessary resources:
     ```yaml
     # api-gateway.yaml
     apiVersion: networking.k8s.io/v1
     kind: Ingress
     metadata:
       name: api-gateway
       annotations:
         nginx.ingress.kubernetes.io/rewrite-target: /
     spec:
       rules:
         - host: <your-domain>
           http:
             paths:
               - path: /
                 pathType: Prefix
                 backend:
                   service:
                     name: <service-name>
                     port:
                       number: 80
     ```

2. **Deploying the API Gateway with K0**
   - Use K0 to apply the configuration:
     ```bash
     k0 apply -f api-gateway.yaml
     ```
   - Check deployment status:
     ```bash
     k0 get ingress
     ```
   - Confirm that the API Gateway is running and accessible.

3. **Configuring Routing Rules**
   - Update the Ingress configuration with multiple service paths to route requests:
     ```yaml
     - path: /service1
       backend:
         service:
           name: service1
           port:
             number: 80
     - path: /service2
       backend:
         service:
           name: service2
           port:
             number: 80
     ```
   - Apply updated configuration with K0:
     ```bash
     k0 apply -f api-gateway.yaml
     ```

---

#### **Part 4: Managing API Gateway Traffic**

1. **Setting Up Load Balancing**
   - Explain load balancing strategies within the API Gateway (e.g., round-robin, least connections).
   - Configure basic load balancing by adding replicas to services:
     ```bash
     k0 scale deployment <service-name> --replicas=3
     ```

2. **Enforcing Rate Limiting and Throttling**
   - Add rate-limiting annotations to the Ingress configuration:
     ```yaml
     metadata:
       annotations:
         nginx.ingress.kubernetes.io/limit-connections: "20"
         nginx.ingress.kubernetes.io/limit-rps: "10"
     ```
   - Apply configuration to enforce rate limiting.

3. **Enabling HTTPS for Secure Traffic**
   - Generate or install SSL certificates.
   - Configure the API Gateway to use HTTPS:
     ```yaml
     spec:
       tls:
         - hosts:
             - <your-domain>
           secretName: tls-secret
     ```
   - Reload the API Gateway with the new HTTPS configuration:
     ```bash
     k0 apply -f api-gateway.yaml
     ```

---

#### **Part 5: Monitoring and Troubleshooting**

1. **Viewing Gateway Logs**
   - Monitor API Gateway logs for request details and error tracking:
     ```bash
     k0 logs -f <gateway-pod-name>
     ```

2. **Troubleshooting Failed Requests**
   - Use K0 to describe resources for troubleshooting:
     ```bash
     k0 describe ingress api-gateway
     k0 describe pod <gateway-pod-name>
     ```

3. **Using K0 for Real-Time Traffic Analysis**
   - Enable real-time monitoring and visualization tools (like Prometheus and Grafana) with K0 integration.

---

#### **Part 6: Scaling and Managing API Gateway Resources**

1. **Auto-scaling the Gateway**
   - Set up auto-scaling policies based on CPU and memory:
     ```yaml
     kind: HorizontalPodAutoscaler
     apiVersion: autoscaling/v2
     metadata:
       name: api-gateway-autoscaler
     spec:
       scaleTargetRef:
         apiVersion: apps/v1
         kind: Deployment
         name: api-gateway
       minReplicas: 2
       maxReplicas: 10
       metrics:
         - type: Resource
           resource:
             name: cpu
             target:
               type: Utilization
               averageUtilization: 80
     ```
   - Apply the auto-scaling configuration using K0:
     ```bash
     k0 apply -f hpa.yaml
     ```

2. **Configuring Custom Metrics for Traffic Patterns**
   - Explain custom metrics for tracking traffic patterns.
   - Apply custom metrics configuration and view traffic insights through monitoring.

---

### **Wrap-Up and Q&A**

- **Review Key K0 Commands and Best Practices**
- **Discuss Advanced API Gateway Use Cases**
- **Q&A Session for Participant Queries**

---

### **Post-Workshop Assignment**

Assign participants to deploy a different API Gateway (such as Kong or Ambassador) using K0. They should configure routing rules, set up SSL, and add basic load balancing.

---

This workshop covers all aspects of deploying and managing an API Gateway with K0, providing hands-on experience in practical scenarios. Let me know if you need more details in any section!
