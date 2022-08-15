---
id: terraform-aws-deployment
title: Terraform AWS Deployment
description: "Deploy Polygon Edge network on AWS cloud provider using Terraform"
keywords:
  - docs
  - polygon
  - edge
  - deployment
  - terraform
  - aws
  - script
---
:::info Production deployment guide
This is the official, production ready, fully automated, AWS deployment guide.   

Manual deployments to the ***[Cloud](set-up-ibft-on-the-cloud)*** or ***[Local](set-up-ibft-locally)*** 
are recommended for testing and/or if your cloud provider is not AWS.
:::

:::info
This deployment is PoA only.   
If PoS mechanism is needed, just follow this ***[guide](/docs/edge/consensus/migration-to-pos)*** on now to make a switch from PoA to PoS.
:::

This guide will, in detail, describe the process of deploying a Polygon Edge blockchain network on the AWS cloud provider, 
that is production ready as the validator nodes are spanned across multiple availability zones.

## Prerequisites

### System tools
* [terraform](https://www.terraform.io/)
* [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
* [aws access key ID and secret access key](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-prereqs.html#getting-started-prereqs-keys)

### Terraform variables
Three variables that must be provided, before running the deployment:

* `account_id` - the AWS account ID that the Polygon Edge blockchain cluster will be deployed on
* `alb_ssl_certificate` - the ARN of the certificate from AWS Certificate Manager to be used by ALB for https protocol.   
  The certificate must be generated before starting the deployment, and it must have **Issued** status
* `premine` - the account that will receive pre mined native currency.
  Value must follow the official [CLI](cli-commands#genesis-flags) flag specification

## Deployment information
### Deployed resources
High level overview of the resources that will be deployed:

* Dedicated VPC
* 4 validator nodes (which are also boot nodes)
* 4 NAT gateways to allow nodes outbound internet traffic
* Lambda function used for generating the first (genesis) block and starting the chain
* Dedicated security groups and IAM roles
* S3 bucket used for storing genesis.json file
* Application Load Balancer used for exposing the JSON-RPC endpoint

### Fault tolerance

Only regions that have 4 availability zones are required for this deployment. Each node is deployed in a single AZ.

By placing each node in a single AZ, the whole blockchain cluster is fault-tolerant to a single node (AZ) failure, as Polygon Edge implements IBFT
consensus which allows a single node to fail in a 4 validator node cluster.

### Command line access

Validator nodes are not exposed in any way to the public internet (JSON-PRC is accessed only via ALB)
and they don't even have public IP addresses attached to them.  
Node command line access is possible only via [AWS Systems Manager - Session Manager](https://aws.amazon.com/systems-manager/features/).

### Base AMI upgrade

This deployment uses `ubuntu-focal-20.04-amd64-server` AWS AMI. It will **not** trigger EC2 *redeployment* if the AWS AMI gets updated.

If, for some reason, base AMI is required to get updated,
it can be achieved by running the `terraform taint` command for each instance, before `terraform apply`.   
Instances can be tainted by running the    
`terraform taint module.instances[<instance_number>].aws_instance.polygon_edge_instance` command.

Example:
```shell
terraform taint module.instances[0].aws_instance.polygon_edge_instance
terraform taint module.instances[1].aws_instance.polygon_edge_instance
terraform taint module.instances[2].aws_instance.polygon_edge_instance
terraform taint module.instances[3].aws_instance.polygon_edge_instance
terraform apply
```

:::info
In a production environment `terraform taint` should be run one-by-one in order to keep the blockchain network functional.
:::

## Deployment procedure

### Pre deployment steps
* read through the [polygon-technology-edge](https://registry.terraform.io/modules/aws-ia/polygon-technology-edge/aws) terraform registry readme
* add the `polygon-technology-edge` module to your `main.tf` file using *provision instructions* on the modules' readme page
* run the `terraform init` command to install all necessary Terraform dependencies
* provide a new certificate in [AWS Certificate Manager](https://aws.amazon.com/certificate-manager/)
* make sure that the provided certificate is in the **Issued** state and take a note of the certificate's **ARN**
* set up your output statement in order to get modules' output in the cli

#### `main.tf` example
```terraform
module "polygon-edge" {
  source  = "aws-ia/polygon-technology-edge/aws"
  version = ">=0.0.1"

  account_id          = var.account_id
  premine             = var.premine
  alb_ssl_certificate = var.alb_ssl_certificate
}

output "json_rpc_dns_name" {
  value       = module.polygon-edge.jsonrpc_dns_name
  description = "The dns name for the JSON-RPC API"
}

variable "account_id" {
  type        = string
  description = "Your AWS Account ID"
}

variable "premine" {
  type        = string
  description = "Public account that will receive premined native currency"
}

variable "alb_ssl_certificate" {
  type        = string
  description = "The ARN of SSL certificate that will be placed on JSON-RPC ALB"
}
```

#### `terraform.tfvars` example
```terraform
account_id          = "123456789"
premine             = "0xFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF"
alb_ssl_certificate = "arn:aws:acm:us-west-2:123456789:certificate/64c7f117-61f5-435e-878b-83186676a8af"
```

### Deployment steps
* create the `terraform.tfvars` file
* set the required terraform variables in this file (as explained above).
  :::info
  There are other non-mandatory variables that can fully customize this deployment. 
  You can override the default values by adding your own to the `terraform.tfvars` file.   

  Specification of all available variables can be found in modules' Terraform ***[registry](https://registry.terraform.io/modules/aws-ia/polygon-technology-edge/aws)***
  :::
* make sure that you've set up an aws cli authentication properly by running `aws s3 ls` - there should be no errors
* deploy the infrastructure `terraform apply`

### Post deployment steps
* once the deployment is finished, take note of the `json_rpc_dns_name` variable value printed in the cli
* create a public dns cname record pointing your domain name to the provided `json_rpc_dns_name` value. For example:
  ```shell
  # BIND syntax
  # NAME                            TTL       CLASS   TYPE      CANONICAL NAME 
  rpc.my-awsome-blockchain.com.               IN      CNAME     jrpc-202208123456879-123456789.us-west-2.elb.amazonaws.com.
  ```
* once the cname record propagates, check if the chain is working properly by calling your JSON-PRC endpoint.   
  From the example above:
  ```shell
    curl  https://rpc.my-awsome-blockchain.com -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_chainId","params":[],"id":1}'
  ```
  
## Destroy procedure
:::warning
The following procedure will permanently delete your entire infrastructure deployed with these terraform scripts.    
Make sure you have proper [blockchain data backups](working-with-node/backup-restore) and/or you're working with a testing environment.
:::

If you need to remove the whole infrastructure, run the following command `terraform destroy`.   
Additionally, you will need to manually remove secrets stored in AWS [Parameter Store](https://aws.amazon.com/systems-manager/features/) 
for the region the deployment took place.