Here's a hands-on guide for setting up and working with **Kubernetes** on Ubuntu 24.04, covering installation, configuration, and practical application. This guide is divided into phases, beginning with the installation of Docker and Kubernetes components using `kubeadm`.

---

### **Phase 1: Prerequisites and Environment Setup**

#### Step 1: Prepare Virtual Machines
   - Set up at least two VMs (one for the Kubernetes Master Node and others as Worker Nodes) with **Ubuntu 24.04**. Each VM should have at least:
     - **2 CPUs**
     - **2 GB RAM** (Master: 4GB recommended)
     - **10 GB of storage**
   - Update packages:
     ```bash
     sudo apt update && sudo apt upgrade -y
     ```

#### Step 2: Install Basic Dependencies
   - Install essential packages:
     ```bash
     sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
     ```

#### Step 3: Disable Swap
   - Kubernetes requires that swap be disabled.
     ```bash
     sudo swapoff -a
     sudo sed -i '/ swap / s/^/#/' /etc/fstab
     ```

---

### **Phase 2: Docker Installation**

Docker is required to containerize applications within Kubernetes.

#### Step 1: Install Docker
   - Install Docker's prerequisites:
     ```bash
     curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
     echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
     ```
   - Install Docker:
     ```bash
     sudo apt update
     sudo apt install -y docker-ce docker-ce-cli containerd.io
     ```

#### Step 2: Enable and Start Docker
   - Start Docker and set it to run at boot:
     ```bash
     sudo systemctl enable docker
     sudo systemctl start docker
     ```

#### Step 3: Verify Docker Installation
   - Run a test to confirm Docker is installed:
     ```bash
     sudo docker run hello-world
     ```

---

### **Phase 3: Install Kubernetes Components**

#### Step 1: Add Kubernetes APT Repository
   - Add Kubernetes’ package repository:
     ```bash
     sudo curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /usr/share/keyrings/kubernetes-archive-keyring.gpg
     echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
     sudo apt update
     ```

#### Step 2: Install `kubeadm`, `kubelet`, and `kubectl`
   - Install Kubernetes components:
     ```bash
     sudo apt install -y kubelet kubeadm kubectl
     sudo apt-mark hold kubelet kubeadm kubectl
     ```

---

### **Phase 4: Configure the Kubernetes Master Node**

#### Step 1: Initialize the Kubernetes Cluster
   - Run the following on the Master Node:
     ```bash
     sudo kubeadm init --pod-network-cidr=10.244.0.0/16
     ```
   - Note the `kubeadm join` command displayed at the end of the initialization process. You will use this to add Worker Nodes later.

#### Step 2: Configure `kubectl` for the Master Node
   - Set up `kubectl` for the master:
     ```bash
     mkdir -p $HOME/.kube
     sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
     sudo chown $(id -u):$(id -g) $HOME/.kube/config
     ```

#### Step 3: Deploy a Network Plugin
   - Use the Flannel CNI plugin to enable networking between pods:
     ```bash
     kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
     ```

---

### **Phase 5: Add Worker Nodes to the Cluster**

#### Step 1: Join Worker Nodes to the Cluster
   - On each Worker Node, use the `kubeadm join` command from the Master Node setup output:
     ```bash
     sudo kubeadm join <MASTER_IP>:<PORT> --token <TOKEN> --discovery-token-ca-cert-hash sha256:<HASH>
     ```
   - You can verify nodes have joined with:
     ```bash
     kubectl get nodes
     ```

---

### **Phase 6: Deploy and Manage Pods**

#### Step 1: Create a Test Deployment
   - Create an NGINX deployment:
     ```bash
     kubectl create deployment nginx --image=nginx
     ```

#### Step 2: Expose the Deployment
   - Expose NGINX deployment to test accessibility:
     ```bash
     kubectl expose deployment nginx --type=NodePort --port=80
     ```
   - Retrieve the service details to get the port number:
     ```bash
     kubectl get svc
     ```

#### Step 3: Scale the Deployment
   - Scale the NGINX deployment to run three replicas:
     ```bash
     kubectl scale deployment nginx --replicas=3
     ```

#### Step 4: Check Deployment and Pods
   - Verify the deployment status and list pods:
     ```bash
     kubectl get deployments
     kubectl get pods
     ```

---

### **Phase 7: Advanced Kubernetes Management**

#### Step 1: Set Up Namespaces
   - Create a custom namespace:
     ```bash
     kubectl create namespace dev
     ```

#### Step 2: Use ConfigMaps and Secrets
   - **ConfigMap**: Store configuration data in Kubernetes.
     ```bash
     kubectl create configmap nginx-config --from-literal=key=value
     ```
   - **Secret**: Securely store sensitive information.
     ```bash
     kubectl create secret generic db-secret --from-literal=username=admin --from-literal=password=secret
     ```

#### Step 3: Implement Autoscaling
   - Enable horizontal pod autoscaling based on CPU usage:
     ```bash
     kubectl autoscale deployment nginx --cpu-percent=50 --min=1 --max=5
     ```

---

### **Phase 8: Monitoring and Logging**

#### Step 1: Set Up Kubernetes Metrics Server
   - Install Metrics Server to enable resource-based autoscaling:
     ```bash
     kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
     ```

#### Step 2: Enable Cluster Logging with Fluentd and Elasticsearch
   - Deploy a logging stack (Fluentd, Elasticsearch, Kibana):
     ```bash
     kubectl apply -f https://k8s.io/examples/cluster-logging/fluentd-es-configmap.yaml
     kubectl apply -f https://k8s.io/examples/cluster-logging/fluentd-es.yaml
     ```

---

### **Phase 9: Backup and Restore**

#### Step 1: Backup ETCD (Kubernetes data store)
   - Create a snapshot of the ETCD data:
     ```bash
     sudo ETCDCTL_API=3 etcdctl snapshot save snapshot.db --endpoints=https://127.0.0.1:2379 --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key
     ```

#### Step 2: Restore ETCD from a Snapshot
   - Restore from the backup in case of failure:
     ```bash
     sudo ETCDCTL_API=3 etcdctl snapshot restore snapshot.db --data-dir /var/lib/etcd-backup
     ```

---

### **Phase 10: Cleanup and Cluster Deletion**

To remove a node from the cluster:
   ```bash
   kubectl drain <node-name> --delete-local-data --force --ignore-daemonsets
   kubectl delete node <node-name>
   ```

To delete the entire cluster, you can reset the nodes:
   ```bash
   sudo kubeadm reset
   sudo systemctl stop kubelet
   sudo systemctl disable kubelet
   ```

---

### Conclusion
This hands-on guide covers everything from the installation of Docker and Kubernetes components on Ubuntu 24.04 to advanced Kubernetes management tasks. By following each phase, you’ll gain a complete understanding of how to set up, manage, and troubleshoot a Kubernetes environment, preparing you for real-world applications of container orchestration.
