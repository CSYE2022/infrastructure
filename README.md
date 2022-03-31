
  

  

# Infrastructure

  

Repo for CSYE6225 infrastructure.

  

## Prerequisites

  

This project would require an **AWS account** and **AWS CLI** to be set up.

  

To set up AWS CLI use the following link: https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html

  

  

## CloudFormation

  

Use the following link to understand CloudFormation template anatomy

  

https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html.

  

## Environment Variables or VM arguments.

  

For **dev** region use:

  

````

  

export AWS_PROFILE=dev

  

export AWS_REGION=us-east-1

  

````

  

  

For **demo** region use:

  

````

  

export AWS_PROFILE=demo

  

export AWS_REGION=us-east-1

  

````

  

  

## Commands to set up the VPC

  

Use the following commands to set up the VPC instance.

  

#### Generic command

  

```

  

aws cloudformation create-stack --stack-name <stack name> --template body

  

file://<filename> --parameters

  

ParameterKey=VPCCidrBlock,ParameterValue="<IP>"

  

ParameterKey=CIDRBlocSubnet1,ParameterValue="<IP>"

  

ParameterKey=CIDRBlocSubnet2,ParameterValue="<IP>"

  

ParameterKey=CIDRBlocSubnet3,ParameterValue="<IP>"

  

ParameterKey=AvailabilityZone1,ParameterValue="<avaiabilityZone(a,b,c)>"

  

ParameterKey=AvailabilityZone2,ParameterValue="<avaiabilityZone(a,b,c)>"

  

ParameterKey=AmiImageId,ParameterValue="<ami-id>"

  

ParameterKey=AvailabilityZone3,ParameterValue="<avaiabilityZone(a,b,c)>"

  

--region <region>

  

```

  

#### Demo for multiple VPCs in same region

  

- To set up first instance

  

```

aws cloudformation create-stack --stack-name demovpc-1 --template-body file://csye6225-infra.yml --parameters ParameterKey=DNSDomain,ParameterValue="prod.ebenezerwilliams.me." ParameterKey=EnvRunning,ParameterValue="prod" ParameterKey=AmiImageId,ParameterValue="ami-05ae6c450a784f9c6" ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-1 --capabilities CAPABILITY_NAMED_IAM

  
  
  

```

  

- To set up second instance

  

```

aws cloudformation create-stack --stack-name demovpc-2 --template-body file://csye6225-infra.yml --parameters ParameterKey=DNSDomain,ParameterValue="prod.ebenezerwilliams.me." ParameterKey=EnvRunning,ParameterValue="prod" ParameterKey=AmiImageId,ParameterValue="ami-05ae6c450a784f9c6" ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-1 --capabilities CAPABILITY_NAMED_IAM

  
  
  

```

##### Delete S3

  

- Use the following command to delete the S3 instance.

  

````

aws s3 rm s3://1d089840-9ff1-11ec-94b9-125e1d1edc59.dev.ebenezerwilliams.me --recursive

  

````

  

##### Delete the servers

  

```

  

aws cloudformation delete-stack --stack-name demovpc-1 --region us-east-1

  

```

  

```

  

aws cloudformation delete-stack --stack-name demovpc-2 --region us-east-1

  

```

  

#### Demo for multiple VPCs in different regions

  

- To set up first instance in us-east-1 (N. Virginia)

  

```

  

aws cloudformation create-stack --stack-name demovpc-1 --template-body file://csye6225-infra.yml --parameters ParameterKey=DNSDomain,ParameterValue="prod.ebenezerwilliams.me." ParameterKey=EnvRunning,ParameterValue="prod" ParameterKey=AmiImageId,ParameterValue="ami-0535925c33eb26de3" ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-1 --capabilities CAPABILITY_NAMED_IAM

  
  

```

  

- To set up second instance in us-east-2 (Ohio)

  

```

  

aws cloudformation create-stack --stack-name demovpc-2 --template-body file://csye6225-infra.yml --parameters

  

ParameterKey=AmiImageId,ParameterValue="ami-0000"

  

ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-2

  

```

  

##### Delete the servers

  

- Use the following commands to delete the servers you created

  

```

  

aws cloudformation delete-stack --stack-name demovpc-1 --region us-east-1

  

```

  

```

  

aws cloudformation delete-stack --stack-name demovpc-2 --region us-east-2

  

```

  

##### Describe stacks

  

- Use the following command to check the server instances at any point.

  

````

  

aws cloudformation describe-stacks

  

````

  

##### Create CI/CD stack

  

- Use the following command to create the ci/cd stack.

  

````

  

aws cloudformation create-stack --stack-name codedeploystack --template-body file://ci-cd-infra.yml --parameters ParameterKey=CodeDeployBucketName,ParameterValue="codedeploy.ebenezerwilliams.me" --region us-east-1 --profile=demo --capabilities CAPABILITY_NAMED_IAM

  
  

````

  
  

##### Update Stack

  

- Use the following command to check the server instances at any point.

  

````

  

aws cloudformation deploy --stack-name demovpc-1 --template-file csye6225-infra.yml --capabilities CAPABILITY_NAMED_IAM

  

````
