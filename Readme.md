



The dependent roles for tomcat Jenkins and nexus are in below repositories.
Clone them and add the path to ansible.cfg

Repositories:
https://github.com/Revathi-Santhosh/ansible-modules.git


# ROLES PATH
For example, I have added roles path for tomcat8, which is in ~/personal/ansible-modules/roles. Here ansible checks for roles in path `roles/`  in the current directory if the roles are not found in current directory roles path then it looks for roles in ~/personal/ansible-modules/roles directory. We can provide any number for roles path and ansible checks in order
roles_path = roles:~/personal/ansible-modules/roles:/etc/ansible/roles


# Using dynamic inventory

Create multiple tags for EC2 instances for keyed_groups

Tags:

Env: dev
App: Tomcat/Devops/python
Name: Individul ec2 names


Use ansible AWS  ec2 plugin for generating dynamic inventory it needs aws key and secrets, and boto 3 needs to be installed.
Below inventory creates all instances with tag value for environments having dev in all groups
And all the hosts having same tag value for tag name App will be created in a single group like prefix_value  here is dev_stack_appvalue
And all the hosts having equal tag value for tag name Name will be created in a single group like prefix_value  here is dev_namevalue
Example:
```

plugin: aws_ec2
regions:
  - us-east-1
filters:
   tag:Env: dev
keyed_groups:
  - key: tags.App
    prefix: dev_stack
  - key: tags.Name
    prefix: dev
hostnames:
  - ip-address

```
For ece2 plugin to work boto3 needs to be installed

Run below command:
Python needs to be installed.

pip install nose tornado boto3


Now creds to used to authenticate to AWS we can use the profile or provide us secret and access key_name

Using Profile:(locally)

Export profile to env variable AWS_PROFILE
export AWS_PROFILE=dev-hand

or

USING AWS SECRET KEY AND ACCESS key (For Jenkins jobs)

SET any one of below env for secret key
env:EC2_SECRET_KEY
env: AWS_SECRET_KEY
env:AWS_SECRET_ACCESS_KEY

and anyone of var of access key


env:EC2_ACCESS_KEY
env:AWS_ACCESS_KEY
env:AWS_ACCESS_KEY_ID

EG:
AWS_SECRET_KEY="sdasfafaa"
AWS_ACCESS_KEY="sadsad"


# Checking you dynamic inventory

ansible-inventory -i aws_ec2.yml -y --graph

Storing inventory in a file
ansible-inventory -i aws_ec2.yml -y --graph --output a.yml
or
ansible-inventory -i aws_ec2.yml -y --list --output a.yml


Check inventory created:
In our scenario we have four instances
all instances have three tags Env, App and name

All instances have tag Env=dev_nexus.

All DevOps instances has tag App=Devops(Nexus and Jenkins instances)

All tomcat insatcnes has tag App=tomcat(addressbook_1,addressbook_2)

each instance has differnt tag name  Name=(jenkins,nexus,addressbook_2,addressbook_1)

Below inventory with prefix value provide in aws_ec2.yaml
```

@all:
  |--@aws_ec2:
  |  |--34.201.12.209
  |  |--34.205.252.232
  |  |--35.174.241.22
  |  |--54.157.112.194
  |--@dev_addressbook_1:
  |  |--35.174.241.22
  |--@dev_addressbook_2:
  |  |--34.201.12.209
  |--@dev_jenkins:
  |  |--54.157.112.194
  |--@dev_nexus:
  |  |--34.205.252.232
  |--@dev_stack_Devops:
  |  |--34.205.252.232
  |  |--54.157.112.194
  |--@dev_stack_Tomcat:
  |  |--34.201.12.209
  |  |--35.174.241.22
  |--@ungrouped:

  ```
