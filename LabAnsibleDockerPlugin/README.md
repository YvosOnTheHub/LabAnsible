##################################
## LAB ANSIBLE - FIRST STEPS
##################################

### SSH into host5

```bash
ssh root@192.168.0.66
password: Netapp1!
```

### Start the *RPCBind* service (required to mount NFS resources)

```bash
service rpcbind start
service rpcbind status
```

### Clone the repository in the centos environment

```bash
git clone https://github.com/YvosOnTheHub/LabAnsible.git
```

### Copy the Trident backend file in /etc/netappdvp

```bash
cp ~/LabAnsible/LabAnsibleDockerPlugin/0-Trident-config/config-ontap-nas.json /etc/netappdvp/
```

### Install Trident as a Docker plugin & check it is active

```bash
docker plugin install netapp/trident-plugin:19.07 --alias ontap-nas --grant-all-permissions config=config-ontap-nas.json 
docker plugin ls
```

### Create persistent docker volumes (one local & one on NetApp backend)

```bash
docker volume create ssh-keys
docker volume create -d ontap-nas --name ansible -o size=1g
docker volume ls
```

### Run docker container with NetApp Ansible modules configured

```bash
docker run -it -v ansible:/etc/ansible -v ssh-keys:/root/.ssh schmots1/netapp-ansible /bin/bash
```

### Depending on what lab you want to showcase, you will find the next steps in the following sub-dir

- [1_Lab_NAS](1_Lab_NAS): Create & Mount 2 NFS volumes on 3 hosts
- [2_Lab_NAS_iSCSI](2_Lab_NAS_iSCSI): Create & Mount NFS volumes & iSCSI LUNs on 3 hosts
- [3_Lab_NAS_SnapMirror](3_Lab_NAS_SnapMirror): Create, Mount & Mirror 2 NFS volumes
- [4_Lab_NetApp_Roles](4_Lab_NetApp_Roles): Use roles provided as part of the NetApp Ansible Collection