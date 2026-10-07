# Microservices: API Gateway + BFF

```mermaid
flowchart TD
    %% Clients
    subgraph Clients
        WC[Web Client]
        MC[Mobile App]
    end

    %% Edge
    WC --> AG[API Gateway]
    MC --> AG

    %% BFFs
    AG --> BFFW[BFF Web]
    AG --> BFFM[BFF Mobile]

    %% Services
    BFFW --> AUTH[Authentication Service]
    BFFW --> CAT[Product Catalog Service]
    BFFM --> ORD[Order Service]
    BFFM --> PAY[Payment Service]

    %% Databases (database-per-service pattern)
    AUTH --> UDB[(User Database)]
    CAT  --> PDB[(Product Database)]
    ORD  --> ODB[(Order Database)]
    PAY  --> TDB[(Transaction Database)]

    %% Async comms
    ORD -- emits events --> BUS[(Event Bus)]
    PAY -- publishes events --> BUS
    CAT -- subscribes --> BUS

    %% Infra
    classDef db fill:#eef,stroke:#00f
    class UDB,PDB,ODB,TDB db

%% Legend / Notes:
%% - API Gateway centralizes routing, auth, throttling.
%% - BFFs tailor APIs for each client to reduce over/under-fetching.
%% - Each microservice owns its data store (loose coupling).
%% - Event bus enables eventual consistency and async workflows.
```

## Legend / Roles

- **Clients** (Web, Mobile) access the platform through the API Gateway.
- **API Gateway** centralizes routing, authentication, and throttling.
- **BFFs** tailor responses for each client type while delegating to services.
- **Microservices** (Authentication, Catalog, Order, Payment) own their logic and data.
- **Event Bus** propagates domain events for asynchronous workflows.
