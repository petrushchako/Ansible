# Ansible Quick Start

## Introduction and Architecture
- Ansible is an automauon engine that allows for agentless system configuration and deployment.
- The real power of Ansible comes from its simplicity.
- Ansible operates over SSH and runs Ansible modules on remote systems in order to complete tasks.
- Ansible is typically installed on a single, lightweight control node where a list of host inventory files, as well as "playbooks", are kept.
- The heavy lifting is generally performed on the remote host as that is where Ansible Modules are executed.
    Use following command to check your machine info:
    ```shell
    cat /proc/cpuinfo
    free -m
    ```

## Installation and Configuration

- In order to Install Ansible, you must configure the EPEL(Extra Packages for Enterprise Linux) repository on your system.
    ```shell
    sudo yum install epel-release
    ```

- Once the EPEL repository is configured, your package manager installs Ansible and manages the
dependencies:
    ```shell
    sudo yum install ansible
    ```
- It is advisable to install some means of source
control as well:
    ```shell
    sudo yum install git
    ```

### Configuration file location
`/etc/ansible/ansible.cfg` is the primary Ansible configuration file.

Notable configurations include:
- Default inventory configuration
- Default remote user

<br>

`/etc/ansible/hosts` is the default Ansible Inventory File.
An inventory is a list of hosts that Ansible manages.

Inventory location may be specified as follows:
- Default: `/etc/ansible/hosts`
- Specified by CLI: `ansible -i`
- Can be set in `ansible.cfg`


Sample host file:
```yaml
##Hosts
#Specify port to be used by Ansible
mail.example.com ansible_port=5556

#Create alias for a host
host2 ansible_host=192.168.1.5

## Create host group
[web-servers]
mail.example.com
host2

#Access hosts with range instead of explicitly listing
host[1:4]
192.186.1.[10:15]
```


### SSH configuration

- While it is possible to connect to a remote host with Ansible via password authentication using `-k` (note lowercase), it is not a common practice as it can cause significant overhead in terms of manual intervention.

- Ansible is best implemented using a common user across all Ansible controlled systems.

- The `ssh-keygen` and `ssh-copy-id` command can facilitate creating a pre-shared key for user authentication.

- `/etc/sudoers` may be edited to allow your selected user to `sudo` any command without a password for the most automated configuration using the line:
    ```yaml
    ansible ALL= (ALL) NOPASSWD: ALL
    ```

- It is also possible to prompt for a sudo password at runtime using `-K` (note uppercase) if desired. Note that this can become a challenge when executing against many systems.

#### SSH config best-practice
In this example we will have **A1**, **N1** and **N2** machines (A-Ansible host, and N1-2 Nodes to be managed by Ansible. )

1. Create a user on **A1**:
    ```shell
    sudo useradd ansible
    ```
    Note: Do not create a password

2. Login into a **N1** machine and create the same user as on A1:

    ```shell
    ssh scoldham2c.mylabserver.com

    sudo useradd ansible
    sudo passwd ansible

    logout
    ```

3. On **A1** machine, switch to **ansible** user, then create **ssh key** and copy it to the H1 machine:

    ```shell
    sudo su - ansible
    ssh-keygen

    ssh-copy-id scoldham2c.mylabserver.com
    ```

4. Test that ssh key got propagated to **H1** by loggin in as ansible use. Note, if ssh key was successfully added, you will not be asked to provide password. 

    ```shell
    ssh scoldham2.mylabserver.com
    ```

5. Add `sudo` priviledges to `ansible` user, by modifying `sudoers` file:

    ```yaml
    sudo visudo
    > ansible ALL=(ALL) NOPASSWD: ALL
    ```

<br><br><br>
### Note, that in some cases home directory might not be created. 
Follow steps :

If you ran the adduser command with a custom home directory specified, and the directory is still not created, there might be an issue. Here are a few things to check:

1. Check for Errors:
    After running the adduser command, check for any error messages that might indicate what went wrong. If there were any issues, they should be displayed during the user creation process.

2. Check Directory Permissions:
    Ensure that the directory where you are trying to create the home directory (`/ansible` in your case) is writable by the user running the command. If there are permission issues, the directory creation might fail.

3. Manual Directory Creation:
    You can manually create the directory and set the correct ownership:
    ```bash
    sudo mkdir /ansible
    sudo chown ansible:ansible /ansible
    ```
    Then, run the adduser command again to see if it successfully uses the existing directory.

4. Check `/etc/passwd`:
    After running the adduser command, check the /etc/passwd file to see if the home directory path is correctly set:
    ```bash
    grep ansible /etc/passwd
    ```
    This should show a line with the user details, including the specified home directory.

5. Review Command Syntax:
    Ensure that the syntax of the adduser command is correct, and there are no typos or mistakes:
    ```bash
    sudo adduser --home /ansible ansible
    ```
    Double-check for any mistakes in the command.

6. Check Disk Space:
    Verify that there is sufficient disk space available on the system. A lack of disk space could prevent the creation of directories.
