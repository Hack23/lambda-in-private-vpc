# 🚀 Lambda in Private VPC

![License](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)  
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://securityscorecards.dev/viewer/?uri=github.com/Hack23/lambda-in-private-vpc)  
[![CI/CD](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml/badge.svg)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml)  
[![Scorecard Security](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml/badge.svg?branch=main)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml)

> **Description:** A highly available system spanning **Ireland** and **Frankfurt** AWS regions, enforcing RTO/RPO via AWS Resilience Hub, chaos‑tested by FIS, and fronted by API Gateway with Route 53 failover & WAF protection.

---

## 📋 Table of Contents

- [🧠 Project Mindmap](#-project-mindmap)  
- [🌐 Overview](#-overview)  
- [📐 Architecture](#-architecture)  
- [🔗 Network Topology](#-network-topology)  
- [🚦 CI/CD Workflow](#-ci-cd-workflow)  
- [🚨 Disaster Recovery Flow](#-disaster-recovery-flow)  
- [🛡️ Resilience Hub Policy](#️-resilience-hub-policy)  
- [🖼️ Screenshots](#️-screenshots)  
- [📦 Templates](#-templates)  
- [🛠️ Tech Stack](#️-tech-stack)  
- [📖 Runbooks](#-runbooks)  
- [🔗 References](#-references)  
- [📄 License](#-license)

---

## 🧠 Project Mindmap

```mermaid
mindmap
  root((Lambda in Private VPC))
    Infra
      VPC
      Subnets
      Endpoints
      ACLs & SGs
    Compute
      LambdaHealth
      LambdaCRUD
    API
      API_Gateway
      CustomDomain
      Route53
    Resilience
      ResilienceHub
      FIS
      WAFv2
    CI_CD
      Linting
      SecurityScans
      Deploy
    Data
      DynamoDBGlobal
      DeadLetterSNS
```

---

## 🌐 Overview

Run Lambda inside private subnets in two regions, with:

- **No public access**: VPC Endpoints for S3, EC2, DynamoDB  
- **Multi-region failover**: Route 53 weighted DNS  
- **Resiliency**: AWS Resilience Hub policies & AWS FIS chaos  
- **Layer7 Security**: AWS WAFv2 rules  
- **CI/CD**: GitHub Actions with CFN lint, cfn-nag, Checkov, ZAP, scoring, and cross‑account deploys  

---

## 📐 Architecture

```mermaid
flowchart LR
  subgraph Ireland [eu-west-1]
    VPC1[VPC: 10.1.0.0/16]
    Subs1["Private Subnets A/B/C"]
    EP1["S3/EC2/DDB Endpoints"]
    Lambdas1["Lambda (Health & CRUD)"]
    APIGW1["API Gateway"]
  end

  subgraph Frankfurt [eu-central-1]
    VPC2[VPC: 10.5.0.0/16]
    Subs2["Private Subnets A/B/C"]
    EP2["S3/EC2/DDB Endpoints"]
    Lambdas2["Lambda (Health & CRUD)"]
    APIGW2["API Gateway"]
  end

  Ireland --> Subs1 --> Lambdas1 --> EP1
  Subs1 --> APIGW1
  Frankfurt --> Subs2 --> Lambdas2 --> EP2
  Subs2 --> APIGW2

  APIGW1 -. Failover .-> Route53
  APIGW2 -. Failover .-> Route53
  classDef region fill:#f9f,stroke:#333,stroke-width:1px;
  class Ireland,Frankfurt region
```

---

## 🔗 Network Topology

```mermaid
graph TD
  VPC[VPC<br/>10.1.0.0/16]
  subgraph Subnets["Private Subnets"]
    S1[10.1.0.0/24]
    S2[10.1.1.0/24]
    S3[10.1.2.0/24]
  end
  VPC --> Subnets
  Subnets --> ACL[Network ACL]
  Subnets --> SG_L[Lambda SG]
  subgraph Endpoints["VPC Endpoints"]
    EP_S3[S3]
    EP_EC2[EC2]
    EP_DDB[DynamoDB]
  end
  Subnets --> Endpoints
```

---

## 🚦 CI/CD Workflow

```mermaid
flowchart TD
  A[Push or Dispatch] --> B{Lint & Security}
  B --> C[cfn-lint]
  B --> D[cfn-nag]
  B --> E[Checkov]
  B --> F[StandardLint]
  F --> G[DependencyReview]
  E --> H[Scorecard]
  C --> I[ZAP API Scan]
  H --> J{Deploy Jobs}
  J --> K[Deploy Ireland]
  K --> L[Collect Outputs]
  L --> M[Deploy Frankfurt]
  M --> N[Route53 Stack]
  N --> O[DR Stack]
  O --> P[ResilienceHub App]
  P --> Q[Tag & Release]
```

---

## 🚨 Disaster Recovery Flow

```mermaid
sequenceDiagram
  participant U as User
  participant R53 as Route53
  participant GW as API_Gateway
  participant L as Lambda
  participant D as DynamoDB
  U->>R53: GET /v1/healthcheck
  R53->>GW: Route to region
  GW->>L: Invoke healthcheck
  L-->>U: "OK"
  Note over FIS: Inject failures
  FIS->>GW: Deny invoke
  FIS->>D: Delete table
  alt Recovery
    R53->>GW: Failover to backup
    GW->>L: Invoke fallback
  end
```

---

### 🛡️ Resilience Hub Policy

```mermaid
stateDiagram-v2
    [*] --> Region
    Region: RTO = 3600s\nRPO = 5s
    Region --> AZ
    AZ: RTO = 1s\nRPO = 1s
    AZ --> Hardware
    Hardware: RTO = 1s\nRPO = 1s
    Hardware --> Software
    Software: RTO = 5400s\nRPO = 300s
    Software --> [*]
```

---

## 🖼️ Screenshots

### Resilience Hub

![Policy](ResilienceHubPolicy.png)  
![App](ResiliencyHub-App.png)  
![Rec1](ResiliencyHub-App-rec1.png)  
![Rec2](ResiliencyHub-App-rec2.png)  
![Region View](ResHub-region.png)

### Infrastructure Diagrams

| Diagram             | Preview                                  |
|---------------------|------------------------------------------|
| Core Infra          | ![Infra](cloudformation/template.png)    |
| Route 53 DNS        | ![Route53](cloudformation/route53.png)   |
| Application Firewall| ![WAF](cloudformation/waf.png)           |

---

## 📦 Templates

```bash
cloudformation/
├─ template.yml           # VPC, subnets, Lambdas, API, DynamoDB
├─ route53.yml            # DNS failover records
├─ app.yml                # Resilience Hub App & Policy
├─ disaster-recovery.yml  # FIS experiments
└─ waf.yml                # WAFv2 rules
```

---

## 🛠️ Tech Stack

- **Infra as Code:** CloudFormation  
- **Serverless:** Lambda (Node.js 20.x)  
- **API:** API Gateway (Regional, Custom Domain)  
- **Storage:** DynamoDB Global Tables  
- **Networking:** VPC, Private Subnets, Endpoints, ACLs, SGs  
- **DNS:** Route 53 Weighted Failover  
- **Resiliency:** AWS Resilience Hub, FIS  
- **Security:** WAFv2, IAM Roles & Policies  
- **CI/CD:** GitHub Actions, cfn-lint, cfn-nag, Checkov, ZAP, Scorecard  

Details: [techstack.md](./techstack.md)

---

## 📖 Runbooks

- **DynamoDB** – SSM Automation for tables/indexes  
- **Lambda** – SSM Automation for functions & aliases  
- **App Runner** – Manage App Runner & domains  
- **IAM** – Automate IAM user/group/role operations  

---

## 🔗 References

- DR I: https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-i-strategies-for-recovery-in-the-cloud/  
- DR IV: https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-iv-multi-site-active-active/  
- Resilience Hub: https://docs.aws.amazon.com/resilience-hub/latest/userguide/  
- Route 53 ARC: https://aws.amazon.com/route53/application-recovery-controller/  
- DNS Firewall: https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html/  
- SLA Tools: https://github.com/mikaelvesavuori/slamax | https://github.com/mikaelvesavuori/cloud-sla  

---

## 📄 License

This project is licensed under the **Apache License 2.0**. See [LICENSE.md](LICENSE.md).  
