# Vertical Slice Architecture

```mermaid
graph TD
    %% Orders vertical slice
    UI[Web UI] --> ORD_API[Orders API Endpoint]
    ORD_API --> ORD_APP[Orders Application Service]
    ORD_APP --> ORD_DOMAIN[Orders Domain Logic]
    ORD_DOMAIN --> ORD_DB[Orders Data Store]

    %% Inventory vertical slice
    UI --> INV_API[Inventory API Endpoint]
    INV_API --> INV_APP[Inventory Application Service]
    INV_APP --> INV_DOMAIN[Inventory Domain Logic]
    INV_DOMAIN --> INV_DB[Inventory Data Store]

    %% Payments vertical slice
    UI --> PAY_API[Payments API Endpoint]
    PAY_API --> PAY_APP[Payments Application Service]
    PAY_APP --> PAY_DOMAIN[Payments Domain Logic]
    PAY_DOMAIN --> PAY_DB[Payments Data Store]

    %% Shared infrastructure that slices can call but not couple to tightly
    ORD_APP --> BUS[Event Bus]
    INV_APP --> BUS
    PAY_APP --> BUS
    BUS --> NOTIFY[Notification Service]

%% Notes:
%% - Each feature slice contains UI, API endpoint, application service, domain logic, and persistence.
%% - Cross-slice communication happens through well-defined events or shared platform services.
%% - Reduces large layered dependencies by keeping changes within a single slice.
```

## Legend / Roles

- **Vertical Slices** represent end-to-end features (Orders, Inventory, Payments).
- **UI** layer routes to feature-specific API endpoints.
- **Application Services** coordinate domain logic within the slice.
- **Domain Logic** encapsulates business rules and workflows.
- **Data Stores** belong to each slice, avoiding cross-slice coupling.
- **Event Bus / Notification** handles cross-cutting integration.
