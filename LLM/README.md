# Ansible Basics

Ansible is an open-source automation tool for IT tasks such as configuration management, application deployment, intraservice orchestration, and provisioning. 

**Key terms:**

- **Playbook:** A playbook is an organized unit of scripts in Ansible that defines work for a server configuration. It is written in YAML format.

- **Role:** An independent, reusable collection of tasks, files, templates, and modules which can be automatically loaded into playbooks.

- **Inventory:** A list of nodes containing the hostname, IP addresses, and ports of the nodes on which tasks should be run.

- **Module:** A reusable, standalone script that Ansible runs on your behalf.

## Installation

You can install Ansible on most Unix-like OS, for example, on Ubuntu:

```sh
sudo apt-get update
sudo apt-get install software-properties-common
sudo apt-add-repository --yes --update ppa:ansible/ansible
sudo apt-get install ansible
```

## Configuration Management

After installing Ansible, a configuration file will be placed at /etc/ansible/ansible.cfg. The default settings can be altered by modifying this file.

## Inventory Files

Inventory files define the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. By default, inventory file is located in `/etc/ansible/hosts`.

Here is an example of how an Ansible inventory file looks:

```sh
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
```

## Ansible Playbook

Ansible playbooks are a way to send commands to remote computers in a scripted way. They are written in YAML.

Here is an example playbook:

```yml
---
- name: Install package
  hosts: all
  tasks:
    - name: Ensure Apache is installed
      apt:
        name: apache2
        state: present
...
```

This playbook ensures that the Apache web server is installed on all hosts.

## Execution of Playbooks

To execute a playbook, use the ansible-playbook command:

```sh
ansible-playbook myplaybook.yml
```

This command will execute the playbook named myplaybook.yml.

## Some Ansible Commands

- **ansible --version:** Displays the version of Ansible installed.
- **ansible all -m ping:** Test the connection and see if all hosts are responsive
