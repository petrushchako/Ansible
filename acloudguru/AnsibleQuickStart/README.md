# Ansible Quick Start

## Ansible Introduction and Architecture
- Ansible is an automauon engine that allows for agentless system configuration and deployment.
- The real power of Ansible comes from its simplicity.
- Ansible operates over SSH and runs Ansible modules on remote systems in order to complete tasks.
- Ansible is typically installed on a single, lightweight control node where a list of host inventory files, as well as "playbooks", are kept.
- The heavy lifting is generally performed on the remote host as that is where Ansible Modules are executed.