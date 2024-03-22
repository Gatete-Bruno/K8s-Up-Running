Bruno GateteBruno Gatete's Blog


Dashboard


Bruno Gatete
K8s | Up & Running
K8s | Up & Running

Bruno Gatete's photo
Bruno Gatete
·
Mar 20, 2024
·
10 min read

Whats is k8s?
Kubernetes, often abbreviated as "K8s," is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications. Originally developed by Google and later donated to the Cloud Native Computing Foundation (CNCF), Kubernetes provides a robust framework for deploying, scaling, and managing applications across clusters of machines. At its core, Kubernetes aims to abstract away the complexities of managing individual containers and instead focuses on orchestrating and coordinating containers to ensure high availability, scalability, and resilience. With its declarative configuration, extensive API, and vibrant ecosystem, Kubernetes has become the de facto standard for container orchestration, enabling organizations to build, deploy, and operate cloud-native applications with ease.

Why Kubernetes Reigns Supreme for Container Orchestration:
Scalability and Versatility:

Kubernetes showcases its prowess in handling intricate distributed systems, seamlessly adapting from modest applications to expansive enterprise infrastructures.

Its adaptable architecture facilitates container deployment across a spectrum of environments, spanning on-premises setups, public clouds, and hybrid configurations.

Automation and Autonomous Recovery:

Kubernetes epitomizes automation, harnessing potent APIs and a declarative configuration paradigm.

Users define application states, empowering Kubernetes to orchestrate tasks like pod scheduling and resource scaling autonomously.

With innate self-healing capabilities, Kubernetes promptly revives failed containers, ensuring sustained application uptime.

Robustness and Resilience:

Kubernetes places a premium on application continuity and resilience, leveraging features like self-healing and pod duplication.

It embraces sophisticated deployment methodologies, including rolling updates and canary deployments, facilitating seamless version transitions and minimizing downtime.

Ecosystem and Collaborative Community:

The Kubernetes ecosystem flourishes with diversity, supported by a dynamic community of enthusiasts and experts.

Users access a plethora of extensions, utilities, and integrations tailored to a myriad of use cases and needs.

The engaged community fosters rapid evolution, perpetual enhancement, and steadfast support for the platform.

Kubernetes Architecture / Components:


API Server: The gateway to Kubernetes, the API Server receives, verifies, and processes RESTful requests, serving as the primary interface for all administrative actions.

Scheduler: The task manager of Kubernetes, the Scheduler assigns pods to nodes based on factors like resource availability, specified constraints, and other policies.

Controller Manager: Acting as the overseer, the Controller Manager handles various controllers responsible for maintaining the desired cluster state, ensuring alignment between the intended and actual configurations.

etcd: Serving as Kubernetes' central memory, etcd is a distributed key-value store storing cluster state and configuration data, crucial for maintaining the cluster's integrity and functionality.

Node Components:

Kubelet: Functioning as the on-ground agent, Kubelet operates on each node, facilitating communication with the master node and overseeing pod management to ensure their proper execution and health.

Kube-Proxy: The networking supervisor of nodes, Kube-Proxy maintains network rules, enabling inter-pod communication and regulating external traffic flow, crucial for maintaining connectivity and service availability.

Understanding Each Component:

API Server: The hub of Kubernetes management, the API Server exposes Kubernetes' functionalities, allowing users and components to interact with the cluster seamlessly through commands like kubectl get pods or kubectl create deployment.

Scheduler: Responsible for pod placement, the Scheduler considers various factors like resource needs and locality to ensure efficient distribution across the cluster, as illustrated by scheduling pods via YAML configurations.

Controller Manager: Overseeing cluster consistency, the Controller Manager manages controllers like ReplicaSet and Deployment, ensuring the desired state, such as maintaining a specific number of pod replicas, as depicted in Deployment YAML examples.

etcd: The backend support, etcd quietly operates in the background, storing critical cluster information, though not directly accessed by users or administrators.

Kubelet: The worker's eyes and ears, Kubelet monitors pod execution, communicating with the API server to receive instructions and update on pod statuses, demonstrated by commands like kubectl describe node <node-name>.

Kube-Proxy: The traffic manager, Kube-Proxy regulates network traffic between pods and external sources, essential for maintaining connectivity and enabling load balancing functionalities.

Fundermental Concepts

Namespaces

COPY
apiVersion: v1
kind: Namespace
metadata:
  name: nginx-namespace
Namespace:

A Namespace provides a way to logically divide cluster resources into virtual groups.

It helps in organizing and managing resources within a Kubernetes cluster, providing isolation and scope.

Metadata:

Metadata provides information about the Namespace, such as its name (nginx-namespace).
Pods

COPY
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
    - image: nginx:latest
      name: nginx-container
      ports:
        - containerPort: 80
          name: http
          protocol: TCP
      resources:
        requests:
          memory: "64Mi"
          cpu: "250m"
        limits:
          memory: "128Mi"
          cpu: "500m"
      readinessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 5
        periodSeconds: 10
      livenessProbe:
        httpGet:
          path: /
          port: 80
        initialDelaySeconds: 10
        periodSeconds: 20
Pod:

A Pod is the smallest deployable unit in Kubernetes, representing a single instance of a running process.

It encapsulates one or more containers, storage resources, a unique network IP, and options that govern how the containers should run.

Metadata:

Metadata provides information about the Pod, such as its name (nginx-pod in this case).
Containers:

Containers section defines the containers to run within the Pod.

In this example, there's only one container named nginx-container.

Image:

Specifies the Docker image to use for the container (nginx:latest).

The image will be pulled from Docker Hub unless otherwise specified.

Ports:

Defines which ports should be open and available for communication inside the container.

Here, port 80 is exposed for HTTP traffic.

Resources:

Specifies the resource requests and limits for the container.

Requests define the minimum amount of resources required for the container (memory: "64Mi", cpu: "250m").

Limits define the maximum amount of resources the container can use (memory: "128Mi", cpu: "500m").

Readiness Probe:

Defines a probe to determine if the container is ready to serve traffic.

The probe makes an HTTP GET request to the path / on port 80 after an initial delay of 5 seconds, repeating every 10 seconds.

Liveness Probe:

Defines a probe to determine if the container is still alive.

Similar to the readiness probe, but it checks the / path on port 80 after an initial delay of 10 seconds, repeating every 20 seconds.

Volume Mounts:

Specifies any volumes that should be mounted into the container.

In this example, it mounts a volume named kuard-data to the path /data. However, this part is not applicable to the nginx pod as it doesn't require volume mounting.

Volumes:

Defines volumes that can be mounted into the Pod.

In the given nginx pod example, this section is omitted as it doesn't require volumes.

ReplicaSet

COPY
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - image: nginx:latest
          name: nginx-container
          ports:
            - containerPort: 80
              name: http
              protocol: TCP
          resources:
            requests:
              memory: "64Mi"
              cpu: "250m"
            limits:
              memory: "128Mi"
              cpu: "500m"
          readinessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 5
            periodSeconds: 10
          livenessProbe:
            httpGet:
              path: /
              port: 80
            initialDelaySeconds: 10
            periodSeconds: 20
ReplicaSet:

ReplicaSet ensures that a specified number of pod replicas are running at any given time.

It maintains the desired number of pods by creating or deleting replicas as needed based on the defined configuration.

Metadata:

Metadata provides information about the ReplicaSet, such as its name (nginx-replicaset).
Spec:

The spec section defines the desired state of the ReplicaSet.

replicas: 3 specifies that three replicas of the Pod should be maintained.

Selector:

Selector specifies how the ReplicaSet identifies which pods it manages.

matchLabels identifies pods with labels matching app: nginx.

Template:

Template defines the pod template used by the ReplicaSet to create new pods.

It includes metadata and spec sections similar to those in a standalone Pod definition.

Deployments

COPY
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
Deployment: A Deployment manages a set of replicated Pods, ensuring that a specified number of replicas are running at any given time.

Replicas: Specifies the desired number of Pod replicas (3 in this case).

Selector: Defines how the Deployment identifies which Pods it manages, based on matching labels (app: nginx).

Template: Specifies the Pod template used for creating new Pods managed by the Deployment.

Containers: Defines the container specifications within the Pods, including the nginx container with port 80 exposed for HTTP traffic.

Service

COPY
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
Service: A Service provides a way to access a set of Pods as a network service.

Selector: Specifies which Pods the Service should route traffic to, based on matching labels (app: nginx).

Ports: Defines the port configuration for the Service, specifying that port 80 on the Service should forward traffic to port 80 on the Pods.

Nginx Ingress Controller Access to Services:

COPY
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: nginx-ingress
spec:
  rules:
    - host: example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: nginx-service
                port:
                  number: 80
Ingress: An Ingress resource manages external access to services in a Kubernetes cluster.

Rules: Defines routing rules based on hostnames and paths.

Host: Specifies the hostname for which traffic should be routed (e.g., example.com).

Paths: Defines paths for routing and how traffic should be directed.

Backend: Specifies the backend Service to which traffic matching the rule should be forwarded, in this case, nginx-service on port 80.

ConfigMaps

COPY
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
data:
  nginx.conf: |
    server {
        listen 80;
        server_name example.com;
        location / {
            proxy_pass http://nginx-service;
        }
    }
ConfigMap: A ConfigMap stores configuration data as key-value pairs.

Metadata: Provides information about the ConfigMap, including its name (nginx-config).

Data: Specifies the configuration data, such as an nginx configuration file (nginx.conf), which can be accessed by Pods.

Secrets:

COPY
apiVersion: v1
kind: Secret
metadata:
  name: nginx-secret
type: Opaque
data:
  username: <base64-encoded-username>
  password: <base64-encoded-password>
Secret: A Secret securely stores sensitive information, such as passwords or API keys.

Metadata: Provides information about the Secret, including its name (nginx-secret).

Type: Specifies the type of Secret (Opaque indicates arbitrary data).

Data: Contains the sensitive information encoded in base64 format for security.

Liveness and Readiness Probes:

COPY
  livenessProbe:
    httpGet:
      path: /health
      port: 80
    initialDelaySeconds: 10
    periodSeconds: 30
  readinessProbe:
    httpGet:
      path: /ready
      port: 80
    initialDelaySeconds: 5
    periodSeconds: 10
Liveness Probe: Checks if the container is alive and healthy. If the probe fails, Kubernetes restarts the container.

Readiness Probe: Checks if the container is ready to serve traffic. If the probe fails, the container is removed from the service endpoint.

Persistent Volumes:


COPY
    yamlCopy codeapiVersion: v1
    kind: PersistentVolume
    metadata:
      name: nginx-pv
    spec:
      capacity:
        storage: 1Gi
      accessModes:
        - ReadWriteOnce
      persistentVolumeReclaimPolicy: Retain
      storageClassName: standard
PersistentVolume: A PersistentVolume represents a piece of storage in the cluster.

Metadata: Provides information about the PersistentVolume, including its name (nginx-pv).

Spec: Defines the specifications for the PersistentVolume, including capacity, access modes, reclaim policy, and storage class.

StatefulSets:

COPY
yamlCopy codeapiVersion: apps/v1
kind: StatefulSet
metadata:
  name: nginx-statefulset
spec:
  replicas: 3
  serviceName: nginx
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx-container
          image: nginx:latest
          ports:
            - containerPort: 80
  volumeClaimTemplates:
    - metadata:
        name: nginx-pvc
      spec:
        accessModes: [ "ReadWriteOnce" ]
        resources:
          requests:
            storage: 1Gi
StatefulSet: A StatefulSet manages stateful applications with unique identities and stable network identifiers.

Replicas: Specifies the desired number of replicas (3 in this case).

ServiceName: Specifies the name of the associated Service (nginx).

Selector: Defines how Pods are selected for the StatefulSet, based on matching labels (app: nginx).

Template: Specifies the Pod template used for creating new Pods managed by the StatefulSet.

VolumeClaimTemplates: Defines templates for PersistentVolumeClaims (PVCs) to be dynamically provisioned for each Pod.

Conclusion
Kubernetes reigns supreme in container orchestration for its scalability, automation, fault tolerance, vibrant ecosystem, and robust community support. It effortlessly manages complex systems across diverse environments, automates mundane tasks, ensures high availability, and offers a rich set of extensions and integrations. With Kubernetes, organizations can deploy and operate cloud-native applications efficiently, making it the go-to choice for modern containerized application development.






Subscribe to my newsletter
Read articles from directly inside your inbox. Subscribe to the newsletter, and don't miss out.

catobrunoisrael@gmail.com
SUBSCRIBE
Kubernetes
Written by

Bruno Gatete
Bruno Gatete
Passionate DevOps and Cloud Engineer dedicated to the seamless integration of development and operations, emphasizing efficiency, collaboration, and automation in the software development lifecycle. Specialized in Cloud Engineering, I focus on designing, implementing, and managing robust cloud infrastructure solutions.

Experience Highlights:

Terraform:

Adept in Infrastructure as Code (IaC) with Terraform, streamlining and automating the deployment and management of infrastructure resources. Ansible:

Proficient in Ansible for automation tasks, configuration management, and application deployment, ensuring consistency and efficiency in infrastructure management. AWS (Amazon Web Services):

Extensive experience with AWS, showcasing expertise in a broad range of cloud services, including compute, storage, databases, and more. Competent in designing scalable and cost-effective solutions on the AWS platform. Kubernetes:

Expertise in container orchestration using Kubernetes, demonstrating the ability to deploy, scale, and efficiently manage containerized applications. Proficient in managing containerized workloads in a cloud-native environment. Docker:

Proficient in Docker for containerization, ensuring consistency in development, testing, and deployment across diverse environments. Capable of creating, deploying, and running applications in lightweight, portable containers. Google Cloud:

Familiarity with Google Cloud Platform (GCP), showcasing a versatile skill set across multiple cloud providers. Able to leverage GCP services for various use cases, including computing, storage, machine learning, and more.

 
MORE ARTICLES

Bruno Gatete's photo
Bruno Gatete
GitHub Actions: A Beginner's Guide
GitHub Actions: A Beginner's Guide
GitHub: GitHub is a web-based platform used for version control and collaboration. It allows develop…


Bruno Gatete's photo
Bruno Gatete
Helm-101
Helm-101
Helm is a package manager for Kubernetes, simplifying the deployment and management of applications …


Bruno Gatete's photo
Bruno Gatete
How to Monitor ArgoCD using kube-prometheus-stack [Part Three]
How to Monitor ArgoCD using kube-prometheus-stack [Part Three]
In our previous blog we managed to deploy a Netflix Clone on ArgoCD, You can check it out here https…

©2024 Bruno Gatete's Blog

Archive
·
Privacy policy
·
Terms
Publish with Hashnode
Powered by Hashnode - Home for tech writers and readers

K8s | Up & Running
