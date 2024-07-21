## Kubernetes (K8s) Architecture
│
├── Cluster
│   │
│   ├── Master Node
│   │   ├── API Server
│   │   ├── Scheduler
│   │   ├── Controller Manager
│   │   └── etcd (Key-Value Store)
│   │
│   └── Worker Nodes
│       ├── Kubelet
│       ├── Kube-Proxy
│       └── Container Runtime (e.g., Docker)
│
├── Pods
│   ├── Containers
│   │   └── Application
│   └── Volumes
│
└── Services
    ├── ClusterIP
    ├── NodePort
    └── LoadBalancer
```

### Detailed Breakdown:

- **Cluster**: The top-level structure that contains both master and worker nodes.
  
- **Master Node**: The control plane of the cluster.
  - **API Server**: Manages communication between components.
  - **Scheduler**: Allocates resources to various pods.
  - **Controller Manager**: Maintains the desired state of the cluster (e.g., replicating pods).
  - **etcd**: Stores all cluster data.

- **Worker Nodes**: Execute the applications and workloads.
  - **Kubelet**: Agent that runs on each node, ensuring containers are running as expected.
  - **Kube-Proxy**: Manages network rules and load balancing.
  - **Container Runtime**: Runs the containers (e.g., Docker).

- **Pods**: The smallest deployable units in Kubernetes.
  - **Containers**: Applications running inside pods.
  - **Volumes**: Storage resources used by containers.

- **Services**: Define a logical set of pods and a policy to access them.
  - **ClusterIP**: Exposes the service on an internal IP in the cluster.
  - **NodePort**: Exposes the service on each node’s IP at a static port.
  - **LoadBalancer**: Exposes the service using a cloud provider's load balancer.

This format should help you grasp the basic components and their relationships within Kubernetes.