The following was generated to be added to your application.

## api-gw recommendations:


### Alarms:

- AWSResilienceHub-RestApiGwLatencyAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub that tracks time between when REST API Gateway receives a request from a client and when it returns a response to the client. Triggers when this interval statistically exceed threshold  
  - Relevant Resource Ids: zhkuxajjsi
- AWSResilienceHub-RestApiGw4xxErrorsAlarm_2020-04-01
  - Description: Alarm by AWS Resilience Hub that tracks the percentage of 4xx responses received from REST API Gateway. Remember that some services may expect a certain percent of legitimate 4xx responses  
  - Relevant Resource Ids: zhkuxajjsi
- AWSResilienceHub-RestApiGw5xxErrorsAlarm_2020-04-01
  - Description: Alarm by AWS Resilience Hub that tracks the percentage of 5xx responses received from API Gateway. Triggers when the 5xx are over ${Threshold} percent  
  - Relevant Resource Ids: zhkuxajjsi
- AWSResilienceHub-RestApiGwErrorCountAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub that tracks the invocation anomalies, when requests count from REST API Gateway abnormally spike or drop.  
  - Relevant Resource Ids: zhkuxajjsi
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

- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyUpdateItemAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to UpdateItem operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBPendingReplicationCountAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub that triggers when the number of item updates that are written to one replica table, but that have not yet been written to another replica in the global table is too high  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyDeleteItemAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to DeleteItem operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyTransactWriteItemsAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to TransactWriteItems operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyPutItemAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to PutItem operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBConditionalCheckFailedRequestsAlarm_2020-04-01
  - Description: Reports when the number of failed attempts to perform conditional writes (Put/Update/Delete) is greater than or equal to the threshold  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyBatchGetItemAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to BatchGetItem operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyGetItemAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to GetItem operation, is equal to or greater than the specified ${Threshold}.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBWriteThrottleEventsAlarm_2020-04-01
  - Description: Reports when amount of write throttle events is greater than threshold  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyBatchWriteItemAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to BatchWriteItem operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBSuccessfulRequestLatencyTransactGetItemsAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub for Amazon DynamoDB, that reports elapsed time for successful requests with regards to TransactGetItems operation, is equal to or greater than the specified threshold.  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBReadThrottleEventsAlarm_2020-04-01
  - Description: Reports when amount of read throttle events is greater than threshold  
  - Relevant Resource Ids: global-table
- AWSResilienceHub-DynamoDBReplicationLatencyAlarm_2020-04-01
  - Description: Alarm by AWS ResilienceHub that reports when replication from one replica table to another takes too much time  
  - Relevant Resource Ids: global-table
---

## lambda recommendations:


### Alarms:

- AWSResilienceHub-LambdaAnomalousInvocationCountAlarm_2020-07-13
  - Description: Reports when invocations count is anomalous  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaThrottlesAlarm_2020-07-13
  - Description: Reports when sustained throttles occur  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaAnomalousMemoryDeviationsAlarm_2020-04-01
  - Description: Reports when average memory consumption is anomalous  
  - Prerequisite: LambdaInsights must be enabled on the function https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Lambda-Insights-Getting-Started.html  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaErrorsAlarm_2020-07-13
  - Description: Reports when sustained errors occur  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaAnomalousMemorySoftLimitAlarm_2020-04-01
  - Description: Reports when memory consumption achieves soft limit  
  - Prerequisite: LambdaInsights must be enabled on the function https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Lambda-Insights-Getting-Started.html  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaAnomalousDurationCountAlarm_2020-04-01
  - Description: alarm by AWS ResilienceHub that that reports when lambda duration is less or higher then normal  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-LambdaConcurrentExecutionsAlarm_2020-04-01
  - Description: Reports concurrent execution status  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
---

## sns recommendations:


### Alarms:

- AWSResilienceHub-SNSNumberOfNotificationsFailedAlarm_2022-04-04
  - Description: Alarm by AWS Resilience Hub that alerts when there are notification failures.  
  - Relevant Resource Ids: arn:aws:sns:eu-west-1:172017021075:lambda-vpc-DeadLetterTopic-N9updhYF6ukA