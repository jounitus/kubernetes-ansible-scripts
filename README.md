# Ansible Scripts to setup Kubernetes Cluster

You can use these Ansible scripts to create and maintain Kubernetes cluster

The cluster will work with:

* Any cloud provider
* Bare metal servers
* Any setup that meets the requirements below


## Requirements

* You need to be able to SSH to the servers (Ansible requirement)
* The slaves must be able to connect the master (either via public ip or VPN, this is a Kubernetes requirement)


## Cluster setup

The cluster contains following nodes:

* control node (this node is used to run these ansible scripts)
* kubernetes master
* kubernetes slaves (1 or more)


## Tested versions

This script has been tested with following versions:

* Ubuntu LTS 18.04 (bionic) 
* Docker version 18.06
* Kubernetes version 1.13


## Quick tips to get started

* This script doesn't help spawn any servers, the servers need to exists when you run these scripts
* You can add more slaves to the setup later easily (just change the `hosts` file) and run the scripts again
* You must be able to `ssh` to all of the servers (because ansible uses this)
* When launching servers it's best to use public/private key to enable ssh:ing to the servers
* Make sure you change the secret in the `hosts` config which enables kubernetes slaves join the master



## Installing Python 3 version of Ansible

Install python3 version of the `pip` tool, since we want to install 
python3 version of ansible. 

We want to use python3 because some scripts will complain about python2


~~~~
apt-get update
apt-get install -y curl python3-distutils

curl https://bootstrap.pypa.io/get-pip.py -o /tmp/get-pip.py
python3 /tmp/get-pip.py
pip install ansible --upgrade
~~~~

## Make sure you can ssh to the servers

Run following commands on the control node, before running ansible playbooks /commands

~~~~
ssh-agent bash
chmod go-rwx ~/.ssh/your-private-key
ssh-add ~/.ssh/your-private-key
~~~~

## Using ansible to run these scripts

Plain version:

`ansible-playbook -i hosts all-tasks.yml`

If you use ansible vault (to help encrypt secrets in config files etc)

`ansible-playbook -i hosts --vault-password-file=~/vault-password.txt all-tasks.yml`


## To remove Kubernetes from the slaves and the master

You can remove kubernetes installation from all the servers by running the script in
`/install/delete-nodes.yml` file

After this you can run the installation script `all-tasks.yml` to get a fresh install
