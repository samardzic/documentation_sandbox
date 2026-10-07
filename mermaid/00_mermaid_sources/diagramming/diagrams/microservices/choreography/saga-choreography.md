# Microservices Saga (Choreography)

```mermaid
graph LR
    ORDER[Order Service] -- order created event --> BUS[Event Bus]
    BUS -- order created --> PAYMENT[Payment Service]
    PAYMENT -- payment completed --> BUS
    BUS -- payment completed --> INVENTORY[Inventory Service]
    INVENTORY -- stock reserved --> BUS
    BUS -- stock reserved --> SHIPPING[Shipping Service]
    SHIPPING -- shipment scheduled --> BUS
    BUS -- shipment scheduled --> NOTIFY[Notification Service]

    BUS -- payment failed --> ORDER
    BUS -- stock not available --> ORDER

%% Notes:
%% - No central orchestrator; services react to domain events and emit follow-up events.
%% - Compensation handled by services publishing failure events that upstream services react to.
%% - Requires robust event schema management and idempotent handlers.
```

## Legend / Roles

- **Order Service** initiates saga with `order created` events and listens for compensations.
- **Payment Service** processes payments upon order creation events.
- **Inventory Service** reserves stock and emits success/failure events.
- **Shipping Service** schedules delivery when inventory is reserved.
- **Notification Service** informs customers of status updates.
- **Event Bus** distributes events to all interested services.
