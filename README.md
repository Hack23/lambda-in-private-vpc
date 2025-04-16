# 🚀 Lambda in Private VPC

[![License](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)](LICENSE.md)  
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://securityscorecards.dev/viewer/?uri=github.com/Hack23/lambda-in-private-vpc)  
[![CI/CD](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml/badge.svg)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml)  
[![Scorecard Security](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml/badge.svg?branch=main)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml)

> **Description:** Run AWS Lambda in private subnets across Ireland and Frankfurt, with multi‑region high availability, RTO/RPO enforcement, chaos testing, DNS failover, and WAF protection.

---

## 📋 Table of Contents

- [🧠 Project Mindmap](#-project-mindmap)  
- [📐 Architecture](#-architecture)  
- [🔗 Network Topology](#-network-topology)  
- [🚦 CI/CD Workflow](#-ci-cd-workflow)  
- [🚨 Disaster Recovery Strategies](#-disaster-recovery-strategies)  
- [🔒 Business Continuity Plan](#-business-continuity-plan)  
- [🛡️ Resilience Hub Policy](#️-resilience-hub-policy)  
- [📦 CloudFormation Templates](#-cloudformation-templates)  
- [🛠️ Tech Stack](#️-tech-stack)  
- [📖 Runbooks](#-runbooks)  
- [🔗 References](#-references)  
- [📄 License](#-license)

---

## 🧠 Project Mindmap

```mermaid
mindmap
  root((Lambda in Private VPC))
    Infrastructure((Infrastructure))
      VPC
      Subnets
      Endpoints
      ACLs & SGs
      FlowLogs
    Compute((Compute))
      HealthLambda
      CrudLambda
    API & DNS((API & DNS))
      APIGateway
      CustomDomain
      Route53
      HealthChecks
    Data((Data))
      DynamoDB_GlobalTable
      DeadLetter_SNS
    Security((Security))
      WAFv2
      IAM_Roles
      KMS
      Scans
        CFN_lint
        CFN_nag
        Checkov
        ZAP
        Scorecard
    Resilience_DR((Resilience & DR))
      ResHub
      RTO_RPO
      HA
      DR_Strategies
        BackupRestore
        PilotLight
        WarmStandby
        MultiSite
      BCP
    CI_CD((CI/CD))
      Linting
      SecurityScans
      Deploy
        Ireland
        Frankfurt
        AuxStacks
      Release
    Observability((Observability))
      CW_Logs
      Alarms
      XRay
    Documentation((Docs))
      README
      Runbooks
      Contributing
      Changelog
      TechStack
```

---

## 📐 Architecture

```mermaid
flowchart LR
  subgraph IR [Ireland (eu-west-1)]
    V1[VPC 10.1.0.0/16]
    S1[Subnets A/B/C]
    EP1[Endpoints: S3/EC2/DDB]
    L1[Lambdas]
    G1[API Gateway]
  end
  subgraph FR [Frankfurt (eu-central-1)]
    V2[VPC 10.5.0.0/16]
    S2[Subnets A/B/C]
    EP2[Endpoints: S3/EC2/DDB]
    L2[Lambdas]
    G2[API Gateway]
  end
  V1 --> S1 --> L1 --> EP1
  S1 --> G1
  V2 --> S2 --> L2 --> EP2
  S2 --> G2
  G1 -. Failover .-> DNS[Route 53]
  G2 -. Failover .-> DNS
  classDef region fill:#e0f7fa,stroke:#006064;
  class IR,FR region
```

---

## 🔗 Network Topology

```mermaid
graph TD
  VPC[VPC: 10.1.0.0/16]
  subgraph Private_Subnets
    S1[10.1.0.0/24]
    S2[10.1.1.0/24]
    S3[10.1.2.0/24]
  end
  VPC --> Private_Subnets
  Private_Subnets --> ACL[Network ACL]
  Private_Subnets --> SG[Security Groups]
  Private_Subnets --> Endpoints{VPC Endpoints}
  Endpoints --> EP_S3[S3]
  Endpoints --> EP_EC2[EC2]
  Endpoints --> EP_DDB[DynamoDB]
```

---

## 🚦 CI/CD Workflow

```mermaid
flowchart TD
  A[Dispatch/Push] --> B{Lint & Scan}
  B --> C[cfn-lint]
  B --> D[cfn-nag]
  B --> E[Checkov]
  B --> F[StandardLint]
  B --> G[Scorecard]
  G --> H[ZAP API Scan]
  H --> I[Configure AWS creds (eu-west-1)]
  I --> J[Deploy Core → Ireland]
  J --> K[Collect Outputs]
  K --> L[Configure AWS creds (eu-central-1)]
  L --> M[Deploy Core → Frankfurt]
  M --> N[Aux Stacks → Route 53/DR/ResHub]
  N --> O[Tag & Release]
```

---

## 🚨 Disaster Recovery Strategies

```mermaid
flowchart TB
  DR[Disaster Recovery Patterns]
  DR --> BR[Backup & Restore]
  DR --> PL[Pilot Light]
  DR --> WS[Warm Standby]
  DR --> MS[Multi‑site Active‑Active]

  subgraph Info1 [Backup & Restore]
    BR1[Backups & Snapshots]
    BR2[Restore in Alt Region]
  end
  subgraph Info2 [Pilot Light]
    PL1[Core Infra On]
    PL2[Scale-on-Demand]
  end
  subgraph Info3 [Warm Standby]
    WS1[Scaled-Down Prod]
    WS2[Instant Scale]
  end
  subgraph Info4 [Multi‑site]
    MS1[Full Prod Everywhere]
    MS2[Global LB]
  end
  DR --> Info1 & Info2 & Info3 & Info4
```

---

## 🔒 Business Continuity Plan

```mermaid
mindmap
  root((BCP Plan))
    ImpactAnalysis
      Financial
      Operational
      Reputational
      Regulatory
    RecoveryObjectives
      RTO
      RPO
      MTTR
      Uptime
    Strategies
      BackupRestore
      PilotLight
      WarmStandby
      MultiSite
    Communication
      Stakeholders
      Channels
      Templates
    Testing
      Quarterly
      SemiAnnual
      Annual
```

```mermaid
timeline
  title RTO/RPO & Uptime Targets
  section RTO (Recovery Time)
    Infra & API : 1h
    Core Lambdas : 2h
    Data Services : 4h
  section RPO (Data Loss)
    Transient Logs : 5m
    User Data : 15m
    Config & State : 1h
  section Uptime
    App Endpoint : 99.9%
    DNS Failover : 99.99%
    Monitoring : 24/7
```

---

## 🛡️ Resilience Hub Policy

```mermaid
stateDiagram-v2
    [*] --> Region
    Region: RTO=3600s\nRPO=5s
    Region --> AZ
    AZ: RTO=1s\nRPO=1s
    AZ --> Hardware
    Hardware: RTO=1s\nRPO=1s
    Hardware --> Software
    Software: RTO=5400s\nRPO=300s
    Software --> [*]
```

---

## 📦 CloudFormation Templates

- **template.yml** – VPC, subnets, endpoints, Lambdas, API, DynamoDB  
- **route53.yml** – Route 53 weighted/failover records  
- **app.yml** – AWS Resilience Hub App & Policy  
- **disaster-recovery.yml** – AWS FIS experiments  
- **waf.yml** – AWS WAFv2 WebACL  

---

## 🛠️ Tech Stack

- AWS CloudFormation  
- AWS Lambda (Node.js 20.x)  
- Amazon API Gateway (Regional)  
- DynamoDB Global Tables  
- VPC Endpoints & Security  
- AWS Resilience Hub & FIS  
- AWS WAFv2 & IAM  
- GitHub Actions, cfn-lint, cfn-nag, Checkov, ZAP, Scorecard  

Details: [techstack.md](techstack.md)

---

## 📖 Runbooks

- **DynamoDB** – AWS Systems Manager  
- **Lambda** – AWS Systems Manager  
- **App Runner** – AWS App Runner  
- **IAM** – AWS IAM Automation  

---

## 🔗 References

- AWS Resilience Hub: https://docs.aws.amazon.com/resilience-hub/latest/userguide/  
- DR I: https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-i-strategies-for-recovery-in-the-cloud/  
- DR IV: https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-iv-multi-site-active-active/  
- AWS SLA: https://aws.amazon.com/legal/service-level-agreements/  

---

## 📄 License

Apache License 2.0 – see [LICENSE.md](LICENSE.md)
