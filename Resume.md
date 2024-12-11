## Document by Mahendar [ART]



![Logo](https://drive.google.com/uc?export=view&id=1GRdjOGgy8zDtZBGdIWy9DAPf2hOffomi)




# Ansible Setup for Ubuntu VMs

This README provides a step-by-step guide to install and configure Ansible on three Ubuntu VMs, set up password-less SSH authentication on both the control node and the target nodes, and run a demo playbook to install Docker on your VMs.

## Prerequisites

- Three Ubuntu VMs (VM1, VM2, VM3)
- SSH access to all VMs

## Configure the Master VM

bash
sudo useradd -m -s /bin/bash <username> 
sudo usermod -aG sudo <username>
su - <username>
sudo whoami
sudo visudo
<username> ALL=(ALL) NOPASSWD:ALL


## Configure the node1 VM

bash
sudo useradd -m -s /bin/bash <username> 
sudo usermod -aG sudo <username>
su - <username>
sudo whoami
sudo visudo
<username> ALL=(ALL) NOPASSWD:ALL


## Configure the node2 VM

bash
sudo useradd -m -s /bin/bash <username> 
sudo usermod -aG sudo <username>
su - <username>
sudo whoami
sudo visudo
<username> ALL=(ALL) NOPASSWD:ALL


## Step 1: Install Ansible on the Control Node

### Update the system
On the control node (the machine from which Ansible will be run), start by updating the system:

bash
sudo apt update -y
sudo apt upgrade -y


### Install Ansible
Run the following commands to install Ansible on Control Node:

bash
sudo apt install software-properties-common -y
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y


You can verify that Ansible is installed correctly by checking its version:

bash
ansible --version


## Step 2: Set Up Password-less SSH Authentication on Control Node
To allow Ansible to manage your Ubuntu VMs without entering a password, you need to set up SSH key-based authentication on your control node.

### Generate SSH Key Pair on the Control Node:
If you don't already have an SSH key pair on your control node, generate one with the following command:

bash
su - <master-username>
ssh-keygen -t rsa -b 2048

Press Enter to save the key to the default location (~/.ssh/id_rsa) and set a passphrase if required.

## Make sure all node VMs are switched to the relevant users that we setup before. 

### Copy the Public Key to Each VM
Use the ssh-copy-id command to copy the public key to each VM:

bash
ssh-copy-id <username>@VM1_IP
ssh-copy-id <username>@VM2_IP


Replace <username> with the respective usernames on the VMs and VM1_IP, VM2_IP, VM3_IP with the respective IP addresses of your VMs.

Alternatively, you can manually copy the contents of your ~/.ssh/id_rsa.pub file to the ~/.ssh/authorized_keys file on each VM.

### Test SSH Access
Test the SSH connection to each VM to ensure password-less authentication is working:

bash
ssh <username>@VM2_IP
ssh <username>@VM3_IP


## Step 3: Create Ansible Inventory File
Create an inventory.ini file to define your VMs:

ini
[ubuntu_vms]
VM1 ansible_host=VM2_IP> ansible_user=<username>
VM2 ansible_host=VM3_IP ansible_user=<username>


Replace VM1_IP, VM2_IP, and the usernames with the appropriate values for your environment.

Now ping all nodes.

bash
ansible ubuntu_vms -i inventory.ini -m ping


## Step 4: Create the Demo Playbook to Install Docker
Create a playbook file named <your_playbook>.yml to install Docker on your VMs:

yaml
---
- name: Install Docker on Ubuntu VMs
  hosts: ubuntu_vms
  become: yes
  tasks:
    - name: Update apt repository
      apt:
        update_cache: yes

    - name: Install required dependencies
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - curl
          - software-properties-common
        state: present

    - name: Add Docker GPG key
      command: curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

    - name: Add Docker repository
      command: add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

    - name: Install Docker
      apt:
        name: docker-ce
        state: present

    - name: Start Docker service
      service:
        name: docker
        state: started
        enabled: yes

    - name: Verify Docker installation
      command: docker --version
      register: docker_version

    - name: Display Docker version
      debug:
        msg: "Docker version: {{ docker_version.stdout }}"


This playbook will:

1. Update the apt repository.
2. Install necessary dependencies for Docker.
3. Add Docker's official GPG key and repository.
4. Install Docker.
5. Ensure the Docker service is started and enabled on boot.
6. Verify the Docker installation by checking the version.

## Step 7: Run the Playbook
To run the playbook and install Docker on your VMs, use the following command:

bash
ansible-playbook ubuntu_vms -i inventory.ini <your_playbook>.yml
