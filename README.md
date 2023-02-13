# lambda-in-private-vpc

status : WIP

POC of multi region active/active site with resilience hub policy compliance with RTO/RPO 5 Min for Application/AZ/Region failures with setup SOP and fault injections(proof of executions of SOP)
Zero trust example

Api gateway with schema request validation and cross region replicated dynamodb. Route53 geolocation  dns setup.

Badges
[![license](https://img.shields.io/github/license/Hack23/lambda-in-private-vpc.svg)]([https://github.com/Hack23/lambda-in-private-vpc](https://github.com/Hack23/lambda-in-private-vpc)/raw/master/LICENSE.md)
[![OpenSSF Scorecard](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc/badge)](https://api.securityscorecards.dev/projects/github.com/Hack23/lambda-in-private-vpc)

![Policy](https://github.com/Hack23/lambda-in-private-vpc/raw/main/ResilienceHubPolicy.png)

![Route53 Policy](https://github.com/Hack23/lambda-in-private-vpc/raw/main/route53-policy.png)

![App](https://github.com/Hack23/lambda-in-private-vpc/raw/main/ResiliencyHub-App.png)

![Infrastructure](https://github.com/Hack23/lambda-in-private-vpc/raw/main/template.png)

![App recommendation](https://github.com/Hack23/lambda-in-private-vpc/raw/main/ResiliencyHub-App-rec1.png)

![App recommendation2](https://github.com/Hack23/lambda-in-private-vpc/raw/main/ResiliencyHub-App-rec2.png)


https://aws.amazon.com/route53/application-recovery-controller/
https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-dns-firewall.html
