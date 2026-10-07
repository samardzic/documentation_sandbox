# Cloud-Native on Kubernetes with Service Mesh

```mermaid
flowchart TB
    subgraph Ingress
        I[Ingress Controller]
    end

    subgraph Mesh[Service Mesh]
        GW[API Gateway]
        A[Auth Service]
        C[Catalog Service]
        O[Order Service]
        P[Payment Service]
        E[(Event Broker)]
    end

    subgraph Data
        UDB[(User DB)]
        PDB[(Product DB)]
        ODB[(Order DB)]
    end

    I --> GW
    GW --> A
    GW --> C
    GW --> O
    O --> P
    O -- emits --> E
    P -- emits --> E

    A --> UDB
    C --> PDB
    O --> ODB

%% Mesh features (implicit): mTLS, retries, timeouts, observability.
%% Kubernetes features: deployments, autoscaling, config, secrets.
```

## Legend / Roles

- **Ingress Controller** exposes cluster services to the internet.
- **Service Mesh** (Istio/Linkerd) provides traffic management and observability.
- **API Gateway** handles external API routing and auth.
- **Microservices** (Auth, Catalog, Order, Payment) communicate securely within the mesh.
- **Event Broker** supports asynchronous messaging.
- **Per-service Databases** isolate data ownership for each service.
