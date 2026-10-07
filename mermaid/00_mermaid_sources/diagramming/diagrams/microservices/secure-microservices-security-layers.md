# Secure Microservices with Defense-in-Depth

```mermaid
flowchart LR
    %% Core path and zero-trust components
    CLIENT[Client Apps];
    CDN[CDN and WAF];
    API_GW[API Gateway];
    POLICY[API Policy and Rate Limiting];
    PEP[Policy Enforcement Point];
    MESH[Service Mesh mTLS];

    CLIENT --> CDN;
    CDN --> API_GW;
    API_GW --> POLICY;
    POLICY --> PEP;
    PEP --> MESH;

    %% Identity and token flow
    IDP[OIDC Identity Provider];
    TOKEN[Token Service];
    API_GW --|token validation|--> IDP;
    IDP --> TOKEN;
    TOKEN --|issue tokens|--> CLIENT;

    %% Services behind the mesh
    AUTH[Authentication Service];
    ORDER[Order Service];
    PAYMENT[Payment Service];
    INVENTORY[Inventory Service];
    MESH --> AUTH;
    MESH --> ORDER;
    MESH --> PAYMENT;
    MESH --> INVENTORY;

    %% Data stores (simple rectangles for compatibility)
    AUTH_DB[Encrypted Auth DB];
    ORD_DB[Encrypted Order DB];
    PAY_DB[PCI Vaulted Data];
    INV_DB[Inventory DB];
    AUTH --> AUTH_DB;
    ORDER --> ORD_DB;
    PAYMENT --> PAY_DB;
    INVENTORY --> INV_DB;

    %% Security services
    subgraph Security Services
        SECRETS[Secrets Manager]
        SIEM[Security Monitoring and SIEM]
        IDS[Runtime Threat Detection]
        DATABASE_AUDIT[Database Audit Trails]
    end

    %% Logging and secrets distribution
    AUTH_DB --|audit logs|--> DATABASE_AUDIT;
    ORD_DB --|audit logs|--> DATABASE_AUDIT;
    PAY_DB --|audit logs|--> DATABASE_AUDIT;
    INV_DB --|audit logs|--> DATABASE_AUDIT;

    SECRETS --|dynamic secrets|--> AUTH;
    SECRETS --|dynamic secrets|--> ORDER;
    SECRETS --|dynamic secrets|--> PAYMENT;
    SECRETS --|dynamic secrets|--> INVENTORY;

    MESH --|telemetry|--> SIEM;
    API_GW --|audit logs|--> SIEM;
    DATABASE_AUDIT --|feed|--> SIEM;

    SIEM --> SOC[SOC Alerting];
    IDS --> SOC;

    SOC --> RESP[Incident Response Runbooks];
```

## Legend / Roles

- **CDN + WAF** absorbs DDoS traffic, caches static assets, and filters malicious requests.
- **API Gateway & Policy Enforcement** validate tokens, apply rate limits, and enforce zero-trust access decisions.
- **Service Mesh** terminates/exchanges mTLS certificates for every internal call and provides fine-grained traffic policies.
- **Domain Services** (Authentication, Order, Payment, Inventory) remain isolated behind the mesh with encrypted data stores.
- **Secrets Manager** rotates dynamic credentials and certificates consumed by services at runtime.
- **SIEM & Runtime Detection** centralize security analytics, trigger SOC alerts, and feed incident response runbooks.
