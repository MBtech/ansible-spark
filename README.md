# Ansible: Spark standalone cluster (on Amazon EC2 and local cluster)
This repository has been created by forking this [repo](https://github.com/phamthuonghai/ansible-spark-ec2). As ansible has been updated in the last few years and several other issues with the old repo, I decided to overhaul it.

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

## Start cluster on EC2
Run the playbook
``` bash
ansible-playbook ds_platform.yaml
```

## Start Local Spark Cluster
In order to configure a private Spark cluster, you will need to create the hosts inventory file.
Take a look at `hosts.ini` as an example.

The run the playbook using
``` bash
ansible-playbook -i <inventory_file> local_platform.yaml
```

### TODO:
- Separate the producer for a YARN cluster setup from a standalone Spark Setup
- Fix the service script for jupyter notebooks
- Fix the Hbase install
