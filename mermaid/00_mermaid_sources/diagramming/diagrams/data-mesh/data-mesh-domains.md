# Data Mesh Domain-Oriented Platform

```mermaid
graph LR
    %% Domain data products
    subgraph Domain_A [Sales Domain]
        SALEX[Sales Transaction Product]
        SALED[Sales Analytics Product]
    end

    subgraph Domain_B [Inventory Domain]
        INVSTOCK[Inventory Product]
        INVFORECAST[Inventory Forecast Product]
    end

    subgraph Domain_C [Customer Domain]
        CUST360[Customer 360 Product]
    end

    %% Self-serve data platform services
    SALEX --> PLATFORM[Self-Serve Data Platform]
    SALED --> PLATFORM
    INVSTOCK --> PLATFORM
    INVFORECAST --> PLATFORM
    CUST360 --> PLATFORM

    PLATFORM --> GOVERNANCE[Governance & Catalog]
    GOVERNANCE --> CONSUMERS[Data Consumers]
    PLATFORM --> CONSUMERS

%% Notes:
%% - Each domain team owns data products with clear SLAs, quality, and documentation.
%% - Self-serve platform provides pipelines, storage, catalog, and observability capabilities.
%% - Governance ensures global policies, identity, and lineage.
```

## Legend / Roles

- **Domain Data Products** encapsulate datasets managed by cross-functional domain teams.
- **Self-Serve Data Platform** provides standardized tooling (data pipelines, storage, observability).
- **Governance & Catalog** maintains metadata, policies, and lineage transparency.
- **Data Consumers** discover and integrate products (analytics, ML, downstream services).
