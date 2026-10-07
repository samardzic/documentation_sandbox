# Machine Learning Pipeline Architecture

```mermaid
graph TD
    DATA[Raw Data Sources] --> INGEST[Data Ingestion Service]
    INGEST --> LAKE[Data Lake]
    LAKE --> CURATE[Data Curation & Feature Store]
    CURATE --> TRAIN[Model Training Pipeline]
    TRAIN --> EVAL[Model Evaluation]
    EVAL --> REGISTRY[Model Registry]

    REGISTRY --> DEPLOY[Model Deployment Service]
    DEPLOY --> SERVE[Online Prediction API]
    DEPLOY --> BATCHPRED[Batch Scoring Jobs]

    SERVE --> MONITOR[Monitoring & Drift Detection]
    MONITOR --> ALERTS[Alerts]
    MONITOR --> CURATE

%% Notes:
%% - Data ingestion collects structured/unstructured input into the data lake.
%% - Feature store standardizes reusable features for training and inference.
%% - Training pipeline produces models logged in the registry after evaluation gates.
%% - Deployment service rolls out models to online and batch endpoints with canary support.
%% - Monitoring detects drift and routes signals back to data curation for retraining.
```

## Legend / Roles

- **Data Sources** provide raw input (transactional DBs, logs, external datasets).
- **Ingestion Service** handles ETL/ELT pipelines and data validation.
- **Data Lake** stores raw data; **Feature Store** provides curated features.
- **Training Pipeline** runs experimentation (distributed training, hyperparameter tuning).
- **Model Registry** tracks versions, metadata, and approvals.
- **Deployment Service** orchestrates rollout to real-time APIs and batch jobs.
- **Monitoring** watches health, latency, and prediction drift, triggering alerts and retraining.
