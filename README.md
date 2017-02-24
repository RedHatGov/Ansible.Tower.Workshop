# Ansible Tower Workshop

![ansible](img/Ansible-Tower-Logotype-Large-RGB-FullGrey-300x124.png)

`Ansible_Tower_Workshop` is a ansible playbook to provision Ansible Tower in AWS. This playbook uses Ansible to wrap Terraform, for provisioning AWS infrastructure and nodes. To find more info about Terraform [check here](https://www.terraform.io/docs/providers/aws/index.html)

These modules all require that you have AWS API keys available to use to provision AWS resources. You also need to have IAM permissions set to allow you to create resources within AWS. There are several methods for setting up you AWS environment on you local machine.

Export the AWS API Keys by hand;

```
export AWS_ACCESS_KEY_ID='****************WFQ'
export AWS_SECRET_ACCESS_KEY='****************TFHJw'
```

This repo also requires that you have Ansible installed on your local machine. For the most upto date methods of installing Ansible for your operating system [check here](http://docs.ansible.com/ansible/intro_installation.html).

This repo also requires that Terraform be installed if you are using the aws.infra.terraform role. For the most upto data methods of installing Terraform for your operating system [check here](https://www.terraform.io/downloads.html).



## AWS Infrastructure Roles


### roles/aws.infra.terraform

To create infrastructure and a Ansible Tower instance via Terraform

```
brew install terraform
```

Then edit `roles/aws.terraform/vars/main.yml` and fill in the vars with your AWS api info. This role can also provide easy domain name mapping to all the instances if you have a domain registered in AWS Route 53. 


```
#####################################################
# Domain Name you own
#####################################################
domain_name: ""
zone_id: ""

#####################################################
# AWS API Keys for terraform.tfvars file
#####################################################
aws_access_key: ""
aws_secret_key: ""
```

```
ansible-playbook -i inventory aws_infra_terraform.yml --tags "tf_create" 
```

To destroy

```
cd /tmp/terraform
terraform destroy
```

## Configure Ansible Tower




### roles/ansible.tower

The ansible.tower role configures the instance with the `tower-cli`. By default the `ansible.tower` role installs 15 student accounts and associates them with a default `Lab Team` & `Lab Organization`.

To target the newly created EC2 instance use the `ec2.py` module located in the `/inventory/` folder. The [ec2.py](http://docs.ansible.com/ansible/intro_dynamic_inventory.html) is a dynamic script that queries Amazon for your instances. 

 
```
ansible-playbook -i inventory 2_aws_ec2.yml
```

## Login to Ansible Tower

browse to the public ip of the EC2 instance and enter the `admin` password located in `roles/ansible.tower/vars/main.yml`. 

![Login](img/ansible-tower.png)



