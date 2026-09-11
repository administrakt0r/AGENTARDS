# DevOps Stack Template

## Detected Technologies
- **CI/CD:** [GitHub Actions/GitLab CI/CircleCI/Jenkins/etc.]
- **Containers:** [Docker/Podman/etc.]
- **Orchestration:** [Kubernetes/Docker Compose/etc.]
- **IaC:** [Terraform/Pulumi/CloudFormation/etc.]
- **Monitoring:** [Prometheus/Grafana/Datadog/etc.]
- **Secrets:** [Vault/Sealed Secrets/SOPS/etc.]

## Common Stack Patterns

### CI/CD Pipelines
- GitHub Actions: `.github/workflows/*.yml`
- GitLab CI: `.gitlab-ci.yml`
- CircleCI: `.circleci/config.yml`
- Jenkins: `Jenkinsfile`
- Pipeline stages: build → test → deploy

### Container Orchestration
- Docker Compose: `docker-compose.yml`
- Kubernetes: `deployments/`, `k8s/`, `manifests/`
- Helm: `charts/`
- Kustomize: `kustomization.yaml`

### Infrastructure as Code
- Terraform: `*.tf`, `modules/`, `environments/`
- Pulumi: `Pulumi.yaml`, program files
- CloudFormation: `template.yaml`

### Monitoring Stack
- Prometheus: `prometheus.yml`, alert rules
- Grafana: dashboards, `provisioning/`
- Logging: ELK, Loki, Fluentd

### Common File Locations
```
├── .github/workflows/    # GitHub Actions
├── docker/               # Dockerfiles
├── deploy/               # Deployment configs
├── k8s/ or kubernetes/   # K8s manifests
├── terraform/            # IaC files
├── scripts/              # Utility scripts
├── monitoring/           # Monitoring configs
└── secrets/              # Encrypted secrets
```

## Project Type Detection Signals
- `.github/workflows/` directory
- `Dockerfile` or `docker-compose.yml`
- `*.tf` files
- `Jenkinsfile`
- `.gitlab-ci.yml`
- `k8s/` or `kubernetes/` directory

## Agent Customizations

### Security
- Secret scanning in CI
- Container image scanning
- Infrastructure drift detection
- Access control audit

### Reliability
- Pipeline flakiness detection
- Rollback procedures
- Health check configuration
- Alert threshold tuning

### Cost
- Resource right-sizing
- Unused resource detection
- Reserved instance planning
- Cost anomaly detection
