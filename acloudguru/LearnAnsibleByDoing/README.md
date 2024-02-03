# Learn Ansible by Doing
- **[Installation and Configuration](#installation-and-configuration-1)**
    - [Deploying Ansible](#deploying-ansible)
    - [Getting Started with Ansible](#getting-started-with-ansible)
    - [Ad-Hoc Ansible Commands](#ad-hoc-ansible-commands)
    - [Working with Ansible Inventories](#working-with-ansible-inventories)
- **[Plays and Playbooks](#plays-and-playbooks-1)**
    - [Ansible Playbooks: The Basics](#ansible-playbooks-the-basics)
    - [Ansible Playbooks - Error Handling](#ansible-playbooks---error-handling)
    - [Working with Ansible Templates, Variables, and Facts](#working-with-ansible-templates-variables-and-facts)
    - [Writing Your First Ansible Playbook](#writing-your-first-ansible-playbook)
    - [Deploying Services Using Ansible](#deploying-services-using-ansible)
    - [Advanced Features in Ansible Playbooks](#advanced-features-in-ansible-playbooks)
- **[Roles](#roles-1)**
    - [Working with Ansible Roles](#working-with-ansible-roles)
- **[Working With Files](#working-with-files-1)**
    - [File Manipulation with Ansible](#file-manipulation-with-ansible)
- **[Ansible Vault](#ansible-vault-1)**
    - [Working with Confidential Data in Ansible](#working-with-confidential-data-in-ansible)




## Installation and Configuration
### Deploying Ansible

- Install Ansible on the control host.
    Run the following commands on the control host:
    ```shell
    sudo yum install epel-release
    sudo yum install ansible
    ```
- Create an `ansible` user on both the control host and workstation host being sure to set a password you can remember.
    On each host, run the noted commands below. Make sure you set a password you can remember (you will need it later).

    Assuming you are logged in as cloud_user:
    ```shell
    sudo useradd ansible
    sudo passwd ansible
    ```
- Configure a pre-shared key for Ansible that allows the user to log in from `control` to `workstation` without a password.
    Assuming you are logged into control as **cloud_user**, run the following commands providing the appropirate passwords when prompted and default options otherwise:
    ```bash
    sudo -i -u ansible # (provide cloud_user a sudo password)
    ssh-keygen # (accept default options by pressing enter)
    ssh-copy-id workstation # (provide ansible user a password)
    logout
    ```
- Configure the Ansible user on the workstation host so that Ansible may sudo without a password.
    Log into the workstation host as cloud_user and run the following commands:
    ```bash
    sudo visudo
    # Add text at the end of the file that is opened:
    ansible ALL=(ALL) NOPASSWD: ALL
    # Save file:
    # (:wq in vim)
    ```

- Create a simple inventory in `/home/ansible/inventory` consisting of only the `workstation` host.

    On the control host as the ansible user run the following commands:
    ```bash
    vim /home/ansible/inventory
    ```
    (note: you may use any text editor with which you are comfortable)
    Add the text "workstation" to the file and save using (`:wq in vim`).

- Write an Ansible playbook in `/home/ansible/git-setup.yml` on the control node that installs `git` on `workstation` then execute the playbook.
    On the control host as the ansible user run the following commands:
    ```bash
    vim /home/ansible/git-setup.yml # (You may use any text editor with which your are comfortable.)
    ```
    Add the following text to the file:
    ```yaml
    --- # install git on target host
    - hosts: workstation
      become: yes
      tasks:
      - name: install git
        yum:
          name: git
          state: latest
    ```
    Save (:wq in vim) and quit the text editor.

    **Run** 
    ```
    ansible-playbook -i /home/ansible/inventory /home/ansible/git-setup.yml
    ```




### Getting Started with Ansible


### Ad-Hoc Ansible Commands


### Working with Ansible Inventories




## Plays and Playbooks
### Ansible Playbooks: The Basics


### Ansible Playbooks - Error Handling


### Working with Ansible Templates, Variables, and Facts


### Writing Your First Ansible Playbook


### Deploying Services Using Ansible


### Advanced Features in Ansible Playbooks




## Roles
### Working with Ansible Roles



## Working With Files
### File Manipulation with Ansible



## Ansible Vault
### Working with Confidential Data in Ansible


