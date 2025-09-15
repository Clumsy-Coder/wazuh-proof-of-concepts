# Wazuh Proof of Concepts

Implementing [Wazuh proof of concepts](https://documentation.wazuh.com/current/proof-of-concept-guide/index.html) using Ansible

## usage

1. create SSH keys for wazuh manager and agents
2. place the public SSH key to wazuh manager and agents accordingly
3. add the wazuh manager IP address, username, location of private SSH key and sudo password in the section `[wazuh_server]` in the file `hosts.ini`
4. add the wazuh agent IP address, username, location of private SSH key and sudo password in the section `[wazuh_agents]` in the file `hosts.ini`
5. run the following command to roles from the `./playbook.yaml` file

```bash
ANSIBLE_CONFIG=./ansible.cfg ansible-playbook ./playbook.yaml
```

