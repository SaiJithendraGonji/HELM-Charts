# ⛵ Helm Charts Collection

> Reusable, production-tested Helm charts for deploying microservices and platform tooling on Kubernetes — built from patterns used across AKS and EKS clusters at Lloyds Banking Group, GlobalLogic, and OneAdvanced.

These charts are parameterised for multi-environment deployment, ArgoCD-compatible for GitOps workflows, and include production-grade defaults that most quickstart charts skip — resource limits, probes, RBAC, network policies, and Vault secret injection.

---

## 📦 Charts

### Application Charts
```
charts/
├── microservice-base/       # Opinionated base chart for Spring Boot / Node.js / Python services
│   ├── Deployment           # With resource requests/limits, security context, probes
│   ├── Service              # ClusterIP with optional external exposure
│   ├── HPA                  # Horizontal Pod Autoscaler, CPU + custom metrics
│   ├── PodDisruptionBudget  # Ensures availability during node drains
│   ├── NetworkPolicy        # Default deny, explicit ingress/egress rules
│   └── ServiceAccount       # With IRSA (AWS) / Workload Identity (Azure) support
│
└── cronjob/                 # Scheduled job template with failure alerting
```

### Platform / Infrastructure Charts
```
charts/
├── observability/
│   ├── prometheus-stack/    # Prometheus + Grafana + Alertmanager (kube-prometheus-stack wrapper)
│   ├── grafana-dashboards/  # Custom dashboards: SLO, cost, cluster health, gateway latency
│   └── otel-collector/      # OpenTelemetry collector with Dynatrace / Datadog exporters
│
├── ingress/
│   ├── nginx-ingress/       # NGINX with TLS termination, rate limiting, WAF annotations
│   └── cert-manager/        # Let's Encrypt + custom CA cert automation
│
└── secrets/
    └── vault-agent/         # HashiCorp Vault agent injector sidecar configuration
```

---

## 🎯 Key Features

- **Multi-environment values** — single chart, separate `values-dev.yaml`, `values-prod.yaml` per environment
- **ArgoCD ready** — structured for Application CRD deployment from a GitOps repo
- **Security defaults** — non-root containers, read-only root filesystem, dropped capabilities, no privilege escalation
- **HPA with custom metrics** — scales on CPU, memory, and application-level Prometheus metrics
- **Vault secret injection** — HashiCorp Vault agent sidecar pattern; no secrets in ConfigMaps or environment variables
- **IRSA / Workload Identity** — ServiceAccount annotations for keyless AWS/Azure credential access
- **PodDisruptionBudgets** — maintains availability during cluster upgrades and node drains

---

## 🚀 Usage

```bash
# Add and update
helm repo add sai-charts https://github.com/SaiJithendraGonji/HELM-Charts
helm repo update

# Deploy a microservice
helm install my-service sai-charts/microservice-base \
  -f values-prod.yaml \
  --namespace my-namespace

# Deploy observability stack
helm install observability sai-charts/observability/prometheus-stack \
  --namespace monitoring \
  --create-namespace

# Lint before deploy
helm lint charts/microservice-base/ -f values-prod.yaml

# Template output for review
helm template my-service charts/microservice-base/ -f values-prod.yaml
```

---

## 📁 Repository Structure

```
├── charts/
│   ├── microservice-base/
│   │   ├── Chart.yaml
│   │   ├── values.yaml            # Defaults (safe for all environments)
│   │   └── templates/
│   │       ├── deployment.yaml
│   │       ├── service.yaml
│   │       ├── hpa.yaml
│   │       ├── pdb.yaml
│   │       ├── networkpolicy.yaml
│   │       └── serviceaccount.yaml
│   └── observability/
│       └── grafana-dashboards/
│           └── dashboards/        # JSON dashboard definitions
├── environments/
│   ├── dev/
│   ├── staging/
│   └── production/
└── .github/
    └── workflows/
        └── helm-lint-test.yml     # Automated chart testing on PR
```

---

## 🌍 Real-World Context

| Organisation | Usage |
|-------------|-------|
| Lloyds Banking Group | AKS-backed backend services within Stratos VDC spokes; ingress/egress gateway configs |
| OneAdvanced | 70+ microservices with Istio service mesh; Grafana dashboards for SLO monitoring |
| GlobalLogic | EKS Fargate with ALB routing; 50+ microservices with Prometheus + CloudWatch |

---

## 🛠️ Tech Stack

![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat&logo=helm&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![ArgoCD](https://img.shields.io/badge/ArgoCD-EF7B4D?style=flat&logo=argo&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat&logo=grafana&logoColor=white)
![HashiCorp Vault](https://img.shields.io/badge/Vault-FFEC6E?style=flat&logo=vault&logoColor=black)
![AWS](https://img.shields.io/badge/AWS-FF9900?style=flat&logo=amazon-aws&logoColor=white)
![Azure](https://img.shields.io/badge/Azure-0078D4?style=flat&logo=microsoft-azure&logoColor=white)

---

## 📖 Related

- [ENDTOEND_DEVSECOPS](https://github.com/SaiJithendraGonji/ENDTOEND_DEVSECOPS) — CI/CD pipeline that deploys these charts
- [Mastering_AWS_Terraform](https://github.com/SaiJithendraGonji/Mastering_AWS_Terraform) — Terraform provisions the clusters these charts run on
- [CKA-Preparation](https://github.com/SaiJithendraGonji/CKA-Preparation) — Kubernetes depth behind the chart internals
