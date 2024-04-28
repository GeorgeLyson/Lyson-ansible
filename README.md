# dg-ansible-playbooks

# Ansible Playbooks Repository
## Overview
This repository contains a collection of central Ansible playbooks used by our team to automate the configuration and management of our infrastructure. These playbooks cover a variety of tasks, including but not limited to provisioning, configuration management, application deployment, and orchestration of our IT environments.

## Pre-requisites
To use these playbooks, you will need to first install ansible on your local

### Installing Ansible on Ubuntu
Ubuntu builds are available in a [PPA here](https://launchpad.net/~ansible/+archive/ubuntu/ansible).

To configure the PPA on your system and install Ansible run these commands:
```
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

### SSH access to remote servers
To use sudo, you must generate a password hash whose value you will add to `passwd` field in the groups_vars below.
1. Install of mkpasswd binary

    Install whois because mkpasswd binary is a part of whois package, eg for ubuntu: 
    ```
    sudo apt install whois
    ```

    Install mkpasswd binary for MacOsx 
    ```
    sudo gem install mkpasswd
    ```
2. Generate your hash with: `mkpasswd -m sha-512 -R 10000`

3. Input your password and get the hash
 

Make sure you have your SSH key added to the remote servers to be managed and assigned sudo by adding details to the following files
1. `inventory/group_vars/all.yml`
2. `roles/users/files/<firstname-lastname>.key.pub`

Ask an Admin to add your keys and credentials by running 
```
ansible-playbook deploy-guru.yml -t users
```

### How to use
Add your sudo password into environ variables
`export ANSIBLE_BECOME_PASS='your_super_secure_sudo_password'` or append the line to your `~/.bashrc` and restart your terminal.

For general deployment, use the convenience playbook
```
ansible-playbook deploy-guru.yml -t users
```

To restrict deployment to particular servers use **limit**
```
ansible-playbook deploy-guru.yml -l web
```

To restrict deployment to tagged tasks, use **tags**
```
ansible-playbook deploy-guru.yml -t nginx
```

To work with individual playbooks, pass path to the actual playbook
```
ansible-playbook playbooks/configure_web_servers.yml
```