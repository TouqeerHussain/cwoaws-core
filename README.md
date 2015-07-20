# CWOAWS CORE Ansible playbook


Note:  This is a core Ansible Playbook, please review, customize per you needs, and test it first before applying on production environments! Please report any issues through GitHub or email me at alex@cloudwebops.com and I'll do my best to get back to you!



Recorded Terminal Session: http://cwoaws.s3-website.eu-central-1.amazonaws.com/session4.html

CWOAWS CORE Ansible playbook have a purpose to setup operational AWS VPC infrastructure, all services on EC2 instances, RDS, etc, and to allow later execution of separated Playbooks for deployments, etc.

CWOAWS CORE Ansible Playbook Tasks:
-  create 1 x VPC with 2 x VPC subnets in differrent AZ zones one AWS region,
-  create 1 x security group,
-  provision 2 x EC2 instances in 2 different AZ,
-  Install webservers role on both instances (includes of other roles is possible, webservers role is required to allow instances registration to ELB),
-  launch and configure public facing VPC ELB (cross_az_load_balancing) and attach VPC subnets,
-  deregister and register EC2 instances on ELB,
-  info about ELB dnsname is registered in vars file and later can be used for DNS configuration,

All information about VPC are defined in main.yml and vpc_info.yml(during playbook execution) files (AZ, regions, keypair, vpc, subnets, elb dns name info) 
.

# Installation

Ansible and Boto installation Ubuntu 14.04 LTS
-  $ sudo apt-get install software-properties-common
-  $ sudo apt-add-repository ppa:ansible/ansible
-  $ sudo apt-get update
-  $ sudo apt-get install ansible
-  $ sudo apt-get install python-pip
-  $ sudo pip install boto

Download to folder /etc/ansible/
-  $ wget https://raw.github.com/ansible/ansible/devel/plugins/inventory/ec2.py
-  $ wget https://raw.github.com/ansible/ansible/devel/plugins/inventory/ec2.ini
-  $ chmod +x /etc/ansible/ec2.py


In aws console create IAM user with PowerUser privileges;
Add in file /home/ubuntu/.boto
-   [Credentials]
-   aws_access_key_id = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
-   aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

Add in clean file /home/ubuntu/.ansible.cfg
-  [defaults]
-  host_key_checking = False
-   private_key_file=/home/ubuntu/keypair.pem

Run Playbook:
-  $ansible-playbook -i hosts deploy-aws-infra.yml 



