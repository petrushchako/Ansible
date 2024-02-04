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



<br><br><br><br>
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



<br><br><br><br>
### Getting Started with Ansible
-  Install Ansible on the control node.
    To install Ansible on the control node, run 
    ```bash
    sudo yum install ansible
    ```

- Configure the `ansible` user on the control node for ssh shared key access to managed nodes. Do not use a passphrase for the key pair.
    - To create a keypair for the ansible user on the control host, run the following: ```sudo su - ansible```
    - ```ssh-keygen``` (accept all defaults: press enter for each prompt)
    - Copy the public key to both **node1** and **node2**.
    - As the `ansible` user on the control host:
        - `ssh-copy-id node1` (accept the host key if prompted, authenticate as ansible user)
        - `ssh-copy-id node2` (accept the host key if prompted, authenticate as ansible user)

- Create a simple Ansible inventory on the control node in `/home/ansible/inventory` containing `node1` and `node2`.
    - On the control host:
    - `sudo su - ansible` (if not already ansible user)
    - `touch /home/ansible/inventory`
    - `echo "node1" >> /home/ansible/inventory`
    - `echo "node2" >> /home/ansible/inventory`

- Configure sudo access for Ansible on `node1` and `node2` such that Ansible may use sudo for any command with no password prompt.
    - Log in to node1 as cloud_user and edit the sudoers file to contain appropriate access for the ansible user:
        - `ssh cloud_user@node1`
        - `sudo visudo`
        - Add the following line to the file and save:
            ```yaml
            ansible    ALL=(ALL)       NOPASSWD: ALL
            ```
        - Repeate these steps for node2.

- Verify each managed node is able to be accessed by Ansible from the control node using the `ping` module. Redirect the output of a successful command to `/home/ansible/output`.
    - To verify each node, run the following as the ansible user from the control host:
        - `ansible -i /home/ansible/inventory node1 -m ping`
        - `ansible -i /home/ansible/inventory node2 -m ping`

    - To redirect output of a successful command to /home/ansible/output:
        - `ansible -i /home/ansible/inventory node1 -m ping > /home/ansible/output`






<br><br><br><br>
### Ad-Hoc Ansible Commands

To summarize, you must do the following:
- Create the user accounts noted in /home/ansible/userlist.txt.
- Copy the authorized_keys file for each user to the correct location so the new accounts can log in with ssh key authentication.
- Ensure auditd is enabled and running on all systems.




![](img/lab3.png)


<br><br><br>
- Create the User Accounts Noted in `/home/ansible/userlist.txt`

    ```bash
    ansible dbsystems -b -m user -a "name=consultant"
    ansible dbsystems -b -m user -a "name=supervisor"
    ```

    > `-b` - become
    > `-m` - module
    > `user` -
    > `-a` -

- Place Key Files in the Correct Location, `/home/$USER/.ssh/authorized_keys`, on Hosts in `dbsystems`
    
    - Create directory and add permissions

        ```bash
        ansible dbsystems -b -m file -a "path=/home/consultant/.ssh state=directory owner=consultant group=consultant mode=0755"
        ```
    - 
        ```bash
        ansible dbsystems -b -m copy -a "src=/home/ansible/keys/consultant/authorized_keys dest=/home/consultant/.ssh/authorized_keys mode=0600 owner=consultant group=consultant"
        ```
    - Create directory and add permissions

        ```bash
        ansible dbsystems -b -m file -a "path=/home/supervisor/.ssh state=directory owner=supervisor group=supervisor mode=0755"
        ```

    - 
        ```bash
        ansible dbsystems -b -m copy -a "src=/home/ansible/keys/supervisor/authorized_keys dest=/home/supervisor/.ssh/authorized_keys mode=0600 owner=supervisor group=supervisor"
        ```

- Ensure `auditd` Is Enabled and Running on All Hosts
    ```bash
    ansible all -b -m service -a "name=auditd state=started enabled=yes"
    ```


<br><br><br><br>
### Working with Ansible Inventories



<br><br><br><br>
## Plays and Playbooks
### Ansible Playbooks: The Basics



<br><br><br><br>
### Ansible Playbooks - Error Handling



<br><br><br><br>
### Working with Ansible Templates, Variables, and Facts



<br><br><br><br>
### Writing Your First Ansible Playbook




<br><br><br><br>
### Deploying Services Using Ansible



<br><br><br><br>
### Advanced Features in Ansible Playbooks




<br><br><br><br>
## Roles
### Working with Ansible Roles




<br><br><br><br>
## Working With Files
### File Manipulation with Ansible




<br><br><br><br>
## Ansible Vault
### Working with Confidential Data in Ansible


