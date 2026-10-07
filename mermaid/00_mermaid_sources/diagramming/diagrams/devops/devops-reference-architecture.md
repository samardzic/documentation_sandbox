# DevOps Reference Architecture

```mermaid
graph TD
    DEV[Developers] --> SCM[Source Control Repository]
    SCM --> PR[Pull Requests & Code Review]
    PR --> CI[Continuous Integration Pipeline]
    CI --> BUILD[Build & Package]
    CI --> TEST[Automated Tests]
    TEST --> QA[Quality Gates]
    QA --> ARTIFACTS[Artifact Repository]

    ARTIFACTS --> CD[Continuous Deployment Pipeline]
    CD --> INFRA[Infrastructure Automation]
    INFRA --> CLOUD[Cloud Environment]
    CD --> DEPLOY[Application Deployment]
    DEPLOY --> CLOUD

    CLOUD --> MON[Monitoring & Observability]
    MON --> ALERTS[Alerting]
    MON --> DASH[Dashboards]
    ALERTS --> INCIDENT[Incident Response]
    INCIDENT --> BACKLOG[Backlog / Hotfix]
    BACKLOG --> DEV

%% Notes:
%% - Source control and PR reviews kick off automated CI for builds and tests.
%% - Passing artifacts flow into CD, which handles infrastructure provisioning and application rollout.
%% - Monitoring feeds alerting and dashboards; incidents create feedback loops into development backlog.
```

## Legend / Roles

- **Developers / Source Control**: manage code, run pull requests, and trigger CI.
- **CI Pipeline**: executes builds, automated tests, and quality gates before publishing artifacts.
- **Artifact Repository**: stores versioned deployable packages.
- **CD Pipeline**: orchestrates deployments and infrastructure automation (IaC).
- **Infrastructure Automation**: provisions cloud resources via Terraform/ARM/CloudFormation, etc.
- **Monitoring & Observability**: collects metrics, logs, traces, and routes alerts.
- **Incident Response & Backlog**: feeds operational learnings back into development.
