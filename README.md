# Wazuh Proof of Concepts

Implementing [Wazuh proof of concepts](https://documentation.wazuh.com/current/proof-of-concept-guide/index.html) using Ansible

## Prerequisites

- Wazuh manager is installed and is able to connect to the internet
  - Wazuh VM: https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
- Wazuh agent is installed to the target OS
- python is installed on the current machine
- ansible is installed on the current machine
  - https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip

## Usage

1. create SSH keys for wazuh manager and agents
2. place the public SSH key to wazuh manager and agents accordingly
3. add the wazuh manager IP address, username, location of private SSH key and sudo password in the section `[wazuh_server]` in the file `hosts.ini`
4. add the wazuh agent IP address, username, location of private SSH key and sudo password in the section `[wazuh_agents]` in the file `hosts.ini`

### Running a specific role

Ansible playbook have been made to run roles individually.

The playbooks are stored in `./playbooks/` directory

To run a playbook, run the following command

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/<playbook name>.yaml
```

example

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/ufw.yaml
```

### Running all roles

To run all roles, run the following command

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/all.yaml
```

## References

- [Wazuh proof of concepts](https://documentation.wazuh.com/current/proof-of-concept-guide/index.html)
- [CIS benchmarks](https://downloads.cisecurity.org/#/)
- [CIS benchmark Ubuntu 24.04 LTS](https://learn.cisecurity.org/l/799323/2025-06-10/4vddgt)
