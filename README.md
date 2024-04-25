# static-site

A simple static site CloudFormation stack.

## Description

This repo deploys AWS resources to support a simple static website. It includes
a CloudFront distribution set up to serve assets from an S3 bucket. The assets
to serve are provided in a Lambda layer, referenced through the
StaticAssetsLayerArn parameter.

## Getting started

static-site is available via the AWS Serverless Application Repository. To include
it in your CloudFormation template and use it to serve your static site:

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: 'AWS::Serverless-2016-10-31'
Description: An example stack showing the use of static-site
Resources:

  StaticAssetsDeployment:
    Type: AWS::Serverless::Application
    Properties:
      Location:
        ApplicationId: 'arn:aws:serverlessrepo:eu-west-2:211125310871:applications/static-site'
        SemanticVersion: <CURRENT_VERSION>
      Parameters:
        StaticAssetsLayerArn: !Ref StaticAssetsLayer

  StaticAssetsLayer:
    Type: AWS::Serverless::LayerVersion
    Properties:
      LayerName: static-assets
      Description: Static assets layer.
      ContentUri: ./static-assets
      CompatibleRuntimes:
        - python3.12
      CompatibleArchitectures:
        - arm64
      RetentionPolicy: Delete
    Metadata:
      BuildMethod: python3.12
      BuildArchitecture: arm64

```

Then you can deploy your stack. These steps assume you have the [SAM CLI installed and set up for your environment](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html):

```
$ sam build
$ sam deploy \
    --capabilities CAPABILITY_IAM CAPABILITY_AUTO_EXPAND \
    --stack-name example-stack \
    --resolve-s3
```
