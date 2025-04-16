# 🚀 Lambda in Private VPC

[![License](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)](LICENSE.md)  
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://securityscorecards.dev/viewer/?uri=github.com/Hack23/lambda-in-private-vpc)  
[![CI/CD](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml/badge.svg)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml)  
[![Scorecard Security](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml/badge.svg?branch=main)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml)

> **Enterprise-grade multi‑region active/active architecture** with automated failover, comprehensive disaster recovery, and strict RTO/RPO enforcement for mission-critical applications.

## 📋 Table of Contents

- [🧠 Project Overview](#-project-overview)
- [📐 Architecture](#-architecture)
- [🔗 Network Topology](#-network-topology)
- [🚦 CI/CD Pipeline](#-cicd-pipeline)
- [🚨 Disaster Recovery Framework](#-disaster-recovery-framework)
- [⏱️ Business Continuity Planning](#️-business-continuity-planning)
- [🔒 Security & Compliance](#-security--compliance)
- [📦 Infrastructure as Code](#-infrastructure-as-code)
- [🛠️ Tech Stack](#️-tech-stack)
- [📖 Runbooks](#-runbooks)
- [🔗 References](#-references)
- [📄 License](#-license)

## 🧠 Project Overview

This project implements a highly resilient, secure serverless architecture using AWS Lambda in private VPCs across multiple regions. It's designed for enterprise-grade applications requiring stringent security, high availability, and disaster recovery capabilities.

```mermaid
mindmap
  root((Lambda in Private VPC))
    Infrastructure
      VPC
      Subnets
      Endpoints
      ACLs & SGs
      FlowLogs
    Compute
      HealthLambda
      CrudLambda
    API & DNS
      APIGateway
      CustomDomain
      Route53
      HealthChecks
    Data
      DynamoDB_GlobalTable
      DeadLetter_SNS
    Security
      WAFv2
      IAM_Roles
      KMS
      Scans
        CFN_lint
        CFN_nag
        Checkov
        ZAP
        Scorecard
    Resilience_DR
      ResHub
      RTO_RPO
      HA
      DR_Strategies
        BackupRestore
        PilotLight
        WarmStandby
        MultiSite
      BCP
    CI_CD
      Linting
      SecurityScans
      Deploy
        Ireland
        Frankfurt
        AuxStacks
      Release
    Observability
      CW_Logs
      Alarms
      XRay
    Documentation
      README
      Runbooks
      TechStack
```

## 📐 Architecture

The architecture implements a multi-region active/active design with isolated private VPCs, comprehensive security controls, and automated failover capabilities.

```mermaid
flowchart TB
    subgraph "Ireland (eu-west-1)"
        subgraph "Ireland VPC (10.1.0.0/16)"
            IPS[Private Subnets]
            IEP[VPC Endpoints]
            ISG[Security Groups]
            
            IPS --> IEP
            IPS --> ISG
        end
        
        IL[Lambda Functions] --> IPS
        IAPI[API Gateway] --> IL
        IDT[DynamoDB Table] --> IL
        IL --> IDT
    end
    
    subgraph "Frankfurt (eu-central-1)"
        subgraph "Frankfurt VPC (10.5.0.0/16)"
            FPS[Private Subnets]
            FEP[VPC Endpoints]
            FSG[Security Groups]
            
            FPS --> FEP
            FPS --> FSG
        end
        
        FL[Lambda Functions] --> FPS
        FAPI[API Gateway] --> FL
        FDT[DynamoDB Table] --> FL
        FL --> FDT
    end
    
    DR[Route 53] --> IAPI
    DR --> FAPI
    
    IDT <-.-> FDT
    
    WAF[WAF] --> IAPI
    WAF --> FAPI
    
    subgraph "Monitoring & Resilience"
        CW[CloudWatch]
        AH[AWS Resilience Hub]
        XR[X-Ray]
        
        CW --> IL
        CW --> FL
        AH --> IL
        AH --> FL
        XR --> IL
        XR --> FL
    end

    classDef primary fill:#e1f5fe,stroke:#0277bd,stroke-width:2px;
    classDef secondary fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    classDef security fill:#ffebee,stroke:#c62828,stroke-width:2px;
    classDef resilience fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    
    class IPS,FPS,IEP,FEP,ISG,FSG primary;
    class IL,FL,IAPI,FAPI,IDT,FDT secondary;
    class WAF,DR security;
    class CW,AH,XR resilience;
```

### Key Architecture Components

- **Multi-Region Deployment**: Active/active setup in Ireland (eu-west-1) and Frankfurt (eu-central-1)
- **VPC Isolation**: Private subnets with no internet access for enhanced security
- **VPC Endpoints**: Secure AWS service access without internet exposure
- **Global Data Replication**: DynamoDB global tables with multi-region consistency
- **Intelligent Routing**: Route 53 health checks with automated failover
- **Identity & Access**: Fine-grained IAM roles following least privilege principle
- **Application Protection**: WAFv2 rules to protect API endpoints
- **Comprehensive Monitoring**: CloudWatch, X-Ray, and custom health checks

## 🔗 Network Topology

Each region implements a secure network topology with private subnets, strict network controls, and comprehensive logging.

```mermaid
graph TB
    subgraph "VPC Architecture"
        VPC["VPC (10.1.0.0/16)"] --> PS["Private Subnets"]
        
        subgraph "Private Subnets"
            PS1["Subnet 1 (10.1.0.0/24)"]
            PS2["Subnet 2 (10.1.1.0/24)"]
            PS3["Subnet 3 (10.1.2.0/24)"]
        end
        
        PS --> ACL["Network ACLs"]
        PS --> SG["Security Groups"]
        PS --> EP["VPC Endpoints"]
        
        subgraph "VPC Endpoints"
            S3["S3 Gateway"]
            DDB["DynamoDB Gateway"]
            EC2["EC2 Interface"]
            CW["CloudWatch Interface"]
            SSM["SSM Interface"]
            KMS["KMS Interface"]
        end
        
        FL["Flow Logs"] --> VPC
    end
    
    Lambda["Lambda Functions"] --> PS
    Lambda --> SG
    Lambda --> EP
    
    classDef vpc fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef network fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef security fill:#ffebee,stroke:#c62828,stroke-width:2px;
    classDef compute fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    
    class VPC,PS,PS1,PS2,PS3 vpc;
    class ACL,SG,FL network;
    class EP,S3,DDB,EC2,CW,SSM,KMS security;
    class Lambda compute;
```

### Network Security Features

| Feature | Implementation | Purpose |
|---------|---------------|---------|
| **Private Subnets** | 3 AZs per region | Isolate compute resources from internet |
| **Security Groups** | Stateful, fine-grained | Control traffic at instance level |
| **Network ACLs** | Stateless, subnet-level | Additional layer of network security |
| **VPC Endpoints** | Gateway and Interface | Secure AWS service access |
| **Flow Logs** | VPC, subnet, and ENI levels | Network traffic visibility and auditing |
| **Transit Encryption** | TLS for all traffic | Data protection in transit |

## 🚦 CI/CD Pipeline

The project implements a comprehensive CI/CD pipeline with security scanning, multi-region deployment, and automated verification.

```mermaid
flowchart TB
    start[Code Push/Dispatch] --> lint{Security Scanning}
    
    lint --> cfn["cfn-lint"]
    lint --> nag["cfn-nag"]
    lint --> chk["Checkov"]
    lint --> score["Scorecard"]
    lint --> zap["ZAP API Scan"]
    
    cfn --> configure_irl["Configure AWS (eu-west-1)"]
    nag --> configure_irl
    chk --> configure_irl
    score --> configure_irl
    zap --> configure_irl
    
    configure_irl --> deploy_irl["Deploy Core → Ireland"]
    deploy_irl --> outputs["Collect Outputs"]
    outputs --> configure_fra["Configure AWS (eu-central-1)"]
    configure_fra --> deploy_fra["Deploy Core → Frankfurt"]
    deploy_fra --> deploy_aux["Deploy Auxiliary Stacks"]
    deploy_aux --> release["Tag & Release"]
    
    classDef start fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef security fill:#ffebee,stroke:#c62828,stroke-width:2px;
    classDef deploy fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef release fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    
    class start start;
    class lint,cfn,nag,chk,score,zap security;
    class configure_irl,deploy_irl,outputs,configure_fra,deploy_fra,deploy_aux deploy;
    class release release;
```

### Pipeline Features

- **Security-First Approach**: Multiple security scanning tools run before deployment
- **Infrastructure Validation**: Templates are validated before deployment
- **Multi-Region Coordination**: Sequential deployment to ensure proper resource creation
- **Output Management**: Cross-region resource information is shared between deployments
- **Automated Release**: Successful deployments trigger release creation with changelogs
- **Rollback Capability**: Failed deployments automatically roll back to previous state

## 🚨 Disaster Recovery Framework

The project implements multiple disaster recovery strategies to achieve resilience against various failure scenarios.

```mermaid
flowchart TB
    DR["Disaster Recovery Strategies"] --> BR["Backup & Restore"]
    DR --> PL["Pilot Light"]
    DR --> WS["Warm Standby"]
    DR --> MS["Multi-site Active/Active"]
    
    subgraph "Recovery Capability Evolution"
        BR --> |"Evolve to"| PL
        PL --> |"Evolve to"| WS
        WS --> |"Evolve to"| MS
    end
    
    subgraph "Recovery Metrics"
        BR --- BRMetrics["RTO: Hours/Days<br>RPO: Hours<br>Cost: Low"]
        PL --- PLMetrics["RTO: Hours<br>RPO: Minutes<br>Cost: Low-Medium"]
        WS --- WSMetrics["RTO: Minutes<br>RPO: Minutes<br>Cost: Medium-High"]
        MS --- MSMetrics["RTO: Near-zero<br>RPO: Near-zero<br>Cost: High"]
    end
    
    subgraph "Implementation"
        BR --- BRImpl["• Regular backups<br>• Restore procedures<br>• Recovery testing"]
        PL --- PLImpl["• Core infra always running<br>• Scaled down resources<br>• Rapid scale-up capacity"]
        WS --- WSImpl["• Fully functional standby<br>• Reduced capacity<br>• Auto-scaling ready"]
        MS --- MSImpl["• Multiple full deployments<br>• All regions active<br>• Load balancing across regions"]
    end
    
    classDef strategy fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef metrics fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef implementation fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    
    class DR,BR,PL,WS,MS strategy;
    class BRMetrics,PLMetrics,WSMetrics,MSMetrics metrics;
    class BRImpl,PLImpl,WSImpl,MSImpl implementation;
```

### DR Strategy Comparison

| Strategy | Recovery Time | Data Loss | Cost | Implementation |
|----------|---------------|-----------|------|---------------|
| **Backup & Restore** | Hours/Days | Hours | $ | Backups with documented restore procedures |
| **Pilot Light** | Hours | Minutes | $$ | Core infrastructure running with rapid scale-up |
| **Warm Standby** | Minutes | Minutes | $$$ | Scaled-down but functional standby environment |
| **Multi-site Active/Active** | Near-zero | Near-zero | $$$$ | Full production deployment in multiple regions |

This project implements the **Multi-site Active/Active** approach for maximum resilience and minimal recovery time.

## ⏱️ Business Continuity Planning

Business continuity is managed through comprehensive impact analysis, recovery objectives, and compliance documentation.

```mermaid
mindmap
  root((Business<br>Continuity))
    Impact Analysis
      Financial Impact
        Revenue loss
        Recovery costs
        Reputational damage
      Operational Impact
        Service interruption
        Business process disruption
        Decision-making capability
      Regulatory Impact
        Compliance violations
        Reporting requirements
        Audit considerations
    Recovery Objectives
      RTO (Recovery Time Objective)
        Authentication: < 5 min
        API Gateway: < 5 min
        Lambda Functions: < 5 min
        Data Access: < 5 min
      RPO (Recovery Point Objective)
        Transaction Data: Near-zero
        Configuration Data: < 15 min
        Log Data: < 60 min
      MTTR (Mean Time To Recovery)
        Infrastructure: < 15 min
        Application: < 10 min
        Data: < 5 min
    Resilience Strategies
      Active/Active Deployment
      Automated Failover
      Health Checks
      Self-Healing Systems
    Testing & Validation
      Regular DR Drills
      Automated Recovery Tests
      Compliance Verification
```

### Business Impact Analysis

| Impact Category | Description | Mitigation Strategy |
|-----------------|-------------|---------------------|
| **Financial Impact** | Revenue loss during outages | Multi-region active/active to minimize downtime |
| **Operational Impact** | Business process disruption | Automated failover for service continuity |
| **Reputational Impact** | Customer trust erosion | Transparent monitoring and communication |
| **Regulatory Impact** | Compliance violations | Comprehensive logging and audit trails |

### Recovery Objectives

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

## 🔒 Security & Compliance

The project implements comprehensive security controls and compliance mechanisms.

```mermaid
mindmap
  root((Security &<br>Compliance))
    Network Security
      Private VPCs
      Security Groups
      Network ACLs
      Flow Logging
      VPC Endpoints
    Identity & Access
      IAM Roles
      Least Privilege
      Resource Policies
      Temporary Credentials
      Role Assumption
    Data Protection
      Encryption at Rest
      Encryption in Transit
      Key Management (KMS)
      Backup Protection
      Data Lifecycle
    Application Security
      Input Validation
      Output Encoding
      Dependency Scanning
      Code Analysis
      OWASP Top 10 Mitigations
    Compliance Controls
      NIST 800-53
      ISO 27001
      GDPR
      PCI-DSS
      SOC 2
    Security Testing
      Static Analysis
      Dynamic Testing
      Infrastructure Scanning
      Penetration Testing
      Vulnerability Management
```

### Compliance Framework Mapping

| Framework | Relevant Controls | Implementation |
|-----------|-------------------|----------------|
| **NIST SP 800-53 Rev. 5** | CP-2, CP-7, CP-9, CP-10, CP-4(2) | Multi-region deployment, automated recovery, regular testing |
| **NIST CSF 2.0** | RC.RP, RC.RP-4, PR.DS-9, ID.BE-5 | Recovery processes, RPO/RTO targets, backup protection, resilience requirements |
| **ISO 27001:2022** | A.17.1.x, A.17.2.1, A.12.3.1 | Continuity planning, availability management, information backup |
| **AWS Well-Architected** | REL01-09, SEC01-10 | Resilient architecture, security at all layers |

### Resilience Hub Policy

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

## 📦 Infrastructure as Code

The project is defined entirely as Infrastructure as Code using AWS CloudFormation.

```mermaid
flowchart TB
    core["template.yml<br>Core Infrastructure"] --> vpc["VPC & Networking"]
    core --> lambda["Lambda Functions"]
    core --> api["API Gateway"]
    core --> dynamo["DynamoDB"]
    core --> iam["IAM Roles"]
    core --> endpoints["VPC Endpoints"]
    
    subgraph "Additional Templates"
        route53["route53.yml<br>DNS Configuration"]
        resiliencehub["app.yml<br>Resilience Hub App"]
        dr["disaster-recovery.yml<br>DR Components"]
        waf["waf.yml<br>WAF Configuration"]
    end
    
    core --> route53
    core --> resiliencehub
    core --> dr
    core --> waf
    
    classDef core fill:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef component fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef additional fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    
    class core core;
    class vpc,lambda,api,dynamo,iam,endpoints component;
    class route53,resiliencehub,dr,waf additional;
```

### CloudFormation Templates

| Template | Purpose | Key Resources |
|----------|---------|--------------|
| **template.yml** | Core infrastructure | VPC, Subnets, Lambda, API Gateway, DynamoDB |
| **route53.yml** | DNS configuration | Route 53 health checks, failover records |
| **app.yml** | Resilience configuration | AWS Resilience Hub app and policy definition |
| **disaster-recovery.yml** | DR testing | AWS FIS experiments for DR validation |
| **waf.yml** | Security rules | WAFv2 WebACL and rule sets |

## 🛠️ Tech Stack

The project leverages a modern, cloud-native technology stack:

```mermaid
mindmap
  root((Technology<br>Stack))
    Infrastructure
      AWS CloudFormation
      AWS VPC
      AWS Route 53
      AWS KMS
      AWS Resilience Hub
    Compute
      AWS Lambda (Node.js 20.x)
      AWS API Gateway
    Data
      Amazon DynamoDB Global Tables
      AWS S3 (for backups)
    Security
      AWS WAFv2
      AWS IAM
      AWS Security Hub
    DevOps
      GitHub Actions
      cfn-lint
      cfn-nag
      Checkov
      ZAP
      Scorecard
    Monitoring
      AWS CloudWatch
      AWS X-Ray
      Custom Dashboards
```

## 📖 Runbooks

Comprehensive documentation is available for operations and recovery:

| Runbook | Purpose | Implementation |
|---------|---------|----------------|
| **[DynamoDB Runbook](runbooks/dynamodb.md)** | DynamoDB recovery | AWS Systems Manager automation |
| **[Lambda Runbook](runbooks/lambda.md)** | Lambda function recovery | AWS Systems Manager automation |
| **[API Gateway Runbook](runbooks/apigateway.md)** | API Gateway recovery | Recovery procedures |
| **[IAM Runbook](runbooks/iam.md)** | Identity management recovery | IAM automation workflows |

## 🔗 References

- [AWS Resilience Hub Documentation](https://docs.aws.amazon.com/resilience-hub/latest/userguide/)
- [Disaster Recovery on AWS - Part I: Strategies for Recovery in the Cloud](https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-i-strategies-for-recovery-in-the-cloud/)
- [Disaster Recovery on AWS - Part IV: Multi-site Active/Active](https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-iv-multi-site-active-active/)
- [AWS Service Level Agreements](https://aws.amazon.com/legal/service-level-agreements/)
- [AWS Well-Architected Framework - Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html)

## 📄 License

This project is licensed under the Apache License 2.0 - see [LICENSE.md](LICENSE.md) for details.
