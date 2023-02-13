The following was generated to be added to your application.

## dynamodb recommendations:


### Sops:

- AWSResilienceHub-RestoreDynamoDBTableToPointInTimeSOP_2020-04-01
  - Description: Used to restore Dynamodb to a specific point in time.  
  - Relevant Resource Ids: global-table
---

## lambda recommendations:


### Sops:

- AWSResilienceHub-ChangeLambdaMemorySizeSOP_2020-10-26
  - Description: Change Memory size  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-ChangeLambdaExecutionTimeLimitSOP_2020-10-26
  - Description: Change execution time limit  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-SwitchLambdaVersionInAliasSOP_2020-10-26
  - Description: Switch Alias of Lambda functions to another version  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-ChangeLambdaConcurrencyLimitSOP_2020-10-26
  - Description: Test Lambda behavior when hitting ReservedConcurrentExecutions value  
  - Prerequisite: Make sure that there is enough unreserved concurrency available, otherwise operation will fail (see the <a href="https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html" target="_blank">docs</a>)  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest
- AWSResilienceHub-ChangeLambdaProvisionedConcurrencySOP_2020-10-26
  - Description: Change Provisioned Concurrency  
  - Prerequisite: Make sure that there is enough unreserved concurrency available, otherwise operation will fail (see the <a href="https://docs.aws.amazon.com/lambda/latest/dg/configuration-concurrency.html" target="_blank">docs</a>)  
  - Relevant Resource Ids: arn:aws:lambda:eu-west-1:172017021075:function:audittest