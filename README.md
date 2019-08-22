# Ansible: Spark cluster (on Amazon EC2 and local cluster)
This repository has been created by forking this [repo](https://github.com/phamthuonghai/ansible-spark-ec2). As ansible has been updated in the last few years and several other issues with the old repo, I decided to overhaul it.

## Compatibility:
I have designed and tested these playbooks with Ansible 2.4.3 and it is supposed to work for  Ubuntu systems (both 16.04 and 18.04 should work but some other versions should be compatible too)

## Prelims
Install Ansible
```bash
sudo apt-get install software-properties-common
sudo apt-add-repository ppa:ansible/ansible
sudo apt-get update
sudo apt-get install ansible
```

### Setup for EC2
* Install python boto package
```bash
sudo apt-get install python-pip
sudo pip install boto
```
* Put your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` to `~/.boto` file:
```
[profile DSPlatform]
aws_access_key_id = ABCDEFGHIJKLMNOPQRST
aws_secret_access_key = XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```
* Check your access to AWS
```
./inventory/ec2.py --list
```
* Add your keypair to Amazon EC2 (see http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html#how-to-generate-your-own-key-and-import-it-to-aws)

## Configurations
Take a look at following two configuration files. You will like need to make some changes there.

Set your variables in `./group_vars/all/main.yml`

Set your ansible configurations in `ansible.cfg`

## Setup and Start cluster on EC2
Run the playbook
``` bash
ansible-playbook -i inventory/ec2.py ds_platform.yaml
```

## Setup and Start Local Spark Cluster
In order to configure a private Spark cluster, you will need to create the hosts inventory file.
Take a look at `hosts.ini` as an example.

The run the playbook using
``` bash
ansible-playbook -i <inventory_file> local_platform.yaml
```

The command above with setup hadoop, spark and start a YARN cluster. If you don't want to setup YARN then you can executed the following command instead
``` bash
ansible-playbook -i <inventory_file> local_platform.yaml --skip-tags "yarn"
```

## Start EC2 cluster with pre-existing AMI
``` bash
ansible-playbook -i inventory/ec2.py ds_platform.yaml --skip-tags "yarn,ami"
```

### TODO:
- Remove the use of ssh in spark setup
- Use Terraform for creating cluster
- Fix the service script for jupyter notebooks
- Fix the Hbase install
