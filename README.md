
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
ParameterKey=AvailabilityZone3,ParameterValue="<avaiabilityZone(a,b,c)>"
--region <region>
``` 
#### Demo for multiple VPCs in same region  
- To set up first instance  
```  
aws cloudformation create-stack --stack-name demovpc-1 --template-body file://csye6225-infra.yml --parameters ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-1
```  
- To set up second instance    
```  
aws cloudformation create-stack --stack-name demovpc-2 --template-body file://csye6225-infra.yml --parameters ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-1
```  
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
aws cloudformation create-stack --stack-name demovpc-1 --template-body file://csye6225-infra.yml --parameters ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-1
```  
- To set up second instance in us-east-2 (Ohio)
```  
aws cloudformation create-stack --stack-name demovpc-2 --template-body file://csye6225-infra.yml --parameters ParameterKey=VPCCidrBlock,ParameterValue="10.2.0.0/16" ParameterKey=CIDRBlocSubnet1,ParameterValue="10.2.1.0/24" ParameterKey=CIDRBlocSubnet2,ParameterValue="10.2.2.0/24" ParameterKey=CIDRBlocSubnet3,ParameterValue="10.2.3.0/24" ParameterKey=AvailabilityZone1,ParameterValue="a" ParameterKey=AvailabilityZone2,ParameterValue="b" ParameterKey=AvailabilityZone3,ParameterValue="c" --region us-east-2
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