# Lambda in Private VPC

**Status:** Work in Progress

This project demonstrates a multi-region active/active site using AWS Resilience Hub policy compliance with an RTO/RPO for Application/AZ/Region failures.

## Badges

[![License](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)](https://github.com/Hack23/lambda-in-private-vpc/raw/master/LICENSE.md) [![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc)

## Concepts

Learn more about AWS Resilience Hub concepts [here](https://docs.aws.amazon.com/resilience-hub/latest/userguide/concepts-terms.html).

## Runbooks

- [DynamoDB Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-ddb.html)
- [Lambda Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-lam.html)
- [Application Bridge Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-abp.html)
- [IAM Runbook](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-ref-iam.html)

## Architecture Diagrams

- [Infrastructure](cloudformation/template.png)
- [DNS Route53](cloudformation/route53.png)
- [Web Application Firewall](cloudformation/waf.png)
- [Disaster Recovery](cloudformation/disaster-recovery.png)


## Resilience Hub Screenshots

- [Resilience Hub Policy](ResilienceHubPolicy.png)
- [Application](ResiliencyHub-App.png)
- [App Recommendation 1](ResiliencyHub-App-rec1.png)
- [App Recommendation 2](ResiliencyHub-App-rec2.png)
- [Region](ResHub-region.png)

## Relevant Links

- [Route53 Application Recovery Controller](https://aws.amazon.com/route53/application-recovery-controller/)
- [Route53 Resolver DNS Firewall](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html)
- [SLA MAX Calculator](https://github.com/mikaelvesavuori/slamax) and [Cloud SLA](https://github.com/mikaelvesavuori/cloud-sla)

For more information on AWS service level agreements, visit the [AWS SLA page](https://aws.amazon.com/legal/service-level-agreements/).

## SLA and SLO

This setup provides an SLA level of 99.45% uptime/availability, which allows for the following periods of downtime/unavailability:

- Daily: 7m 55s
- Weekly: 55m 26s
- Monthly: 3h 59m 4.8s
- Quarterly: 11h 57m 14s
- Yearly: 1d 23h 48m 58s

A multi-region active/active architecture is more resilient to regional failures, and DynamoDB global tables offer a 99.999% availability.

For more information on AWS Proactive Event Support, visit [AWS Premium Support](https://aws.amazon.com/premiumsupport/technology/pes/).
