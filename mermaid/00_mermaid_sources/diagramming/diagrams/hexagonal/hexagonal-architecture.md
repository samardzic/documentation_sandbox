# Hexagonal Architecture (Ports & Adapters)

```mermaid
graph LR
    REST[REST Controller] --> PORTS[Ports]
    CLI[CLI Adapter] --> PORTS
    EVT[Event Listener] --> PORTS

    PORTS --> APP[Application Core]

    APP --> PORTS
    PORTS --> DB[PostgreSQL Adapter]
    PORTS --> CACHE[Redis Adapter]
    PORTS --> EXT[External API Adapter]

%% Notes:
%% - Domain core is technology-agnostic and depends only on ports.
%% - Adapters implement ports to interact with external systems.
%% - Replace adapters without changing domain logic.
```

## Legend / Roles

- **Application Core** contains domain logic without infrastructure details.
- **Ports** define the interfaces the domain expects to use.
- **Inbound Adapters** (REST, CLI, Event) translate external input into port calls.
- **Outbound Adapters** (DB, Cache, External API) implement outbound ports to reach external systems.
