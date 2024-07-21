## Low-Level Kubernetes (K8s) Architecture

### 1. Cluster
- **Master Node**
  - **API Server**
    - **Receives Requests**:
      - HTTP requests (e.g., GET, POST) from `kubectl` and other clients.
      - Processes requests based on API version and endpoint.
    - **Authentication**:
      - Validates bearer tokens or client certificates.
    - **Authorization**:
      - Checks permissions using RBAC (Role-Based Access Control).
    - **Validation**:
      - Ensures requests conform to the Kubernetes API specifications.
    - **Routing**:
      - Routes requests to the appropriate controller or handler.
    - **Interaction with etcd**:
      - **Read Operation**:
        - Retrieves resource data from the etcd key-value store.
      - **Write Operation**:
        - Updates resource state in etcd based on client requests.
    - **Construct HTTP Response**:
      - **Status Code**: Indicates the result (e.g., 200 OK, 404 Not Found).
      - **Headers**: Includes metadata (e.g., content-type).
      - **Body**: Contains requested data or error messages.
  - **Scheduler**
    - **Scheduling Decisions**:
      - Receives pod specifications from the API Server.
      - Determines which node a pod should run on based on resource availability.
    - **Interacts with API Server**:
      - Updates the pod’s status with scheduling decisions.
  - **Controller Manager**
    - **Controllers**:
      - **Node Controller**:
        - Monitors the health of nodes.
        - Updates the API Server with node status.
      - **Replication Controller**:
        - Ensures the desired number of pod replicas.
        - Creates or deletes pods as needed.
      - **Deployment Controller**:
        - Manages deployments, ensuring desired state.
        - Updates the API Server with deployment status.
      - **Other Controllers**: Handles various resources (e.g., StatefulSets, Jobs).
  - **etcd (Key-Value Store)**
    - **Stores Cluster State**:
      - Stores configuration and status data for all Kubernetes resources.
    - **Read and Write Operations**:
      - Processes requests from the API Server to retrieve or update resource data.
    - **Data Consistency**:
      - Ensures strong consistency and replication across etcd instances.

### 2. Worker Nodes
- **Kubelet**
  - **Node Agent**:
    - **Pod Management**:
      - Receives pod specifications from the API Server.
      - Ensures containers are running as specified.
    - **Health Monitoring**:
      - Reports node and pod status to the API Server.
    - **Container Management**:
      - Communicates with the Container Runtime to manage containers.
  - **Kube-Proxy**
    - **Service Proxy**:
      - Maintains network rules on nodes.
      - Implements Service abstraction by load-balancing network traffic to pods.
    - **Network Routing**:
      - Manages traffic routing to ensure service discovery and load balancing.
  - **Container Runtime**
    - **Container Management**:
      - Runs and manages containerized applications (e.g., Docker, containerd).
    - **Image Handling**:
      - Pulls container images from registries and runs containers.
    - **Resource Allocation**:
      - Allocates resources (CPU, memory) to containers.

### 3. Pods
- **Containers**
  - **Application Containers**:
    - **Run Applications**:
      - Executes application code within containers.
    - **Inter-process Communication**:
      - Communicates with other containers in the same pod via localhost.
- **Volumes**
  - **Storage Management**:
    - **Attach Storage**:
      - Provides persistent storage to containers.
    - **Volume Types**:
      - Different types (e.g., Persistent Volumes, ConfigMaps) for various storage needs.

### 4. Services
- **ClusterIP**
  - **Internal Service Access**:
    - **Service Discovery**:
      - Provides a stable IP address for accessing services within the cluster.
- **NodePort**
  - **External Access**:
    - **Expose Service on Node Port**:
      - Exposes the service on a static port across all nodes.
- **LoadBalancer**
  - **External Load Balancing**:
    - **Provisioning by Cloud Provider**:
      - Automatically provisions an external load balancer to distribute traffic to service endpoints.

### Low-Level Tree Format

```plaintext
Cluster
   ├── Master Node
   │   ├── API Server
   │   │   ├── Receives Requests
   │   │   ├── Authentication
   │   │   ├── Authorization
   │   │   ├── Validation
   │   │   ├── Routing
   │   │   ├── Interaction with etcd
   │   │   │   ├── Read Operation
   │   │   │   └── Write Operation
   │   │   └── Construct HTTP Response
   │   │       ├── Status Code
   │   │       ├── Headers
   │   │       └── Body
   │   ├── Scheduler
   │   │   ├── Scheduling Decisions
   │   │   └── Interacts with API Server
   │   ├── Controller Manager
   │   │   ├── Node Controller
   │   │   ├── Replication Controller
   │   │   ├── Deployment Controller
   │   │   └── Other Controllers
   │   └── etcd (Key-Value Store)
   │       ├── Stores Cluster State
   │       ├── Read and Write Operations
   │       └── Data Consistency
   └── Worker Nodes
       ├── Kubelet
       │   ├── Node Agent
       │   ├── Pod Management
       │   ├── Health Monitoring
       │   └── Container Management
       ├── Kube-Proxy
       │   ├── Service Proxy
       │   └── Network Routing
       └── Container Runtime
           ├── Container Management
           ├── Image Handling
           └── Resource Allocation
           └── Containers
               ├── Application Containers
               └── Volumes
                   ├── Storage Management
                   └── Volume Types
                       ├── Persistent Volumes
                       └── ConfigMaps
   └── Services
       ├── ClusterIP
       │   └── Internal Service Access
       ├── NodePort
       │   └── External Access
       └── LoadBalancer
    
