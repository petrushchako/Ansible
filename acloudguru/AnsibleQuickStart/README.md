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

- In order to Install Ansible, you must configure the EPEL repository on your system.

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