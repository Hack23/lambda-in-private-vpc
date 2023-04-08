
# Lambda in Private VPC

**Status:** Work in Progress

This project demonstrates a multi-region active/active site using AWS Resilience Hub policy compliance with an RTO/RPO for Application/AZ/Region failures, ensuring high availability and fault tolerance.

## Badges

[![License](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)](https://github.com/Hack23/lambda-in-private-vpc/raw/master/LICENSE.md) [![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc)

## Concepts

Learn more about AWS Resilience Hub concepts and understand the key terms and principles involved in building resilient applications [here](https://docs.aws.amazon.com/resilience-hub/latest/userguide/concepts-terms.html).

## Runbooks

- [DynamoDB Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-ddb.html) - Automates the management of DynamoDB tables and indexes.
- [Lambda Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-lam.html) - Helps manage Lambda functions, layers, and aliases.
- [Application Bridge Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-abp.html) - Supports management of Amazon App Runner services and custom domains.
- [IAM Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-iam.html) - Facilitates IAM user, group, role, and policy management.

## Architecture Diagrams

- [Infrastructure](cloudformation/template.png) - Depicts the overall infrastructure, including AWS services and components.
- [DNS Route53](cloudformation/route53.png) - Shows the Route 53 configuration for DNS routing and failover.
- [Web Application Firewall](cloudformation/waf.png) - Displays the setup of the Web Application Firewall for securing your application.
- [Disaster Recovery](cloudformation/disaster-recovery.png) - Illustrates the disaster recovery strategy for the application.

## Resilience Hub Screenshots

- [Resilience Hub Policy](ResilienceHubPolicy.png) - Overview of the policy settings in AWS Resilience Hub.
- [Application](ResiliencyHub-App.png) - The application setup and components in AWS Resilience Hub.
- [App Recommendation 1](ResiliencyHub-App-rec1.png) - First set of recommendations for improving application resiliency.
- [App Recommendation 2](ResiliencyHub-App-rec2.png) - Second set of recommendations for enhancing application resiliency.
- [Region](ResHub-region.png) - Regional recommendations

## Relevant Links

- [Route53 Application Recovery Controller](https://aws.amazon.com/route53/application-recovery-controller/) - Service for managing and testing application recovery across AWS Regions.
- [Route53 Resolver DNS Firewall](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html) - A managed DNS firewall service to protect applications from malicious DNS activity.
- [SLA MAX Calculator](https://github.com/mikaelvesavuori/slamax) and [Cloud SLA](https://github.com/mikaelvesavuori/cloud-sla) - Tools for calculating and comparing cloud service SLAs.

For more information on AWS service level agreements, visit the [AWS SLA page](
