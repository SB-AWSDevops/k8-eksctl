## High-Level Interaction of `kubectl` with Kubernetes API Server

### 1. User Command
- User executes a command (e.g., `kubectl get pods`).

### 2. `kubectl`
- **Parse Command**: Parses the command and options specified by the user.
- **Construct HTTP Request**:
  - **HTTP Method**: GET, POST, PUT, DELETE
  - **URL**: `https://<api-server-address>/api/v1/pods`
  - **Headers**: Includes authorization tokens and content-type headers.
  - **Body**: Included for non-GET requests (POST, PUT, DELETE).

### 3. HTTP Request Transmission
- **TLS Encryption**: The request is encrypted using TLS (Transport Layer Security).
- **Authorization Header**: Includes bearer token or client certificate for authentication.
- **Send to API Server**: HTTP request is transmitted to the Kubernetes API Server.

### 4. Kubernetes API Server
- **Receive Request**: Listens on port 6443 by default.
- **Authenticate**: Validates the authorization header.
- **Authorize**: Checks permissions based on RBAC.
- **Validate Request**: Ensures the request format is correct.
- **Route to Endpoint**: Routes request to the appropriate API endpoint.
- **Interact with etcd**:
  - **Read Operation**: For GET requests, retrieves data from etcd.
  - **Write Operation**: For POST, PUT, DELETE requests, updates state in etcd.
- **Construct HTTP Response**:
  - **Status Code**: Indicates the result of the request.
  - **Headers**: Response headers.
  - **Body**: Contains the requested data or an error message.

### 5. HTTP Response Transmission
- **TLS Encryption**: The response is encrypted using TLS.
- **Send Response to `kubectl`**: HTTP response is transmitted back to `kubectl`.

### 6. `kubectl`
- **Parse Response**: Parses the HTTP response received fr
