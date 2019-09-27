
# open bash terminal in VSCODE + clone code repository locally
(in vscode) 
terminal --> new terminal 

(in new terminal)  
cd Documents
git clone https://github.com/YvosOnTheHub/LabAnsible.git
cd LabAnsible/LabAnsibleDockerPlugin

# ssh into "Rhel5"
ssh root@192.168.0.66   
password: Netapp1!

# clone the repository in the centos environment
git clone https://github.com/YvosOnTheHub/LabAnsible.git

# copy the Trident backend file in /etc/netappdvp
cp ~/LabAnsible/LabAnsibleDockerPlugin/config-ontap-nas-default.json /etc/netappdvp/

# install Trident as a Docker plugin
docker plugin install netapp/trident-plugin:18.07 --alias ontap-nas --grant-all-permissions config=config-ontap-nas-default.json

# create persistent docker volumes (one local & one on NetApp backend)
docker volume create ssh-keys
docker volume create -d ontap-nas --name ansible -o size=1g
docker volume ls

# Run docker container with NetApp Ansible modules configured
docker run -it -v ansible:/etc/ansible -v ssh-keys:/root/.ssh schmot1s/netapp-ansible /bin/bash

### you are now in the container

# clone github repo
cd /etc/ansible/
git clone https://github.com/YvosOnTheHub/LabAnsible.git
cd LabAnsible/LabAnsibleDockerPlugin

# create and share ssh-keys with remote RHEL host
ssh-keygen 
ssh-copy-id root@192.168.0.66

# copy hosts file to correct directory
cp hosts /etc/ansible/

# ping host to check connectivity with RHEL host 
ansible -m ping rhel

# install NFS utils on RHEL Host with ansible playbook  (change into repository directory!)
ansible-playbook install-nfs-utils.yml

# run playbook for single volume just to try it out
ansible-playbook flexvol-create.yml

# run role to configure ONTAP cluster (inspect it in VSCODE to see what it does!)
ansible-playbook cluster-role.yml 


