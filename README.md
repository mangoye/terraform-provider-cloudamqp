# Terraform provider for CloudAMQP

Setup your CloudAMQP cluster from Terraform

## Getting Started (As a Terraform User)

### Prerequisites
Golang, Dep

## Install

* Install golang: https://golang.org/dl/

* Install go dep: https://golang.github.io/dep/docs/installation.html

* Install terraform: https://learn.hashicorp.com/terraform/getting-started/install.html

* Create a CloudAMQP account if you haven't already:

    * Go to https://www.cloudamqp.com/

    * Click "Sign Up"

    * Sign in

    * Go to API access (https://customer.cloudamqp.com/apikeys) and create a key.
      (note that this is the API key for one of the two APIs CloudAMQP supports.
      See https://docs.cloudamqp.com/cloudamqp_api.html.  We will discuss the other
      later.)

### Install CloudAMQP Terraform Provider
```sh
go get -d -u -v github.com/cloudamqp/terraform-provider-cloudamqp
cd $GOPATH/src/github.com/cloudamqp/terraform-provider
make depupdate
make install
```

Now the provider is installed in the terraform plugins folder and ready to be used.

### Example Usage: Deploying a First CloudAMQP RMQ server

(See the examples.tf file in the repo.  It has a bunny VPC example and a simple lemur example.)

```sh
cd terraform-provider  #This is the root of the repo where examples.tf lives.
terraform plan
```
When prompted paste in your CloudAMQP API key (created above).

This will give you output on stdout that tells you what would have been created:
* rmq_lemur

Next run--
```sh
terraform apply
```

Again, paste in your API key.  This should create an actual CloudAMQP instance.

## Resources
Resource documentation can be found [here](https://docs.cloudamqp.com/cloudamqp_terraform.html)

## Import

Import existing infrastructure into state and bring the resource under Terraform management. Information about the resource will be added to the terraform.state file. Then add manually the given information to the .tf file. Once this is done, run terraform plan to see that the resource is under Terraform management. From here it's possible to add more resources such as alarm.

### Instance:

Import cloudamqp instance and bring it under Terraform management. First declare an empty instance resource in the .tf file. Followed by running the terraform import command
```
resource "cloudamqp_instance"."rmq_url" {}
```

Generic form of terraform import command
```
terraform import {resource_type}.{resource_name} {resource_id}
```

Example of terraform import command (with resource_id=80)
```
terraform import cloudamqp_instance.rmq_url 80
```

### Resources depending on an instance:

All resources depending on the instance resource also needs the instance id when using terraform import, in order to make correct API calls. Resource id and instance id is seperated with ",".

Resource affected by this is:
- cloudamqp_notification
- cloudmaqp_alarm

First declare two empty notification and alarm resources in the .tf file. Followed by running the terraform import command.
```
resource "cloudamqp_notification"."recipient_01" {}
resource "cloudamqp_alarm"."alarm_01" {}
```

Generic form of terraform import command
```
terraform import {resource_type}.{resource_name} {resource_id},{instance_id}
```

Example of terraform import command (with instance_id=80)
```
terraform import cloudamqp_notification.recipient_01 10,80
terraform import cloudamqp_alarm.alarm_01 65,80
```
