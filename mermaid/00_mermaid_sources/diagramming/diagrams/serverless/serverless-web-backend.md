# Serverless Web Backend

```mermaid
graph TD
    C[Web Client] --> CDN[CDN + Static Hosting]
    CDN --> APIG[API Gateway]
    APIG --> FN[Function Auth Logic]
    FN --> DB[Managed NoSQL Database]
    FN --> QUEUE[Queue]
    QUEUE --> WFN[Worker Function]
    WFN --> OBJ[Object Storage]

%% Notes:
%% - Static assets served via CDN; dynamic calls via API Gateway.
%% - Functions scale to zero; pay-per-invocation.
%% - Async processing via queue and worker function.
```

## Legend / Roles

- **Web Client** loads static assets and calls APIs.
- **CDN + Static Hosting** serves front-end files from edge locations.
- **API Gateway** authorizes and routes API calls to functions.
- **Functions** run backend logic on demand.
- **Managed NoSQL Database** persists application data.
- **Queue** buffers long-running tasks handled by worker functions.
- **Object Storage** stores generated artifacts or media.
