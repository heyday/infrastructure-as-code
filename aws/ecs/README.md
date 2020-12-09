# Elastic Container Service (ECS) Stack

This stack will set up ECS cluster-service-task-container with options to include database (RDS MySQL Aurora Read Replica) and storage (S3 Public Bucket).

- [Stack Summary](#stack-summary)
- [AWS CLI Procedure](#aws-cli-procedure)
- [AWS CLI RAW Template Procedure](#aws-cli-raw-template-procedure)
- [Future Improvements](#future-improvements)

## Stack Summary

* ECS with Fargate launch type
* Application Load Balancer to manage traffic between 2 tasks in www service
* Redis Service launched from ECS as service discovery
* RDS MySQL Aurora with Read Replica _(Optional)_
* S3 Public Bucket _(Optional)_

## AWS CLI Procedure

1. Go to terminal in project root folder.

2. Copy parameters file example: `cp vendor/heyday/infrastructure-as-code/aws/ecs/parameters.json.example any-file-name.json`.

3. Change the parameter values into relevant value according to your task. Below is an example with mandatory fields:
    ```
    [
    {
        "ParameterKey": "AppName",
        "ParameterValue": "risk"
    },
    {
        "ParameterKey": "AppEnv",
        "ParameterValue": "preprod"
    }
    ]
    ```
    * `AppName` must be __alphanumeric only__
    * `AppEnv` must be __alphanumeric only__, and better be actual environment name such as `production`,`prod`,`test`,`staging`, etc...

    Below is an example with optional fields:
    ```
    [
    {
        "ParameterKey": "CreateRelationalDatabaseSystem",
        "ParameterValue": "true"
    },
    {
        "ParameterKey": "CreateSimpleStorageService",
        "ParameterValue": "true"
    },
    {
        "ParameterKey": "S3ARN",
        "ParameterValue": "aws-arn-s3-permission-here"
    },
    {
        "ParameterKey": "SourceCidr",
        "ParameterValue": "1.2.3.4/32"
    }
    ] 
    ```
    * `CreateRelationalDatabaseSystem` set to string `"true"` if you want to create RDS database. _Default: "true"_
    * `CreateSimpleStorageService` set to string `"true"` if you want to create S3 bucket. _Default: "true"_
    * `S3ARN` is role ARN created in AWS console IAM role which has policy to execute S3 actions. _Default: ""_
    * `SourceCidr` is the ip that can initally access the newly created stack, `0.0.0.0/0` means it's publicly accessible. Change it to another ip restrict public access. _Default: "0.0.0.0/0"_

4. Execute command `vendor/heyday/infrastructure-as-code/aws/ecs/create any-file-name.json`.

5. Once stack is created, modify RDS database password [here](https://aws.amazon.com/premiumsupport/knowledge-center/reset-master-user-password-rds/).

## AWS CLI RAW Template Procedure

This method requires [AWS CloudFormation](https://aws.amazon.com/cloudformation/) knowledge. If you prefer **flexibility** over **simplicity**, use this method.

1. Go to terminal in project root folder.

2. Execute command: `vendor/heyday/infrastructure-as-code/aws/ecs/create-template any-stack-name`.
    > Parameter 1: Stack Name

3. A YML file should be created. This will create a file which contains entire infrastructure codes.

4. Roll out the infrastructure: `vendor/heyday/infrastructure-as-code/aws/ecs/deploy-template any-stack-name aws-profile`.
    > Parameter 1: Stack Name
    > Parameter 2: AWS Profile (must be set on [AWS Credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html))

## Future Improvements

- [x] Remove the needs of pulling image the first time (use default image instead)
- [x] Create tester ip parameter (avoid making it public the first time)
- [x] Simplify aws cli command (maybe create alias)
- [x] Refactor template (so it can be managed easier)
- [x] Create ECS task role if one does not exist yet
- [x] Raw template options (more flexibility)
- [ ] AWS credentials as environment variables (easier accounts switching)
- [ ] Improve dynamics of Availability Zone Distribution
- [ ] Buddy with IaC