AWS EKS
├── **EKS Cluster**
│   ├── **Master Node**
│   │   ├── API Server
│   │   ├── Controller Manager
│   │   ├── Scheduler
│   │   └── etcd
│   └── **Worker Node**
│       ├── Kubelet
│       ├── Kube-Proxy
│       └── Container Runtime (e.g., Docker)
│
├── **kubectl**
│   ├── Commands for Managing Kubernetes Resources
│   │   ├── `kubectl get nodes` - List Nodes in the cluster
│   │   ├── `kubectl describe pod <pod-name>` - Describe a specific Pod
│   │   └── `kubectl apply -f <file>.yaml` - Apply configuration from a file
│   └── Configuration
│       ├── Accesses the Kubernetes API
│       └── Uses the kubeconfig file to authenticate and configure access
│
├── **eksctl**
│   ├── **Cluster Management**
│   │   ├── `eksctl create cluster` - Create a new EKS cluster
│   │   ├── `eksctl delete cluster` - Delete an EKS cluster
│   │   └── `eksctl upgrade cluster` - Upgrade an existing EKS cluster
│   └── **Node Management**
│       ├── `eksctl create nodegroup` - Create a new node group
│       ├── `eksctl delete nodegroup` - Delete a node group
│       └── `eksctl scale nodegroup` - Scale a node group up or down
│
└── **Helm (optional)**
    ├── Package Manager for Kubernetes
    │   ├── `helm install <chart>` - Install a Helm chart
    │   ├── `helm upgrade <release>` - Upgrade a Helm release
    │   └── `helm uninstall <release>` - Uninstall a Helm release
    └── **Charts**
        ├── Pre-configured Kubernetes resources
        └── Stored in repositories for reuse
