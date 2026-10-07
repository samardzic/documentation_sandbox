# Copilot Prompt Templates for Mermaid Architecture Diagrams

Use these prompts in VS Code with GitHub Copilot to generate or extend Mermaid diagrams. Replace placeholders in {braces}.

## General Guidance

- Ask for a specific architecture style and context (domain, scale, constraints).
- Require: explicit component names, comments explaining roles/interactions, a title/legend, and clean indentation.
- Prefer flowchart TD/LR for simplicity; consider sequence/er diagrams for specific scenarios.

## Base Prompt

```text
Use Mermaid to draw a {architecture-style} architecture for {domain/use-case}.
Requirements:
- Clear title or legend inside the diagram.
- Explicit components with descriptive names (e.g., Web Client, API Gateway, Authentication Service, PostgreSQL Database).
- Comments explaining roles and interactions (use lines starting with %% in Mermaid).
- Clean structure and indentation. Group related elements using subgraph.
- Show both sync (direct arrows) and async paths (event bus/queue) if applicable.
Also add 1-2 alternative variants of the diagram to show different design choices.
Return only Mermaid code blocks.
```

## Style-Specific Starters

### Monolithic

```text
Create a Mermaid flowchart for a traditional monolithic web application.
Include: Load Balancer, Monolithic Application, PostgreSQL Database, Redis Cache.
Add comments on scaling, single deployable unit, and shared DB.
```

### Microservices with API Gateway + BFF

```text
Draw a microservices architecture with an API Gateway and separate BFFs for Web and Mobile.
Include services: Authentication, Product Catalog, Order, Payment; each with its own database.
Add an Event Bus and annotate publish/subscribe relations.
```

### Hexagonal (Ports & Adapters)

```text
Diagram a hexagonal architecture separating domain core from inbound/outbound adapters.
Show REST/CLI/Event inbound and DB/Cache/External API outbound adapters.
Annotate ports and technology-agnostic domain.
```

### Event-Driven + CQRS

```text
Model an event-driven system with CQRS.
Include Command API, Command Handler, Aggregates, Event Store, Event Bus, Projection Service, Read Models, Query API.
Explain write/read separation and eventual consistency.
```

### Serverless Web Backend

```text
Sketch a serverless backend: CDN+Static Hosting, API Gateway, Functions, NoSQL DB, Queue, Worker Function, Object Storage.
Comment on scale-to-zero and async processing.
```

### Cloud-Native on Kubernetes

```text
Show a cloud-native microservices system on Kubernetes with a service mesh.
Include Ingress, API Gateway, multiple services, Event Broker, and per-service databases.
Note mesh features: mTLS, retries, timeouts; K8s features: autoscaling, config, secrets.
```

## Quality Checklist

- Is there a title? Are names explicit? Are comments present and helpful?
- Are related components grouped? Are async flows clearly labeled?
- Are variants provided where meaningful?
