# 🚀 Lambda in Private VPC

[![License](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)](LICENSE.md)  
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://securityscorecards.dev/viewer/?uri=github.com/Hack23/lambda-in-private-vpc)  
[![CI/CD](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml/badge.svg)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/main.yml)  
[![Scorecard Security](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml/badge.svg?branch=main)](https://github.com/Hack23/lambda-in-private-vpc/actions/workflows/scorecard.yml)

> **Enterprise-grade multi-region active/active architecture** with near-zero recovery time, comprehensive DNS failover, and AWS Resilience Hub policy compliance for mission-critical applications.

## 📋 Table of Contents

- [📑 Project Overview](#-project-overview)
- [🏗️ Architecture](#️-architecture)
- [🔐 Network & Security](#-network--security)
- [🧪 Resilience Testing](#-resilience-testing)
- [⏱️ Recovery Objectives](#️-recovery-objectives)
- [🔄 CI/CD Pipeline](#-cicd-pipeline)
- [📦 Infrastructure as Code](#-infrastructure-as-code)
- [📚 Documentation](#-documentation)
- [📄 License](#-license)

## 📑 Project Overview

This project implements a highly resilient serverless architecture with AWS Lambda functions deployed in private VPCs across multiple AWS regions (Ireland and Frankfurt). It features comprehensive security controls, automated failover mechanisms, and stringent disaster recovery capabilities through AWS Resilience Hub policy enforcement.

```mermaid
mindmap
  root((Lambda in Private VPC))
    Infrastructure
      Dual-Region VPCs
      Private Subnet Isolation
      VPC Endpoints
      DNS Firewall
      Flow Logs
    Compute & API
      Lambda Functions
      API Gateway
      Custom Domain
      Route 53 Failover
      Health Checks
    Security
      Private DNS
      WAFv2 Protection
      Network ACLs
      Security Groups
      KMS Encryption
    Resilience
      Mission-Critical Policy
      RTO/RPO Enforcement
      Multi-Region Active/Active
      Automated Failover
      Chaos Engineering
    Data Layer
      Global Tables
      Cross-Region Replication
      Point-in-Time Recovery
      Dead Letter Queues
    CI/CD & Observability
      Automated Deployment
      Security Scanning
      Alarms & Notifications
      CloudWatch Monitoring
      X-Ray Tracing
```

## 🏗️ Architecture

A true active/active multi-region architecture with isolated private subnets, global data replication, and automated failover systems.

```mermaid
flowchart TB
    subgraph "Multi-Region Active/Active Architecture"
        subgraph "Ireland (eu-west-1)"
            IR_VPC["VPC 10.1.0.0/16"] --> IR_SUBNETS["Private Subnets (3 AZs)"]
            IR_SUBNETS --> IR_LAMBDA["Lambda Functions"]
            IR_LAMBDA --> IR_DYNAMO["DynamoDB\nGlobal Table"]
            IR_LAMBDA --> IR_API["API Gateway"]
            IR_API --> IR_DOMAIN["Custom Domain"]
            IR_VPC --> IR_FLOW["Flow Logs"]
            IR_VPC --> IR_DNS["DNS Firewall"]
            IR_SUBNETS --> IR_EP["VPC Endpoints"]
        end
        
        subgraph "Frankfurt (eu-central-1)"
            FR_VPC["VPC 10.5.0.0/16"] --> FR_SUBNETS["Private Subnets (3 AZs)"]
            FR_SUBNETS --> FR_LAMBDA["Lambda Functions"]
            FR_LAMBDA --> FR_DYNAMO["DynamoDB\nGlobal Table"]
            FR_LAMBDA --> FR_API["API Gateway"]
            FR_API --> FR_DOMAIN["Custom Domain"]
            FR_VPC --> FR_FLOW["Flow Logs"]
            FR_VPC --> FR_DNS["DNS Firewall"]
            FR_SUBNETS --> FR_EP["VPC Endpoints"]
        end
        
        IR_DOMAIN -.-> R53["Route 53\nWeighted/Failover"]
        FR_DOMAIN -.-> R53
        IR_DYNAMO <--> FR_DYNAMO
        
        WAF["WAF v2"] --> IR_API
        WAF --> FR_API
        
        HC["Health Checks"] --> IR_API
        HC --> FR_API
        HC -.-> R53
        
        REH["AWS Resilience Hub\nMission Critical Policy"] --> IR_LAMBDA
        REH --> FR_LAMBDA
        REH --> IR_DYNAMO
        REH --> FR_DYNAMO
    end

    classDef ireland fill:#81c784,stroke:#2e7d32,stroke-width:2px,color:#000;
    classDef frankfurt fill:#64b5f6,stroke:#1565c0,stroke-width:2px,color:#000;
    classDef security fill:#ef5350,stroke:#c62828,stroke-width:2px,color:#fff;
    classDef routing fill:#ffab40,stroke:#f57c00,stroke-width:2px,color:#000;
    classDef resilience fill:#ba68c8,stroke:#7b1fa2,stroke-width:2px,color:#fff;

    class IR_VPC,IR_SUBNETS,IR_LAMBDA,IR_DYNAMO,IR_API,IR_DOMAIN,IR_FLOW,IR_DNS,IR_EP ireland;
    class FR_VPC,FR_SUBNETS,FR_LAMBDA,FR_DYNAMO,FR_API,FR_DOMAIN,FR_FLOW,FR_DNS,FR_EP frankfurt;
    class WAF,HC security;
    class R53 routing;
    class REH resilience;
```

### Key Components

- **Isolated Private VPCs**: Dedicated VPCs in each region with no internet access
- **Multi-AZ Deployment**: 3 private subnets across availability zones for high availability
- **Private Network Controls**: Security groups, NACLs, and flow logs for comprehensive protection
- **Global Data Layer**: DynamoDB global tables with automatic multi-region replication
- **Intelligent Routing**: Route 53 health checks and weighted routing with automatic failover
- **API Gateway**: Regional endpoints with custom domain names and WAF protection

## 🔐 Network & Security

```mermaid
graph TD
    subgraph "Network Security Architecture"
        VPC["VPC (10.1.0.0/16)"]
        
        subgraph "Private Subnet Security"
            NACL["Network ACLs"] --> DENY["Deny RDP (3389)"]
            NACL --> ALLOW_OUT["Allow HTTPS Outbound (443)"]
            SGFVPC["VPC Endpoint SG"]
            SGLMB["Lambda SG"]
            
            SGFVPC --> ALLOW_SG["Allow HTTPS from Lambda SG"]
            SGLMB --> ALLOW_VPC["Allow HTTPS to VPC Endpoints"]
        end
        
        VPC --> SUBNET["Private Subnets"]
        SUBNET --> NACL
        SUBNET --> SGFVPC
        SUBNET --> SGLMB
        
        VPC --> DNS_FW["DNS Firewall"]
        DNS_FW --> ALLOW_AWS["Allow *.amazonaws.com"]
        DNS_FW --> BLOCK_ALL["Block All Other Domains"]
        
        VPC --> FLOW["Flow Logs"]
        FLOW --> CWLOGS["CloudWatch Logs"]
        
        KMS["KMS Encryption"]
        KMS --> SNS_ENC["SNS Topic Encryption"]
        KMS --> LOGS_ENC["Log Group Encryption"]
        
        WAF["WAFv2"] --> RULES["AWS Managed Rules"]
        RULES --> IP_REP["IP Reputation List"]
        RULES --> ANON_IP["Anonymous IP List"]
        RULES --> COMMON["Common Rule Set"]
        RULES --> BAD_IN["Known Bad Inputs"]
        RULES --> LINUX["Linux Rule Set"]
        RULES --> UNIX["Unix Rule Set"]
    end
    
    classDef vpc fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    classDef nacl fill:#e1f5fe,stroke:#0277bd,stroke-width:2px;
    classDef sg fill:#fff8e1,stroke:#ff8f00,stroke-width:2px;
    classDef dns fill:#f3e5f5,stroke:#7b1fa2,stroke-width:2px;
    classDef encryption fill:#ffebee,stroke:#c62828,stroke-width:2px;
    classDef waf fill:#fce4ec,stroke:#c2185b,stroke-width:2px;
    
    class VPC,SUBNET vpc;
    class NACL,DENY,ALLOW_OUT nacl;
    class SGFVPC,SGLMB,ALLOW_SG,ALLOW_VPC sg;
    class DNS_FW,ALLOW_AWS,BLOCK_ALL dns;
    class KMS,SNS_ENC,LOGS_ENC encryption;
    class WAF,RULES,IP_REP,ANON_IP,COMMON,BAD_IN,LINUX,UNIX waf;
```

### Security Features

- **Private VPC Design**: No internet gateways, isolated subnets
- **DNS Firewall**: Allows only AWS domains, blocks all other outbound DNS queries
- **Network ACLs**: Customized ingress/egress rules with RDP blocking
- **Security Groups**: Least-privilege access between Lambda and VPC endpoints
- **Comprehensive WAF Protection**: Six AWS managed rule groups for API security
- **VPC Flow Logs**: Network traffic visibility and auditing with encrypted logs
- **KMS Encryption**: Custom KMS keys for SNS topics and CloudWatch logs
- **IAM Least Privilege**: Detailed IAM roles and policies for all components

## 🧪 Resilience Testing

```mermaid
flowchart TB
    subgraph "Fault Injection Experiments"
        DR["Disaster Recovery\nTest Framework"]
        
        DR --> API_FAIL["API Gateway\nLambda Access\nDenial"]
        DR --> DDB_DEL["DynamoDB Table\nDeletion"]
        DR --> PITR["Point-In-Time\nRecovery"]
        DR --> BACKUP["Backup\nRestoration"]
        
        API_FAIL --> SSM_API["SSM Automation\nDeny Access"]
        DDB_DEL --> SSM_DEL["SSM Automation\nDelete Table"]
        PITR --> SSM_PITR["SSM Automation\nRestore PITR"]
        BACKUP --> SSM_BACK["SSM Automation\nRestore Backup"]
        
        SSM_API --> MONITOR["Health Check\nMonitoring"]
        SSM_DEL --> MONITOR
        SSM_PITR --> MONITOR
        SSM_BACK --> MONITOR
        
        MONITOR --> FAILOVER["Automatic\nRoute 53\nFailover"]
        MONITOR --> RESTORE["Recovery\nAutomation"]
    end

    classDef framework fill:#e1bee7,stroke:#8e24aa,stroke-width:2px;
    classDef experiment fill:#bbdefb,stroke:#1976d2,stroke-width:2px;
    classDef automation fill:#c8e6c9,stroke:#388e3c,stroke-width:2px;
    classDef monitoring fill:#ffecb3,stroke:#ffa000,stroke-width:2px;
    classDef recovery fill:#ffcdd2,stroke:#d32f2f,stroke-width:2px;
    
    class DR framework;
    class API_FAIL,DDB_DEL,PITR,BACKUP experiment;
    class SSM_API,SSM_DEL,SSM_PITR,SSM_BACK automation;
    class MONITOR monitoring;
    class FAILOVER,RESTORE recovery;
```

### Chaos Engineering Capabilities

- **AWS Fault Injection Service**: Predefined experiments to simulate failures
- **Lambda Access Denial**: Tests API Gateway resilience during Lambda failures
- **DynamoDB Failure Scenarios**: Table deletion and recovery testing
- **Point-in-Time Recovery**: Automated restore procedures with defined RPOs
- **Backup Restoration**: Complete data recovery from backups
- **SSM Automation Documents**: Pre-defined recovery runbooks for all scenarios

## ⏱️ Recovery Objectives

This architecture achieves stringent recovery time objectives (RTO) and recovery point objectives (RPO) through AWS Resilience Hub policy enforcement.

```mermaid
stateDiagram-v2
    [*] --> MissionCritical
    
    state MissionCritical {
        [*] --> Region
        Region: "Regional Failure"
        Region: RTO: 3600s (1h)
        Region: RPO: 5s
        
        Region --> AZ
        AZ: "AZ Failure"
        AZ: RTO: 1s
        AZ: RPO: 1s
        
        AZ --> Hardware
        Hardware: "Hardware Failure"
        Hardware: RTO: 1s
        Hardware: RPO: 1s
        
        Hardware --> Software
        Software: "Software Failure"
        Software: RTO: 5400s (90m)
        Software: RPO: 300s (5m)
        
        Software --> [*]
    }
```

### Recovery Metrics

| Failure Scenario | Recovery Time Objective | Recovery Point Objective | Implementation |
|------------------|--------------------------|--------------------------|----------------|
| **Regional Failure** | 3600s (1 hour) | 5s | Multi-region active/active with Route 53 failover |
| **Availability Zone Failure** | 1s | 1s | Multi-AZ deployment in each region |
| **Hardware Failure** | 1s | 1s | AWS managed infrastructure redundancy |
| **Software Failure** | 5400s (90 min) | 300s (5 min) | Automated recovery procedures and global tables |

## 🔄 CI/CD Pipeline

```mermaid
flowchart TD
    GH_PUSH["GitHub Push/Workflow Dispatch"] --> SEC_SCAN{"Security Scanning"}
    
    SEC_SCAN --> CFN_LINT["cfn-lint"]
    SEC_SCAN --> CFN_NAG["cfn-nag"]
    SEC_SCAN --> CHECKOV["Checkov"]
    SEC_SCAN --> SCORECARD["Scorecard"]
    SEC_SCAN --> ZAP["ZAP API Scan"]
    
    CFN_LINT & CFN_NAG & CHECKOV & SCORECARD & ZAP --> CONFIG_IR["Configure AWS (eu-west-1)"]
    
    CONFIG_IR --> DEPLOY_IR["Deploy Core → Ireland"]
    DEPLOY_IR --> OUTPUTS["Collect Outputs"]
    OUTPUTS --> CONFIG_FR["Configure AWS (eu-central-1)"]
    CONFIG_FR --> DEPLOY_FR["Deploy Core → Frankfurt"]
    
    DEPLOY_FR --> DEPLOY_AUX["Deploy Auxiliary Stacks"]
    DEPLOY_AUX --> DEPLOY_R53["Route 53 Configuration"]
    DEPLOY_AUX --> DEPLOY_WAF["WAF Configuration"]
    DEPLOY_AUX --> DEPLOY_RHB["Resilience Hub App"]
    DEPLOY_AUX --> DEPLOY_DR["Disaster Recovery Tests"]
    
    DEPLOY_R53 & DEPLOY_WAF & DEPLOY_RHB & DEPLOY_DR --> TAG["Tag & Release"]
    
    classDef github fill:#f8cecc,stroke:#b85450,stroke-width:2px;
    classDef security fill:#d5e8d4,stroke:#82b366,stroke-width:2px;
    classDef deploy fill:#dae8fc,stroke:#6c8ebf,stroke-width:2px;
    classDef auxiliary fill:#fff2cc,stroke:#d6b656,stroke-width:2px;
    classDef release fill:#e1d5e7,stroke:#9673a6,stroke-width:2px;
    
    class GH_PUSH github;
    class SEC_SCAN,CFN_LINT,CFN_NAG,CHECKOV,SCORECARD,ZAP security;
    class CONFIG_IR,DEPLOY_IR,OUTPUTS,CONFIG_FR,DEPLOY_FR deploy;
    class DEPLOY_AUX,DEPLOY_R53,DEPLOY_WAF,DEPLOY_RHB,DEPLOY_DR auxiliary;
    class TAG release;
```

### Automated Workflow

1. **Security Scanning**: Multiple tools scan CloudFormation templates for issues
2. **Sequential Deployment**: Ireland deployment, followed by Frankfurt
3. **Cross-Region Integration**: Output collection and sharing between regions
4. **Auxiliary Resources**: Route 53, WAF, Resilience Hub, and DR test configuration
5. **Automated Release**: Version tagging and release notes generation

## 📦 Infrastructure as Code

This project is entirely defined as CloudFormation templates with comprehensive resource definitions.

### Template Structure

| Template | Purpose | Key Components |
|----------|---------|----------------|
| **template.yml** | Core Infrastructure | VPCs, Subnets, Lambda, API Gateway, DynamoDB, DNS Firewall, Security Groups |
| **route53.yml** | DNS Configuration | Weighted A/AAAA records, Health check integration, Failover configuration |
| **app.yml** | Resilience Hub | Mission Critical policy definition, RTO/RPO targets, Multi-resource mapping |
| **disaster-recovery.yml** | DR Testing | FIS experiments, SSM automation documents, Recovery procedures |
| **waf.yml** | Security Rules | WAF WebACL with AWS managed rules for API protection |

### Key Resource Highlights

- **DNS Firewall**: Allows only AWS domains, blocks all others
- **Private DNS Configuration**: Secure VPC DNS settings
- **Network ACLs**: Custom ingress/egress rules
- **Health Checks**: Route 53 health checks for API endpoints
- **WAF Protection**: Six AWS managed rule groups
- **Global Tables**: Cross-region data replication
- **IAM Roles**: Least privilege principle implementation

## 📚 Documentation

### Runbooks

- **DynamoDB Recovery**: Automated systems manager runbook
- **Lambda Function Recovery**: Automated systems manager runbook
- **API Gateway Recovery**: Step-by-step recovery procedures
- **IAM Automation**: Identity and access management workflows

### References

- [AWS Resilience Hub Documentation](https://docs.aws.amazon.com/resilience-hub/latest/userguide/)
- [Disaster Recovery on AWS - Part I: Strategies for Recovery in the Cloud](https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-i-strategies-for-recovery-in-the-cloud/)
- [Disaster Recovery on AWS - Part IV: Multi-site Active/Active](https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-iv-multi-site-active-active/)
- [AWS Service Level Agreements](https://aws.amazon.com/legal/service-level-agreements/)

## 📄 License

This project is licensed under the Apache License 2.0 - see [LICENSE.md](LICENSE.md) for details.
