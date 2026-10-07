# Layered Architecture

```mermaid
flowchart TB
    subgraph Presentation Layer
        UI[Web UI]
        API[REST API]
    end

    subgraph Business Layer
        SVC[Domain Services]
        RULES[Business Rules]
    end

    subgraph Data Access Layer
        REPO[Repositories]
        ORM[ORM / Data Mappers]
    end

    subgraph Infrastructure Layer
        DB[(PostgreSQL Database)]
        MQ[(Message Broker)]
        CACHE[(Redis Cache)]
    end

    UI --> API
    API --> SVC
    SVC --> RULES
    SVC --> REPO
    REPO --> ORM
    ORM --> DB
    SVC --> MQ
    SVC --> CACHE

%% Notes:
%% - Strict layering enforces separation of concerns.
%% - Presentation does not access data directly; goes through domain.
%% - Infrastructure concerns are isolated.
```

## Legend / Roles

- **Presentation Layer** hosts user interfaces and API endpoints.
- **Business Layer** holds domain services and business rules.
- **Data Access Layer** abstracts persistence interactions.
- **Infrastructure Layer** provides databases, messaging, and caching services.
