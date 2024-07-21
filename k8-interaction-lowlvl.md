## Low-Level Interaction of `kubectl` with Kubernetes API Server

### 1. User Command
- User executes a command (e.g., `kubectl get pods`).

### 2. `kubectl`
- **Parse Command**: Parses the user command and options.
- **Construct HTTP Request**:
  - **HTTP Method**: GET, POST, PUT, DELETE
  - **URL**: `https://<api-server-address>/api/v1/pods`
  - **Headers**:
    - Authorization: Bearer token or client certificate
    - Content-Type: application/json or other
  - **Body**: Included for non-GET requests (POST, PUT, DELETE)

### 3. HTTP Request Transmission
- **TLS Encryption**: Encrypts the request for secure transmission.
- **Authorization Header**: Includes authentication token or client certificate.
- **Send to API Server**: HTTP request is transmitted to the Kubernetes API Server.

### 4. Kubernetes API Server
- **Receive Request**:
  - Listens on port 6443 by default.
- **Authentication**:
  - Validates the authorization header.
- **Authorization**:
  - Checks requestor’s permissions using RBAC.
- **Validate Request**:
  - Ensures the request conforms to API specifications.
- **Route to Endpoint**:
  - Routes request to the appropriate API endpoint (e.g., `/api/v1/pods`).
- **Interaction with etcd**:
  - **Read Operation**: For GET requests, retrieves data from etcd.
  - **Write Operation**: For POST, PUT, DELETE requests, updates state in etcd.
- **Construct HTTP Response**:
  - **Status Code**: HTTP status code indicating result.
  - **Headers**: Response headers.
  - **Body**: Contains data or error message.

### 5. HTTP Response Transmission
- **TLS Encryption**: Encrypts the response for secure transmission.
- **Send Response to `kubectl`**: HTTP response is transmitted back to `kubectl`.

### 6. `kubectl`
- **Parse Response**: Parses the HTTP response.
- **Handle Errors**: Processes any errors returned by the API Server.
- **Format and Display Output**:
  - Formats the response data into a user-friendly format (e.g., tables, JSON, YAML).
  - Displays the result in the terminal.

### Low-Level Tree Format

```plaintext
User Command
   |
   v
kubectl
   ├── Parse Command
   ├── Construct HTTP Request
   │   ├── HTTP Method
   │   ├── URL
   │   ├── Headers (Authorization, Content-Type)
   │   └── Body (for non-GET requests)
   |
   v
HTTP Request Transmission
   ├── TLS Encryption
   ├── Authorization Header
   └── Send to API Server
   |
   v
Kubernetes API Server
   ├── Receive Request
   ├── Authentication
   ├── Authorization
   ├── Validate Request
   ├── Route to Endpoint
   ├── Interaction with etcd
   │   ├── Read Operation (for GET requests)
   │   └── Write Operation (for POST, PUT, DELETE requests)
   └── Construct HTTP Response
       ├── Status Code
       ├── Headers
       └── Body (Data or Error Message)
   |
   v
HTTP Response Transmission
   ├── TLS Encryption
   └── Send Response to kubectl
   |
   v
kubectl
   ├── Parse Response
   ├── Handle Errors
   └── Format and Display Output
