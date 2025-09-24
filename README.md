# Wazuh Proof of Concepts

Implementing [Wazuh proof of concepts](https://documentation.wazuh.com/current/proof-of-concept-guide/index.html) using Ansible

## Prerequisites

- Wazuh manager is installed and is able to connect to the internet
  - Wazuh VM: https://documentation.wazuh.com/current/deployment-options/virtual-machine/virtual-machine.html
- Wazuh agent is installed to the target OS and is able to connect to the internet
- Python is installed on the current machine
- Ansible is installed on the current machine
  - https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible-with-pip

## Usage

1. Create SSH keys for wazuh manager and agents
2. Place the public SSH key to wazuh manager and agents accordingly
3. Add the wazuh manager IP address, username, location of private SSH key and sudo password in the section `[wazuh_manager]` in the file `hosts.ini`
4. Add the wazuh agent IP address, username, location of private SSH key and sudo password in the section `[wazuh_agent]` in the file `hosts.ini`

### Running a specific role

Ansible playbook have been made to run roles individually.

The playbooks are stored in `./playbooks/` directory

To run a playbook, run the following command

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/<playbook name>.yaml
```

Example

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/ufw.yaml
```

### Running all roles

To run all roles, run the following command

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbooks/all.yaml
```

## How it works

### Ansible groups

Ansible groups are a way of grouping a list of target hosts under one entity. This allows Ansible to
run a playbook, role or task to run for a specific target.

Check
- https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html
- https://docs.ansible.com/ansible/latest/inventory_guide/intro_patterns.html

For this repo, Ansible has two groups of machines, `wazuh_manager` and `wazuh_agent` defined in the host file `hosts.ini`

This groupings can be used in the Ansible tasks to limit a certain command to run in wazuh manager and another command run on wazuh agent

Ex: when running the role [ `block-shellshock-attempt` ](./roles/block-shellshock-attempt/tasks/main.yaml)

It will

1. Enable Wazuh module `firewall-drop` in **`wazuh_manager`**
2. Install Apache web server in **`wazuh_agent`**
3. Add Apache web server logs to wazuh in **`wazuh_agent`**
4. Add active response to block Shellshock attempt in **`wazuh_manager`**
5. Restart wazuh manager
6. Restart wazuh agent

Even though all the steps mentioned is placed in one file, some of the tasks are only meant for
either `wazuh_manager` or `wazuh_agent`.

In order to only run a task in either of those groups, a `when` condition is required to limit the
execution

To limit running a task to `wazuh_manager`, add the following to the task

```yaml
  when: inventory_hostname in groups["wazuh_manager"]
```

To limit running a task to `wazuh_agent`, add the following to the task

```yaml
  when: inventory_hostname in groups["wazuh_agent"]
```

This will prevent running a task on a machine that is not designed for.

In the future, when developing more roles and tasks, make sure to use these conditions to limit the
task execution. Not using this condition will cause that task to run all machines (because there was no limit)

Check https://serverfault.com/a/1074413/1308762 for more details

---

## References

- [Wazuh proof of concepts](https://documentation.wazuh.com/current/proof-of-concept-guide/index.html)
- [CIS benchmarks](https://downloads.cisecurity.org/#/)
- [CIS benchmark Ubuntu 24.04 LTS](https://learn.cisecurity.org/l/799323/2025-06-10/4vddgt)
- [Ansible CIS lockdown for Ubuntu 24.04 LTS](https://github.com/ansible-lockdown/UBUNTU24-CIS)
