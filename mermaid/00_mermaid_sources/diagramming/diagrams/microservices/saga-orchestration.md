# Microservices Saga (Orchestration)

```mermaid
flowchart LR
    ORCH[Saga Orchestrator] --> O[Order Service]
    ORCH --> P[Payment Service]
    ORCH --> I[Inventory Service]
    ORCH --> S[Shipping Service]

    O -- reserve order --> ORCH
    ORCH -- request payment --> P
    P -- confirm/decline --> ORCH
    ORCH -- reserve stock --> I
    I -- reserved/failed --> ORCH
    ORCH -- schedule shipment --> S

%% Notes:
%% - Orchestrator coordinates local transactions across services.
%% - Compensating actions triggered on failure.
```

## Legend / Roles

- **Saga Orchestrator** coordinates the sequence of local transactions.
- **Order Service** creates or cancels orders.
- **Payment Service** captures or refunds payments.
- **Inventory Service** reserves or releases stock.
- **Shipping Service** schedules fulfillment or handles rollback steps.
