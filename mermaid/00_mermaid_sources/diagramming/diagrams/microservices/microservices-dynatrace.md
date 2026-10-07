# Modern Microservices with Dynatrace Monitoring

```mermaid
graph LR
    CLIENT[Web & Mobile Clients] --> EDGE[API Gateway]
    EDGE --> AUTH[Auth Service]
    EDGE --> CATALOG[Catalog Service]
    EDGE --> ORDER[Order Service]
    EDGE --> PAYMENT[Payment Service]

    AUTH --> AUTH_DB[Auth Database]
    CATALOG --> CAT_DB[Product Database]
    ORDER --> ORD_DB[Order Database]
    PAYMENT --> PAY_DB[Payment Database]

    ORDER --> EVENTBUS[Event Bus]
    PAYMENT --> EVENTBUS
    INVENTORY[Inventory Service] --> EVENTBUS
    EVENTBUS --> SHIPPING[Shipping Service]
    EVENTBUS --> NOTIFY[Notification Service]

    %% Dynatrace components
    AUTH -. tracing .-> DYNATRACE[Dynatrace OneAgent]
    CATALOG -. tracing .-> DYNATRACE
    ORDER -. tracing .-> DYNATRACE
    PAYMENT -. tracing .-> DYNATRACE
    INVENTORY -. tracing .-> DYNATRACE
    SHIPPING -. tracing .-> DYNATRACE
    NOTIFY -. tracing .-> DYNATRACE

    DYNATRACE --> PLATFORM[Dynatrace Platform]
    PLATFORM --> DASHBOARDS[Dashboards & Analytics]
    PLATFORM --> ALERTS[Automated Alerts]
    PLATFORM --> SLO[SLO Reporting]

    ALERTS --> INCIDENTS[Incident Response]
    INCIDENTS --> EDGE

%% Notes:
%% - Services expose APIs through a gateway and communicate asynchronously via an event bus.
%% - Each service owns its data store, enabling independent scaling and deployments.
%% - Dynatrace OneAgent injects tracing/metrics from every service instance.
%% - Observability data flows into Dynatrace Platform for dashboards, alerting, and SLO tracking.
```

## Legend / Roles

- **API Gateway** secures and routes external traffic to backend services.
- **Domain Services** (Auth, Catalog, Order, Payment, Inventory, Shipping, Notification) encapsulate business capabilities with their own data stores.
- **Event Bus** supports asynchronous workflows and decoupling.
- **Dynatrace OneAgent** collects metrics, traces, and logs automatically from service runtimes.
- **Dynatrace Platform** aggregates observability data, powering dashboards, alerts, and SLOs.
- **Incident Response** uses alerts to triage issues and drive fixes.
