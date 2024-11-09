

### **Practice Test: Cluster Installation using `kubeadm`**
```
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

   **Answer:** **c) mkdir -p $HOME/.kube && sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config && sudo chown $(id -u):$(id -g) $HOME/.kube/config**

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

This practice test provides an overview of key concepts in **cluster installation using `kubeadm`** and helps reinforce understanding of commands, configurations, and troubleshooting steps necessary for a Kubernetes setup.
