

### **Practice Test: Cluster Installation using `kubeadm`**

#### **1. What is the purpose of `kubeadm init`?**
   - a) To initialize the `kubelet` service on the node
   - b) To initialize the control plane on the master node
   - c) To add a new node to an existing cluster
   - d) To install Kubernetes on the system

   **Answer:** **b) To initialize the control plane on the master node**

---

#### **2. Which command would you use to join a new worker node to the existing Kubernetes cluster?**
   - a) `kubeadm join <cluster_token>`
   - b) `kubeadm join <master_ip>:<port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>`
   - c) `kubectl add node --token <token>`
   - d) `kubeadm add worker --token <token>`

   **Answer:** **b) kubeadm join <master_ip>:<port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>**

---

#### **3. Which file contains the default Kubernetes configuration after running `kubeadm init`?**
   - a) `/etc/kubernetes/admin.conf`
   - b) `/var/lib/kubelet/config.yaml`
   - c) `/etc/kubernetes/kubeconfig.yaml`
   - d) `/etc/kubernetes/kubelet.conf`

   **Answer:** **a) /etc/kubernetes/admin.conf**

---

#### **4. What is the purpose of the `--pod-network-cidr` flag during `kubeadm init`?**
   - a) To define the IP range for worker nodes
   - b) To specify the IP range for the pod network
   - c) To allocate IP addresses to services in the cluster
   - d) To assign IP addresses to external access points

   **Answer:** **b) To specify the IP range for the pod network**

---

#### **5. Which command would you use to apply a network plugin (e.g., Calico) after initializing the cluster?**
   - a) `kubectl apply -f calico.yaml`
   - b) `kubeadm network apply calico`
   - c) `kubelet plugin apply calico`
   - d) `kubectl add network calico`

   **Answer:** **a) kubectl apply -f calico.yaml**

---

#### **6. What component must be configured on all nodes to enable inter-pod communication within a Kubernetes cluster?**
   - a) kube-apiserver
   - b) Pod network plugin (e.g., Calico, Flannel)
   - c) kube-scheduler
   - d) kube-proxy

   **Answer:** **b) Pod network plugin (e.g., Calico, Flannel)**

---

#### **7. After initializing a Kubernetes cluster, which command allows a non-root user to interact with the cluster?**
   - a) `kubectl init user`
   - b) `kubectl config set-context`
   - c) `mkdir -p $HOME/.kube && sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config && sudo chown $(id -u):$(id -g) $HOME/.kube/config`
   - d) `kubeadm add user`
```
   **Answer:** **c) mkdir -p $HOME/.kube && sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config && sudo chown $(id -u):$(id -g) $HOME/.kube/config**
```
---

#### **8. Which of the following is NOT a necessary step in preparing a node for Kubernetes installation?**
   - a) Disabling swap
   - b) Installing `kubeadm`, `kubelet`, and `kubectl`
   - c) Enabling IPv6 on all interfaces
   - d) Setting up the container runtime (e.g., Docker, containerd)

   **Answer:** **c) Enabling IPv6 on all interfaces**

---

#### **9. How do you confirm that all nodes in the cluster are in a “Ready” state after cluster initialization?**
   - a) `kubectl check nodes`
   - b) `kubectl get nodes`
   - c) `kubectl status nodes`
   - d) `kubectl verify nodes`

   **Answer:** **b) kubectl get nodes**

---

#### **10. After setting up a Kubernetes cluster, you encounter network issues between pods. Which of the following is the most likely cause?**
   - a) kube-scheduler is not configured correctly
   - b) The pod network plugin was not installed or configured properly
   - c) kube-controller-manager is down
   - d) There are no worker nodes in the cluster

   **Answer:** **b) The pod network plugin was not installed or configured properly**

---

#### **11. Which command is used to reset a node that was previously initialized by `kubeadm`?**
   - a) `kubeadm remove`
   - b) `kubeadm reset`
   - c) `kubectl reset`
   - d) `kubectl delete node`

   **Answer:** **b) kubeadm reset**

---

#### **12. Which Kubernetes component is responsible for managing the cluster’s network rules and is deployed as a DaemonSet across all nodes?**
   - a) kube-apiserver
   - b) kube-proxy
   - c) kube-controller-manager
   - d) etcd

   **Answer:** **b) kube-proxy**

---

#### **13. Which configuration file should be modified to permanently disable swap on a node for Kubernetes?**
   - a) `/etc/kubernetes/config.yaml`
   - b) `/etc/systemd/swap.conf`
   - c) `/etc/fstab`
   - d) `/etc/kubernetes/admin.conf`

   **Answer:** **c) /etc/fstab**

---

#### **14. What command verifies the status of each Kubernetes component running on the control plane node?**
   - a) `kubectl get pods -n kube-system`
   - b) `kubeadm verify status`
   - c) `kubectl check controlplane`
   - d) `kubectl components status`

   **Answer:** **a) kubectl get pods -n kube-system**

---

#### **15. After initializing a cluster with `kubeadm init`, you need to copy the `kubeconfig` file to allow root users to access the cluster. Where is this configuration file located by default?**
   - a) `/etc/kubernetes/kubelet.conf`
   - b) `/etc/kubernetes/admin.conf`
   - c) `/var/lib/kubernetes/admin.conf`
   - d) `/usr/local/kube/admin.conf`

   **Answer:** **b) /etc/kubernetes/admin.conf**

---



### **Practice Test: Cluster Installation using K0**

#### **1. What is K0 primarily used for in Kubernetes?**
   - a) To create virtual machines
   - b) To simplify the installation of Kubernetes clusters
   - c) To manage Kubernetes configurations
   - d) To monitor Kubernetes clusters

   **Answer:** **b) To simplify the installation of Kubernetes clusters**

---

#### **2. Which command initializes a Kubernetes cluster using K0?**
   - a) `k0 create cluster`
   - b) `k0 install`
   - c) `k0 cluster init`
   - d) `k0 start`

   **Answer:** **a) k0 create cluster**

---

#### **3. By default, what is the name of the context that K0 uses when creating a Kubernetes cluster?**
   - a) `k0-default`
   - b) `default-cluster`
   - c) `k0-cluster`
   - d) `kube-admin`

   **Answer:** **c) k0-cluster**

---

#### **4. How can you verify the status of a Kubernetes cluster created with K0?**
   - a) `k0 status`
   - b) `k0 check cluster`
   - c) `k0 get status`
   - d) `kubectl get nodes`

   **Answer:** **d) kubectl get nodes**

---

#### **5. When using K0, which command allows you to delete a Kubernetes cluster?**
   - a) `k0 delete`
   - b) `k0 delete cluster`
   - c) `k0 stop`
   - d) `kubectl delete cluster`

   **Answer:** **b) k0 delete cluster**

---

#### **6. Which of the following is NOT typically required before installing a Kubernetes cluster with K0?**
   - a) Disabling swap on the nodes
   - b) Installing Docker or another container runtime
   - c) Pre-configuring pod network settings
   - d) Setting up a control plane manually

   **Answer:** **d) Setting up a control plane manually**

---

#### **7. Which network plugin is recommended to be installed after setting up a Kubernetes cluster with K0 for inter-pod communication?**
   - a) kube-proxy
   - b) Flannel or Calico
   - c) Network Manager
   - d) Podman

   **Answer:** **b) Flannel or Calico**

---

#### **8. After installing K0, what command would you use to list all the running pods in all namespaces to check cluster health?**
   - a) `k0 get pods --all-namespaces`
   - b) `kubectl get pods --all-namespaces`
   - c) `k0 get all`
   - d) `k0 list pods`

   **Answer:** **b) kubectl get pods --all-namespaces**

---

#### **9. What is the purpose of the K0 tool in cluster installations?**
   - a) To configure only the network settings for Kubernetes
   - b) To provide a zero-friction way to deploy, manage, and test Kubernetes clusters
   - c) To monitor Kubernetes clusters
   - d) To scale up and down Kubernetes clusters

   **Answer:** **b) To provide a zero-friction way to deploy, manage, and test Kubernetes clusters**

---

#### **10. If you need to view logs for a specific pod in the K0-created cluster, which command would you use?**
   - a) `k0 get logs <pod-name>`
   - b) `kubectl logs <pod-name>`
   - c) `k0 show logs <pod-name>`
   - d) `kubectl get logs <pod-name>`

   **Answer:** **b) kubectl logs <pod-name>**

---

#### **11. Which configuration file typically contains the kubeconfig for a cluster installed with K0?**
   - a) `/etc/kubernetes/k0-config`
   - b) `/etc/kubernetes/kubelet.conf`
   - c) `~/.kube/config`
   - d) `/var/lib/kube/config`

   **Answer:** **c) ~/.kube/config**

---

#### **12. In a K0-created cluster, how would you check the version of Kubernetes running?**
   - a) `k0 version`
   - b) `kubectl version`
   - c) `k0 get version`
   - d) `kubectl get version`

   **Answer:** **b) kubectl version**

---

#### **13. What command would you use to access the Kubernetes dashboard after installing K0?**
   - a) `k0 dashboard`
   - b) `kubectl dashboard`
   - c) `k0 create dashboard`
   - d) `kubectl proxy`

   **Answer:** **d) kubectl proxy**

---

#### **14. Which of the following is a main feature of using K0 for cluster installation?**
   - a) Requires advanced networking setup
   - b) Provides a fast and simplified installation experience for Kubernetes clusters
   - c) Requires manual configuration for each node
   - d) Only supports single-node clusters

   **Answer:** **b) Provides a fast and simplified installation experience for Kubernetes clusters**

---

#### **15. What is the command to initialize a worker node and add it to a K0-managed cluster?**
   - a) `k0 join <cluster_ip>`
   - b) `kubectl add worker`
   - c) `k0 worker add`
   - d) `k0 join --token <token> <master_ip>:<port>`

   **Answer:** **d) k0 join --token <token> <master_ip>:<port>**

---

