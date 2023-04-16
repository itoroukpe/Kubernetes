# Kubernetes
Introduction to Kubernetes:
Kubernetes Components:
Kubernetes Pods:
Kubernetes Services:
Kubernetes Deployments:
Kubernetes ConfigMaps and Secrets:
Kubernetes StatefulSets:
Kubernetes Persistent Volumes and Persistent Volume Claims:
Kubernetes Monitoring and Logging:
Kubernetes Security:
Kubernetes Advanced Topics:

# Introduction to Kubernetes:
- ## Overview of Kubernetes architecture
- ## Kubernetes vs traditional infrastructure management
- ## Installation and setup of Kubernetes cluster
# Kubernetes Components:
- ## Kubernetes API server
- ## etcd data store
- ## Kubernetes controller manager
## Kubernetes scheduler
- ## Kubernetes kubelet
- ## Kubernetes container runtime interface (CRI)
# Kubernetes Pods:
- ## Pod lifecycle management
- ## Multi-container pods
-## Pod networking
# Kubernetes Services:
- ## Service types and their use cases
Service discovery
- ## Load balancing with Kubernetes services
# Kubernetes Deployments:
- ## Deploying applications with Kubernetes
- ## Rolling updates and rollbacks
- ## Scaling applications with Kubernetes
# Kubernetes ConfigMaps and Secrets:
- ## Managing application configuration with ConfigMaps
- ## Managing secrets with Kubernetes Secrets
# Kubernetes StatefulSets:
Kubernetes StatefulSets is a type of workload controller that manages stateful applications that require stable and unique network identifiers, persistent storage, ordered and graceful scaling, and consistent pod placement. StatefulSets provide a way to deploy and manage stateful applications, such as databases, message queues, and key-value stores, in Kubernetes.

Here are some key features and benefits of StatefulSets:

- Stable and unique network identifiers: StatefulSets provide stable and unique network identifiers for each pod in the set, which enables the stateful application to use DNS names or IP addresses to access other pods in the set.

- Ordered and graceful scaling: StatefulSets allow for ordered and graceful scaling, meaning that the pods are scaled up or down one at a time, in a specific order, to ensure that the stateful application maintains its consistency.

- Persistent storage: StatefulSets support persistent storage, meaning that each pod in the set has its own persistent volume that is mounted to the same path. This ensures that the stateful application's data is stored on the same volume, which provides consistency and durability.

- Consistent pod placement: StatefulSets ensure that each pod in the set is placed on the same node, unless there is a failure or constraint that prevents it. This allows for the stateful application to maintain consistency across pod restarts and rescheduling.

- Headless services: StatefulSets also support headless services, which allows each pod to have its own DNS name and IP address, making it easy for the stateful application to discover and communicate with other pods in the set.

Some common use cases for StatefulSets include:

- Running databases, such as MySQL, PostgreSQL, and MongoDB, that require persistent storage and ordered scaling.
- Running message queues, such as Kafka and RabbitMQ, that require consistent pod placement and stable network identifiers.
- Running key-value stores, such as Redis and Memcached, that require ordered scaling and consistent pod placement.
- Overall, StatefulSets provide a powerful tool for deploying and managing stateful applications in Kubernetes.

- ## Managing stateful applications with Kubernetes
Managing stateful applications with Kubernetes requires a different approach compared to stateless applications. Kubernetes provides several tools and features to manage stateful applications effectively.

- StatefulSets: Kubernetes StatefulSets are designed to manage stateful applications, such as databases, message queues, and key-value stores. StatefulSets ensure that each pod in the set has a stable and unique network identifier, persistent storage, and consistent pod placement, which makes them ideal for managing stateful applications.

- Persistent Volumes: Kubernetes provides Persistent Volumes (PVs) that allow stateful applications to store data persistently. PVs can be dynamically provisioned, and they are mounted as volumes in the containers. PVs can also be used with StatefulSets to provide persistent storage for each pod in the set.

- ConfigMaps: Kubernetes ConfigMaps can be used to store configuration data for stateful applications. ConfigMaps can be mounted as volumes in the containers, and they can be updated without restarting the containers.

- Headless Services: Kubernetes Headless Services are used to provide a stable network identity for each pod in a StatefulSet. Headless Services allow each pod to have its own DNS name and IP address, which makes it easy to communicate with other pods in the set.

- Rolling Updates: Kubernetes supports rolling updates for StatefulSets, which allow you to update the StatefulSet without disrupting the application. Rolling updates ensure that only one pod is updated at a time, and they can be paused and resumed as needed.

- Stateful Application Backup and Restore: Kubernetes provides features to backup and restore stateful applications, such as databases. Kubernetes Operators can be used to automate backup and restore tasks, which makes managing stateful applications easier.

Overall, managing stateful applications with Kubernetes requires a different approach compared to stateless applications. Kubernetes provides several tools and features to manage stateful applications effectively, including StatefulSets, Persistent Volumes, ConfigMaps, Headless Services, Rolling Updates, and Stateful Application Backup and Restore.

- ## Using StatefulSets for database deployments
StatefulSets are particularly useful for managing stateful applications that require ordered scaling, persistent storage, and stable network identities. One common use case for StatefulSets is deploying and managing databases in Kubernetes.

Here are the steps involved in using StatefulSets for database deployments:

- Define the StatefulSet: First, you need to define the StatefulSet manifest file with the necessary configuration settings. This includes defining the container image, specifying the persistent volume claims, and defining the service to expose the database.

To define a StatefulSet in Kubernetes, you can follow these steps:

Define your StatefulSet in a YAML file. Here's an example:
```
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: my-statefulset
spec:
  serviceName: my-service
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: my-container
        image: my-image
        ports:
        - containerPort: 80
        volumeMounts:
        - name: my-volume
          mountPath: /data
  volumeClaimTemplates:
  - metadata:
      name: my-volume
    spec:
      accessModes:
        - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi
```
In this example, we are creating a StatefulSet called my-statefulset that consists of three replicas of a container called my-container that will use an image called my-image. The StatefulSet will create a headless Service called my-service to manage the network identity of the pods. Each replica will mount a Persistent Volume Claim called my-volume that requests 1GB of storage.

Apply the YAML file to create the StatefulSet:
```
kubectl apply -f my-statefulset.yaml
```
This will create a new StatefulSet called my-statefulset.

Verify that the StatefulSet has been created:
```
kubectl get statefulset
```
This will display a list of all StatefulSets in the current namespace. You should see my-statefulset in the list.

Once you have created your StatefulSet, Kubernetes will manage the creation and scaling of the pods for you, ensuring that each pod has a unique identity and persistent storage.

- Create the Persistent Volume Claims: Next, you need to create the Persistent Volume Claims (PVCs) for the StatefulSet. PVCs ensure that each pod in the set has its own persistent volume that is mounted to the same path. This ensures that the database's data is stored on the same volume, providing consistency and durability.

To create a Persistent Volume Claim (PVC), you can follow these steps:

Define your storage requirements in a YAML file. Here's an example:
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
```
In this example, we are creating a PVC called my-pvc that requires 1GB of storage and will be mounted with ReadWriteOnce access mode.

Apply the YAML file to create the PVC:
```
kubectl apply -f my-pvc.yaml
```
This will create a new PVC called my-pvc.

Verify that the PVC has been created:
```
kubectl get pvc
```
This will display a list of all PVCs in the current namespace. You should see my-pvc in the list.

Once you have created your PVC, you can use it to request storage from a Persistent Volume (PV) that meets the specified requirements.

- Deploy the StatefulSet: Once the StatefulSet and PVCs are defined, you can deploy the StatefulSet using kubectl apply command.

- Connect to the database: Once the StatefulSet is deployed, you can connect to the database using the service that is defined in the StatefulSet manifest. You can use the DNS name or IP address of the service to connect to the database.

- Scale the StatefulSet: You can scale the StatefulSet up or down by changing the replicas field in the StatefulSet manifest file. StatefulSets ensure that pods are scaled up or down one at a time, in a specific order, to ensure that the database maintains its consistency.

- Update the StatefulSet: If you need to update the database version or configuration, you can update the StatefulSet manifest file and apply the changes using kubectl apply command. Rolling updates ensure that only one pod is updated at a time, and they can be paused and resumed as needed.

Overall, using StatefulSets for database deployments in Kubernetes provides several benefits, such as consistent pod placement, ordered and graceful scaling, and persistent storage. StatefulSets provide a reliable and scalable way to deploy and manage stateful applications such as databases in Kubernetes.

# Kubernetes Persistent Volumes and Persistent Volume Claims:
- ## Persistent storage in Kubernetes
Persistent storage in Kubernetes refers to the ability to store data beyond the life cycle of a pod. Kubernetes provides persistent volume (PV) and persistent volume claim (PVC) objects to enable this functionality.

- A persistent volume is a piece of storage in the cluster that has been provisioned by an administrator or dynamically created using a storage class. It exists independently of any pod that uses it and can be used by different pods at different times.

- A persistent volume claim is a request for a specific amount of storage by a pod. When a pod requests a certain amount of storage, Kubernetes creates a PVC object that matches the request. The PVC then binds to a suitable PV that meets the request's criteria.

- Persistent volumes can be provisioned from a variety of storage systems, such as cloud storage or on-premises storage arrays. Kubernetes also provides storage classes that define the underlying storage system and its characteristics, such as access mode, reclaim policy, and storage class parameters.

Overall, persistent storage in Kubernetes provides a way to store data beyond the life cycle of a pod, making it possible to preserve data even if the pod is deleted or recreated.

- ## Provisioning and managing persistent volumes
- 1. Define storage classes: Define storage classes that describe the type of storage to be used and its characteristics, such as access mode, reclaim policy, and storage class parameters. For example, a storage class could define storage that is optimized for high-performance read/write operations.

- 2. Provision the persistent volume: Once you have defined a storage class, you can create a persistent volume using that class. You can create a persistent volume statically or dynamically. Statically created volumes are created by an administrator, while dynamically created volumes are created by Kubernetes when a PVC is created.

- 3. Create a persistent volume claim: A persistent volume claim (PVC) is a request for storage from a pod. When a pod requests storage, Kubernetes creates a PVC object that matches the request. The PVC then binds to a suitable PV that meets the request's criteria.

- 4. Mount the persistent volume in a pod: Once a PVC has been bound to a PV, you can mount the PV in a pod by defining a volume in the pod's specification that references the PVC.

- 5. Manage the persistent volume: Kubernetes provides various tools for managing persistent volumes, including monitoring and resizing. You can monitor the status of persistent volumes using Kubernetes tools such as kubectl or the Kubernetes dashboard. You can also resize a PVC by editing its specification to request more or less storage.

Overall, managing persistent volumes in Kubernetes involves defining storage classes, provisioning the persistent volume, creating a persistent volume claim, mounting the persistent volume in a pod, and managing the persistent volume using Kubernetes tools.

- ## Using persistent volumes in Kubernetes deployments
Using persistent volumes in Kubernetes deployments involves incorporating the persistent volume into the deployment specification. Here are the general steps to use persistent volumes in Kubernetes deployments:

- 1. Create a persistent volume: Before you can use a persistent volume in a deployment, you need to create a persistent volume that matches your requirements. This can be done manually or dynamically using a storage class.

- 2. Create a persistent volume claim: Create a persistent volume claim that references the persistent volume you created in step 1. The persistent volume claim specifies the size and access mode of the storage needed by the deployment.

- 3. Update the deployment specification: Add a volume definition to the deployment specification that references the persistent volume claim. This volume definition tells Kubernetes to mount the persistent volume in the container when the deployment is created.

- 4. Mount the persistent volume in the container: In the container specification of the deployment, define a volume mount that mounts the volume in the container at the desired path. The path will be relative to the container's root file system.

- 5. Deploy the application: Deploy the application using the updated deployment specification. Kubernetes will ensure that the persistent volume is available and mounted in the container before the application starts.

- 6. Access and manage the persistent volume: Once the application is running, you can access and manage the data stored in the persistent volume using the container's file system. You can also manage the persistent volume using Kubernetes tools such as kubectl.

Overall, using persistent volumes in Kubernetes deployments involves creating a persistent volume, creating a persistent volume claim, updating the deployment specification, mounting the persistent volume in the container, deploying the application, and managing the persistent volume using Kubernetes tools.

### Here's an example of using persistent volumes in a Kubernetes deployment:

Assuming you have a MySQL database application that needs to store its data on a persistent volume. Here are the steps to use persistent volumes in the deployment:

Create a persistent volume: Create a persistent volume that matches the requirements of the application. For example, you could create a persistent volume with 10GB of storage using a storage class that supports ReadWriteOnce access mode.

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mysql-pv
spec:
  capacity:
    storage: 10Gi
  accessModes:
    - ReadWriteOnce
  storageClassName: standard
  hostPath:
    path: /data/mysql
```
Create a persistent volume claim: Create a persistent volume claim that references the persistent volume you created in step 1.
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mysql-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
  storageClassName: standard
```
Update the deployment specification: Add a volume definition to the deployment specification that references the persistent volume claim.
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
        - name: mysql
          image: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: mysql-secret
                  key: mysql-root-password
          ports:
            - containerPort: 3306
          volumeMounts:
            - name: mysql-pv-storage
              mountPath: /var/lib/mysql
      volumes:
        - name: mysql-pv-storage
          persistentVolumeClaim:
            claimName: mysql-pvc
```
Mount the persistent volume in the container: In the container specification of the deployment, define a volume mount that mounts the volume in the container at the desired path.
```
volumeMounts:
  - name: mysql-pv-storage
    mountPath: /var/lib/mysql
```
Deploy the application: Deploy the application using the updated deployment specification.
```
kubectl apply -f mysql-deployment.yaml
```
Access and manage the persistent volume: Once the application is running, you can access and manage the data stored in the persistent volume using the container's file system. You can also manage the persistent volume using Kubernetes tools such as kubectl.
Overall, using persistent volumes in Kubernetes deployments involves creating a persistent volume, creating a persistent volume claim, updating the deployment specification, mounting the persistent volume in the container, deploying the application, and managing the persistent volume using Kubernetes tools.

# Kubernetes Monitoring and Logging:
Kubernetes monitoring and logging are critical for maintaining the health and performance of your cluster and applications. Here are some common tools and techniques used for monitoring and logging in Kubernetes:

- Metrics collection: Kubernetes provides a metrics API that can be used to collect metrics from the Kubernetes control plane and the applications running on the cluster. The most common tool for collecting and visualizing Kubernetes metrics is Prometheus. Prometheus can be integrated with Kubernetes using the Prometheus Operator, which makes it easy to configure and manage Prometheus instances in a Kubernetes cluster.

- Logging: Kubernetes provides several mechanisms for collecting logs from containers running in pods. The most common method is to use a logging agent like Fluentd or Logstash, which can collect logs from multiple sources and forward them to a centralized logging platform like Elasticsearch or Splunk. Kubernetes also provides a native logging mechanism called Stackdriver, which integrates with Google Cloud Platform services.

- Tracing: Tracing is a method of monitoring the performance and behavior of applications by tracking requests as they flow through the system. Kubernetes provides support for distributed tracing through the OpenTracing API, which can be integrated with tracing tools like Jaeger or Zipkin.

- Alerting: Alerting is a critical component of any monitoring system, allowing you to receive notifications when certain thresholds are exceeded or when critical errors occur. Kubernetes provides several tools for setting up alerts, including the Kubernetes Event API and tools like Prometheus Alertmanager.

By implementing a comprehensive monitoring and logging strategy in Kubernetes, you can gain greater visibility into the health and performance of your applications, and quickly detect and resolve issues before they impact your users.

- ## Kubernetes monitoring with Prometheus
Prometheus is a popular open-source monitoring solution that is often used to monitor Kubernetes clusters. Here are the steps to set up Prometheus to monitor a Kubernetes cluster:

- Install and configure Prometheus: You can download and install Prometheus from the official website. Once installed, you will need to configure Prometheus to scrape metrics from Kubernetes components and applications running in the cluster. This can be done by defining scrape targets in a prometheus.yml configuration file.

- Install and configure the Prometheus Operator: The Prometheus Operator is a Kubernetes-native tool that simplifies the deployment and management of Prometheus. It provides custom resource definitions (CRDs) for defining Prometheus instances, ServiceMonitors for configuring monitoring targets, and other resources needed for monitoring Kubernetes with Prometheus.

- Deploy the Prometheus Operator: You can deploy the Prometheus Operator using a YAML file provided by the project. Once deployed, the Operator will automatically create a Prometheus instance based on the configuration defined in the CRD.

- Configure monitoring targets: To monitor Kubernetes components and applications, you will need to create ServiceMonitors to define the targets to be scraped by Prometheus. ServiceMonitors are custom resources defined by the Prometheus Operator that allow you to configure the scraping of metrics from Kubernetes Services and Pods.

- Visualize metrics: Prometheus provides a built-in web UI for visualizing metrics, called the Prometheus Expression Browser. You can use this tool to write queries and create graphs of your metrics.

By following these steps, you can set up Prometheus to monitor a Kubernetes cluster and gain greater visibility into the performance and health of your applications.

Here is an example of how to set up Prometheus to monitor a Kubernetes cluster:

- Install Prometheus: You can download and install Prometheus from the official website or using a package manager like Homebrew. Once installed, you will need to create a configuration file prometheus.yml to specify the targets you want to monitor.

- Configure Prometheus: In the prometheus.yml configuration file, you will need to specify the targets you want to monitor. For example, to monitor the Kubernetes API server, you can add the following target:

```
- job_name: 'kubernetes-apiserver'
  kubernetes_sd_configs:
  - role: endpoints
    api_server: https://kubernetes.default.svc:443
    tls_config:
      ca_file: /var/run/secrets/kubernetes.io/serviceaccount/ca.crt
      cert_file: /var/run/secrets/kubernetes.io/serviceaccount/client.crt
      key_file: /var/run/secrets/kubernetes.io/serviceaccount/client.key
  bearer_token_file: /var/run/secrets/kubernetes.io/serviceaccount/token
  relabel_configs:
  - source_labels: [__meta_kubernetes_namespace, __meta_kubernetes_service_name, __meta_kubernetes_endpoint_port_name]
    separator: ;
    regex: default;kubernetes;https
    replacement: kubernetes
    target_label: job
```
This configuration specifies that Prometheus should scrape the Kubernetes API server using the Kubernetes SD configuration, which discovers endpoints based on Kubernetes service discovery. The relabel_configs section specifies how to label the scraped data.

- Install the Prometheus Operator: The Prometheus Operator is a Kubernetes-native tool that simplifies the deployment and management of Prometheus. You can deploy the Operator using the following command:
```
kubectl apply -f https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/v0.49.0/bundle.yaml
```
- Deploy Prometheus: You can deploy a Prometheus instance using a custom resource definition (CRD) provided by the Prometheus Operator. You can create a prometheus.yaml file with the following contents:
```
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  name: example-prometheus
spec:
  replicas: 1
  serviceAccountName: prometheus
  serviceMonitorSelector:
    matchLabels:
      app: example-app
  ruleSelector:
    matchLabels:
      prometheus: example-prometheus
  prometheusSpec:
    serviceMonitorNamespaceSelector:
      matchNames:
      - default
    serviceMonitorSelector:
      matchLabels:
        app: example-app
    ruleNamespaceSelector:
      matchNames:
      - default
    ruleSelector:
      matchLabels:
        prometheus: example-prometheus
    scrapeInterval: 15s
    scrapeTimeout: 10s
    evaluationInterval: 15s
    externalLabels:
      cluster: example-cluster
```
This CRD specifies the configuration for a Prometheus instance named example-prometheus, including the number of replicas, the service account to use, and the selector for ServiceMonitors and rules.

- Deploy ServiceMonitors: ServiceMonitors are Kubernetes resources that define the endpoints to be monitored by Prometheus. You can create a servicemonitor.yaml file with the following contents:
```
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: example-app
  labels:
    app: example-app
spec:
  selector:
    matchLabels:
      app: example-app
  endpoints:
  - port: http
    path: /metrics
```
This ServiceMonitor specifies that Prometheus should monitor the example-app Service for the /metrics endpoint.

Verify monitoring: You can verify

- ## Kubernetes logging with Fluentd and Elasticsearch
Fluentd is a popular open-source log collector that can be used to collect logs from containers running in Kubernetes and forward them to Elasticsearch for storage and analysis. Here are the steps to set up Fluentd and Elasticsearch for logging in Kubernetes:

- Install and configure Elasticsearch: You can download and install Elasticsearch from the official website. Once installed, you will need to configure Elasticsearch to receive and store logs from Fluentd.

- Install and configure Fluentd: You can install Fluentd on each Kubernetes node as a DaemonSet. Fluentd provides a Kubernetes plugin that can be used to collect logs from containers running in Kubernetes. You will need to configure Fluentd to send logs to Elasticsearch.

- Deploy Fluentd: You can deploy Fluentd using a YAML file provided by the project. Once deployed, Fluentd will automatically collect logs from containers running in Kubernetes and forward them to Elasticsearch.

- Configure log retention and rotation: Elasticsearch can be configured to retain and rotate logs based on various criteria, such as time or size. You will need to configure these settings to ensure that logs are retained for an appropriate period of time and do not consume too much storage.

- Visualize logs: Elasticsearch provides a built-in web UI called Kibana for visualizing logs. You can use Kibana to search and filter logs and create dashboards and visualizations.

By following these steps, you can set up Fluentd and Elasticsearch for logging in Kubernetes and gain greater visibility into the behavior and performance of your applications.

### Here is an example of how to set up Kubernetes logging with Fluentd and Elasticsearch:

- Install Elasticsearch: You can download and install Elasticsearch from the official website or using a package manager like Homebrew. Once installed, you will need to configure Elasticsearch to receive and store logs from Fluentd.

- Install and configure Fluentd: You can install Fluentd on each Kubernetes node as a DaemonSet. Fluentd provides a Kubernetes plugin that can be used to collect logs from containers running in Kubernetes. You will need to configure Fluentd to send logs to Elasticsearch.

```
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
data:
  fluent.conf: |
    <match kubernetes.**>
      @type elasticsearch
      host "#{ENV['FLUENT_ELASTICSEARCH_HOST']}"
      port "#{ENV['FLUENT_ELASTICSEARCH_PORT']}"
      scheme "#{ENV['FLUENT_ELASTICSEARCH_SCHEME'] || 'http'}"
      logstash_format true
      type_name "#{ENV['FLUENT_ELASTICSEARCH_INDEX_PREFIX']}.%{[kubernetes.labels.app]}-%{+YYYY.MM.dd}"
      tag_key @log_name
      user "#{ENV['FLUENT_ELASTICSEARCH_USER']}"
      password "#{ENV['FLUENT_ELASTICSEARCH_PASSWORD']}"
    </match>
```
This Fluentd configuration specifies that Fluentd should collect logs from Kubernetes pods and send them to Elasticsearch. The type elasticsearch plugin sends the logs to Elasticsearch with the logstash_format option set to true.

- Deploy Fluentd: You can deploy Fluentd using a YAML file provided by the project. Once deployed, Fluentd will automatically collect logs from containers running in Kubernetes and forward them to Elasticsearch.
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
  namespace: kube-system
data:
  fluent.conf: |
    <match kubernetes.**>
      @type elasticsearch
      host elasticsearch-logging
      port 9200
      index_name fluentd
      type_name fluentd
      logstash_prefix fluentd
      logstash_format true
      flush_interval 10s
      include_tag_key true
      tag_key kubernetes.tag
      time_key time
      reload_connections true
      reconnect_on_error true
      reload_on_failure true
      user elastic
      password changeme
    </match>

---
apiVersion: extensions/v1beta1
kind: DaemonSet
metadata:
  name: fluentd
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
    version: v1
    kubernetes.io/cluster-service: "true"
spec:
  template:
    metadata:
      labels:
        k8s-app: fluentd-logging
        version: v1
        kubernetes.io/cluster-service: "true"
    spec:
      serviceAccount: fluentd
      serviceAccountName: fluentd
      tolerations:
      - key: node-role.kubernetes.io/master
        effect: NoSchedule
      - operator: Exists
      containers:
      - name: fluentd
        image: fluent/fluentd-kubernetes-daemonset:v1.13.1-debian-elasticsearch7-1.1
        env:
        - name: FLUENT_ELASTICSEARCH_HOST
          value: "elasticsearch-logging"
        - name: FLUENT_ELASTICSEARCH_PORT
          value: "9200"
        - name: FLUENT_ELASTICSEARCH_SCHEME
          value: "http"
        - name: FL
```

# Kubernetes Security:
- ## Securing Kubernetes API server
- ## Kubernetes network security
Role-Based Access Control (RBAC) in Kubernetes
# Kubernetes Advanced Topics:
- ## Kubernetes Operators
- ## Custom Resource Definitions (CRDs)
- ## Kubernetes API extensions

This curriculum covers the basics of Kubernetes, including key components, pods, services, deployments, and persistent storage. It also includes more advanced topics such as monitoring, security, and Kubernetes extensions. As you progress through the curriculum, you'll gain a deep understanding of how to manage containerized applications with Kubernetes.
