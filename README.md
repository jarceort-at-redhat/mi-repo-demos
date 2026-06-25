# Red Hat Connectivity Link (Kuadrant) — GitOps "App of Apps" Demo

This repository contains the complete enterprise-grade automation to deploy a **Red Hat Connectivity Link (RHCL)** (powered by the upstream **Kuadrant** project) and **Red Hat OpenShift Service Mesh v3 (Istio)** environment on OpenShift.

The deployment leverages the **App of Apps** pattern combined with **Argo CD Sync Waves** to dynamically orchestrate multi-namespace infrastructure, automated operators, and an isolated sample application microservice complete with **Edge TLS Termination**, **HTTP-to-HTTPS Redirection**, and advanced **Rate Limiting (L7)**.

---

## 🏛️ Repository Architecture

The project is structured following production GitOps standards. Rather than employing a single massive Helm execution, it uses a top-level Helm orchestrator to establish decoupled, standalone Argo CD sub-applications for each domain:

```text
mi-repo-demos/
├── bootstrap/
│   └── root-app.yaml               # The "Mother" Application (Manually applied)
└── apps/
    ├── 00-root/
    │   ├── Chart.yaml              # App of Apps Helm configurations
    │   ├── values.yaml             # Global Git target definitions
    │   └── templates/
    │       ├── app-service-mesh.yaml # Multi-namespace sub-app 01
    │       ├── app-kuadrant.yaml     # Multi-namespace sub-app 02
    │       ├── app-api-service.yaml  # Multi-namespace sub-app 03
    │       └── app-api-sample.yaml   # Multi-namespace sub-app 04
    └── charts/
        ├── service-mesh/           # Helm Chart: Istio 3.x Control Plane & Sail Operator
        ├── kuadrant/               # Helm Chart: Kuadrant Core & Operator Subscription
        ├── api-service/            # Helm Chart: Sample App Container, Gateway & HTTPRoute
        └── api-sample/             # Helm Chart: Automated UI Plugin Registration