# Client-Server Architecture (Basic)

```mermaid
flowchart LR
    WC[Web Client] --> RP[Reverse Proxy / Load Balancer]
    RP --> APP1[Application Server 1]
    RP --> APP2[Application Server 2]
    APP1 --> DB[(PostgreSQL Database)]
    APP2 --> DB

%% Explanation:
%% - Clients connect through a reverse proxy for SSL termination and load balancing.
%% - Multiple stateless app servers scale horizontally.
%% - A single relational database stores application state.
```

## Legend / Roles

- **Web Client** initiates requests over HTTPS.
- **Reverse Proxy / Load Balancer** terminates TLS and balances requests.
- **Application Servers** run identical stateless workloads.
- **PostgreSQL Database** stores persistent application data.
