# Event-Driven Architecture with CQRS

```mermaid
flowchart LR
    %% Diagram: Event-Driven Architecture (EDA) with CQRS
    %% Write path
    API[Command API] --> CMDH[Command Handler]
    CMDH --> AGG[Aggregates]
    AGG --> EVTST[(Event Store)]

    %% Event flow
    EVTST -- publish events --> BUS[(Event Bus / Stream)]
    BUS -- deliver events --> PRJ[Projection Service]

    %% Read path
    PRJ --> VIEWS[(Read Models / Views)]
    QUERY[Query API] --> VIEWS

%% Notes:
%% - Commands mutate state on the write side; events are appended to the event store.
%% - Projection service rebuilds read models for fast queries.
%% - Event bus enables decoupled communication and eventual consistency.
```

## Legend / Roles

- **Command API** receives external requests.
- **Command Handler** validates and routes commands to aggregates.
- **Aggregates** enforce domain invariants and emit events to the store.
- **Event Store** persists domain events and publishes them to the bus.
- **Event Bus / Stream** distributes events to downstream consumers.
- **Projection Service** updates read models in response to events.
- **Read Models / Views** power fast queries through the **Query API**.
