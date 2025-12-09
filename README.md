# golden-path-idp
A self-service portal that triggers a GitOps workflow (ArgoCD) to provision cloud infrastructure

# 🚀 The Golden Path: Internal Developer Platform (IDP)

![Platform Engineering](https://img.shields.io/badge/Platform%20Engineering-IDP-blue?style=for-the-badge&logo=kubernetes)
![GitOps](https://img.shields.io/badge/GitOps-ArgoCD-orange?style=for-the-badge&logo=argo)
![IaC](https://img.shields.io/badge/Infrastructure-Crossplane-green?style=for-the-badge&logo=terraform)

## 📋 Executive Summary
This project demonstrates a production-grade **Internal Developer Platform (IDP)** designed to bridge the gap between Development and Operations.

Moving away from traditional "TicketOps," this platform empowers developers to self-service cloud-native infrastructure (like AWS RDS, S3, and EKS namespaces) using a "Golden Path" approach. It utilizes **Backstage** for the frontend catalog, **ArgoCD** for GitOps delivery, and **Crossplane** for Kubernetes-native infrastructure provisioning.

---

## 🏗️ High-Level Architecture

The system follows a strict **GitOps** workflow. No infrastructure is provisioned manually. The "State of the World" is entirely defined in Git.

```mermaid
graph TD
    subgraph "Developer Experience Layer"
        Dev[User / Developer] -->|1. Clicks Create| Backstage[Backstage Portal]
    end

    subgraph "Control Plane (Kubernetes)"
        Backstage -->|2. Commits Config| Git[GitHub Repo]
        Argo[ArgoCD Controller] -->|3. Syncs Changes| Git
        Argo -->|4. Applies Manifests| K8s[K8s Cluster]
        
        XP[Crossplane Controller] -->|5. Detects Claim| K8s
    end

    subgraph "Cloud Provider (AWS)"
        XP -->|6. Provisions Resource| RDS[(AWS RDS Database)]
        XP -->|7. Provisions IAM| IAM[IAM Roles]
        RDS -->|8. Returns Connection Secret| XP
    end

    XP -->|9. Injects Secret| K8s
    Dev -->|10. Deploys Code| K8s


    