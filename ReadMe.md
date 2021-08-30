# Ansible DevOps.
#Push configuration tool where server/master pushes to node/client.
#Playbooks
#Inventory
#Ansible Tower

## Objectives
- To setup Ansible master and client nodes with Docker containers


## Maual Steps to create Ansible Master
- docker pull ubuntu:latest

- docker run -itd --name ansible_master ubuntu /bin/bash
- docker ps
- docker attach  ansible_master
- apt update
- apt install python ansible openssh-client vim iputils-ping -y
- Get IP Addresses of master and client containers.
  docker network inspect bridge
  IP Address = 172.17.0.2
- SSH Key generation for master-client communcation
  ssh-keygen
- SSH copy ID with client IDP Address
  ssh-copy-id  root@172.17.0.3
- check if master able to connect to client with ssh
  ssh root@172.17.0.3
- exit

- Make client machine/container entry in master ansible Inventory
  vim /etc/ansible/hosts
  add client ip under [myclients]
- Ping client machine from master
  ansible -m ping 172.17.0.3


## Maual Steps to create Ansible client node

- docker run -itd --name ansible_client ubuntu /bin/bash
- docker attach  ansible_client
- Install dependecies
  apt update
  apt install ssh vim python -y
- Set root password
  passwd root # welcome
- Check SSH configuration on client and update permitRootLogin to yes
  vim /etc/ssh/sshd_config
- restart ssh service
  service ssh restart

## Get IP Addresses of master and client containers.
- docker network inspect bridge

## Build Ansible master from Dockerfile
- docker build -f ./Dockerfile.ansible-master .

## Build Ansible client from Dockerfile
- docker build -f ./Dockerfile.ansible-client .