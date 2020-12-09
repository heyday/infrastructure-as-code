# Amazon Web Services (AWS)

AWS provides [AWS CloudFormation](https://aws.amazon.com/cloudformation/) as a solution to create Infrastructure as Code (IAC). 

To start using this feature, make sure:

* Access to AWS Console is granted
* Installation and configuration of [AWS Command Line Interface (CLI)](https://aws.amazon.com/cli/) is set up. 
* [jq](https://stedolan.github.io/jq/download/) package is installed if AWS CLI is preferred

By default the project will ignore .json files on [aws folder and subfolders](/aws).

## Available Stacks

* __[Elastic Container Service (ECS) Stack](/aws/ecs/README.md)__
  