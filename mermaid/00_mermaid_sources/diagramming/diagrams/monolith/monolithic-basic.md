# Monolithic Architecture (Basic)

```mermaid
flowchart TD
    C[Web Client] --> LB[Load Balancer]
    LB --> APP[Monolithic Application]
    APP --> DB[(PostgreSQL Database)]
    APP --> CACHE[(Redis Cache)]

%% Explanation:
%% - A single deployable unit handles UI, business logic, and data access.
%% - Horizontal scaling via multiple app instances behind a load balancer.
%% - Shared database and optional cache for performance.
```

## Legend / Roles

- **Web Client** issues HTTP requests to the application.
- **Load Balancer** distributes incoming traffic across application instances.
- **Monolithic Application** contains UI, business logic, and data access.
- **PostgreSQL Database** stores persistent data for all application modules.
- **Redis Cache** speeds up repeated reads and session storage.
