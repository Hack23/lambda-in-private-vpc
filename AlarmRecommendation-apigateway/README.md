The following was generated to be added to your application.

## api-gw recommendations:


### Alarms:

- AWSResilienceHub-RestApiGwErrorCountAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub that tracks the invocation anomalies, when requests count from REST API Gateway abnormally spike or drop.  
  - Relevant Resource Ids: qpp5m03en4, 0uq060248k
- AWSResilienceHub-RestApiGw4xxErrorsAlarm_2020-04-01
  - Description: Alarm by AWS Resilience Hub that tracks the percentage of 4xx responses received from REST API Gateway. Remember that some services may expect a certain percent of legitimate 4xx responses  
  - Relevant Resource Ids: 0uq060248k, qpp5m03en4
- AWSResilienceHub-RestApiGwLatencyAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub that tracks time between when REST API Gateway receives a request from a client and when it returns a response to the client. Triggers when this interval statistically exceed threshold  
  - Relevant Resource Ids: qpp5m03en4, 0uq060248k
- AWSResilienceHub-RestApiGw5xxErrorsAlarm_2020-04-01
  - Description: Alarm by AWS Resilience Hub that tracks the percentage of 5xx responses received from API Gateway. Triggers when the 5xx are over ${Threshold} percent  
  - Relevant Resource Ids: qpp5m03en4, 0uq060248k
---

## app_common recommendations:


### Alarms:

- AWSResilienceHub-SyntheticCanaryInRegionAlarm_2021-04-01
  - Description: A monitor for the entire application, configured to constantly verify that the application API/endpoints are available  
  - Prerequisite: Make sure CloudWatch Synthetics is setup to monitor the application (see the <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html" target="_blank">docs</a>). 
Make sure that the Synthetics Name passed in the alarm dimension matches the name of the Synthetic Canary. It Defaults to the name of the application.
  
  - Relevant Resource Ids: eu-west-1
- AWSResilienceHub-SyntheticCanaryXRegionAlarm_2021-04-01
  - Description: A fallback monitor setup in an independent region to provide alerts in case of a whole region failure.  
  - Prerequisite: Make sure CloudWatch Synthetics is setup to monitor the application (see the <a href="https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Synthetics_Canaries.html" target="_blank">docs</a>). 
Make sure that the Synthetics Name passed in the alarm dimension matches the name of the Synthetic Canary. It Defaults to the name of the application.
  
  - Relevant Resource Ids: eu-west-2
---

## dynamodb recommendations:


### Alarms:

- AWSResilienceHub-DynamoDBConditionalCheckFailedRequestsAlarm_2023-06-03
  - Description: Alarm by AWS Resilience Hub that reports when conditional errors exceed the threshold limit  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBUserErrorsAlarm_2023-06-03
  - Description: Alarm by AWS ResilienceHub that reports when user errors exceed 2% of total read + write requests  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBReadThrottlingAlarm_2023-06-03
  - Description: Alarm by AWS Resilience Hub that reports when read throttle requests exceed 2% of total number of read requests  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBSystemErrorsAlarm_2023-06-03
  - Description: Alarm by AWS ResilienceHub that reports when system errors exceed 2% of total read + write requests  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBAccountLimitReadsAlarm_2023-06-03
  - Description: Alarm by AWS Resilience Hub that reports when consumed table reads approach the account limit  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBReplicationLatencyAlarm_2023-06-03
  - Description: Alarm by AWS ResilienceHub that reports when replication from one replica table to another takes too much time  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-west-1:172017021075:table/global-table, arn:aws:dynamodb:eu-central-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBWriteThrottlingAlarm_2023-06-03
  - Description: Alarm by AWS Resilience Hub that reports when write throttle requests exceed 2% of total number of write requests  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBTransactionConflictAlarm_2023-06-03
  - Description: Alarm that reports when number of transaction conflicts exceed threshold limit of 100  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
- AWSResilienceHub-DynamoDBAccountLimitWritesAlarm_2023-06-03
  - Description: Alarm by AWS Resilience Hub that reports when consumed table writes approach the account limit  
  - Relevant Resource Ids: arn:aws:dynamodb:eu-central-1:172017021075:table/global-table, arn:aws:dynamodb:eu-west-1:172017021075:table/global-table
---

## lambda recommendations:


### Alarms:

- AWSResilienceHub-LambdaAnomalousMemoryDeviationsAlarm_2020-04-01
  - Description: Reports when average memory consumption is anomalous  
  - Prerequisite: LambdaInsights must be enabled on the function https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Lambda-Insights-Getting-Started.html  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaConcurrentExecutionsAlarm_2020-04-01
  - Description: Reports concurrent execution status  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaAnomalousMemorySoftLimitAlarm_2020-04-01
  - Description: Reports when memory consumption achieves soft limit  
  - Prerequisite: LambdaInsights must be enabled on the function https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Lambda-Insights-Getting-Started.html  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaErrorsAlarm_2020-07-13
  - Description: Reports when sustained errors occur  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaAnomalousMemoryDeviationsAlarm_2020-04-01
  - Description: Reports when average memory consumption is anomalous  
  - Prerequisite: LambdaInsights must be enabled on the function https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Lambda-Insights-Getting-Started.html  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaThrottlesAlarm_2020-07-13
  - Description: Reports when sustained throttles occur  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaAnomalousMemorySoftLimitAlarm_2020-04-01
  - Description: Reports when memory consumption achieves soft limit  
  - Prerequisite: LambdaInsights must be enabled on the function https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Lambda-Insights-Getting-Started.html  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaAnomalousDurationCountAlarm_2020-04-01
  - Description: alarm by AWS ResilienceHub that that reports when lambda duration is less or higher then normal  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaAnomalousInvocationCountAlarm_2020-07-13
  - Description: Reports when invocations count is anomalous  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaErrorsAlarm_2020-07-13
  - Description: Reports when sustained errors occur  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaConcurrentExecutionsAlarm_2020-04-01
  - Description: Reports concurrent execution status  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:database, arn:aws:lambda:eu-central-1:172017021075:function:database
- AWSResilienceHub-LambdaAnomalousInvocationCountAlarm_2020-07-13
  - Description: Reports when invocations count is anomalous  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaThrottlesAlarm_2020-07-13
  - Description: Reports when sustained throttles occur  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaAnomalousDurationCountAlarm_2020-04-01
  - Description: alarm by AWS ResilienceHub that that reports when lambda duration is less or higher then normal  
  - Relevant Resource Ids: arn:aws:lambda:eu-central-1:172017021075:function:audittest, arn:aws:lambda:eu-west-1:172017021075:function:audittest
